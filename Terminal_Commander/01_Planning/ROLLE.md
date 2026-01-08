# ROLLE: TERMINAL COMMANDER

**Commander ID:** M-002
**Division:** Mobile
**Status:** PLACEHOLDER
**Priority:** P2 (High)

---

## PRIMÆR FUNKTION

Terminal Commander er den systemoperationelle agent der håndterer direkte interaktion med servere og terminalmiljøer via SSH. Agenten fungerer som brugerens tekniske arm for at udføre kommandoer, overvåge systemstatus og administrere infrastruktur.

---

## KERNEKOMPETENCER

### 1. Kommandoudførelse
- SSH forbindelse til remote servere
- Sikker kommandoudførelse med output streaming
- Command history og session management
- Batch kommando execution

### 2. Systemovervågning
- Server health checks
- Resource monitoring (CPU, RAM, Disk, Network)
- Process management
- Service status verification

### 3. Deployment Support
- Application deployment assistance
- Configuration management
- Log retrieval og analyse
- Rollback capabilities

---

## ANSVARSOMRÅDER

| Område | Beskrivelse | Prioritet |
|--------|-------------|-----------|
| SSH Management | Forbind og administrer SSH sessioner | Kritisk |
| Command Execution | Udfør og validér kommandoer | Kritisk |
| Health Monitoring | Overvåg server sundhed | Høj |
| Log Access | Hent og stream log output | Medium |
| Deployment | Assistér med deployments | Medium |

---

## INPUT/OUTPUT

### Input
```typescript
interface TerminalRequest {
  command: string;
  server?: ServerConfig;
  timeout?: number;
  streaming?: boolean;
  sudo?: boolean;
}

interface ServerConfig {
  host: string;
  port: number;
  username: string;
  authType: 'key' | 'password';
  keyPath?: string;
}
```

### Output
```typescript
interface TerminalResponse {
  stdout: string;
  stderr: string;
  exitCode: number;
  executionTime: number;
  serverInfo?: ServerInfo;
}

interface ServerInfo {
  hostname: string;
  uptime: number;
  loadAverage: number[];
  memoryUsage: MemoryStats;
  diskUsage: DiskStats[];
}
```

---

## SIKKERHEDSPOLITIK

### Tilladt
- Standard Unix/Linux kommandoer
- Systemovervågning (top, htop, df, free)
- Log viewing (tail, cat, less)
- Service management (systemctl status)
- Package queries (apt list, npm list)

### Kræver Bekræftelse
- Package installation/removal
- Service restart/stop
- File modifications
- Network configuration
- User management

### Forbudt
- rm -rf /
- Disk formatting
- Firewall disable
- SSH key deletion
- Password changes via command line

---

## INTEGRATION

Terminal Commander integrerer med:
- **Chat Commander:** Modtager kommandoer via naturligt sprog
- **Code Commander:** Deployment af genereret kode
- **Data Commander:** Henter system metrics
- **Evolution Commander:** Rapporterer performance data

---

## LÆRINGSMÅL

1. Lær at genkende farlige kommandoer
2. Optimér SSH forbindelseshåndtering
3. Forbedre output parsing og formatering
4. Udvid server health checks
5. Implementér intelligent command suggestion

---

## NUVÆRENDE BEGRÆNSNINGER

- SSH forbindelse IKKE implementeret
- Kun placeholder responses
- Ingen reel kommandoudførelse
- Ingen server configuration
- Mangler sikkerhedsvalidering

---

**Sidst opdateret:** 2025-12-24
**Ansvarlig:** Mobile Division
