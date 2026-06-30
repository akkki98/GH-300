# GitHub Copilot Customization Guide
### Custom Agents · Skills · Hooks

This guide walks through three core ways to customize GitHub Copilot's behavior in a repository: **custom agents** (who Copilot is), **skills** (what Copilot knows how to do, on demand), and **hooks** (automation that fires deterministically around Copilot's actions). Each section includes the file format, step-by-step setup, and a real-world example you can copy into a project.

---

## 1. Copilot Skills (Agent Skills)

### What they are

Skills are **folders of specialized, on-demand knowledge** — instructions, scripts, and reference files — that Copilot loads into context only when relevant to the task at hand, instead of bloating every single request. Think of a skill as a runbook: "if the task looks like X, here's exactly how to do it."

Skills are an open, portable spec — the same `SKILL.md` format works across Copilot, the Copilot CLI, VS Code, and other compatible agents.

### Step-by-step setup

**Step 1 — Choose a location**

| Scope | Location |
|---|---|
| Project-specific (shared with team, committed to repo) | `.github/skills/` |
| Personal (used across all your projects) | `~/.copilot/skills/` |

**Step 2 — Create a folder for the skill**

Folder name should be lowercase, hyphenated, and descriptive:
```
.github/skills/incident-triage/
```

**Step 3 — Write the `SKILL.md` file**

Required frontmatter fields: `name` (must match the folder name) and `description` (this is what Copilot uses to decide *when* to load the skill — be specific about trigger conditions).

```
.github/skills/incident-triage/SKILL.md
```

**Step 4 — Add supporting resources (optional)**

Drop in templates, scripts, or examples that the skill's instructions reference:
```
.github/skills/incident-triage/
├── SKILL.md
├── postmortem-template.md
└── check-logs.sh
```

**Step 5 — Test discovery**

In VS Code, run `/skills` in chat to open the Configure Skills menu and confirm the skill is detected. You can also force-load it directly with `/incident-triage`.

### Real-world example

**Scenario:** Your on-call engineers want Copilot to walk through a consistent triage process the moment a production incident is mentioned, instead of improvising each time.

`.github/skills/incident-triage/SKILL.md`:
```yaml
---
name: incident-triage
description: 'Triage production incidents and failed deployments. Use when: a pipeline fails, an alert fires, or someone needs a first response / status update for stakeholders.'
---
# Incident Triage

## When to use this
- A CI/CD pipeline failed in `prod`
- A pager/alert fired
- Someone needs a stakeholder-facing status update

## Procedure
1. Confirm the affected service, environment, and blast radius.
2. Pull the last 3 deploys for the affected service and check timestamps
   against the alert firing time.
3. Run `check-logs.sh <service-name>` to pull the last 200 error-level logs.
4. Classify severity using `postmortem-template.md`'s severity matrix.
5. Draft a status update using this format:
   - **What's affected:**
   - **Customer impact:**
   - **Current status:**
   - **Next update by:**
6. If severity is SEV-1 or SEV-2, recommend opening a postmortem doc
   using `postmortem-template.md`.

## Resources in this folder
- `check-logs.sh` — pulls recent error logs for a given service
- `postmortem-template.md` — standard postmortem doc structure and severity matrix
```

**Result:** when a developer types "the checkout service alert just fired, help me figure out what's going on," Copilot recognizes the description match, loads this skill's full procedure into context, runs `check-logs.sh`, and produces a structured status update — every time, the same way, regardless of who's on call.

---

## 2. Copilot Custom Agents

### What they are

A custom agent is a **persona** — a Markdown file with YAML frontmatter that gives Copilot a specific identity, a restricted toolset, and standing instructions, so instead of one generalist assistant you get a team of specialists (`@test-agent`, `@docs-agent`, `@security-agent`) that behave consistently every time they're invoked.

### Step-by-step setup

**Step 1 — Choose a location**

| Scope | Location |
|---|---|
| Repository (shared with team) | `.github/agents/` |
| Personal (across all projects) | `~/.copilot/agents/` |
| Enterprise-wide | `/agents/` in the org's `.github-private` repo |

**Step 2 — Create the agent file**

Filename must end in `.agent.md`:
```
.github/agents/test-coverage.agent.md
```

**Step 3 — Write the frontmatter**

Only `description` is strictly required. Common fields:

| Field | Purpose |
|---|---|
| `name` | Display name (defaults to filename if omitted) |
| `description` | What the agent does — required |
| `tools` | Restrict which tools it can use (omit = all tools) |
| `model` | Pin a specific model (VS Code / IDEs) |
| `mcp-servers` | Attach agent-specific MCP servers |
| `handoffs` | Define hand-off buttons to other agents |

**Step 4 — Write the prompt body**

Below the frontmatter, define the persona, scope, and explicit boundaries — what it should always do, ask about, and never do. Vague personas ("a helpful assistant") perform poorly; specific ones with hard boundaries perform well.

**Step 5 — Commit and merge to the default branch**

For Copilot cloud agent on GitHub.com, the file must be merged into the default branch before it appears as an option.

**Step 6 — Use it**
- In chat: select it from the agent dropdown, or `@agent-name`
- In Copilot CLI: `/agent` slash command
- When assigning Copilot to a GitHub issue: pick it from the custom agent dropdown

### Real-world example

**Scenario:** Your team keeps asking Copilot to "write some tests," getting inconsistent coverage and occasionally getting source files modified by accident. You want a dedicated testing specialist that only touches the `tests/` directory and always targets 80%+ coverage.

`.github/agents/test-coverage.agent.md`:
```yaml
---
name: test-coverage
description: Analyzes code for test coverage gaps and writes unit/integration tests targeting 80%+ coverage
tools: ["read", "search", "edit", "execute"]
---
# Role and Identity

You are a senior test engineer focused exclusively on test coverage. You
analyze the codebase for untested or under-tested code paths and write
comprehensive unit and integration tests using Vitest, matching the
patterns already established in `tests/`.

## Always do
- Run `npm run test:coverage` first to see the current coverage report
  before writing anything.
- Target 80%+ line and branch coverage for any file you touch.
- Follow existing test file naming: `<source-file>.test.ts`.
- Write to the `tests/` directory only.

## Ask first
- Before adding a new test dependency or mocking library.
- Before modifying existing test fixtures shared across multiple test files.

## Never do
- Never modify files in `src/` to make a test pass.
- Never delete or skip a failing test to "fix" coverage — report it instead.
- Never weaken an assertion just to get a test green.

## Output
After running tests, report: tests added, coverage delta (before → after),
and any source-code bugs discovered during testing (do not fix them — list
them for the team to triage).
```

**Result:** a developer selects `@test-coverage` from the agent dropdown, says "improve coverage for the checkout module," and gets a consistent, bounded outcome — new tests in the right place, in the right style, with a clear report — instead of a generic agent that might wander into refactoring source code.

---

## 3. Copilot Hooks

### What they are

Hooks are **event-driven automation** — shell commands that fire automatically at specific points during a Copilot agent session. Unlike agents and skills, hooks don't involve the model "deciding" anything: they're deterministic. A hook can inject extra instructions, block a tool call outright, or run a side action like logging or linting, regardless of what the agent intends to do.

Common hook events:

| Event | Fires when |
|---|---|
| `sessionStart` | A new Copilot agent session begins |
| `userPromptSubmitted` | The user submits a chat prompt |
| `preToolUse` | Just before Copilot runs a tool (e.g. a shell command or file edit) |
| `postToolUse` | Just after a tool finishes running |
| `errorOccurred` | A tool call or session hits an error |

### Step-by-step setup

**Step 1 — Choose a location**

| Scope | Location |
|---|---|
| Repository (shared with team) | `.github/hooks/` |
| Personal (across all projects) | `~/.copilot/hooks/` |

**Step 2 — Create a hooks configuration file**

Name it however you like, but it must be `.json` and live in the hooks folder:
```
.github/hooks/safety-checks.json
```

**Step 3 — Define the hook(s)**

Each hook entry specifies an `event` to listen for, plus the command to run. Include **both** a `bash` key (Linux/macOS) and a `powershell` key (Windows) so the hook works cross-platform — Copilot automatically picks the right one for the user's OS.

```json
{
  "hooks": [
    {
      "event": "preToolUse",
      "bash": "./scripts/block-force-push.sh",
      "powershell": "./scripts/block-force-push.ps1"
    }
  ]
}
```

**Step 4 — Write the script the hook calls**

The hook receives session/tool data as JSON on stdin (e.g. `toolName`, `toolArgs`, `cwd`, `timestamp`). Your script reads that, decides what to do, and can return output that Copilot uses to block or modify the action.

**Step 5 — Test locally before committing**

Pipe sample input into your script directly to confirm it behaves correctly:
```bash
echo '{"timestamp":1704614400000,"cwd":"/repo","toolName":"bash","toolArgs":"{\"command\":\"git push --force\"}"}' | ./scripts/block-force-push.sh
echo $?              # check exit code
./scripts/block-force-push.sh | jq .   # validate output is valid JSON
```

**Step 6 — Commit to the default branch**

Like agents and skills, hooks must be merged into the default branch to take effect for Copilot cloud agent sessions.

### Real-world example

**Scenario:** Your team has been burned before by an agent accidentally force-pushing over a teammate's commits. You want a hard safety net that blocks `git push --force` no matter which agent or developer triggers it — independent of any prompt or instruction file, which an agent could in theory ignore under pressure from a confusing task.

`.github/hooks/safety-checks.json`:
```json
{
  "hooks": [
    {
      "event": "preToolUse",
      "bash": "scripts/hooks/block-dangerous-commands.sh",
      "powershell": "scripts/hooks/block-dangerous-commands.ps1"
    }
  ]
}
```

`scripts/hooks/block-dangerous-commands.sh`:
```bash
#!/bin/bash
INPUT=$(cat)
COMMAND=$(echo "$INPUT" | jq -r '.toolArgs' | jq -r '.command // empty')

if [[ "$COMMAND" == *"git push --force"* || "$COMMAND" == *"git push -f"* ]]; then
  echo '{"block": true, "reason": "Force pushes are disabled by team policy. Use --force-with-lease and get explicit approval, or ask a maintainer to push manually."}'
  exit 0
fi

echo '{"block": false}'
```

A second, lighter-weight example — automatically reminding the agent to run tests after it edits source code:

`.github/hooks/post-edit-reminder.json`:
```json
{
  "hooks": [
    {
      "event": "postToolUse",
      "bash": "scripts/hooks/remind-tests.sh",
      "powershell": "scripts/hooks/remind-tests.ps1"
    }
  ]
}
```

`scripts/hooks/remind-tests.sh`:
```bash
#!/bin/bash
INPUT=$(cat)
TOOL=$(echo "$INPUT" | jq -r '.toolName')

if [[ "$TOOL" == "edit" ]]; then
  echo '{"inject": "Reminder: run the test suite (npm test) before reporting this task as complete."}'
fi
```

**Result:** no matter which custom agent is running, what skill it loaded, or how the prompt was phrased, the `preToolUse` hook intercepts and blocks the dangerous command before it executes — and the `postToolUse` hook keeps nudging the agent toward good habits after every edit. This is a safety layer that sits below the model's judgment, not a suggestion to it.

---

## How the three work together

| Primitive | Answers | Lifespan |
|---|---|---|
| **Skills** | "What's the exact procedure for this kind of task?" | Loaded only when relevant |
| **Custom agents** | "Who is doing the work, with what tools and boundaries?" | Active only when that agent is selected |
| **Hooks** | "What must always happen — or never happen — regardless of what the agent decides?" | Fires automatically on matching events, every session |

A realistic combined setup: the `@security-agent` custom agent is a persona restricted to read/search tools that never edits source code. When someone asks that agent about a production alert, it automatically pulls in the `incident-triage` skill for the step-by-step procedure. Underneath both, a `preToolUse` hook blocks destructive commands like `git push --force` no matter which agent is active or what the prompt says. All three are committed to the repo, version-controlled, and reviewed in pull requests like any other code.
