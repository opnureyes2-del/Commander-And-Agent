# TECHNICAL SPECIFICATIONS - CSA COMMANDER

## In-Depth Technical Details & API Documentation

**Commander ID:** H-002
**Version:** 1.0.0
**Last Updated:** 2025-12-24

---

## SYSTEM ARCHITECTURE

### Component Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         CSA COMMANDER ARCHITECTURE                       │
│                                                                          │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐              │
│  │   Event      │───▶│  Scheduling  │───▶│   Reminder   │              │
│  │   Manager    │    │    Engine    │    │    Engine    │              │
│  └──────────────┘    └──────────────┘    └──────────────┘              │
│         │                   │                   │                       │
│         ▼                   ▼                   ▼                       │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐              │
│  │   Calendar   │    │    Time      │    │ Notification │              │
│  │     Sync     │    │   Analyzer   │    │   Service    │              │
│  └──────────────┘    └──────────────┘    └──────────────┘              │
│         │                   │                   │                       │
│         └───────────────────┴───────────────────┘                       │
│                             │                                           │
│                             ▼                                           │
│                    ┌──────────────────┐                                │
│                    │   Data Store     │                                │
│                    │   (SQLite)       │                                │
│                    └──────────────────┘                                │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## API SPECIFICATION

### Core Interface

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass, field
from typing import List, Dict, Optional, Any
from datetime import datetime, date, timedelta

class CSACommander:
    """CSA Commander main interface"""

    # Event Management
    async def create_event(self, event: EventData) -> CalendarEvent:
        """Create a new calendar event"""
        pass

    async def update_event(self, event_id: str, updates: EventUpdate) -> CalendarEvent:
        """Update an existing event"""
        pass

    async def delete_event(self, event_id: str) -> bool:
        """Delete an event"""
        pass

    async def get_event(self, event_id: str) -> CalendarEvent:
        """Get event by ID"""
        pass

    async def list_events(self, query: EventQuery) -> List[CalendarEvent]:
        """List events matching query"""
        pass

    # Scheduling
    async def find_free_slots(self, request: FreeSlotRequest) -> List[TimeSlot]:
        """Find available time slots"""
        pass

    async def suggest_times(self, request: SuggestionRequest) -> List[SuggestedSlot]:
        """Suggest optimal meeting times"""
        pass

    async def check_conflicts(self, event: EventData) -> List[Conflict]:
        """Check for scheduling conflicts"""
        pass

    # Reminders
    async def set_reminder(self, event_id: str, config: ReminderConfig) -> Reminder:
        """Set reminder for event"""
        pass

    async def snooze_reminder(self, reminder_id: str, duration: int) -> Reminder:
        """Snooze a reminder"""
        pass

    # Sync
    async def sync_calendar(self, source_id: str) -> SyncResult:
        """Sync with external calendar"""
        pass

    async def sync_all(self) -> Dict[str, SyncResult]:
        """Sync all connected calendars"""
        pass

    # Analytics
    def get_time_analytics(self, period: DateRange) -> TimeAnalytics:
        """Get time usage analytics"""
        pass

    # Health
    async def health_check(self) -> HealthStatus:
        """Check system health"""
        pass
```

### Data Types

```python
# Event Types
@dataclass
class CalendarEvent:
    id: str
    title: str
    start_time: datetime
    end_time: datetime
    timezone: str = "UTC"
    location: Optional[str] = None
    description: Optional[str] = None
    attendees: List[Attendee] = field(default_factory=list)
    recurrence: Optional[RecurrenceRule] = None
    reminders: List[Reminder] = field(default_factory=list)
    calendar_id: str = "default"
    priority: str = "medium"  # critical, high, medium, low
    status: str = "confirmed"
    created_at: datetime = field(default_factory=datetime.now)
    updated_at: datetime = field(default_factory=datetime.now)

@dataclass
class EventData:
    title: str
    start_time: datetime
    end_time: datetime
    timezone: str = "UTC"
    location: Optional[str] = None
    description: Optional[str] = None
    attendees: List[str] = field(default_factory=list)
    recurrence: Optional[RecurrenceRule] = None
    reminder_minutes: List[int] = field(default_factory=lambda: [15])
    priority: str = "medium"
    calendar_id: str = "default"

@dataclass
class EventUpdate:
    title: Optional[str] = None
    start_time: Optional[datetime] = None
    end_time: Optional[datetime] = None
    location: Optional[str] = None
    description: Optional[str] = None
    attendees: Optional[List[str]] = None
    priority: Optional[str] = None

# Recurrence Types
@dataclass
class RecurrenceRule:
    frequency: str  # daily, weekly, monthly, yearly
    interval: int = 1
    count: Optional[int] = None
    until: Optional[datetime] = None
    by_day: Optional[List[str]] = None  # MO, TU, WE, etc.
    by_month_day: Optional[List[int]] = None
    by_month: Optional[List[int]] = None

# Scheduling Types
@dataclass
class TimeSlot:
    start: datetime
    end: datetime
    duration_minutes: int
    score: float = 1.0  # preference score

@dataclass
class FreeSlotRequest:
    duration_minutes: int
    start_date: date
    end_date: date
    working_hours: Optional[TimeRange] = None
    buffer_minutes: int = 0
    exclude_calendars: List[str] = field(default_factory=list)

@dataclass
class SuggestionRequest:
    duration_minutes: int
    attendees: List[str]
    date_range: DateRange
    preferences: Optional[SchedulingPreferences] = None

@dataclass
class SuggestedSlot:
    slot: TimeSlot
    score: float
    conflicts: List[str]  # attendee conflicts
    reasons: List[str]  # why this slot was suggested

@dataclass
class Conflict:
    event_id: str
    title: str
    overlap_start: datetime
    overlap_end: datetime
    overlap_minutes: int
    severity: str  # full, partial, buffer

# Reminder Types
@dataclass
class Reminder:
    id: str
    event_id: str
    trigger_time: datetime
    lead_minutes: int
    channel: str  # push, email, sms
    status: str  # pending, sent, acknowledged, snoozed
    snooze_count: int = 0

@dataclass
class ReminderConfig:
    lead_minutes: List[int]
    channels: List[str]
    escalation_enabled: bool = False
    max_reminders: int = 3

# Sync Types
@dataclass
class SyncResult:
    source_id: str
    success: bool
    events_created: int
    events_updated: int
    events_deleted: int
    conflicts_found: int
    errors: List[str]
    sync_time: datetime
    next_sync: datetime

# Analytics Types
@dataclass
class TimeAnalytics:
    period: DateRange
    total_events: int
    total_hours: float
    by_category: Dict[str, float]
    by_day: Dict[str, float]
    meeting_density: float
    focus_time_hours: float
    patterns: List[Pattern]
    insights: List[Insight]
```

---

## EVENT MANAGER SPECIFICATION

### Configuration

```python
@dataclass
class EventManagerConfig:
    default_calendar: str = "default"
    default_timezone: str = "UTC"
    default_reminder_minutes: List[int] = field(default_factory=lambda: [15])
    max_recurrence_instances: int = 365
    conflict_buffer_minutes: int = 0

class EventManager:
    """Manages calendar event lifecycle"""

    def __init__(self, config: EventManagerConfig, store: DataStore):
        self.config = config
        self.store = store

    async def create(self, data: EventData) -> CalendarEvent:
        """Create new event with validation"""
        pass

    async def update(self, event_id: str, updates: EventUpdate) -> CalendarEvent:
        """Update event with conflict check"""
        pass

    async def delete(self, event_id: str, notify: bool = True) -> bool:
        """Delete event and clean up reminders"""
        pass

    async def expand_recurrence(
        self,
        rule: RecurrenceRule,
        start: datetime,
        end: datetime
    ) -> List[datetime]:
        """Expand recurrence rule to instances"""
        pass
```

---

## SCHEDULING ENGINE SPECIFICATION

### Configuration

```python
@dataclass
class SchedulingConfig:
    working_hours: TimeRange = field(default_factory=lambda: TimeRange(9, 17))
    preferred_duration: int = 30  # minutes
    buffer_between: int = 5  # minutes
    travel_time_enabled: bool = True
    default_travel_minutes: int = 15

class SchedulingEngine:
    """Intelligent scheduling and conflict detection"""

    def __init__(self, config: SchedulingConfig, event_manager: EventManager):
        self.config = config
        self.event_manager = event_manager

    def find_conflicts(
        self,
        event: EventData,
        existing: List[CalendarEvent]
    ) -> List[Conflict]:
        """Detect all scheduling conflicts"""
        pass

    def find_free_slots(
        self,
        duration: int,
        start_date: date,
        end_date: date,
        constraints: SchedulingConstraints
    ) -> List[TimeSlot]:
        """Find available time slots"""
        pass

    def suggest_times(
        self,
        request: SuggestionRequest,
        attendee_calendars: Dict[str, List[CalendarEvent]]
    ) -> List[SuggestedSlot]:
        """Generate ranked time suggestions"""
        pass

    def calculate_travel_time(
        self,
        from_location: str,
        to_location: str
    ) -> int:
        """Estimate travel time in minutes"""
        pass
```

---

## REMINDER ENGINE SPECIFICATION

### Configuration

```python
@dataclass
class ReminderEngineConfig:
    default_channels: List[str] = field(default_factory=lambda: ["push"])
    max_snooze_count: int = 3
    snooze_durations: List[int] = field(default_factory=lambda: [5, 15, 30])
    escalation_interval: int = 10  # minutes

class ReminderEngine:
    """Manages reminder scheduling and delivery"""

    def __init__(self, config: ReminderEngineConfig, notification_service):
        self.config = config
        self.notification_service = notification_service
        self.scheduler = ReminderScheduler()

    async def schedule(
        self,
        event: CalendarEvent,
        config: ReminderConfig
    ) -> List[Reminder]:
        """Schedule reminders for event"""
        pass

    async def trigger(self, reminder: Reminder) -> DeliveryResult:
        """Trigger reminder delivery"""
        pass

    async def snooze(
        self,
        reminder_id: str,
        minutes: int
    ) -> Reminder:
        """Snooze reminder"""
        pass

    async def cancel(self, event_id: str) -> int:
        """Cancel all reminders for event"""
        pass
```

---

## CALENDAR SYNC SPECIFICATION

### Configuration

```python
@dataclass
class CalendarSyncConfig:
    sync_interval_minutes: int = 15
    conflict_resolution: str = "newest"  # newest, local, remote, manual
    bidirectional: bool = True
    max_sync_period_days: int = 365

class CalendarSync:
    """Multi-source calendar synchronization"""

    def __init__(self, config: CalendarSyncConfig):
        self.config = config
        self.providers: Dict[str, CalendarProvider] = {}
        self.sync_queue = SyncQueue()

    async def add_source(self, source: CalendarSource) -> bool:
        """Add calendar source"""
        pass

    async def remove_source(self, source_id: str) -> bool:
        """Remove calendar source"""
        pass

    async def sync(self, source_id: str) -> SyncResult:
        """Sync single source"""
        pass

    async def sync_all(self) -> Dict[str, SyncResult]:
        """Sync all sources"""
        pass

    async def resolve_conflict(
        self,
        local: CalendarEvent,
        remote: CalendarEvent
    ) -> CalendarEvent:
        """Resolve sync conflict"""
        pass
```

---

## PROCESSING PIPELINE

### Event Creation Flow

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Input   │───▶│ Validate │───▶│ Check    │───▶│ Create   │
│  Event   │    │  Data    │    │ Conflicts│    │  Event   │
└──────────┘    └──────────┘    └──────────┘    └────┬─────┘
                                                     │
                     ┌───────────────────────────────┘
                     │
                     ▼
              ┌──────────┐    ┌──────────┐    ┌──────────┐
              │ Schedule │───▶│ Sync to  │───▶│  Return  │
              │ Reminders│    │ External │    │  Result  │
              └──────────┘    └──────────┘    └──────────┘
```

---

## ERROR HANDLING

### Error Categories

```python
class CSAError(Exception):
    """Base CSA error"""
    pass

class EventError(CSAError):
    """Event operation errors"""
    pass

class ConflictError(CSAError):
    """Scheduling conflict errors"""
    pass

class SyncError(CSAError):
    """Calendar sync errors"""
    pass

class ReminderError(CSAError):
    """Reminder operation errors"""
    pass

class ValidationError(CSAError):
    """Data validation errors"""
    pass
```

### Recovery Strategies

| Error Type | Strategy | Fallback |
|------------|----------|----------|
| Sync Failure | Retry 3x | Use cached data |
| Conflict Detected | User prompt | Suggest alternatives |
| Reminder Failed | Retry + escalate | Log for review |
| Validation Error | Reject with details | N/A |

---

## PERFORMANCE REQUIREMENTS

### Latency Targets

| Operation | Target | P95 | P99 |
|-----------|--------|-----|-----|
| Event Create | 200ms | 400ms | 800ms |
| Conflict Check | 100ms | 200ms | 400ms |
| Free Slot Find | 300ms | 600ms | 1000ms |
| Suggestion Gen | 500ms | 1000ms | 2000ms |
| Full Sync | 2s | 5s | 10s |

### Resource Limits

| Resource | Limit | Notes |
|----------|-------|-------|
| Memory | 256MB | Maximum heap |
| Storage | 100MB | Event database |
| Connections | 10 | External calendar |

---

## SECURITY CONSIDERATIONS

### Data Handling

- All calendar data encrypted at rest
- OAuth tokens in secure storage
- No PII in logs
- Audit trail for modifications

### Access Control

```python
class AccessControl:
    def can_read(self, user_id: str, calendar_id: str) -> bool:
        pass

    def can_write(self, user_id: str, calendar_id: str) -> bool:
        pass

    def can_share(self, user_id: str, calendar_id: str) -> bool:
        pass
```

---

## MONITORING & OBSERVABILITY

### Metrics Collected

| Metric | Type | Description |
|--------|------|-------------|
| csa_events_created | Counter | Events created |
| csa_events_updated | Counter | Events updated |
| csa_conflicts_detected | Counter | Conflicts found |
| csa_sync_duration | Histogram | Sync latency |
| csa_reminders_sent | Counter | Reminders delivered |

### Health Check

```python
async def health_check() -> HealthStatus:
    event_health = await check_event_store()
    sync_health = await check_sync_status()
    reminder_health = await check_reminder_engine()

    return HealthStatus(
        status=derive_overall_status(event_health, sync_health, reminder_health),
        events=event_health,
        sync=sync_health,
        reminders=reminder_health
    )
```

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Next Review:** Upon implementation changes

