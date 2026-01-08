# DATABASE SCHEMA - Commander-and-Agent

**Project:** Commander-and-Agent (Documentation Framework)
**Version:** 2.0.0
**Status:** DOKUMENTATION 100% - Implementation 0%
**Last Updated:** 2025-12-29
**Created:** 2025-12-24

---

## CURRENT STATUS

**Current Implementation Phase:** Documentation Only

The commander-and-agent project is currently in the documentation phase. No database schema has been implemented, and no database infrastructure exists yet.

---

## PLANNED DATABASE ARCHITECTURE

### Database Selection Rationale

For the Commander-and-Agent system, the recommended database technology is:

- **Primary:** PostgreSQL 15+
- **Secondary Storage:** Redis (for caching and session management)
- **Document Store:** MongoDB (optional, for flexible logging)

Rationale:
- PostgreSQL provides ACID compliance and strong data integrity required for mission-critical research operations
- Supports JSON/JSONB for flexible Commander state storage
- Horizontal scalability via replication for 25+ concurrent agents
- Full-text search capabilities for research data
- PostGIS extension for geographic operations (Geo-Tech Commanders)

---

## PROPOSED CORE ENTITY SCHEMA

### Future Implementation Reference

When implementation begins in Phase 1, the following core entities should be created:

#### COMMANDERS Table
```sql
CREATE TABLE commanders (
    id SERIAL PRIMARY KEY,
    commander_id VARCHAR(10) UNIQUE NOT NULL,  -- e.g., M-001, R-013, S-005
    name VARCHAR(255) NOT NULL,
    division VARCHAR(20) NOT NULL,              -- MOBILE, HASA, RESEARCH, SPECIALIST
    status VARCHAR(50) NOT NULL,                -- INITIATED, IN_PROGRESS, COMPLETE
    priority VARCHAR(5) NOT NULL,               -- P1, P2, P3
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    metadata JSONB
);
```

#### TASKS Table
```sql
CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    task_id VARCHAR(50) UNIQUE NOT NULL,       -- e.g., TASK 1.1.1
    commander_id VARCHAR(10) NOT NULL,
    task_name VARCHAR(500) NOT NULL,
    phase INTEGER NOT NULL,
    sequence_order INTEGER NOT NULL,
    status VARCHAR(50),                        -- NOT_STARTED, IN_PROGRESS, BLOCKED, COMPLETE
    dependencies TEXT[],
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (commander_id) REFERENCES commanders(commander_id)
);
```

#### DOCUMENTATION Table
```sql
CREATE TABLE documentation (
    id SERIAL PRIMARY KEY,
    commander_id VARCHAR(10) NOT NULL,
    doc_type VARCHAR(50) NOT NULL,             -- ROLLE, FORBINDELSER, TECH_SPEC, etc.
    content TEXT,
    path VARCHAR(500),
    version VARCHAR(20) DEFAULT '1.0.0',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (commander_id) REFERENCES commanders(commander_id)
);
```

#### CONNECTIONS Table
```sql
CREATE TABLE connections (
    id SERIAL PRIMARY KEY,
    from_commander VARCHAR(10) NOT NULL,
    to_commander VARCHAR(10) NOT NULL,
    connection_type VARCHAR(100),              -- RESEARCH, SUPPORT, CONTROL, REPORT
    bidirectional BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (from_commander) REFERENCES commanders(commander_id),
    FOREIGN KEY (to_commander) REFERENCES commanders(commander_id)
);
```

#### PERFORMANCE_METRICS Table
```sql
CREATE TABLE performance_metrics (
    id SERIAL PRIMARY KEY,
    commander_id VARCHAR(10) NOT NULL,
    metric_name VARCHAR(100) NOT NULL,
    metric_value FLOAT,
    recorded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (commander_id) REFERENCES commanders(commander_id)
);
```

#### STATUS_HISTORY Table
```sql
CREATE TABLE status_history (
    id SERIAL PRIMARY KEY,
    commander_id VARCHAR(10) NOT NULL,
    status VARCHAR(50) NOT NULL,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    notes TEXT,
    FOREIGN KEY (commander_id) REFERENCES commanders(commander_id)
);
```

---

## DATA RELATIONSHIPS

### Hierarchical Structure

```
COMMANDERS (25 total)
├── Organized by DIVISION
│   ├── MOBILE (5)
│   ├── HASA (2)
│   ├── RESEARCH (13)
│   └── SPECIALIST (5)
├── Link to TASKS (multiple per commander)
├── Link to DOCUMENTATION (multiple per commander)
├── Link to CONNECTIONS (multiple per commander)
└── Link to PERFORMANCE_METRICS (ongoing per commander)
```

### Connection Matrix

All inter-commander connections defined in FORBINDELSER.md documents should be stored in the CONNECTIONS table for quick query access.

Example: Research Commander (R-001) connects to:
- Chat Commander (M-001) - ORCHESTRATION
- Web Researcher (R-002) - DELEGATION
- Research Analyst (R-003) - COORDINATION

---

## INDEXING STRATEGY

Recommended indexes for optimal query performance:

```sql
-- Primary lookups
CREATE INDEX idx_commanders_id ON commanders(commander_id);
CREATE INDEX idx_commands_division ON commanders(division);
CREATE INDEX idx_commanders_status ON commanders(status);

-- Task lookups
CREATE INDEX idx_tasks_commander ON tasks(commander_id);
CREATE INDEX idx_tasks_phase ON tasks(phase);
CREATE INDEX idx_tasks_status ON tasks(status);

-- Documentation lookups
CREATE INDEX idx_docs_commander ON documentation(commander_id);
CREATE INDEX idx_docs_type ON documentation(doc_type);

-- Connection lookups
CREATE INDEX idx_connections_from ON connections(from_commander);
CREATE INDEX idx_connections_to ON connections(to_commander);

-- Historical queries
CREATE INDEX idx_status_history_commander ON status_history(commander_id);
CREATE INDEX idx_status_history_timestamp ON status_history(timestamp);

-- Full-text search (if content grows large)
CREATE INDEX idx_documentation_content ON documentation USING GIN (to_tsvector('english', content));
```

---

## SCALING CONSIDERATIONS

### Phase 1 (Foundation)
- ~500MB-1GB initial database size
- Single PostgreSQL instance sufficient
- Daily backups recommended

### Phase 2 (Expansion)
- ~2-5GB estimated data volume
- Begin considering replication
- Read replicas for documentation queries
- Connection pooling (PgBouncer) recommended

### Phase 3 (Specialists)
- ~5-10GB estimated data volume
- Production-grade backup strategy
- Multi-region failover planning
- Sharding strategy if needed

### Phase 4 (Optimization)
- Production deployment
- Horizontal scaling with read replicas
- Load balancing across multiple instances
- Real-time sync capabilities

---

## BACKUP & DISASTER RECOVERY

### Planned Strategy (Pre-Implementation)

| Frequency | Method | Retention |
|-----------|--------|-----------|
| Daily | Full backup | 30 days |
| Weekly | Incremental | 12 weeks |
| Monthly | Archive | 1 year |

### Point-in-Time Recovery
- Transaction logs retained for 7 days
- Enables recovery to any point within 7 days
- Full backup + log replay for older dates

---

## COMPLIANCE & SECURITY

### Data Protection
- All sensitive Commander metadata encrypted at rest
- Connections use SSL/TLS
- Query logging disabled for sensitive operations
- Audit trail maintained for access logging

### Compliance Notes
- GDPR-ready field structure (personal data flagging)
- Data retention policies configurable per division
- Right-to-be-forgotten support (soft deletes)

---

## MIGRATION STRATEGY

### From Documentation to Database

When implementation begins:

1. **Phase 1A:** Parse all ROLE.md files → COMMANDERS table
2. **Phase 1B:** Extract TODO.md entries → TASKS table
3. **Phase 1C:** Link FORBINDELSER.md → CONNECTIONS table
4. **Phase 2:** Implement connection pooling and caching
5. **Phase 3:** Add performance monitoring and analytics
6. **Phase 4:** Production hardening and scaling

---

## TODO - IMPLEMENTATION CHECKLIST

- [ ] PostgreSQL 15+ server deployment
- [ ] Database initialization scripts
- [ ] Schema creation and indexing
- [ ] Initial data seeding from documentation
- [ ] Connection pooling configuration
- [ ] Backup automation setup
- [ ] Replication configuration
- [ ] Monitoring and alerting setup
- [ ] Performance baseline testing
- [ ] Security audit and hardening

---

## ÆNDRINGSLOG

| Dato | Tid | Handling | Af |
|------|-----|----------|-----|
| 2026-01-01 | 23:50 | 11_DATABASE_SCHEMA.md oprettet | Kv1nt |

---

**Document Status:** TEMPLATE FOR FUTURE IMPLEMENTATION
**Authority:** Development Team
**Next Step:** Begin Phase 1 implementation with database setup
