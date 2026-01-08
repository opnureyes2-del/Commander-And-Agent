# 60_CLAUDE_MD - INDEX
**Sektion:** AI Instruktioner og Projekt Kontekst
**Sidst Opdateret:** 2026-01-08 16:35
**Version:** v2.0.0

---

## OVERSIGT

Denne sektion indeholder CLAUDE.md filen for commander-and-agent - commander system dokumentation.

---

## FILER I DENNE SEKTION

| Fil | Beskrivelse |
|-----|-------------|
| CLAUDE.md | AI instruktioner for commander-and-agent |

**Location:** `/home/rasmus/Desktop/projekts/projects/commander-and-agent/CLAUDE.md`

---

## HURTIG REFERENCE

### Projekt Details
- **Type:** FastAPI Multi-Agent Commander System
- **Agents:** 25 commanders (5 divisions)
- **Language:** Python 3.12+
- **Framework:** FastAPI + CrewAI
- **Status:** ~60% Complete (autonomy pending)

---

## FEJLHÅNDTERING

### Problem 1: CLAUDE.md References Old Agent Names Or Structure

**Symptom:** CLAUDE.md mentions commanders that don't exist, old folder structure, or deprecated agent names

**Årsag:**
- Commander structure refactored (monolith → 5 divisions)
- Agent names changed during restructuring
- CLAUDE.md not updated after major refactor
- References point to archived agents

**Diagnosticering:**
```bash
cd /home/rasmus/Desktop/projekts/projects/commander-and-agent
find . -name "*commander*" -type f
ls -la commanders/
grep -i "commander\|agent" CLAUDE.md | head -20
```

**Fix:**
1. List current 25 active commanders
2. Document 5 divisions: Combat, Engineering, Intelligence, Logistics, Science
3. Update file structure references
4. Remove references to archived agents
5. Add current autonomy status (ExecutionEngine)
6. Update API endpoints to match current routes

**Prevention:**
- Update CLAUDE.md whenever commander structure changes
- Link CLAUDE.md updates to git commits
- Add CLAUDE.md verification to pre-commit hooks

---

### Problem 2: CLAUDE.md Missing XP System Reset Documentation (2025-12-26)

**Symptom:** CLAUDE.md doesn't mention that XP was reset, old XP values referenced, confusion about current progress

**Årsag:**
- XP system reset on 2025-12-26 (all commanders 0 XP)
- CLAUDE.md shows old XP values
- Reset rationale not documented
- Current training status unclear

**Diagnosticering:**
```bash
grep -i "xp\|experience\|level" CLAUDE.md
psql -d commander_db -c "SELECT commander_name, xp FROM commanders ORDER BY xp DESC LIMIT 10;"
git log --since="2025-12-20" --grep="xp\|reset" --oneline
```

**Fix:**
1. Add "CRITICAL: XP Reset 2025-12-26" section to CLAUDE.md
2. Document reset reason: "Clean slate for Phase 1 autonomy"
3. List all 25 commanders at 0 XP currently
4. Add training roadmap: Phase 1 → 100 XP target per commander
5. Document XP earning system: Task completion, autonomy milestones
6. Add verification: Query database to confirm current XP values

**Prevention:**
- Document all major system resets immediately
- Add XP system changes to CHANGELOG
- Link to decision documentation (why reset)

---

### Problem 3: CLAUDE.md Doesn't Explain ExecutionEngine Or Autonomy Blockers

**Symptom:** CLAUDE.md doesn't mention ExecutionEngine, autonomy status unclear, Phase 1 blockers not documented

**Årsag:**
- ExecutionEngine is new infrastructure (not in old CLAUDE.md)
- Autonomy implementation in progress but not documented
- Phase 1 blockers not clearly stated
- Current capabilities vs planned capabilities unclear

**Diagnosticering:**
```bash
cd /home/rasmus/Desktop/projekts/projects/commander-and-agent
find . -name "*execution*" -o -name "*autonomy*"
grep -r "ExecutionEngine" --include="*.py"
grep -i "autonomous\|autonomy" CLAUDE.md
```

**Fix:**
1. Add "Autonomy Status" section to CLAUDE.md
2. Document ExecutionEngine: Purpose, current status, Phase 1 goal
3. List Phase 1 blockers:
   - ExecutionEngine registry incomplete
   - File write permissions for autonomous actions
   - Safety guardrails not implemented
4. Document current state: Manual delegation only (no autonomy yet)
5. Add roadmap: Phase 1 → basic autonomy, Phase 2 → full autonomy
6. Link to 10_ARKITEKTUR.md for technical details

**Prevention:**
- Update CLAUDE.md when major features added
- Document blockers immediately when identified
- Link CLAUDE.md to project roadmap

---

## CHANGELOG

| Dato | Tid | Handling | Af |
|------|-----|----------|-----|
| 2026-01-08 | 16:35 | Added FEJLHÅNDTERING section (3 problems) | Kv1nt |
| 2026-01-08 | 13:30 | Initial INDEX oprettet | Claude |

---

*60_CLAUDE_MD.md - Opdateret 2026-01-08 16:35*
