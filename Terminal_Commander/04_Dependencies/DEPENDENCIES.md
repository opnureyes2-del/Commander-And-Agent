# DEPENDENCIES: TERMINAL COMMANDER

**Commander ID:** M-002
**Division:** Mobile
**Dependency Count:** 12

---

## DEPENDENCY OVERVIEW

```
┌─────────────────────────────────────────────────────────────┐
│                    TERMINAL COMMANDER                        │
│                      DEPENDENCIES                            │
├─────────────────────────────────────────────────────────────┤
│ RUNTIME          │ DEVELOPMENT      │ EXTERNAL               │
│ ─────────────    │ ──────────────   │ ────────────────       │
│ ssh2             │ TypeScript       │ SSH Servers            │
│ websocket        │ @types/ssh2      │ Backend Proxy          │
│ React Native     │ Jest             │ Key Store              │
└─────────────────────────────────────────────────────────────┘
```

---

## RUNTIME DEPENDENCIES

### 1. ssh2 (npm)
| Aspekt | Detalje |
|--------|---------|
| Version | ^1.15.0 |
| Purpose | SSH client implementation |
| Status | NOT INSTALLED |
| Critical | YES |

```json
{
  "ssh2": "^1.15.0"
}
```

### 2. websocket
| Aspekt | Detalje |
|--------|---------|
| Version | Existing in project |
| Purpose | Mobile-backend communication |
| Status | AVAILABLE |
| Critical | YES |

### 3. React Native Core
| Aspekt | Detalje |
|--------|---------|
| Version | 0.73+ |
| Purpose | Mobile app framework |
| Status | AVAILABLE |
| Critical | YES |

### 4. AsyncStorage
| Aspekt | Detalje |
|--------|---------|
| Version | Existing |
| Purpose | Store SSH configurations |
| Status | AVAILABLE |
| Critical | MEDIUM |

---

## DEVELOPMENT DEPENDENCIES

### TypeScript
```json
{
  "typescript": "^5.0.0",
  "@types/node": "^20.0.0",
  "@types/ssh2": "^1.11.0"
}
```

### Testing
```json
{
  "jest": "^29.0.0",
  "@testing-library/react-native": "^12.0.0"
}
```

### Linting
```json
{
  "eslint": "^8.0.0",
  "@typescript-eslint/parser": "^6.0.0"
}
```

---

## EXTERNAL DEPENDENCIES

### 1. Backend SSH Proxy Service
| Aspekt | Detalje |
|--------|---------|
| Type | Internal Service |
| Purpose | Bridge mobile to SSH servers |
| Protocol | WebSocket + SSH |
| Status | NOT BUILT |

**Required API:**
```typescript
interface SSHProxyAPI {
  connect(config: ServerConfig): Promise<SessionId>;
  execute(sessionId: string, command: string): Promise<CommandResult>;
  disconnect(sessionId: string): Promise<void>;
}
```

### 2. SSH Servers
| Aspekt | Detalje |
|--------|---------|
| Type | External Infrastructure |
| Purpose | Target servers for commands |
| Protocol | SSH (port 22) |
| Status | User-configured |

### 3. SSH Key Storage
| Aspekt | Detalje |
|--------|---------|
| Type | Secure Storage |
| Purpose | Store private keys |
| Implementation | Keychain (iOS) / Keystore (Android) |
| Status | PARTIAL |

---

## PEER DEPENDENCIES

### Required Commanders
| Commander | Purpose | Interface |
|-----------|---------|-----------|
| Chat Commander (M-001) | Task delegation | processMessage() |
| Code Commander (M-003) | Deployment support | deploy() |
| Data Commander (M-004) | Metrics collection | pushMetrics() |

### Optional Commanders
| Commander | Purpose | Interface |
|-----------|---------|-----------|
| Evolution (M-005) | Performance learning | reportMetrics() |

---

## DEPENDENCY GRAPH

```
Terminal Commander (M-002)
│
├── Runtime
│   ├── ssh2 ──────────────────► SSH Protocol
│   ├── websocket ─────────────► Backend Communication
│   ├── React Native ──────────► UI Framework
│   └── AsyncStorage ──────────► Local Storage
│
├── External
│   ├── Backend SSH Proxy ─────► Server Access
│   ├── SSH Servers ───────────► Target Systems
│   └── Keychain/Keystore ─────► Key Storage
│
└── Peer Commanders
    ├── Chat (M-001) ──────────► Task Source
    ├── Code (M-003) ──────────► Deployment
    └── Data (M-004) ──────────► Metrics
```

---

## INSTALLATION CHECKLIST

### npm packages
```bash
# Required
npm install ssh2
npm install @types/ssh2 --save-dev

# Already available
# websocket (existing)
# react-native (existing)
```

### Backend Setup
```bash
# SSH Proxy service (TBD)
# Configuration for target servers
# Key management system
```

---

## VERSION COMPATIBILITY

| Dependency | Min Version | Max Version | Tested |
|------------|-------------|-------------|--------|
| ssh2 | 1.11.0 | 1.15.x | NO |
| React Native | 0.72.0 | 0.73.x | YES |
| TypeScript | 5.0.0 | 5.3.x | YES |
| Node.js | 18.0.0 | 21.x | YES |

---

## SECURITY CONSIDERATIONS

| Dependency | Risk | Mitigation |
|------------|------|------------|
| ssh2 | Key exposure | Secure storage |
| websocket | Man-in-middle | TLS encryption |
| AsyncStorage | Data leakage | Encryption at rest |

---

**Sidst opdateret:** 2025-12-24
**Næste dependency review:** Ved implementation start
