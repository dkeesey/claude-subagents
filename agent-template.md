# New Agent Template

## Creating a New CC Subagent

### 1. Agent Definition (Add to ~/.claude/CLAUDE.md)

```markdown
### [Agent Name] Agent
**Usage**: [Specific use cases and when to use this agent]
**Trigger**: "Use [agent-name] agent to..." or [automatic trigger conditions]
**Specializes in**: [Domain expertise areas, specific knowledge]
**Tools**: [Available tools: Read, Edit, Write, Bash, WebFetch, etc.]
**State**: `~/.claude/agent-state/[agent-name]-state.json`
```

### 2. Initial State File

Create: `~/.claude/agent-state/[agent-name]-state.json`

```json
{
  "lastUpdated": "2025-08-31T00:00:00Z",
  "domain": "[agent-domain]",
  "version": "1.0.0",
  "configuration": {
    // Agent-specific configuration
  },
  "currentContext": {
    // Current working context
  },
  "knownIssues": [
    // Array of known issues/patterns
  ],
  "recentActivity": {
    // Last operations performed
  },
  "references": {
    // Key resources, docs, endpoints
  }
}
```

### 3. State Schema Examples

#### Development Agent State
```json
{
  "lastUpdated": "2025-08-31T00:00:00Z",
  "domain": "development",
  "currentProject": "",
  "framework": "",
  "environment": {
    "local": "",
    "staging": "",
    "production": ""
  },
  "dependencies": [],
  "knownIssues": [],
  "recentWork": {
    "task": "",
    "progress": "",
    "nextSteps": []
  }
}
```

#### Configuration Agent State
```json
{
  "lastUpdated": "2025-08-31T00:00:00Z",
  "domain": "configuration",
  "services": {},
  "endpoints": {},
  "authMethods": {},
  "lastSuccessfulOperation": "",
  "knownIssues": []
}
```

#### Testing Agent State
```json
{
  "lastUpdated": "2025-08-31T00:00:00Z",
  "domain": "testing",
  "testSuites": [],
  "coverage": {},
  "lastRun": "",
  "failingTests": [],
  "knownIssues": []
}
```

### 4. Agent Activation Patterns

#### Explicit Triggers
```bash
"Use [agent-name] agent to [specific task]"
```

#### Automatic Triggers
- Keywords in user messages
- Error conditions detected
- Project context matches
- File patterns recognized

#### Context-Based Activation
```javascript
// Examples of automatic activation conditions
if (message.includes("test failures")) {
  activateAgent("testing");
}

if (currentProject === "specific-project") {
  activateAgent("project-specific");
}

if (errorType === "authentication") {
  activateAgent("auth-config");  
}
```

### 5. Agent Implementation Guidelines

#### State Management
```javascript
// Read state at start of operation
const state = JSON.parse(readFile('~/.claude/agent-state/agent-name-state.json'));

// Update relevant sections
state.lastUpdated = new Date().toISOString();
state.recentActivity = {
  operation: "task-description",
  timestamp: new Date().toISOString(),
  result: "success/failure"
};

// Write updated state
writeFile('~/.claude/agent-state/agent-name-state.json', JSON.stringify(state, null, 2));
```

#### Cross-Agent Communication
```javascript
// Read other agent states when needed
const relatedState = JSON.parse(readFile('~/.claude/agent-state/related-agent-state.json'));

// Check for relevant information
if (relatedState.knownIssues.length > 0) {
  // Coordinate with related agent's findings
}
```

#### Error Handling
```javascript
// Always wrap state operations
try {
  const state = JSON.parse(readFile('~/.claude/agent-state/agent-name-state.json'));
  // ... operations
} catch (error) {
  console.warn(`State file corrupt or missing, initializing default state`);
  // Initialize with default state
}
```

### 6. Testing Your Agent

#### Initial Validation
```bash
# Test explicit invocation
"Use [agent-name] agent to perform a simple test task"

# Verify state file creation/update
cat ~/.claude/agent-state/[agent-name]-state.json

# Test automatic triggering (if applicable)
"[trigger phrase or condition]"
```

#### State Verification
```bash
# Check state file syntax
python3 -c "import json; json.load(open('~/.claude/agent-state/[agent-name]-state.json'))"

# Verify state updates
# Before: note timestamp
# After operation: verify timestamp updated
```

### 7. Best Practices

#### State Design
- Keep state **minimal and focused**
- Use **ISO timestamps** for dates
- Structure data for **easy querying**
- **Archive old data** periodically

#### Agent Scope
- **Single domain** of expertise
- **Clear boundaries** with other agents
- **Specific trigger conditions**
- **Well-defined responsibilities**

#### Performance
- **Lazy load** large data structures
- **Cache frequently accessed** information
- **Clean up obsolete** state regularly
- **Minimize state file size**

#### Maintenance
- **Regular state cleanup**
- **Version state schema** changes
- **Document agent capabilities**
- **Test cross-agent interactions**

## Example: Creating a "Database Agent"

### Definition
```markdown
### Database Agent
**Usage**: Database schema changes, query optimization, migration management
**Trigger**: "Use database agent to..." or database error keywords  
**Specializes in**: SQL optimization, schema design, migration scripts, performance analysis
**Tools**: Read, Edit, Write, Bash
**State**: `~/.claude/agent-state/database-state.json`
```

### Initial State
```json
{
  "lastUpdated": "2025-08-31T12:00:00Z",
  "domain": "database",
  "connections": {
    "local": {
      "type": "mysql",
      "host": "localhost",
      "port": 3306,
      "database": "local_db"
    },
    "production": {
      "type": "mysql", 
      "host": "prod.example.com",
      "port": 3306,
      "database": "prod_db"
    }
  },
  "schemas": {
    "version": "1.2.0",
    "lastMigration": "2025-08-30",
    "pendingMigrations": []
  },
  "performance": {
    "slowQueries": [],
    "indexRecommendations": []
  },
  "knownIssues": []
}
```

### Usage
```bash
"Use database agent to optimize the user lookup query"
"Use database agent to create migration for new meditation_sessions table" 
"Database connection timeout errors" # Automatic trigger
```

This template provides a complete framework for creating domain-specific CC subagents that integrate seamlessly with the existing agent ecosystem.