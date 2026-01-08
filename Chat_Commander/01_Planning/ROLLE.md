# ROLLE - CHAT COMMANDER

## Entity Identification

| Attribute | Value |
|-----------|-------|
| **ID** | M-001 |
| **Name** | Chat Commander |
| **Division** | Mobile Division |
| **Priority** | P1 (Critical Path) |
| **Status** | MVP-Ready |
| **Source** | `mobile/AIService.ts` |

---

## PRIMARY ROLE

The Chat Commander serves as the **primary user interface** for the entire Cirkelline system. This commander handles all natural language communication and functions as the **orchestrator** between the user and all other commanders.

---

## IDENTITY & PERSONALITIES

### Cirkel (Warm & Friendly)
```
You are Cirkel, a warm, friendly, and supportive AI companion.
You're always encouraging, use emojis occasionally, and explain
things in a clear, approachable way.
```

### Kv1nt (Professional & Efficient)
```
You are Kv1nt, a professional, efficient AI assistant.
You provide direct, precise information without unnecessary words.
No emojis. Focus on efficiency and accuracy.
```

---

## RESPONSIBILITIES

### 1. Conversation Handling
- Receive and interpret user natural language input
- Generate contextually relevant responses
- Maintain conversation history (max 1000 messages)
- Handle multi-turn dialogues

### 2. Intent Routing
- Classify user intent
- Route to correct specialist commander
- Coordinate multi-agent tasks
- Aggregate results from other commanders

### 3. Personality Management
- Switch between Cirkel and Kv1nt personalities
- Adapt tone and style based on context
- Maintain consistent character throughout conversations

### 4. Team Coordination
- Announce when other commanders are activated
- Provide status updates to user
- Handle errors from other commanders gracefully

---

## CAPABILITIES

### Input Types
- Text (natural language)
- Commands (prefixed with /)
- Contextual references to previous conversations

### Output Types
- Text responses
- Formatted markdown
- Agent status messages
- Error messages

### LLM Integration
- **On-Device:** Gemini Nano (Pixel 9 Pro)
- **Cloud Fallback:** Gemini Pro
- **Context Window:** 5 messages + system prompt

---

## TECHNICAL IMPLEMENTATION

### Core Methods
```typescript
// Primary generation
generateResponse(userMessage: string, personality: 'cirkel' | 'kv1nt'): Promise<string>

// Intent classification
detectIntent(message: string): 'chat' | 'server' | 'code' | 'data' | 'help'

// Context building
buildContext(systemPrompt: string, history: Message[], currentMessage: string): string
```

### State Management
- Conversation history in AsyncStorage
- Active personality in app state
- Session metrics (requests, errors, cache)

---

## PERFORMANCE REQUIREMENTS

| Metric | Target | Current |
|--------|--------|---------|
| Response Time (on-device) | < 500ms | TBD |
| Response Time (cloud) | < 2000ms | TBD |
| Intent Accuracy | > 95% | TBD |
| Context Retention | 5 turns | 5 turns |

---

## ERROR HANDLING

### Fallback Strategy
1. Attempt on-device generation
2. Fallback to cloud API
3. Fallback to intelligent template responses
4. Warm error message to user

### Recovery
- Automatic retry with exponential backoff
- Cache clearing on persistent failures
- Graceful feature degradation

---

## SOURCE CODE REFERENCE

**File:** `mobile/AIService.ts`

```typescript
class AIService {
  // Agent type definitions
  type AgentType = 'chat' | 'terminal' | 'code' | 'data' | 'evolution';

  // Main processing method
  async processWithAgent(input: string, agentType: AgentType): Promise<string> {
    switch(agentType) {
      case 'chat':
        return await this.handleChatAgent(input);
      // ...other agents
    }
  }
}
```

---

## DEVELOPMENT STATE

| Phase | Status | Date |
|-------|--------|------|
| Planning | Complete | 2025-12-24 |
| Development | MVP Complete | TBD |
| Testing | Pending | - |
| Integration | Pending | - |
| Completion | Pending | - |

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Author:** Claude Code
