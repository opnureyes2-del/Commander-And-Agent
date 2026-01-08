# TECHNICAL SPECIFICATIONS: TERMINAL COMMANDER

**Commander ID:** M-002
**Version:** 1.0.0

---

## API SPECIFICATION

```typescript
class TerminalCommander {
  /**
   * Terminal Agent for SSH command execution
   */

  // Connection Management
  async connect(config: ServerConfig): Promise<ConnectionResult>;
  async disconnect(sessionId: string): Promise<void>;
  async getConnectionStatus(sessionId: string): Promise<ConnectionStatus>;

  // Command Execution
  async executeCommand(request: CommandRequest): Promise<CommandResult>;
  async executeBatch(commands: CommandRequest[]): Promise<BatchResult>;
  async cancelCommand(commandId: string): Promise<boolean>;

  // System Monitoring
  async checkServerStatus(sessionId: string): Promise<ServerStatus>;
  async getResourceMetrics(sessionId: string): Promise<ResourceMetrics>;
  async streamLogs(sessionId: string, logPath: string): AsyncIterator<LogEntry>;

  // File Operations
  async uploadFile(sessionId: string, file: FileTransfer): Promise<TransferResult>;
  async downloadFile(sessionId: string, remotePath: string): Promise<Blob>;
}
```

---

## DATA TYPES

### Connection Types

```typescript
interface ServerConfig {
  id: string;
  host: string;
  port: number;
  username: string;
  authMethod: 'key' | 'password';
  privateKey?: string;
  password?: string;
  passphrase?: string;
  timeout: number;
  keepAlive: boolean;
  proxyConfig?: ProxyConfig;
}

interface ProxyConfig {
  type: 'http' | 'socks5';
  host: string;
  port: number;
  auth?: { username: string; password: string };
}

interface ConnectionResult {
  success: boolean;
  sessionId: string;
  serverInfo: ServerInfo;
  error?: ConnectionError;
}

interface ConnectionStatus {
  connected: boolean;
  sessionId: string;
  connectedAt: Date;
  lastActivity: Date;
  bytesTransferred: { rx: number; tx: number };
}
```

### Command Types

```typescript
interface CommandRequest {
  id?: string;
  command: string;
  timeout?: number;
  workingDirectory?: string;
  environment?: Record<string, string>;
  sudo?: boolean;
  streaming?: boolean;
  pty?: boolean;
}

interface CommandResult {
  id: string;
  command: string;
  stdout: string;
  stderr: string;
  exitCode: number;
  startTime: Date;
  endTime: Date;
  executionTimeMs: number;
  truncated: boolean;
}

interface BatchResult {
  totalCommands: number;
  successful: number;
  failed: number;
  results: CommandResult[];
  totalExecutionTimeMs: number;
}
```

### Monitoring Types

```typescript
interface ServerStatus {
  hostname: string;
  os: string;
  kernel: string;
  uptime: number;
  lastBoot: Date;
  loadAverage: [number, number, number];
  users: number;
}

interface ResourceMetrics {
  timestamp: Date;
  cpu: {
    usage: number;
    cores: number;
    model: string;
  };
  memory: {
    total: number;
    used: number;
    free: number;
    cached: number;
    swap: { total: number; used: number };
  };
  disk: DiskInfo[];
  network: NetworkInterface[];
}

interface DiskInfo {
  mount: string;
  filesystem: string;
  total: number;
  used: number;
  available: number;
  usagePercent: number;
}

interface NetworkInterface {
  name: string;
  ip: string;
  mac: string;
  rx: number;
  tx: number;
  status: 'up' | 'down';
}
```

### Error Types

```typescript
interface ConnectionError {
  code: 'AUTH_FAILED' | 'TIMEOUT' | 'HOST_UNREACHABLE' |
        'KEY_INVALID' | 'PERMISSION_DENIED' | 'UNKNOWN';
  message: string;
  details?: any;
  retryable: boolean;
}

interface CommandError {
  code: 'TIMEOUT' | 'BLOCKED' | 'VALIDATION_FAILED' |
        'SESSION_EXPIRED' | 'EXECUTION_FAILED';
  message: string;
  command: string;
  exitCode?: number;
}
```

---

## SECURITY SPECIFICATION

### Command Validation

```typescript
interface CommandValidator {
  // Validate command before execution
  validate(command: string): ValidationResult;

  // Check command risk level
  assessRisk(command: string): RiskAssessment;

  // Sanitize command input
  sanitize(command: string): string;
}

interface ValidationResult {
  valid: boolean;
  blocked: boolean;
  requiresConfirmation: boolean;
  warnings: string[];
  sanitizedCommand?: string;
}

interface RiskAssessment {
  level: 'safe' | 'low' | 'medium' | 'high' | 'critical';
  reasons: string[];
  reversible: boolean;
  auditRequired: boolean;
}
```

### Blocked Commands

```typescript
const BLOCKED_COMMANDS: string[] = [
  'rm -rf /',
  'rm -rf /*',
  'mkfs',
  'dd if=/dev/zero',
  ':(){:|:&};:',  // Fork bomb
  'chmod -R 777 /',
  '> /dev/sda',
  'mv /* /dev/null',
];

const CONFIRMATION_REQUIRED: string[] = [
  'rm -rf',
  'systemctl stop',
  'systemctl restart',
  'kill -9',
  'shutdown',
  'reboot',
  'apt remove',
  'pip uninstall',
];
```

---

## PERFORMANCE REQUIREMENTS

| Operation | Target | Maximum | Notes |
|-----------|--------|---------|-------|
| SSH Connect | < 2s | 5s | Including auth |
| Simple Command | < 500ms | 2s | ls, cat, echo |
| Complex Command | < 5s | 30s | grep, find |
| File Upload (1MB) | < 3s | 10s | Depends on network |
| Health Check | < 1s | 3s | Combined metrics |

---

## ARCHITECTURE

```
┌──────────────────────────────────────────────────────────────┐
│                     MOBILE APPLICATION                        │
├──────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                  Terminal Commander                      │ │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │ │
│  │  │ Connection   │  │  Command     │  │  Validator   │   │ │
│  │  │ Manager      │  │  Executor    │  │  Service     │   │ │
│  │  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘   │ │
│  │         │                 │                 │           │ │
│  │         └────────────┬────┴────────────────┘           │ │
│  │                      │                                   │ │
│  │              ┌───────┴───────┐                          │ │
│  │              │  WebSocket    │                          │ │
│  │              │  Client       │                          │ │
│  │              └───────┬───────┘                          │ │
│  └──────────────────────┼──────────────────────────────────┘ │
└──────────────────────────┼──────────────────────────────────┘
                           │ WSS
                           ▼
┌──────────────────────────────────────────────────────────────┐
│                     BACKEND SERVER                            │
├──────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────────┐│
│  │                   SSH Proxy Service                       ││
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   ││
│  │  │ Session      │  │  SSH Client  │  │  Audit       │   ││
│  │  │ Manager      │  │  Pool        │  │  Logger      │   ││
│  │  └──────────────┘  └──────┬───────┘  └──────────────┘   ││
│  └──────────────────────────┼────────────────────────────────┘│
└──────────────────────────────┼────────────────────────────────┘
                               │ SSH
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    TARGET SERVER(S)                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  Server A    │  │  Server B    │  │  Server C    │       │
│  │  (prod)      │  │  (staging)   │  │  (dev)       │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
└──────────────────────────────────────────────────────────────┘
```

---

## CONFIGURATION

```typescript
interface TerminalConfig {
  // Connection settings
  defaultTimeout: number;       // 30000ms
  maxRetries: number;           // 3
  keepAliveInterval: number;    // 10000ms
  maxConcurrentSessions: number; // 5

  // Security settings
  enableCommandValidation: boolean; // true
  enableAuditLogging: boolean;      // true
  blockDangerousCommands: boolean;  // true
  maxCommandLength: number;         // 10000

  // Performance settings
  connectionPoolSize: number;   // 3
  outputBufferSize: number;     // 1MB
  streamChunkSize: number;      // 16KB

  // Backend settings
  proxyUrl: string;
  proxyTimeout: number;
}
```

---

## IMPLEMENTATION NOTES

### Connection Flow
1. User initiates connect via Chat Commander
2. Terminal Commander validates ServerConfig
3. WebSocket message sent to backend
4. Backend establishes SSH connection
5. Session ID returned to mobile
6. Connection status monitored via heartbeat

### Command Flow
1. Command received from Chat Commander
2. Validator checks command safety
3. If blocked → reject with explanation
4. If requires confirmation → prompt user
5. If approved → send to backend
6. Backend executes via SSH
7. Output streamed back
8. Result logged and returned

---

**Document Status:** COMPLETE
