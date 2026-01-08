# ROLLE - CSA COMMANDER

## Calendar & Scheduling Agent Commander

**Commander ID:** H-002
**Division:** HASA (Home-Automation & Scheduling Agents)
**Priority:** P1 (Phase 1 - Foundation)
**Status:** INITIATED

---

## IDENTITY

### Official Designation
**Name:** CSA Commander (Calendar & Scheduling Agent)
**Type:** Master Commander
**Domain:** Time Management & Scheduling Intelligence

### Visual Identity
```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│     ██████╗███████╗ █████╗                                              │
│    ██╔════╝██╔════╝██╔══██╗                                             │
│    ██║     ███████╗███████║                                             │
│    ██║     ╚════██║██╔══██║                                             │
│    ╚██████╗███████║██║  ██║                                             │
│     ╚═════╝╚══════╝╚═╝  ╚═╝                                             │
│                                                                          │
│    Calendar & Scheduling Agent                                          │
│    "Your Intelligent Time Companion"                                     │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## ROLE DESCRIPTION

### Primary Purpose
CSA Commander manages all calendar and scheduling operations for the Cirkelline ecosystem. It handles event creation, modification, conflict detection, intelligent scheduling suggestions, and time-aware reminders. The CSA ensures users maintain optimal time management through proactive assistance.

### Core Responsibilities

1. **Calendar Management**
   - Event creation and modification
   - Multi-calendar synchronization
   - Calendar visualization and navigation
   - Recurring event handling

2. **Intelligent Scheduling**
   - Optimal time slot suggestions
   - Conflict detection and resolution
   - Travel time estimation
   - Workload balancing

3. **Reminder System**
   - Smart reminder generation
   - Context-aware notifications
   - Escalating reminder chains
   - Deadline tracking

4. **Time Analytics**
   - Time usage patterns
   - Productivity insights
   - Schedule optimization suggestions
   - Meeting efficiency metrics

### Scope
- **In Scope:** Calendar CRUD, scheduling optimization, reminders, time analytics
- **Out of Scope:** Deep research (delegate to Research Division), real-time monitoring (delegate to FEIA)

---

## CAPABILITIES

### Technical Capabilities

| Capability | Description | Status |
|------------|-------------|--------|
| Event Management | Full CRUD for calendar events | Planned |
| Conflict Detection | Identify scheduling conflicts | Planned |
| Smart Scheduling | AI-powered time slot suggestions | Planned |
| Reminder Engine | Intelligent notification system | Planned |
| Calendar Sync | Multi-source calendar integration | Planned |
| Time Analytics | Usage pattern analysis | Planned |

### Operational Parameters

```typescript
interface CSAConfig {
  calendars: {
    sources: CalendarSource[];
    syncInterval: number;        // milliseconds
    conflictThreshold: number;   // minutes of overlap
  };
  scheduling: {
    workingHours: TimeRange;
    preferredMeetingDuration: number;
    bufferBetweenEvents: number;
    travelTimeEnabled: boolean;
  };
  reminders: {
    defaultLeadTime: number;     // minutes
    escalationEnabled: boolean;
    maxRemindersPerEvent: number;
  };
  analytics: {
    trackingEnabled: boolean;
    historyDepth: number;        // days
    insightsFrequency: string;   // 'daily' | 'weekly'
  };
}
```

### Functional Boundaries

| Function | CSA Handles | Delegates To |
|----------|-------------|--------------|
| Event creation | ✓ | - |
| Scheduling optimization | ✓ | - |
| Reminders | ✓ | - |
| Time analytics | ✓ | - |
| Real-time monitoring | ✗ | FEIA Commander |
| User interaction | ✗ | Chat Commander |
| Location research | ✗ | Research Division |

---

## INTEGRATION POINTS

### Upstream Connections (Receives From)
| Source | Data Type | Purpose |
|--------|-----------|---------|
| Chat Commander | User requests | Calendar commands |
| FEIA Commander | Context triggers | Schedule-aware alerts |
| External Calendars | Calendar data | Event synchronization |

### Downstream Connections (Sends To)
| Target | Data Type | Purpose |
|--------|-----------|---------|
| Chat Commander | Confirmations | Event notifications |
| FEIA Commander | Schedule events | Trigger monitoring |
| Notification Service | Reminders | User alerts |

---

## BEHAVIORAL GUIDELINES

### Scheduling Intelligence
```
PRIORITY MATRIX
├── Critical (Cannot move)
│   ├── Hard deadlines
│   ├── External commitments
│   └── Recurring obligations
│
├── High (Prefer not to move)
│   ├── Important meetings
│   ├── Focus time blocks
│   └── Key deliverables
│
├── Medium (Flexible)
│   ├── Internal meetings
│   ├── Routine tasks
│   └── Optional activities
│
└── Low (Easily movable)
    ├── Buffer time
    ├── Optional catch-ups
    └── Administrative tasks
```

### Conflict Resolution
1. Preserve higher priority events
2. Suggest alternative times for conflicts
3. Consider travel/transition time
4. Respect working hour boundaries
5. Account for user preferences

### Communication Style
- Clear, time-specific language
- Proactive conflict warnings
- Concise reminder messages
- Helpful scheduling suggestions

---

## PERFORMANCE EXPECTATIONS

### Latency Targets

| Operation | Target | Maximum |
|-----------|--------|---------|
| Event Creation | < 200ms | 500ms |
| Conflict Check | < 100ms | 300ms |
| Schedule Suggestion | < 500ms | 1500ms |
| Calendar Sync | < 2s | 5s |

### Quality Metrics

| Metric | Target | Threshold |
|--------|--------|-----------|
| Scheduling Accuracy | > 95% | > 90% |
| Conflict Detection | 100% | 99% |
| Reminder Delivery | > 99% | > 95% |
| Sync Reliability | > 99.5% | > 98% |

---

## SOURCE REFERENCE

### Original Source File
**Path:** `backend/agents/csa/`
**Language:** Python
**Framework:** AGNO v2

### Key Components
| Component | File | Purpose |
|-----------|------|---------|
| EventManager | event_manager.py | CRUD operations |
| Scheduler | scheduler.py | Optimization logic |
| ReminderEngine | reminders.py | Notification handling |
| CalendarSync | sync.py | Multi-source sync |
| TimeAnalyzer | analytics.py | Usage analysis |

---

## LEARNING REQUIREMENTS

### Prerequisites
- Calendar system fundamentals
- Time zone handling
- Scheduling algorithms
- Notification patterns

### Certification Modules
1. Calendar Management Fundamentals
2. Intelligent Scheduling Algorithms
3. Reminder System Design
4. Calendar Integration Patterns
5. Time Analytics Methods

---

## APPENDIX

### A. Glossary

| Term | Definition |
|------|------------|
| Event | A scheduled calendar entry with time and metadata |
| Conflict | Overlapping events that cannot coexist |
| Buffer | Protected time between events |
| Recurrence | Pattern for repeating events |

### B. Related Documents

- FORBINDELSER.md - Connection specifications
- LEARNING_CURRICULUM.md - Training modules
- TECH_SPEC.md - Technical specifications

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Next Review:** Upon implementation start

