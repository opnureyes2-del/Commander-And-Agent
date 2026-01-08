# FORBINDELSER - SOCRATIC TEACHER

## Connection Documentation

**Commander ID:** R-004
**Connection Count:** 4 Direct Connections
**Last Updated:** 2025-12-24

---

## CONNECTION DIAGRAM

```
┌─────────────────┐              ┌─────────────────────────────┐
│    RESEARCH     │◀────────────▶│                             │
│   COMMANDER     │    Tasks     │                             │
│    (R-001)      │              │                             │
└─────────────────┘              │                             │
                                 │   SOCRATIC TEACHER          │
┌─────────────────┐              │        (R-004)              │
│   RESEARCH      │─────────────▶│                             │
│    ANALYST      │  Knowledge   │   Educational Specialist    │
│    (R-003)      │              │                             │
└─────────────────┘              │                             │
                                 │                             │
┌─────────────────┐              │                             │
│     CHAT        │◀────────────▶│                             │
│   COMMANDER     │ Explanations └─────────────────────────────┘
│    (M-001)      │
└─────────────────┘
```

---

## CONNECTION REGISTRY

| ID | Target | Direction | Purpose |
|----|--------|-----------|---------|
| R004-C01 | Research Commander | Bidirectional | Task assignment |
| R004-C02 | Research Analyst | Inbound | Knowledge source |
| R004-C03 | Chat Commander | Bidirectional | User delivery |

---

## DATA FLOW

```
Knowledge ────▶ Simplify ────▶ Structure ────▶ Deliver
                   │                              │
                   ▼                              ▼
              Add Examples                   Check Understanding
```

---

## COMMUNICATION PROTOCOLS

```python
# Teaching Request Protocol
@dataclass
class TeachingRequest:
    topic: str
    audience_level: str  # beginner, intermediate, advanced
    style: str  # conversational, academic, practical, socratic
    include_examples: bool = True
    max_length: int = 500
    context: Optional[str] = None

# Teaching Response Protocol
@dataclass
class TeachingResponse:
    explanation: str
    key_concepts: List[Concept]
    examples: List[Example]
    analogies: List[str]
    follow_up_questions: List[str]
    further_reading: List[Resource]
    quiz_questions: Optional[List[Question]]
```

---

**Document Status:** COMPLETE

