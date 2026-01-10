# Commander and Agent

> **25+ Specialiserede AI Kommandører til Cirkelline Ecosystem**

[![Version](https://img.shields.io/badge/version-3.0.0-blue.svg)](./CLAUDE.md)
[![Agents](https://img.shields.io/badge/agents-25+-green.svg)]()
[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)]()

## Oversigt

Commander and Agent er en samling af 25+ specialiserede AI-agenter organiseret i kommandør-hierarkier. Hver agent har et specifikt ansvarsområde og kan koordinere med andre agenter.

## Kommandør Kategorier

### M-Series (Core Commanders)

| Agent | Funktion | Port |
|-------|----------|------|
| **Chat Commander** | Primær brugerinteraktion | 7789 |
| **Terminal Commander** | Shell & system kommandoer | 7790 |
| **Code Commander** | Kodegenerering & refactoring | 7791 |
| **Data Commander** | Data processing & analyse | 7792 |
| **Evolution Commander** | AI self-improvement | 7793 |

### S-Series (Specialist Agents)

| Agent | Funktion | Port |
|-------|----------|------|
| **Video Specialist** | Video processing | 7794 |
| **Document Specialist** | Dokument håndtering | 7795 |
| **Audio Specialist** | Audio transcription | 7796 |
| **Image Specialist** | Image analysis | 7797 |
| **Research Specialist** | Deep research | 7798 |

### H-Series (Helper Agents)

| Agent | Funktion |
|-------|----------|
| **FEIA Commander** | Fast Error Impact Analysis |
| **CSA Commander** | Context-Sensitive Assistant |

### R-Series (Research Team)

| Agent | Specialisering |
|-------|----------------|
| **Research Commander** | Team koordinering |
| **Web Researcher** | Internet research |
| **Research Analyst** | Data analyse |
| **Geo Tech West** | Vestlig teknologi |
| **Geo Tech Eurasia** | Eurasisk teknologi |
| **Geo Tech Asia** | Asiatisk teknologi |

### Meta Agents

| Agent | Funktion |
|-------|----------|
| **Socratic Teacher** | Læringsassistent |
| **Reflection Agent** | Self-evaluation |
| **Query Deconstructor** | Query parsing |
| **Horizon Scanner** | Trend detection |
| **BlindSpot Illuminator** | Gap analysis |
| **Threat Detector** | Security scanning |
| **Browser Agent** | Web automation |

## Projekt Struktur

```
commander-and-agent/
├── Chat_Commander/
├── Terminal_Commander/
├── Code_Commander/
├── Data_Commander/
├── Evolution_Commander/
├── Video_Specialist/
├── Document_Specialist/
├── Audio_Specialist/
├── Image_Specialist/
├── Research_Commander/
├── Research_Analyst/
├── Research_Specialist/
├── Web_Researcher/
├── Geo_Tech_West/
├── Geo_Tech_Eurasia/
├── Geo_Tech_Asia/
├── FEIA_Commander/
├── CSA_Commander/
├── Socratic_Teacher/
├── Reflection_Agent/
├── Query_Deconstructor/
├── Horizon_Scanner/
├── BlindSpot_Illuminator/
├── Threat_Detector/
├── Browser_Agent/
├── integration/
├── INTRO/
├── AGENT_INDEX.md
├── BASELINE.md
├── ROADMAP.md
└── MASTER_BASELINE.md
```

## Installation

```bash
# Clone repository
git clone https://github.com/opnureyes2-del/Commander-and-Agent.git
cd commander-and-agent

# Setup virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies (per agent)
cd Chat_Commander
pip install -r requirements.txt
```

## Integration

Agenter integreres med:
- **Mastermind Controller** (Port 7799) - Orchestration
- **ELLE.md Agents** - Systemd services
- **Cirkelline System** - Backend API

## API

Hver agent eksponerer standard endpoints:

```
GET  /health          - Health check
GET  /api/status      - Agent status
POST /api/invoke      - Invoke agent
POST /api/release     - Release resources
```

## Dokumentation

- [AGENT_INDEX.md](./AGENT_INDEX.md) - Komplet agent oversigt
- [BASELINE.md](./BASELINE.md) - Nuværende tilstand
- [ROADMAP.md](./ROADMAP.md) - Udviklingsplan
- [MASTER_BASELINE.md](./MASTER_BASELINE.md) - Master reference
- [INTRO/](./INTRO/) - INTRO dokumentation

## Status

| Kategori | Agenter | Status |
|----------|---------|--------|
| M-Series | 5 | 80% |
| S-Series | 5 | 60% |
| H-Series | 2 | 100% |
| R-Series | 6 | 70% |
| Meta | 6 | 50% |

**Overall:** 70% komplet

## Links

- **GitHub:** [opnureyes2-del/Commander-and-Agent](https://github.com/opnureyes2-del/Commander-and-Agent)
- **Mastermind:** [commando-center](../commando-center/)

---

**Commander and Agent** - Del af Cirkelline Ecosystem v1.3.8

*Sidst opdateret: 2026-01-10*
