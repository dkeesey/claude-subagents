# Claude Code Subagents: Complete Usage Guide

## Overview

Claude Code subagents are **context-isolated specialists** within the same Claude process, as opposed to EIM's multi-process orchestration. They provide 80% of multi-agent benefits with significantly less complexity.

## Architecture: CC Subagents vs EIM

| Approach | Architecture | Isolation | Complexity | Use Case |
|----------|--------------|-----------|------------|----------|
| **CC Subagents** | Single process, multiple contexts | Context isolation | Low | Task specialization within sessions |
| **EIM Multi-Process** | Multiple Claude CLI instances | Process isolation | High | Cross-session orchestration |

**Decision**: Use CC subagents for task delegation, keep EIM simple for tmux integration.

## Subagent System Components

### 1. Agent Definitions (CLAUDE.md)
Located in `~/.claude/CLAUDE.md`, agents are defined with:
- **Usage**: When to trigger the agent
- **Trigger**: Specific phrases or conditions
- **Specializes in**: Domain expertise
- **Tools**: Available tool access
- **State**: JSON state file location

### 2. State Persistence (agent-state/)
Each agent maintains JSON state in `~/.claude/agent-state/`:
- Project context
- Configuration state
- Known issues
- Last activity timestamps

### 3. Agent Pattern
```
Read state JSON → Work → Update state JSON
```

## Current Agents

### WordPress Development Agent
```
Trigger: "Use wordpress-dev agent to..." or Walt Opie work
Domain: PHP templating, WordPress hooks, dharma talks
State: ~/.claude/agent-state/wordpress-dev-state.json
```

### MCP Configuration Agent  
```
Trigger: "Use mcp-config agent to..." or MCP auth failures
Domain: MCP debugging, authentication flows, API integration
State: ~/.claude/agent-state/mcp-config-state.json
```

### Security Audit Agent
```
Trigger: "Use security-audit agent to..." or security issues
Domain: File integrity, plugin vulnerabilities, security protocols
State: ~/.claude/agent-state/security-audit-state.json
```

### Audio Content Manager Agent
```
Trigger: "Use audio-manager agent to..." or meditation audio work
Domain: Audio verification, database content, MP3 organization
State: ~/.claude/agent-state/audio-manager-state.json
```

## Usage Patterns

### Explicit Invocation
```bash
"Use wordpress-dev agent to debug the theme hierarchy issue"
"Use mcp-config agent to fix JWT authentication"
"Use security-audit agent to scan for malware"
```

### Automatic Triggering
Agents automatically activate based on:
- Project context (Walt Opie site → wordpress-dev)
- Error conditions (MCP auth failure → mcp-config) 
- Security events (malware detection → security-audit)

### State Management
Agents maintain continuity through JSON state files:
```json
{
  "lastUpdated": "2025-08-30T22:50:00Z",
  "currentProject": "walt-opie-local",
  "wordpressVersion": "6.6",
  "activeTheme": "walt-opie-child",
  "knownIssues": []
}
```

## Best Practices

### When to Use CC Subagents
- **Domain-specific tasks** requiring specialized knowledge
- **Stateful workflows** that span multiple interactions
- **Complex troubleshooting** with historical context
- **Repeated operations** with learned patterns

### When NOT to Use CC Subagents
- **Simple one-off tasks** without domain complexity
- **General development work** without specialization needs
- **Tasks requiring process isolation** (use EIM instead)

### Agent Design Principles
1. **Domain-focused**: Each agent owns a specific knowledge area
2. **Stateful**: Maintain context across interactions
3. **Minimal**: Simple JSON state, avoid over-engineering
4. **Triggered**: Automatic activation based on context/keywords

## Creating New Agents

### 1. Define in CLAUDE.md
```markdown
### Your New Agent
**Usage**: Specific use case description
**Trigger**: "Use your-agent to..." or automatic conditions
**Specializes in**: Domain expertise areas
**Tools**: Available tools (Read, Edit, Write, Bash, etc.)
**State**: `~/.claude/agent-state/your-agent-state.json`
```

### 2. Initialize State File
```json
{
  "lastUpdated": "2025-08-31T10:00:00Z",
  "domain": "your-domain",
  "configuration": {},
  "knownIssues": [],
  "lastActivity": null
}
```

### 3. Test Invocation
```bash
"Use your-agent to perform initial task"
```

## Task Delegation with CC Subagents

### Traditional Approach
```bash
# Spawn new Claude Code instance
SESSION_NAME="delegate-worker-$(date +%s)"
tmux new-session -d -s "$SESSION_NAME"
# Start Claude Code in that session
# Send task as prompt
```

### CC Subagents Approach
```bash
"Use [appropriate-agent] to [specific task]"
# Agent automatically loads with:
# - Domain-specific context
# - Historical state
# - Specialized tool access
```

## Integration with EIM

### Simplified EIM Role
After this analysis, EIM's role is **purely tmux integration**:
- Create terminal sessions
- Execute commands in sessions
- List/switch/close sessions
- Directory management

### CC Subagents Role
Handle **task specialization**:
- Domain expertise
- Stateful workflows  
- Context preservation
- Automated triggering

### Workflow
```bash
1. Use EIM for tmux session management
2. Use CC subagents for specialized work within sessions
3. Avoid redundant multi-process orchestration
```

## Advanced Patterns

### Agent Composition
```bash
"Use wordpress-dev agent to update theme, then use security-audit agent to scan for vulnerabilities"
```

### State Sharing
Agents can read each other's state for coordination:
```javascript
// wordpress-dev agent checking security state
const securityState = JSON.parse(readFile('~/.claude/agent-state/security-audit-state.json'));
if (securityState.knownIssues.length > 0) {
  // Handle security concerns during development
}
```

### Cross-Session Continuity
State persistence enables resuming complex workflows:
```bash
# Session 1: "Use wordpress-dev agent to start theme refactoring"
# Session 2: "Use wordpress-dev agent to continue theme work" 
# → Agent loads previous state automatically
```

## Troubleshooting

### Agent Not Triggering
1. Check trigger phrase in CLAUDE.md
2. Verify state file exists and is readable
3. Confirm domain context matches

### State Corruption
1. Check JSON syntax in state file
2. Reset to minimal state structure
3. Update lastUpdated timestamp

### Performance Issues  
1. Keep state files minimal (< 5KB recommended)
2. Avoid complex nested structures
3. Clean up old/irrelevant state data

## Conclusion

CC subagents provide an elegant middle ground between single-context Claude and complex multi-process systems. They offer:

- **Specialization** without complexity
- **State persistence** without overhead  
- **Context isolation** without process boundaries
- **Automatic activation** without manual orchestration

The combination of simple EIM (tmux integration) + CC subagents (task specialization) delivers the benefits of multi-agent systems while avoiding the architectural complexity of EIM's original design.

---

**Result**: Standardized multi-agent workflow using built-in CC capabilities, with EIM simplified to its essential tmux integration role.