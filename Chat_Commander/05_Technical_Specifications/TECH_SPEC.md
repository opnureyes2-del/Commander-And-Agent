# TECHNICAL SPECIFICATIONS - CHAT COMMANDER

## In-Depth Technical Details & API Documentation

**Commander ID:** M-001
**Version:** 1.0.0
**Last Updated:** 2025-12-24

---

## SYSTEM ARCHITECTURE

### Component Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         CHAT COMMANDER ARCHITECTURE                      │
│                                                                          │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐              │
│  │   UI Layer   │───▶│   Service    │───▶│  LLM Layer   │              │
│  │              │    │    Layer     │    │              │              │
│  └──────────────┘    └──────────────┘    └──────────────┘              │
│         │                   │                   │                       │
│         ▼                   ▼                   ▼                       │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐              │
│  │   Storage    │    │   Router     │    │   Cache      │              │
│  │   Layer      │    │   Layer      │    │   Layer      │              │
│  └──────────────┘    └──────────────┘    └──────────────┘              │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## API SPECIFICATION

### Core Interface

```typescript
interface ChatCommander {
  // Primary entry point
  processMessage(input: ChatInput): Promise<ChatOutput>;

  // Personality management
  setPersonality(personality: Personality): void;
  getPersonality(): Personality;

  // Context management
  getHistory(): Message[];
  clearHistory(): void;

  // Health & status
  healthCheck(): Promise<HealthStatus>;
  getMetrics(): CommanderMetrics;
}
```

### Data Types

```typescript
// Input types
interface ChatInput {
  message: string;
  context?: {
    sessionId: string;
    userId?: string;
    metadata?: Record<string, any>;
  };
}

// Output types
interface ChatOutput {
  response: string;
  routing?: {
    targetAgent: string;
    action: string;
    payload: any;
  };
  metadata: {
    processingTime: number;
    modelUsed: string;
    confidence: number;
  };
}

// Message types
interface Message {
  id: string;
  type: 'user' | 'agent' | 'system';
  content: string;
  timestamp: number;
  agentType?: string;
  personality?: Personality;
}

// Personality type
type Personality = 'cirkel' | 'kv1nt';

// Health status
interface HealthStatus {
  status: 'healthy' | 'degraded' | 'unhealthy';
  details: {
    llmAvailable: boolean;
    storageAvailable: boolean;
    cacheHitRate: number;
    avgResponseTime: number;
  };
}

// Metrics
interface CommanderMetrics {
  totalRequests: number;
  successfulRequests: number;
  failedRequests: number;
  avgResponseTime: number;
  cacheHitRate: number;
  activePersonality: Personality;
}
```

---

## INTEGRATION POINTS

### LLM Integration

```typescript
// Gemini Nano (On-Device)
interface GeminiNanoConfig {
  model: 'gemini-nano';
  maxTokens: 512;
  temperature: 0.7;
  fallbackToCloud: boolean;
}

// Gemini Pro (Cloud)
interface GeminiProConfig {
  model: 'gemini-pro';
  apiKey: string;
  maxTokens: 1024;
  temperature: 0.7;
}

// LLM Service Interface
interface LLMService {
  generate(prompt: string, config: LLMConfig): Promise<string>;
  isAvailable(): Promise<boolean>;
  getCapabilities(): LLMCapabilities;
}
```

### Storage Integration

```typescript
// Storage Service Interface
interface StorageService {
  // Message operations
  saveMessages(messages: Message[]): Promise<boolean>;
  loadMessages(): Promise<Message[]>;
  clearMessages(): Promise<boolean>;

  // Settings operations
  saveSettings(settings: AppSettings): Promise<boolean>;
  loadSettings(): Promise<AppSettings>;

  // Secure storage
  saveSecureCredential(key: string, value: string): Promise<boolean>;
  loadSecureCredential(key: string): Promise<string | null>;
}
```

### Agent Router Integration

```typescript
// Router Interface
interface AgentRouter {
  route(intent: Intent): RouteResult;
  getAvailableAgents(): AgentInfo[];
  isAgentAvailable(agentId: string): boolean;
}

interface RouteResult {
  targetAgent: string;
  confidence: number;
  alternativeAgents: string[];
}

interface Intent {
  primary: string;
  secondary?: string[];
  entities?: Record<string, string>;
  confidence: number;
}
```

---

## UNDERLYING TECHNOLOGIES

### Runtime Environment

| Component | Technology | Version |
|-----------|------------|---------|
| Runtime | React Native | 0.73+ |
| Language | TypeScript | 5.0+ |
| State Management | React Context | - |
| Storage | AsyncStorage | Latest |
| Secure Storage | react-native-keychain | Latest |

### LLM Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| On-Device | Gemini Nano | Primary inference |
| Cloud | Gemini Pro | Fallback |
| SDK | @google/generative-ai | API access |

### Dependencies

```json
{
  "dependencies": {
    "@google/generative-ai": "^0.1.0",
    "@react-native-async-storage/async-storage": "^1.21.0",
    "react-native-keychain": "^8.1.2"
  }
}
```

---

## CONFIGURATION

### Default Configuration

```typescript
const DEFAULT_CONFIG = {
  // LLM settings
  llm: {
    preferOnDevice: true,
    cloudFallbackEnabled: true,
    maxRetries: 3,
    timeout: 30000,
  },

  // Context settings
  context: {
    maxHistoryLength: 1000,
    contextWindowSize: 5,
    trimStrategy: 'oldest-first',
  },

  // Cache settings
  cache: {
    enabled: true,
    ttl: 300000, // 5 minutes
    maxSize: 100,
  },

  // Personality settings
  personality: {
    default: 'cirkel',
    persistAcrossSessions: true,
  },

  // Performance settings
  performance: {
    metricsEnabled: true,
    samplingRate: 1.0,
    logLevel: 'info',
  },
};
```

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| GEMINI_API_KEY | Cloud API key | For cloud mode |
| LOG_LEVEL | Logging verbosity | No (default: info) |
| CACHE_ENABLED | Enable caching | No (default: true) |

---

## PROCESSING PIPELINE

### Request Flow

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Input   │───▶│  Parse   │───▶│ Classify │───▶│  Route   │
│ Received │    │  & Valid │    │  Intent  │    │          │
└──────────┘    └──────────┘    └──────────┘    └────┬─────┘
                                                     │
                     ┌───────────────────────────────┘
                     │
                     ▼
              ┌──────────┐    ┌──────────┐    ┌──────────┐
              │ Generate │───▶│  Format  │───▶│  Return  │
              │ Response │    │  Output  │    │          │
              └──────────┘    └──────────┘    └──────────┘
```

### Step Details

1. **Input Received:** Message received from UI layer
2. **Parse & Validate:** Sanitize input, check length, validate format
3. **Classify Intent:** Determine user intent using NLP
4. **Route:** Decide if handling locally or routing to specialist
5. **Generate Response:** Query LLM with context
6. **Format Output:** Apply personality formatting
7. **Return:** Send response to UI layer

---

## ERROR HANDLING

### Error Categories

```typescript
enum ErrorCategory {
  LLM_FAILURE = 'llm_failure',
  NETWORK_ERROR = 'network_error',
  STORAGE_ERROR = 'storage_error',
  VALIDATION_ERROR = 'validation_error',
  ROUTING_ERROR = 'routing_error',
  TIMEOUT_ERROR = 'timeout_error',
}

interface CommanderError {
  category: ErrorCategory;
  code: string;
  message: string;
  recoverable: boolean;
  retryable: boolean;
  userMessage: string;
}
```

### Recovery Strategies

| Error Type | Strategy | Fallback |
|------------|----------|----------|
| LLM Failure | Retry with backoff | Template response |
| Network Error | Retry 3x | Offline mode |
| Storage Error | In-memory | Notify user |
| Timeout | Extend timeout | Partial response |

---

## PERFORMANCE REQUIREMENTS

### Latency Targets

| Operation | Target | P95 | P99 |
|-----------|--------|-----|-----|
| Intent Classification | 50ms | 100ms | 200ms |
| On-Device Generation | 300ms | 500ms | 800ms |
| Cloud Generation | 1500ms | 2500ms | 4000ms |
| Total Response | 500ms | 1000ms | 2000ms |

### Resource Limits

| Resource | Limit | Notes |
|----------|-------|-------|
| Memory | 100MB | Maximum heap usage |
| Storage | 50MB | Message history |
| Cache | 10MB | Response cache |
| Network | 1MB/req | Max request size |

---

## SECURITY CONSIDERATIONS

### Data Handling

- All credentials stored in secure keychain
- No PII logged
- Message history encrypted at rest
- API keys never exposed to client

### Input Validation

```typescript
function validateInput(input: ChatInput): ValidationResult {
  const checks = [
    () => input.message.length > 0,
    () => input.message.length <= 10000,
    () => !containsInjectionPatterns(input.message),
    () => isValidUTF8(input.message),
  ];

  return checks.every(check => check());
}
```

---

## MONITORING & OBSERVABILITY

### Metrics Collected

| Metric | Type | Description |
|--------|------|-------------|
| request_count | Counter | Total requests |
| request_duration | Histogram | Response times |
| error_count | Counter | Errors by type |
| cache_hit_ratio | Gauge | Cache effectiveness |
| active_sessions | Gauge | Concurrent users |

### Health Check Endpoint

```typescript
async function healthCheck(): Promise<HealthStatus> {
  const llmHealth = await checkLLM();
  const storageHealth = await checkStorage();
  const cacheHealth = checkCache();

  return {
    status: deriveOverallStatus(llmHealth, storageHealth, cacheHealth),
    details: {
      llmAvailable: llmHealth.available,
      storageAvailable: storageHealth.available,
      cacheHitRate: cacheHealth.hitRate,
      avgResponseTime: getAvgResponseTime(),
    },
  };
}
```

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Next Review:** Upon implementation changes
