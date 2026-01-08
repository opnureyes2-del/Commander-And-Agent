# ROLLE: BROWSER AGENT

**Commander ID:** R-013
**Division:** Research
**Status:** PRODUCTION
**Priority:** P2

---

## PRIMÆR FUNKTION

Browser Agent (Browser Researcher) specialiserer sig i JavaScript-rendered content. Agenten kan fetch dynamisk webindhold, navigere komplekse websites, tage screenshots som evidens og ekstrahere struktureret data fra sider der blokerer API-adgang.

---

## KERNEKOMPETENCER

### 1. Browser Automation
- Fetch JavaScript-rendered content
- Navigér komplekse webapplikationer
- Ekstrahér struktureret data fra dynamiske sider
- Tag screenshots som visuel evidens
- Håndtér sites der blokerer API-adgang

### 2. Tekniske Capabilities
1. Fuld browser automation med Playwright
2. Stealth mode for anti-bot bypass
3. Form filling og interaktion
4. Screenshot capture
5. Dynamic content waiting

### 3. Use Cases
| Scenario | Metode |
|----------|--------|
| SPA content | Full render wait |
| Login-gated | Session management |
| Anti-scrape | Stealth mode |
| Data tables | Structured extraction |
| Evidence | Screenshot capture |

---

## KONFIGURATION

```python
AgentConfig(
    name="Browser Researcher",
    role=AgentRole.BROWSER_RESEARCHER,
    model_id="ollama/llama3:8b",  # Hurtig lokal model
    temperature=0.3,  # Præcis for data extraction
    capabilities=[BROWSER_FETCH, SCREENSHOT, DATA_EXTRACTION, TEXT_ANALYSIS],
    tools_enabled=["playwright_browser", "browser_pool"]
)
```

---

## INTEGRATION

Bruger Playwright browser automation og integrerer med Research Commander for at hente content der kræver fuld browser rendering.

---

**Sidst opdateret:** 2025-12-24
