# Claude Code Subagents: Practical Examples

## Real-World Usage Patterns

### WordPress Development Workflows

#### Theme Debugging
```bash
# Automatic triggering when working on Walt Opie
"I'm seeing a 404 on the dharma talks page"
→ wordpress-dev agent activates automatically
→ Loads current theme state, checks custom post types
→ Provides specific WordPress debugging steps
```

#### Custom Post Type Issues
```bash
"Use wordpress-dev agent to debug the dharma_talk archive page"
→ Agent loads ACF configuration from state
→ Checks URL routing and template hierarchy
→ Provides targeted solutions based on project history
```

### MCP Authentication Troubleshooting

#### JWT Token Issues
```bash
"MCP authentication failing for waltopie.com"
→ mcp-config agent triggers on auth failure keywords  
→ Loads endpoint configuration from state
→ Tests JWT endpoints and provides token refresh steps
```

#### Cross-Environment Sync
```bash
"Use mcp-config agent to sync local to production"
→ Agent loads both endpoint configurations
→ Validates authentication for both environments
→ Provides specific MCP command sequences
```

### Security Response Workflows

#### Malware Detection
```bash
"Wordfence flagged suspicious files"
→ security-audit agent triggers on security keywords
→ Loads current security posture from state
→ Provides file integrity checking procedures
```

#### Vulnerability Assessment
```bash
"Use security-audit agent to audit plugin vulnerabilities"
→ Agent loads active plugins list from wordpress-dev state
→ Cross-references with known vulnerability databases  
→ Provides specific remediation steps
```

### Audio Content Management

#### File Verification
```bash
"Use audio-manager agent to verify dharma talk uploads"
→ Agent loads audio database schema from state
→ Checks file integrity and metadata consistency
→ Provides validation reports and fix procedures
```

## Agent Interaction Patterns

### Sequential Delegation
```bash
# Step 1: Development work
"Use wordpress-dev agent to add new dharma talk post type"

# Step 2: Security validation  
"Use security-audit agent to verify the new post type security"

# Step 3: Content management
"Use audio-manager agent to set up audio file handling for new post type"
```

### Contextual Handoffs
```bash
# WordPress issue leads to MCP debugging
"Theme not loading correctly" 
→ wordpress-dev agent investigates
→ Discovers MCP authentication issue
→ "Use mcp-config agent to fix the authentication problem"
```

### State-Driven Workflows
```bash
# Agent remembers previous work
"Use wordpress-dev agent to continue theme refactoring"
→ Loads state: "lastUpdated": "2025-08-30", "currentProject": "walt-opie-theme-refactor"
→ Resumes where previous session left off
→ References previous decisions and progress
```

## Integration with EIM

### Combined Workflow Example
```bash
# 1. Create development session with EIM
"Create a new terminal session for WordPress development"
→ EIM creates tmux session with iTerm integration

# 2. Delegate specialized work to CC subagent
"Use wordpress-dev agent to implement the new meditation timer feature"
→ wordpress-dev agent handles domain-specific implementation
→ Updates state with progress and decisions

# 3. Continue with security review
"Use security-audit agent to review the timer implementation"
→ security-audit agent performs specialized security analysis
```

### Session Management
```bash
# EIM handles process management
"List current terminal sessions" → EIM lists tmux sessions
"Switch to development session" → EIM handles iTerm/tmux integration
"Execute npm build in session" → EIM sends commands to tmux

# CC Subagents handle domain expertise  
"Use wordpress-dev agent to debug build errors" → Domain-specific debugging
"Use mcp-config agent to test API endpoints" → Authentication troubleshooting
```

## State Management Examples

### WordPress Development State
```json
{
  "lastUpdated": "2025-08-31T15:30:00Z",
  "currentProject": "walt-opie-meditation-timer",
  "wordpressVersion": "6.6",
  "activeTheme": "walt-opie-child", 
  "customPostTypes": ["dharma_talk", "events", "meditation_session"],
  "recentWork": {
    "task": "meditation timer implementation",
    "progress": "frontend complete, backend API in progress",
    "nextSteps": ["complete REST API", "add timer persistence"]
  },
  "knownIssues": [
    {
      "issue": "Timer state not persisting across page reloads",
      "severity": "medium",
      "investigationDate": "2025-08-31"
    }
  ]
}
```

### MCP Configuration State  
```json
{
  "lastUpdated": "2025-08-31T15:30:00Z",
  "authMethods": {
    "jwt": {
      "installed": true,
      "configured": true,
      "lastTested": "2025-08-31",
      "tokenExpiry": "2025-09-01T15:30:00Z"
    }
  },
  "endpoints": {
    "localhost": {
      "baseUrl": "http://localhost:8080/wp-json",
      "status": "active",
      "lastSuccess": "2025-08-31T15:25:00Z"
    },
    "production": {
      "baseUrl": "https://waltopie.com/wp-json",
      "status": "active", 
      "lastSuccess": "2025-08-31T14:45:00Z"
    }
  },
  "recentActivity": {
    "operation": "cross-environment sync",
    "success": true,
    "recordsSynced": 15
  }
}
```

## Error Handling Patterns

### Automatic Problem Detection
```bash
# MCP authentication fails
→ mcp-config agent automatically suggests token refresh
→ Provides exact command sequence based on current configuration
→ Updates state with success/failure results
```

### Cross-Agent Coordination
```bash
# Security issue affects development work
"Use security-audit agent to investigate file modifications"
→ security-audit agent identifies theme file changes
→ "Use wordpress-dev agent to review theme modifications"  
→ wordpress-dev agent analyzes changes in development context
```

### State Recovery
```bash
# Agent state becomes inconsistent
"Use wordpress-dev agent to reset project state"
→ Agent detects state inconsistency
→ Rebuilds state from current environment
→ Prompts for confirmation of detected configuration
```

## Performance Optimization

### Efficient State Updates
```json
// Only update changed sections
{
  "lastUpdated": "2025-08-31T15:30:00Z",
  "recentWork": {
    // Updated section
    "progress": "backend API complete, testing in progress"
  }
  // Other sections remain unchanged
}
```

### Minimal State Footprint
```javascript
// Keep state focused and relevant
// Remove completed items, archive old issues
// Maintain only actionable information
```

### Smart Agent Selection
```bash
# Let context determine the right agent
"The dharma talks aren't displaying properly"
→ Context suggests WordPress issue → wordpress-dev agent
→ Not generic "website broken" → specific domain expertise

"JWT token expired error in MCP"  
→ Clear MCP issue → mcp-config agent
→ Agent has specific authentication expertise
```

## Conclusion

CC subagents excel when:
- **Domain expertise** is required
- **Historical context** matters  
- **State persistence** enables better solutions
- **Automatic activation** reduces cognitive overhead

Combined with simplified EIM for tmux integration, this provides a clean separation of concerns:
- **EIM**: Terminal/session management
- **CC Subagents**: Domain-specific expertise and state management

The result is a powerful but simple multi-agent system that leverages Claude Code's built-in capabilities.