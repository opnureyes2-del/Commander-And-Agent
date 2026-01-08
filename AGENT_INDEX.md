# AGENT INDEX - CIRKELLINE COMMANDER SYSTEM

## Central Reference for All AI Commanders

**Version:** 2.0.0
**Created:** 2025-12-24
**Total Commanders:** 25
**Protocol Compliance:** FULL

---

## QUICK REFERENCE TABLE

| ID | Commander | Division | Priority | Status | Folder |
|----|-----------|----------|----------|--------|--------|
| M-001 | Chat Commander | Mobile | P1 | MVP-Ready | `Chat_Commander/` |
| M-002 | Terminal Commander | Mobile | P2 | Placeholder | `Terminal_Commander/` |
| M-003 | Code Commander | Mobile | P2 | Placeholder | `Code_Commander/` |
| M-004 | Data Commander | Mobile | P3 | Placeholder | `Data_Commander/` |
| M-005 | Evolution Commander | Mobile | P3 | Placeholder | `Evolution_Commander/` |
| H-001 | FEIA Commander | HASA | P1 | Production | `FEIA_Commander/` |
| H-002 | CSA Commander | HASA | P1 | Production | `CSA_Commander/` |
| R-001 | Research Commander | Research | P1 | Production | `Research_Commander/` |
| R-002 | Web Researcher | Research | P1 | Production | `Web_Researcher/` |
| R-003 | Research Analyst | Research | P1 | Production | `Research_Analyst/` |
| R-004 | Geo-Tech Asia | Research | P2 | Production | `GeoTech_Asia/` |
| R-005 | Geo-Tech Eurasia | Research | P2 | Production | `GeoTech_Eurasia/` |
| R-006 | Geo-Tech West | Research | P2 | Production | `GeoTech_West/` |
| R-007 | Global Threat Analyst | Research | P2 | Production | `Global_Threat_Analyst/` |
| R-008 | Horizon Scanner | Research | P2 | Production | `Horizon_Scanner/` |
| R-009 | Query Adaptor | Research | P2 | Production | `Query_Adaptor/` |
| R-010 | Blind Spot Detector | Research | P2 | Production | `Blind_Spot_Detector/` |
| R-011 | Reflection Analyst | Research | P2 | Production | `Reflection_Analyst/` |
| R-012 | Browser Researcher | Research | P2 | Production | `Browser_Researcher/` |
| R-013 | Socratic Teacher | Research | P1 | Production | `Socratic_Teacher/` |
| S-001 | Video Specialist | Specialist | P3 | Reference | `Video_Specialist/` |
| S-002 | Document Specialist | Specialist | P3 | Reference | `Document_Specialist/` |
| S-003 | Audio Specialist | Specialist | P3 | Reference | `Audio_Specialist/` |
| S-004 | Image Specialist | Specialist | P3 | Reference | `Image_Specialist/` |
| S-005 | Research Specialist | Specialist | P3 | Reference | `Research_Specialist/` |

---

## DIVISION SUMMARY

### Mobile Division (5 Commanders)
Primary user-facing agents handling direct interaction and system control.

| Commander | Primary Function | Source |
|-----------|-----------------|--------|
| Chat | Natural language interface | `mobile/AIService.ts` |
| Terminal | SSH & command execution | `mobile/AIService.ts` |
| Code | Code generation & analysis | `mobile/AIService.ts` |
| Data | Data processing | `mobile/AIService.ts` |
| Evolution | Self-improvement | `mobile/AIService.ts` |

### HASA Division (2 Commanders)
Human Accessibility and Speech Agents providing accessibility features.

| Commander | Primary Function | Source |
|-----------|-----------------|--------|
| FEIA | Speech-to-text, intent | `core/feia_agent.py` |
| CSA | Text simplification | `core/csa_agent.py` |

### Research Division (13 Commanders)
Comprehensive research team with specialized capabilities.

| Commander | Primary Function | Source |
|-----------|-----------------|--------|
| Research Commander | Team orchestration | `research/research_agents.py` |
| Web Researcher | Multi-engine search | `research/research_agents.py` |
| Research Analyst | 5-step synthesis | `research/research_agents.py` |
| Geo-Tech Asia | Asia-Pacific sources | `research/research_agents.py` |
| Geo-Tech Eurasia | Eurasia/ME sources | `research/research_agents.py` |
| Geo-Tech West | Americas/Europe sources | `research/research_agents.py` |
| Global Threat Analyst | Security analysis | `research/research_agents.py` |
| Horizon Scanner | Future trends | `research/research_agents.py` |
| Query Adaptor | Query optimization | `research/research_agents.py` |
| Blind Spot Detector | Gap analysis | `research/research_agents.py` |
| Reflection Analyst | MDT scoring & QA | `research/research_agents.py` |
| Browser Researcher | Dynamic content | `research/research_agents.py` |
| Socratic Teacher | Deep teaching | `research/socratic_teacher.py` |

### Specialist Division (5 Commanders)
Media processing and specialized analysis agents.

| Commander | Primary Function | Source |
|-----------|-----------------|--------|
| Video Specialist | Video processing | `specialists/video_specialist/` |
| Document Specialist | Document OCR | `specialists/document_specialist/` |
| Audio Specialist | Audio processing | `specialists/audio_specialist/` |
| Image Specialist | Image recognition | `specialists/image_specialist/` |
| Research Specialist | Deep research | `specialists/research_specialist/` |

---

## STATUS DEFINITIONS

| Status | Description |
|--------|-------------|
| Production | Fully operational, tested, deployed |
| MVP-Ready | Minimum viable product complete |
| Placeholder | Stub implementation, awaiting development |
| Reference | Code exists, requires integration |
| Initiated | Development started, not operational |
| In Progress | Active development underway |
| Complete | All documentation and testing finished |

---

## PRIORITY DEFINITIONS

| Priority | Description | Timeline |
|----------|-------------|----------|
| P1 | Critical path, immediate implementation | Phase 1 |
| P2 | Important, follows P1 completion | Phase 2 |
| P3 | Enhancement, after core stability | Phase 3 |

---

## DOCUMENTATION STATUS

| Commander | 01_Planning | 02_Tasks | 03_DevLog | 04_Status | 05_Tech | 06_Test | 07_Perf | 08_Change | 09_Review |
|-----------|-------------|----------|-----------|-----------|---------|---------|---------|-----------|-----------|
| M-001 Chat | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| M-002 Terminal | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| M-003 Code | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| M-004 Data | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| M-005 Evolution | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| H-001 FEIA | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| H-002 CSA | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-001 Research | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-002 Web | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-003 Analyst | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-004 Asia | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-005 Eurasia | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-006 West | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-007 Threat | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-008 Horizon | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-009 Query | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-010 BlindSpot | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-011 Reflect | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-012 Browser | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| R-013 Socratic | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| S-001 Video | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| S-002 Document | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| S-003 Audio | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| S-004 Image | □ | □ | □ | □ | □ | □ | □ | □ | □ |
| S-005 Research | □ | □ | □ | □ | □ | □ | □ | □ | □ |

**Legend:** □ = Not Started | ◐ = In Progress | ✓ = Complete

---

## SOURCE FILE MAPPING

### TypeScript Sources
```
mobile/
├── AIService.ts         → M-001, M-002, M-003, M-004, M-005
├── GeminiService.ts     → Cloud LLM integration
├── GeminiNanoService.ts → On-device LLM
├── OptimizedGeminiService.ts → Optimized wrapper
├── StorageService.ts    → Secure storage
├── App.tsx              → React Native app
└── index.js             → WebSocket server
```

### Python Sources
```
core/
├── feia_agent.py        → H-001 FEIA
├── csa_agent.py         → H-002 CSA
└── lib_admin_core/      → Base framework

research/
├── research_agents.py   → R-001 through R-012
├── socratic_teacher.py  → R-013
└── cosmic_base_agent.py → Research framework

specialists/
├── video_specialist/    → S-001
├── document_specialist/ → S-002
├── audio_specialist/    → S-003
├── image_specialist/    → S-004
└── research_specialist/ → S-005
```

---

## REVISION HISTORY

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-12-24 | Claude Code | Initial index |
| 2.0.0 | 2025-12-24 | Claude Code | Protocol compliance |

---

**Document Status:** ACTIVE
**Last Updated:** 2025-12-24
