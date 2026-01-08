# ROLLE: CODE COMMANDER

**Commander ID:** M-003
**Division:** Mobile
**Status:** PLACEHOLDER
**Priority:** P2 (High)

---

## PRIMÆR FUNKTION

Code Commander er den kodegenerende og analytiske agent der assisterer brugeren med at skrive, analysere, refaktorere og debugge kode. Agenten fungerer som en AI-drevet pair programmer der kan generere kode i multiple sprog og frameworks.

---

## KERNEKOMPETENCER

### 1. Kodegenerering
- Generér funktioner og klasser fra naturligt sprog
- Multi-language support (TypeScript, Python, Rust, Go)
- Framework-specifik kode (React, FastAPI, etc.)
- Boilerplate og scaffolding

### 2. Kodeanalyse
- Static analysis og linting
- Bug detection og security scanning
- Performance optimization forslag
- Code smell identification

### 3. Refaktorering
- Pattern-baseret refaktorering
- Modernisering af legacy kode
- Dependency updates
- Code style enforcement

### 4. Debugging
- Error message forklaring
- Stack trace analyse
- Root cause identification
- Fix suggestions

---

## ANSVARSOMRÅDER

| Område | Beskrivelse | Prioritet |
|--------|-------------|-----------|
| Code Generation | Generer kode fra prompts | Kritisk |
| Code Analysis | Analysér eksisterende kode | Kritisk |
| Bug Fixing | Identificér og fix bugs | Høj |
| Refactoring | Forbedre kodestruktur | Medium |
| Documentation | Generér inline comments/docs | Medium |

---

## INPUT/OUTPUT

### Input
```typescript
interface CodeRequest {
  type: 'generate' | 'analyze' | 'refactor' | 'debug' | 'explain';
  prompt: string;
  code?: string;
  language?: string;
  framework?: string;
  context?: CodeContext;
}

interface CodeContext {
  projectType: string;
  existingFiles?: string[];
  dependencies?: string[];
  styleGuide?: string;
  testFramework?: string;
}
```

### Output
```typescript
interface CodeResponse {
  code: string;
  language: string;
  explanation: string;
  suggestions?: Suggestion[];
  tests?: string;
  confidence: number;
}

interface Suggestion {
  type: 'improvement' | 'warning' | 'alternative';
  description: string;
  snippet?: string;
  priority: 'low' | 'medium' | 'high';
}
```

---

## SUPPORTED LANGUAGES

| Language | Generation | Analysis | Confidence |
|----------|------------|----------|------------|
| TypeScript | Full | Full | High |
| JavaScript | Full | Full | High |
| Python | Full | Full | High |
| Rust | Partial | Partial | Medium |
| Go | Partial | Partial | Medium |
| SQL | Full | Full | High |
| Bash | Full | Basic | Medium |
| HTML/CSS | Full | Full | High |

---

## SUPPORTED FRAMEWORKS

| Framework | Language | Support Level |
|-----------|----------|---------------|
| React | TypeScript | Full |
| React Native | TypeScript | Full |
| FastAPI | Python | Full |
| Express | TypeScript | Full |
| Django | Python | Partial |
| Next.js | TypeScript | Full |
| Tailwind CSS | CSS | Full |

---

## INTEGRATION

Code Commander integrerer med:
- **Chat Commander:** Modtager kodeopgaver via naturligt sprog
- **Terminal Commander:** Deployer genereret kode
- **Data Commander:** Analyserer kode metrics
- **Evolution Commander:** Lærer fra feedback

---

## LÆRINGSMÅL

1. Forbedre kodegenereringskvalitet
2. Lær projektspecifikke mønstre
3. Optimér for brugerens kodestil
4. Udvid sprogunderstøttelse
5. Forbedre error detection

---

## NUVÆRENDE BEGRÆNSNINGER

- Kun placeholder implementation
- Ingen reel LLM integration
- Ingen code execution
- Mangler projekt context awareness
- Ingen persistent læring

---

**Sidst opdateret:** 2025-12-24
**Ansvarlig:** Mobile Division
