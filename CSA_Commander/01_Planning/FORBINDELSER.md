# FORBINDELSER - CSA COMMANDER

## Connection Documentation & Integration Specifications

**Commander ID:** H-002
**Connection Count:** 5 Direct Connections
**Last Updated:** 2025-12-24

---

## CONNECTION DIAGRAM

```
                                    ┌─────────────────┐
                                    │   EXTERNAL      │
                                    │   CALENDARS     │
                                    │ (Google, Apple) │
                                    └────────┬────────┘
                                             │
                                             ▼
┌─────────────────┐              ┌─────────────────────────────┐
│     CHAT        │◀────────────▶│                             │
│   COMMANDER     │   Commands   │                             │
│    (M-001)      │   & Status   │                             │
└─────────────────┘              │                             │
                                 │     CSA COMMANDER           │
┌─────────────────┐              │        (H-002)              │
│     FEIA        │◀────────────▶│                             │
│   COMMANDER     │   Events &   │   Calendar & Scheduling     │
│    (H-001)      │   Triggers   │        Agent                │
└─────────────────┘              │                             │
                                 │                             │
┌─────────────────┐              │                             │
│   RESEARCH      │◀─────────────│                             │
│   COMMANDER     │   Location   └─────────────────────────────┘
│    (R-001)      │   Queries              │
└─────────────────┘                        │
                                           ▼
                                 ┌─────────────────┐
                                 │  NOTIFICATION   │
                                 │    SERVICE      │
                                 └─────────────────┘
```

---

## CONNECTION REGISTRY

### Direct Connections

| ID | Target | Direction | Protocol | Purpose |
|----|--------|-----------|----------|---------|
| H002-C01 | Chat Commander (M-001) | Bidirectional | Internal API | Commands & Status |
| H002-C02 | FEIA Commander (H-001) | Bidirectional | Internal API | Events & Triggers |
| H002-C03 | Research Commander (R-001) | Outbound | Internal API | Location Queries |
| H002-C04 | External Calendars | Inbound/Outbound | REST/OAuth | Calendar Sync |
| H002-C05 | Notification Service | Outbound | Push API | Reminders |

### Connection Matrix

```
            ┌─────┬─────┬─────┬─────┬─────┬─────┬─────┐
            │M-001│M-002│H-001│R-001│R-002│S-001│ EXT │
      ┌─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
      │H-002│  ⬌  │  -  │  ⬌  │  →  │  -  │  -  │  ⬌  │
      └─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘

      Legend: ⬌ = Bidirectional, → = Outbound, ← = Inbound, - = None
```

---

## CONNECTION SPECIFICATIONS

### H002-C01: Chat Commander Connection

**Purpose:** Primary interface for calendar commands and confirmations

```typescript
// Calendar Command Protocol
interface CalendarCommand {
  commandId: string;
  type: 'create' | 'update' | 'delete' | 'query' | 'suggest';
  payload: {
    event?: EventData;
    query?: CalendarQuery;
    preferences?: SchedulingPreferences;
  };
  context: {
    userId: string;
    sessionId: string;
  };
}

// Calendar Response Protocol
interface CalendarResponse {
  commandId: string;
  success: boolean;
  result: {
    events?: CalendarEvent[];
    suggestions?: TimeSlot[];
    conflicts?: Conflict[];
    message: string;
  };
  metadata: {
    processingTime: number;
    affectedCalendars: string[];
  };
}

// Event Data Structure
interface EventData {
  title: string;
  startTime: number;
  endTime: number;
  location?: string;
  description?: string;
  attendees?: string[];
  recurrence?: RecurrenceRule;
  reminders?: ReminderConfig[];
  priority?: 'critical' | 'high' | 'medium' | 'low';
}
```

**Communication Flow:**
```
Chat Commander                       CSA Commander
      │                                   │
      │──── Calendar Command ────────────▶│
      │                                   │
      │◀──── Processing Status ───────────│
      │                                   │
      │◀──── Command Response ────────────│
      │                                   │
      │◀──── Reminder Notification ───────│
      │                                   │
```

---

### H002-C02: FEIA Commander Connection

**Purpose:** Schedule-aware monitoring and event-triggered actions

```typescript
// Schedule Event Notification
interface ScheduleEventNotification {
  eventType: 'upcoming' | 'started' | 'ended' | 'changed' | 'cancelled';
  event: CalendarEvent;
  context: {
    leadTime: number;       // minutes until event
    relatedEvents: string[];
  };
}

// Monitoring Request
interface ScheduleMonitorRequest {
  requestId: string;
  eventId: string;
  triggers: {
    onStart: boolean;
    onEnd: boolean;
    beforeMinutes?: number[];
  };
  actions: {
    type: 'alert' | 'context' | 'research';
    params: Record<string, any>;
  }[];
}

// Schedule Context
interface ScheduleContext {
  currentEvent?: CalendarEvent;
  nextEvent?: CalendarEvent;
  todayEvents: CalendarEvent[];
  busyUntil: number;
  freeSlots: TimeSlot[];
}
```

**Communication Flow:**
```
CSA Commander                       FEIA Commander
      │                                   │
      │──── Schedule Event ──────────────▶│
      │                                   │
      │──── Monitor Request ─────────────▶│
      │                                   │
      │◀──── Context Trigger ─────────────│
      │                                   │
      │──── Schedule Context ────────────▶│
      │                                   │
```

---

### H002-C03: Research Commander Connection

**Purpose:** Location and venue research for scheduling

```typescript
// Location Query
interface LocationQuery {
  queryId: string;
  type: 'venue' | 'route' | 'availability';
  params: {
    location?: string;
    origin?: string;
    destination?: string;
    timeConstraints?: TimeRange;
  };
}

// Location Response
interface LocationResponse {
  queryId: string;
  result: {
    travelTime?: number;       // minutes
    distance?: number;         // km
    venueInfo?: VenueDetails;
    availability?: TimeSlot[];
  };
  confidence: number;
  sources: string[];
}
```

---

### H002-C04: External Calendar Connection

**Purpose:** Multi-source calendar synchronization

```typescript
// Calendar Source Configuration
interface CalendarSource {
  id: string;
  type: 'google' | 'apple' | 'outlook' | 'caldav';
  name: string;
  credentials: {
    type: 'oauth' | 'api_key' | 'basic';
    data: EncryptedCredential;
  };
  syncSettings: {
    interval: number;
    bidirectional: boolean;
    conflictResolution: 'local' | 'remote' | 'manual';
  };
}

// Sync Operation
interface SyncOperation {
  operationId: string;
  sourceId: string;
  direction: 'pull' | 'push' | 'both';
  changes: {
    created: CalendarEvent[];
    updated: CalendarEvent[];
    deleted: string[];
  };
  status: 'pending' | 'in_progress' | 'completed' | 'failed';
}
```

---

### H002-C05: Notification Service Connection

**Purpose:** Reminder and alert delivery

```typescript
// Reminder Delivery
interface ReminderDelivery {
  reminderId: string;
  eventId: string;
  userId: string;
  channel: 'push' | 'email' | 'sms' | 'in_app';
  content: {
    title: string;
    body: string;
    actions?: NotificationAction[];
  };
  scheduledTime: number;
  priority: 'high' | 'normal' | 'low';
}

// Delivery Result
interface DeliveryResult {
  reminderId: string;
  delivered: boolean;
  deliveredAt?: number;
  channel: string;
  error?: string;
}
```

---

## DATA FLOW DIAGRAMS

### Event Creation Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   User       │────▶│   Validate   │────▶│   Check      │
│   Request    │     │   Input      │     │   Conflicts  │
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
                     ┌───────────────────────────┘
                     │
                     ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   External   │◀────│   Create     │◀────│   Schedule   │
│   Sync       │     │   Event      │     │   Reminders  │
└──────────────┘     └──────────────┘     └──────────────┘
```

### Scheduling Suggestion Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Query      │────▶│   Analyze    │────▶│   Apply      │
│   Free Slots │     │   Patterns   │     │   Constraints│
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
                     ┌───────────────────────────┘
                     │
                     ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Response   │◀────│   Rank       │◀────│   Generate   │
│              │     │   Options    │     │   Suggestions│
└──────────────┘     └──────────────┘     └──────────────┘
```

---

## ERROR HANDLING

### Connection Failure Protocols

| Error Type | Detection | Response | Recovery |
|------------|-----------|----------|----------|
| Calendar Sync Failed | API error | Use cached data | Retry with backoff |
| Conflict Detected | Overlap check | Notify user | Suggest alternatives |
| Notification Failed | Delivery error | Queue retry | Escalate channel |
| External API Timeout | > 5s response | Use stale data | Background retry |

### Graceful Degradation

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     CSA DEGRADATION LEVELS                               │
│                                                                          │
│   LEVEL 0: FULL OPERATION                                               │
│   ├── Real-time sync                                                    │
│   ├── Smart scheduling                                                  │
│   └── Full notifications                                                │
│                                                                          │
│   LEVEL 1: REDUCED SYNC                                                 │
│   ├── Periodic sync only                                                │
│   ├── Basic scheduling                                                  │
│   └── Priority notifications                                            │
│                                                                          │
│   LEVEL 2: LOCAL ONLY                                                   │
│   ├── No external sync                                                  │
│   ├── Manual scheduling                                                 │
│   └── Critical notifications                                            │
│                                                                          │
│   LEVEL 3: READ ONLY                                                    │
│   ├── View only mode                                                    │
│   ├── No modifications                                                  │
│   └── Emergency alerts only                                             │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## SECURITY PROTOCOLS

### Authentication

| Connection | Auth Method | Token Refresh |
|------------|-------------|---------------|
| Internal Agents | JWT | 1 hour |
| Google Calendar | OAuth 2.0 | Auto |
| Apple Calendar | OAuth 2.0 | Auto |
| Notification Service | API Key | 30 days |

### Data Protection

- All calendar data encrypted at rest
- OAuth tokens stored securely
- No PII in logs
- Audit trail for all modifications

---

## MONITORING

### Health Checks

```typescript
interface CSAHealth {
  status: 'healthy' | 'degraded' | 'unhealthy';
  calendars: {
    connected: number;
    total: number;
    lastSync: Record<string, number>;
  };
  events: {
    todayCount: number;
    upcomingCount: number;
    pendingReminders: number;
  };
  sync: {
    lastSuccess: number;
    errorCount: number;
    avgLatency: number;
  };
}
```

### Connection Status

| Connection | Status | Latency | Last Check |
|------------|--------|---------|------------|
| Chat Commander | Pending | - | - |
| FEIA Commander | Pending | - | - |
| Research Commander | Pending | - | - |
| External Calendars | Pending | - | - |
| Notification Service | Pending | - | - |

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Next Review:** Upon implementation start

