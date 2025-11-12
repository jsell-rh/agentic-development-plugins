# Agent Memory Template

This template defines the format for agent memory files in `.agent-memory/`.

## File Structure

Each agent maintains its own memory file: `.agent-memory/<agent-name>.md`

## Format

```markdown
# Agent Memory: <agent-name>

## Purpose
[One-line description of what this agent learns and tracks]

## Learnings

### YYYY-MM-DDTHH:MM:SSZ
- **Pattern**: [What pattern/issue was observed]
- **Action**: [What to do differently next time]
- **Context**: [Stage/gate/task where this occurred]

### YYYY-MM-DDTHH:MM:SSZ
- **Pattern**: [Description]
- **Action**: [Actionable change]
- **Context**: [Where/when]
```

## Guidelines

### What to Track
- ✅ Recurring patterns (ambiguities, gaps, errors)
- ✅ Successful solutions to problems
- ✅ Project-specific standards
- ✅ Common failure modes
- ✅ Actionable insights

### What NOT to Track
- ❌ One-off issues
- ❌ Vague observations
- ❌ Non-actionable information
- ❌ Code snippets (specs handle that)
- ❌ General knowledge (agents already have this)

### Entry Format
- **Timestamp**: ISO 8601 format (YYYY-MM-DDTHH:MM:SSZ)
- **Pattern**: Concise description of what was observed
- **Action**: Specific, actionable change for future iterations
- **Context**: Where in the flow this occurred (stage, gate, task)

### Maintenance
- Max 50 entries per file
- When limit reached, archive oldest entries
- Keep most impactful learnings
- Remove outdated patterns

## Examples

### Good Entry
```markdown
### 2025-01-12T14:30:00Z
- **Pattern**: Requirements missing authentication method specification
- **Action**: Always explicitly ask "What authentication method?" in initial validation
- **Context**: Stage 0, loop #3 - user assumed OAuth but wanted JWT
```

### Bad Entry
```markdown
### 2025-01-12T14:30:00Z
- **Pattern**: Something was unclear
- **Action**: Be more careful
- **Context**: Somewhere
```

## Agent-Specific Examples

### requirements-refiner.md
```markdown
# Agent Memory: requirements-refiner

## Purpose
Track common ambiguities and clarification patterns for this project

## Learnings

### 2025-01-12T14:30:00Z
- **Pattern**: "API" mentioned without specifying REST vs GraphQL vs gRPC
- **Action**: Add API protocol to standard clarification questions
- **Context**: Gate 0 failure, requirements unclear
```

### security-expert.md
```markdown
# Agent Memory: security-expert

## Purpose
Track vulnerability patterns and security fixes applied in this project

## Learnings

### 2025-01-12T15:45:00Z
- **Pattern**: SQL queries built with string concatenation in persistence layer
- **Action**: Enforce parameterized queries check in all persistence.py files
- **Context**: Stage 6, found in user module persistence
```

### team-lead.md
```markdown
# Agent Memory: team-lead

## Purpose
Track task assignment patterns and dependency learnings

## Learnings

### 2025-01-12T16:00:00Z
- **Pattern**: Database models must be complete before API endpoints
- **Action**: Always assign Python Expert for models before FastAPI Expert for routes
- **Context**: Stage 5, FastAPI expert blocked without complete models
```
