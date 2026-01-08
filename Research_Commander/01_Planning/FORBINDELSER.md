# FORBINDELSER - RESEARCH COMMANDER

## Connection Documentation & Integration Specifications

**Commander ID:** R-001
**Connection Count:** 15+ Direct Connections
**Last Updated:** 2025-12-24

---

## CONNECTION DIAGRAM

```
                              ┌─────────────────┐
                              │      CHAT       │
                              │   COMMANDER     │
                              │    (M-001)      │
                              └────────┬────────┘
                                       │
                                       ▼
┌─────────────────┐        ┌─────────────────────────────────────┐
│     FEIA        │◀──────▶│                                     │
│   COMMANDER     │        │                                     │
│    (H-001)      │        │      RESEARCH COMMANDER             │
└─────────────────┘        │           (R-001)                   │
                           │                                     │
┌─────────────────┐        │   Master Orchestrator               │
│     CSA         │◀───────│   MDT Quality Assurance             │
│   COMMANDER     │        │   12 Specialist Agents              │
│    (H-002)      │        │                                     │
└─────────────────┘        └───────────────┬─────────────────────┘
                                           │
          ┌────────────────────────────────┼────────────────────────────────┐
          │                                │                                │
          ▼                                ▼                                ▼
┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│  WEB RESEARCHER │              │ RESEARCH ANALYST│              │SOCRATIC TEACHER │
│     (R-002)     │              │     (R-003)     │              │     (R-004)     │
└─────────────────┘              └─────────────────┘              └─────────────────┘
          │                                │                                │
          ▼                                ▼                                ▼
┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│   GEO-TECH      │              │    THREAT       │              │    HORIZON      │
│  SPECIALISTS    │              │   DETECTOR      │              │    SCANNER      │
│  (R-005/6/7)    │              │    (R-008)      │              │    (R-009)      │
└─────────────────┘              └─────────────────┘              └─────────────────┘
          │                                │                                │
          ▼                                ▼                                ▼
┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│     QUERY       │              │   BLINDSPOT     │              │   REFLECTION    │
│ DECONSTRUCTOR   │              │  ILLUMINATOR    │              │     AGENT       │
│    (R-010)      │              │    (R-011)      │              │    (R-012)      │
└─────────────────┘              └─────────────────┘              └─────────────────┘
                                           │
                                           ▼
                                 ┌─────────────────┐
                                 │    BROWSER      │
                                 │     AGENT       │
                                 │    (R-013)      │
                                 └─────────────────┘
```

---

## CONNECTION REGISTRY

### External Connections (Outside Division)

| ID | Target | Direction | Protocol | Purpose |
|----|--------|-----------|----------|---------|
| R001-E01 | Chat Commander (M-001) | Bidirectional | Internal API | User queries & results |
| R001-E02 | FEIA Commander (H-001) | Bidirectional | Internal API | Facts & research triggers |
| R001-E03 | CSA Commander (H-002) | Outbound | Internal API | Location research |

### Internal Division Connections

| ID | Target | Direction | Protocol | Purpose |
|----|--------|-----------|----------|---------|
| R001-I01 | Web Researcher (R-002) | Bidirectional | Internal | Web search tasks |
| R001-I02 | Research Analyst (R-003) | Bidirectional | Internal | Analysis tasks |
| R001-I03 | Socratic Teacher (R-004) | Bidirectional | Internal | Educational delivery |
| R001-I04 | Geo-Tech Detector (R-005) | Bidirectional | Internal | Geo verification |
| R001-I05 | Geo-Tech Synthesizer (R-006) | Bidirectional | Internal | Location synthesis |
| R001-I06 | Geo-Tech Knowledge (R-007) | Bidirectional | Internal | Geo expertise |
| R001-I07 | Threat Detector (R-008) | Bidirectional | Internal | Threat research |
| R001-I08 | Horizon Scanner (R-009) | Bidirectional | Internal | Trend detection |
| R001-I09 | Query Deconstructor (R-010) | Bidirectional | Internal | Query analysis |
| R001-I10 | BlindSpot Illuminator (R-011) | Bidirectional | Internal | Missing angles |
| R001-I11 | Reflection Agent (R-012) | Bidirectional | Internal | Validation |
| R001-I12 | Browser Agent (R-013) | Bidirectional | Internal | Live web access |

---

## CONNECTION SPECIFICATIONS

### R001-E01: Chat Commander Connection

**Purpose:** Receive research requests and deliver results

```python
# Research Request Protocol
@dataclass
class ResearchRequest:
    request_id: str
    query: str
    context: Optional[Dict[str, Any]]
    priority: str  # urgent, high, normal, low
    depth: str     # quick, standard, comprehensive
    domains: List[str]  # restrict to specific domains
    max_sources: int
    deadline: Optional[datetime]

# Research Response Protocol
@dataclass
class ResearchResponse:
    request_id: str
    summary: str
    detailed_findings: List[Finding]
    sources: List[Source]
    mdt_score: MDTScore
    methodology: str
    limitations: List[str]
    follow_up_questions: List[str]
    processing_time: float

@dataclass
class Finding:
    claim: str
    evidence: List[Evidence]
    confidence: float
    bias_assessment: Optional[BiasReport]

@dataclass
class MDTScore:
    overall: float
    source_credibility: float
    factual_accuracy: float
    methodology_rigor: float
    bias_awareness: float
    temporal_relevance: float
```

---

### R001-I01: Web Researcher Connection

**Purpose:** Delegate web search and verification tasks

```python
# Web Search Task
@dataclass
class WebSearchTask:
    task_id: str
    query: str
    search_engines: List[str]
    domain_filters: List[str]
    max_results: int
    verification_required: bool
    timeout_seconds: int

# Web Search Result
@dataclass
class WebSearchResult:
    task_id: str
    results: List[SearchResult]
    metadata: SearchMetadata
    verification_status: VerificationStatus
```

---

### R001-I03: Socratic Teacher Connection

**Purpose:** Educational delivery and concept explanation

```python
# Teaching Request
@dataclass
class TeachingRequest:
    topic: str
    audience_level: str  # beginner, intermediate, advanced
    style: str  # conversational, academic, practical
    include_examples: bool
    max_length: int

# Teaching Response
@dataclass
class TeachingResponse:
    explanation: str
    key_concepts: List[Concept]
    examples: List[Example]
    further_reading: List[Resource]
    quiz_questions: Optional[List[Question]]
```

---

### R001-I10: Query Deconstructor Connection

**Purpose:** Break complex queries into researchable components

```python
# Query Analysis Request
@dataclass
class QueryAnalysisRequest:
    query: str
    context: Optional[str]
    depth: str

# Query Analysis Result
@dataclass
class QueryAnalysisResult:
    original_query: str
    sub_questions: List[SubQuestion]
    key_concepts: List[str]
    required_agents: List[str]
    suggested_approach: str
    estimated_complexity: str

@dataclass
class SubQuestion:
    question: str
    priority: int
    assigned_agent: str
    dependencies: List[str]
```

---

### R001-I11: BlindSpot Illuminator Connection

**Purpose:** Identify overlooked perspectives and angles

```python
# BlindSpot Request
@dataclass
class BlindSpotRequest:
    topic: str
    current_findings: List[Finding]
    perspective: str  # user's current viewpoint

# BlindSpot Response
@dataclass
class BlindSpotResponse:
    overlooked_angles: List[Angle]
    contrarian_views: List[View]
    unexplored_questions: List[str]
    potential_biases: List[Bias]
```

---

### R001-I12: Reflection Agent Connection

**Purpose:** Validate research methodology and findings

```python
# Validation Request
@dataclass
class ValidationRequest:
    findings: List[Finding]
    methodology: str
    sources: List[Source]

# Validation Response
@dataclass
class ValidationResponse:
    methodology_score: float
    issues_found: List[Issue]
    recommendations: List[str]
    confidence_adjustment: float
    approved: bool
```

---

## DATA FLOW DIAGRAMS

### Research Orchestration Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Receive    │────▶│   Analyze    │────▶│  Decompose   │
│   Request    │     │    Query     │     │   (R-010)    │
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
         ┌───────────────────────────────────────┘
         │
         ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Delegate   │────▶│   Execute    │────▶│   Validate   │
│   to Agents  │     │   Research   │     │   (R-012)    │
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
         ┌───────────────────────────────────────┘
         │
         ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Check      │────▶│  Synthesize  │────▶│   Deliver    │
│   BlindSpots │     │   Results    │     │   Response   │
│   (R-011)    │     │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

### MDT Scoring Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Collect    │────▶│   Evaluate   │────▶│   Weight     │
│   Sources    │     │   Dimensions │     │   Scores     │
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
                     ┌───────────────────────────┘
                     │
                     ▼
              ┌──────────────┐     ┌──────────────┐
              │   Compute    │────▶│   Report     │
              │   Composite  │     │   Score      │
              └──────────────┘     └──────────────┘
```

---

## ERROR HANDLING

### Connection Failure Protocols

| Error Type | Detection | Response | Recovery |
|------------|-----------|----------|----------|
| Agent Timeout | > 60s response | Use partial results | Retry once |
| Agent Failure | Error returned | Route to backup | Log issue |
| Low MDT Score | < 0.6 threshold | Flag for review | Request verification |
| Source Unavailable | Connection error | Use cached data | Mark confidence |

### Graceful Degradation

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    RESEARCH DEGRADATION LEVELS                           │
│                                                                          │
│   LEVEL 0: FULL CAPABILITY                                              │
│   ├── All 12 agents available                                           │
│   ├── Full MDT scoring                                                  │
│   └── Comprehensive research                                            │
│                                                                          │
│   LEVEL 1: REDUCED AGENTS                                               │
│   ├── Core agents only (R-002, R-003, R-010)                           │
│   ├── Basic MDT scoring                                                 │
│   └── Standard research                                                 │
│                                                                          │
│   LEVEL 2: MINIMAL CAPABILITY                                           │
│   ├── Web Researcher only                                               │
│   ├── No MDT scoring                                                    │
│   └── Quick research only                                               │
│                                                                          │
│   LEVEL 3: CACHE ONLY                                                   │
│   ├── No active research                                                │
│   ├── Historical results only                                           │
│   └── Notify user of limitation                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## MONITORING

### Health Checks

```python
@dataclass
class ResearchDivisionHealth:
    status: str  # healthy, degraded, unhealthy
    commander_status: str
    agents: Dict[str, AgentHealth]
    active_tasks: int
    completed_tasks_24h: int
    avg_mdt_score: float
    avg_response_time: float

@dataclass
class AgentHealth:
    agent_id: str
    status: str
    last_active: datetime
    tasks_completed: int
    error_rate: float
```

### Agent Status Dashboard

| Agent | Status | Tasks/Hour | Avg Time | MDT Score |
|-------|--------|------------|----------|-----------|
| R-002 | Pending | - | - | - |
| R-003 | Pending | - | - | - |
| R-004 | Pending | - | - | - |
| R-005 | Pending | - | - | - |
| R-006 | Pending | - | - | - |
| R-007 | Pending | - | - | - |
| R-008 | Pending | - | - | - |
| R-009 | Pending | - | - | - |
| R-010 | Pending | - | - | - |
| R-011 | Pending | - | - | - |
| R-012 | Pending | - | - | - |
| R-013 | Pending | - | - | - |

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Next Review:** Upon implementation start

