# Claude Instructions

This repository contains the agentic-development plugin for Claude Code.

## Philosophy

This plugin embodies a **zero-assumption, spec-driven, professional execution** approach to software development. Every design decision reflects these principles:

1. **Zero Assumptions**: Fail on ambiguity, never guess user intent
2. **Professional Tone**: Concise, technical, direct - no fluff
3. **Token Consciousness**: Minimize context usage at every level
4. **Quality Gates**: Validation checkpoints cannot be skipped
5. **Self-Learning**: Persistent memory improves performance over time

## Working on This Repository

For detailed instructions on adding/modifying agents, see **@AGENTS.md**.

### Quick Reference

**Adding a Stage 6 agent:**
1. Create `agents/<agent-name>.md` using template in @AGENTS.md
2. Update `skills/orchestrator/SKILL.md` Available Agents list
3. Test with orchestrator
4. Update README.md

**Modifying existing agents:**
1. Read @AGENTS.md "Modifying Existing Agents" section
2. Maintain consistency: concise, professional, no fluff
3. Never remove constraints
4. Test thoroughly if changing gates

**Understanding the flow:**
- Review mermaid diagram in README.md
- Read `skills/orchestrator/SKILL.md`
- Trace through agent definitions in order

## Key Constraints

### Tone & Style

**Required:**
- Concise, technical, direct
- Token-efficient (no verbose explanations)
- Professional, objective
- Code examples over prose

**Prohibited:**
- Marketing language ("amazing", "powerful", "revolutionary")
- Subjective claims without evidence
- Fluff, redundancy, obvious statements
- Condescending language ("easy", "simple", "just")

### Agent Design

All agents must:
- Include YAML frontmatter with `name` and `description`
- Have Memory Management section
- Define clear Inputs and Outputs
- Specify Token Efficiency requirements
- List explicit Constraints

Gates must:
- Define exhaustive PASS criteria
- Provide concrete FAIL examples
- Specify routing logic (loop to which stage?)
- Return only PASS or FAIL with specific feedback

### File Organization

```
agents/          # Individual agent definitions (one per file)
skills/          # Orchestrator skill + templates
.claude-plugin/  # Plugin manifest
```

## Development Workflow

When working on this repository:

1. **Understand the change**: Read @AGENTS.md for context
2. **Make minimal changes**: One agent or one section at a time
3. **Maintain consistency**: Match existing tone and structure
4. **Test thoroughly**: Especially for gate changes
5. **Update docs**: README.md if user-facing, @AGENTS.md if developer-facing
6. **Commit atomically**: One logical change per commit

## Commit Messages

Use conventional commits:

```
feat(agent): add go-expert for Go code generation
fix(orchestrator): correct Stage 5-6 loop logic
docs(readme): add mermaid flow diagram
refactor(team-lead): clarify commit responsibilities
```

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

Scope: `agent`, `orchestrator`, `readme`, `docs`, or specific agent name

## Anti-Patterns to Avoid

❌ **Verbose agent definitions**: Keep instructions concise
❌ **Vague PASS/FAIL criteria**: Be exhaustive and specific
❌ **Missing memory management**: All agents must track learnings
❌ **Bypassing gates**: Orchestrator has no override authority
❌ **Assumption-making**: Fail and ask, never guess
❌ **Redundant documentation**: Don't repeat what's in code

## Testing Changes

1. Read the modified agent definition aloud - does it sound concise?
2. Check for contradictions with other agents
3. Verify memory management section is present
4. Test with orchestrator on a sample project
5. Confirm atomic commits are created correctly (if modifying team-lead)

## Architecture Decisions

### Why file-based context?
Token efficiency. Orchestrator sees paths, not content.

### Why memory files?
Persistent learning across iterations without bloating agent definitions.

### Why gates can't be overridden?
Enforces quality. No shortcuts. Professional execution.

### Why Team Lead does commits?
Has full task context. Natural checkpoint after Gate 7 pass. Atomic commits per task.

### Why zero assumptions?
User knows their domain. Agent doesn't. Failing fast prevents wasted work.

## Questions While Working

1. **Is this change concise?** Review "Tone & Style" above
2. **Does this maintain zero-assumption philosophy?** No guessing allowed
3. **Will this work with the orchestrator?** Check `skills/orchestrator/SKILL.md`
4. **Is memory management included?** Required for all agents
5. **Did I update all affected docs?** README.md and @AGENTS.md as needed

## Reference Documents

- **@AGENTS.md**: Detailed instructions for adding/modifying agents
- **README.md**: User-facing documentation
- **skills/orchestrator/SKILL.md**: Orchestration flow and logic
- **skills/orchestrator/memory-template.md**: Memory file format

## Contact

For questions or issues, file an issue at: https://github.com/jsell-rh/agentic-development
