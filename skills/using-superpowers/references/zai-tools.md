# z.ai Tool Mapping

Skills use Claude Code tool names as primary reference. On z.ai, most tools share the same names:

| Skill references | z.ai equivalent | Notes |
|-----------------|---------------|-------|
| `Read` (file reading) | `Read` | Same tool name |
| `Write` (file creation) | `Write` | Same tool name |
| `Edit` (file editing) | `Edit` | Same tool name |
| `MultiEdit` (batch edits) | `MultiEdit` | Same tool name |
| `Bash` (run commands) | `Bash` | Same tool name, 120s default timeout |
| `Grep` (search file content) | `Grep` | Uses ripgrep internally |
| `Glob` (search files by name) | `Glob` | Pattern matching |
| `LS` (list directories) | `LS` | Directory listing |
| `Skill` tool (invoke a skill) | `Skill` | Same mechanism as Claude Code |
| `Task` tool (dispatch subagent) | `Task` | Subagent types: general-purpose, Explore, Plan, full-stack-developer, frontend-styling-expert |
| Multiple `Task` calls (parallel) | Multiple `Task` calls | Send in single message for parallel execution |
| `TodoWrite` (task tracking) | `TodoWrite` | Same tool name |
| `TodoRead` (read tasks) | `TodoRead` | Same tool name |
| `WebSearch` | `web-search` via Skill | Invoke web-search skill |
| `WebFetch` | `web-reader` via Skill | Invoke web-reader skill |
| `Complete` (project completion) | `Complete` | Required after Next.js web dev projects |

## z.ai-specific conventions

- **Output directory**: All generated files go to `/home/z/my-project/download/`
- **Worklog**: Track progress at `/home/z/my-project/worklog.md`
- **No symlinks**: z.ai environment does not allow symbolic links; use file copies instead
- **IM mode**: When `send_message` tool is available, use it to deliver files to users
- **Subagent dispatch**: Subagents receive compressed context, not full conversation history
- **`<SUBAGENT-STOP>` tag**: Not functional on z.ai; use natural language guard instead
- **Available skills**: Subagents can see `<available_skills>` list and invoke skills via Skill tool
- **Timeout handling**: Default 120s; can specify up to 600000ms (10 minutes)

## Key differences from Claude Code

1. **No native EnterPlanMode/ExitPlanMode**: z.ai doesn't have a plan mode toggle. Skills that reference "EnterPlanMode" should interpret this as "beginning the planning phase" and route through the brainstorming skill instead.

2. **Subagent context isolation**: z.ai subagents don't inherit the main agent's full context. Always pass complete task descriptions and relevant context in the prompt.

3. **Tool name compatibility**: z.ai intentionally mirrors Claude Code tool names for maximum skill compatibility. Most skills work without modification.
