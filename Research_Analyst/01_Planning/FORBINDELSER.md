# FORBINDELSER - RESEARCH ANALYST

## Connection Documentation

**Commander ID:** R-003
**Connection Count:** 5 Direct Connections
**Last Updated:** 2025-12-24

---

## CONNECTION DIAGRAM

```
┌─────────────────┐              ┌─────────────────────────────┐
│    RESEARCH     │◀────────────▶│                             │
│   COMMANDER     │    Tasks &   │                             │
│    (R-001)      │    Results   │                             │
└─────────────────┘              │                             │
                                 │   RESEARCH ANALYST          │
┌─────────────────┐              │        (R-003)              │
│     WEB         │─────────────▶│                             │
│   RESEARCHER    │   Raw Data   │   Analysis Specialist       │
│    (R-002)      │              │                             │
└─────────────────┘              │                             │
                                 │                             │
┌─────────────────┐              │                             │
│   SOCRATIC      │◀─────────────│                             │
│    TEACHER      │  Structured  └─────────────────────────────┘
│    (R-004)      │   Knowledge          │
└─────────────────┘                      │
                                         ▼
┌─────────────────┐              ┌─────────────────┐
│   BLINDSPOT     │─────────────▶│   REFLECTION    │
│  ILLUMINATOR    │              │     AGENT       │
│    (R-011)      │              │    (R-012)      │
└─────────────────┘              └─────────────────┘
```

---

## CONNECTION REGISTRY

| ID | Target | Direction | Purpose |
|----|--------|-----------|---------|
| R003-C01 | Research Commander | Bidirectional | Task & results |
| R003-C02 | Web Researcher | Inbound | Raw data |
| R003-C03 | Socratic Teacher | Outbound | Knowledge delivery |
| R003-C04 | BlindSpot Illuminator | Inbound | Alternative views |
| R003-C05 | Reflection Agent | Outbound | Validation |

---

## DATA FLOW

```
Raw Data ────▶ Analysis ────▶ Synthesis ────▶ Insights
                  │                              │
                  ▼                              ▼
             Patterns                       Conclusions
```

---

**Document Status:** COMPLETE

