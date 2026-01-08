# FORBINDELSER - WEB RESEARCHER

## Connection Documentation & Integration Specifications

**Commander ID:** R-002
**Connection Count:** 6 Direct Connections
**Last Updated:** 2025-12-24

---

## CONNECTION DIAGRAM

```
┌─────────────────┐              ┌─────────────────────────────┐
│    RESEARCH     │◀────────────▶│                             │
│   COMMANDER     │    Tasks &   │                             │
│    (R-001)      │    Results   │                             │
└─────────────────┘              │                             │
                                 │    WEB RESEARCHER           │
┌─────────────────┐              │        (R-002)              │
│     QUERY       │─────────────▶│                             │
│ DECONSTRUCTOR   │  Sub-queries │   Web Search Specialist     │
│    (R-010)      │              │                             │
└─────────────────┘              │                             │
                                 │                             │
┌─────────────────┐              │                             │
│   RESEARCH      │◀─────────────│                             │
│    ANALYST      │   Raw Data   └─────────────────────────────┘
│    (R-003)      │                        │
└─────────────────┘                        │
                                           ▼
┌─────────────────┐              ┌─────────────────┐
│   REFLECTION    │◀─────────────│   EXTERNAL      │
│     AGENT       │   Sources    │   SEARCH APIs   │
│    (R-012)      │              │  (Exa/Tavily)   │
└─────────────────┘              └─────────────────┘
```

---

## CONNECTION REGISTRY

### Direct Connections

| ID | Target | Direction | Protocol | Purpose |
|----|--------|-----------|----------|---------|
| R002-C01 | Research Commander (R-001) | Bidirectional | Internal | Task assignment & results |
| R002-C02 | Query Deconstructor (R-010) | Inbound | Internal | Decomposed queries |
| R002-C03 | Research Analyst (R-003) | Outbound | Internal | Raw data delivery |
| R002-C04 | Reflection Agent (R-012) | Outbound | Internal | Source verification |
| R002-C05 | Exa Search API | Outbound | REST | Semantic search |
| R002-C06 | Tavily Search API | Outbound | REST | Comprehensive search |

---

## CONNECTION SPECIFICATIONS

### R002-C01: Research Commander Connection

**Purpose:** Receive search tasks and deliver results

```python
# Search Task Protocol
@dataclass
class WebSearchTask:
    task_id: str
    query: str
    engines: List[str]
    filters: SearchFilters
    max_results: int
    verification_required: bool
    timeout_seconds: int

@dataclass
class SearchFilters:
    domains: List[str]
    exclude_domains: List[str]
    freshness_days: Optional[int]
    language: str
    content_type: Optional[str]

# Search Result Protocol
@dataclass
class WebSearchResult:
    task_id: str
    query: str
    results: List[SearchResultItem]
    sources_verified: int
    credibility_summary: CredibilitySummary
    processing_time_ms: int

@dataclass
class SearchResultItem:
    url: str
    title: str
    snippet: str
    content: Optional[str]
    source: SourceInfo
    credibility_score: float
    extracted_facts: List[str]
```

---

### R002-C05/C06: Search API Connections

**Purpose:** Execute web searches

```python
# Exa Search Integration
async def search_exa(
    query: str,
    num_results: int = 10,
    use_autoprompt: bool = True,
    type: str = "auto"
) -> List[ExaResult]:
    """Execute semantic search via Exa"""
    pass

# Tavily Search Integration
async def search_tavily(
    query: str,
    search_depth: str = "advanced",
    max_results: int = 10,
    include_answer: bool = True
) -> TavilyResponse:
    """Execute comprehensive search via Tavily"""
    pass
```

---

## DATA FLOW DIAGRAMS

### Search Execution Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Receive    │────▶│   Select     │────▶│   Execute    │
│    Task      │     │   Engines    │     │   Searches   │
└──────────────┘     └──────────────┘     └──────────────┘
                                                 │
         ┌───────────────────────────────────────┘
         │
         ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Aggregate  │────▶│   Verify     │────▶│   Return     │
│   Results    │     │   Sources    │     │   Results    │
└──────────────┘     └──────────────┘     └──────────────┘
```

---

## ERROR HANDLING

| Error Type | Detection | Response | Recovery |
|------------|-----------|----------|----------|
| API Timeout | > 5s | Retry once | Return partial |
| Rate Limited | 429 response | Wait & retry | Use alt engine |
| No Results | Empty response | Broaden query | Report gap |
| Invalid Source | Verification fail | Flag result | Continue |

---

## MONITORING

### Health Check

```python
@dataclass
class WebResearcherHealth:
    status: str
    api_status: Dict[str, str]
    searches_completed: int
    avg_response_time: float
    cache_hit_rate: float
```

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24

