# Learning to build Claude Code agents: open-source repo scan

Scan date: 2026-10-08. Stars, last push, license and open-issue counts come from GitHub repository search on that date. "Last push" is the repo's last push to any branch. Every repo listed was pushed within the last 2 months.

## Verdict

You can learn this from about four repos, read in order. Anthropic's own repos are the reference for file formats (subagents, skills, plugins, the GitHub Action). Two community repos add a beginner course and a large set of example agents. The rest are either huge catalogs you only need to browse, or orchestration frameworks that are more than you need right now. Nothing I found targets a content or YouTube pipeline like AutonoMotion's in a useful way. The one exception is `hassancs91/claude-youtube-editor` from the previous scan, which is a working example of a Claude Code project for a YouTube creator.

## Recommended path (read in this order)

1. **[wesammustafa/Claude-Code-Everything-You-Need-to-Know](https://github.com/wesammustafa/Claude-Code-Everything-You-Need-to-Know)**: the beginner course.
   - About 2½ hours of hands-on lessons with checked exercises: install, permission and plan modes, first change to commit, keeping a session on track, CLAUDE.md project memory, then a capstone.
   - The intermediate level covers subagents, skills, hooks, MCP and plugins.
   - Its Advanced level is being rebuilt, so stop at Intermediate.
2. **[anthropics/claude-code `plugins/`](https://github.com/anthropics/claude-code/tree/main/plugins)**: official worked examples of every building block.
   - `feature-dev`: one command that drives three agents (`code-explorer`, `code-architect`, `code-reviewer`). This is the clearest example of splitting a job across subagents.
   - `code-review` and `pr-review-toolkit`: parallel review agents.
   - `plugin-dev`: an `agent-creator` agent and a `plugin-validator`, which help you write your own.
   - `security-guidance` (PreToolUse hook) and `ralph-wiggum` (Stop hook): small hook examples.
   - Each plugin uses the standard layout: `.claude-plugin/plugin.json`, then `agents/`, `commands/`, `skills/`, `hooks/` and an optional `.mcp.json`.
3. **[anthropics/skills](https://github.com/anthropics/skills)**: the format for skills (reusable instructions Claude loads when relevant).
   - Each skill is a folder with a `SKILL.md`: YAML frontmatter with `name` and `description`, then the instructions.
   - Start from `./template`. The spec is in `./spec`.
4. **[VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents)**: 160+ example subagents to read, not install wholesale.
   - Each agent is one Markdown file with frontmatter: `name`, `description` (when to use it), `tools`, `model`. The body is the agent's system prompt.
   - Put project agents in `.claude/agents/` and personal ones in `~/.claude/agents/`.
   - The most relevant to you are in the Research & Analysis group (`research-analyst`, `search-specialist`, `data-researcher`) and `technical-writer`.

## Comparison table

Verdict key: **Learn from** = read closely. **Browse** = dip in for examples. **Later** = useful once the basics are done. **Skip** = not worth your time now.

### Official (Anthropic)

| Repo | Stars | Last push | License | What it teaches | Verdict |
|---|---|---|---|---|---|
| [anthropics/claude-code](https://github.com/anthropics/claude-code) | 149,697 | 2026-10-08 | None detected (Claude Code itself is not open source; the repo holds issues and example plugins) | `plugins/` folder: official example agents, commands, skills and hooks | **Learn from** (`plugins/` only) |
| [anthropics/skills](https://github.com/anthropics/skills) | 179,978 | 2026-10-08 | Mixed: most skills Apache-2.0; the `docx`, `pdf`, `pptx`, `xlsx` skills are source-available, not open source | Skill format, template, spec, example skills | **Learn from** |
| [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) | 37,544 | 2026-10-08 | Apache-2.0 | Curated directory of reviewed plugins | **Browse** |
| [anthropics/claude-code-action](https://github.com/anthropics/claude-code-action) | 9,447 | 2026-10-08 | MIT | Runs Claude Code in GitHub Actions: answers `@claude` mentions on issues and PRs, reviews PRs, implements changes, runs scheduled jobs. Set up with `/install-github-app` (needs repo admin) | **Later**: the main way to use Claude Code *with* GitHub repos |
| [anthropics/claude-agent-sdk-python](https://github.com/anthropics/claude-agent-sdk-python) | 8,224 | 2026-10-08 | MIT | Build agents in your own Python code with the same engine as Claude Code | **Later**: fits AutonoMotion's Python pipeline once you want agents to run without you |
| [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks) | 53,277 | 2026-09-28 | MIT | Notebooks on tool use, citations, agent patterns via the API | **Browse** |

### Community

| Repo | Stars | Last push | License | What it teaches | Verdict |
|---|---|---|---|---|---|
| [wesammustafa/Claude-Code-Everything-You-Need-to-Know](https://github.com/wesammustafa/Claude-Code-Everything-You-Need-to-Know) | 3,104 | 2026-10-06 | MIT | Structured course with exercises (above) | **Learn from**: best starting point |
| [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) | 25,590 | 2026-10-05 | MIT | 160+ subagent files in 10 categories, including research | **Learn from** (read a few) |
| [OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files) | 27,341 | 2026-10-06 | MIT | Keeps long-task state in `task_plan.md`, `findings.md`, `progress.md`, with hooks that re-inject the plan after `/clear` or compaction | **Learn from**: shows what hooks are for |
| [centminmod/my-claude-code-setup](https://github.com/centminmod/my-claude-code-setup) | 2,657 | 2026-10-08 | MIT | One person's full working setup: CLAUDE.md memory files, subagents, hooks | **Browse**: a realistic example of a configured project |
| [wshobson/agents](https://github.com/wshobson/agents) | 40,304 | 2026-10-05 | MIT | 94 plugins, 200+ agents, 180+ skills, mostly software development | **Browse**: too big to adopt; `docs/agents.md` covers model choice per agent |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | 55,257 | 2026-10-08 | Non-standard (GitHub can't identify it) | Curated link list: skills, agents, hooks, status lines, tools | **Browse**: for finding things later |
| [matt1398/claude-devtools](https://github.com/matt1398/claude-devtools) | 3,968 | 2026-09-26 | MIT | Desktop app to inspect session logs, tool calls, subagents and token use | **Later**: helpful once you're debugging your own agents (macOS-focused) |
| [Yeachan-Heo/oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) | 39,691 | 2026-10-08 | MIT | Multi-agent "teams" framework on top of Claude Code | **Skip for now**: adds a layer you don't need while learning |
| [hassancs91/claude-youtube-editor](https://github.com/hassancs91/claude-youtube-editor) | 324 | 2026-08-18 | MIT | A Claude Code project for a YouTube creator's own voiceover + graphics (from `repo_scan.md`) | **Learn from**: closest example to your use case |

Not included: big orchestration platforms (`agent-orchestrator`, `omnigent`, `munder-difflin`), which run fleets of coding agents, and small Agent SDK example repos with under 100 stars.

## How this maps to AutonoMotion

Once you know the formats, a natural first project is moving pieces of your pipeline into `.claude/` in your AutonoMotion repo:
- **A subagent per stage**, for example a `research-checker` that reads the daily report and flags facts without a source URL, and a `script-writer` that knows the nine-segment structure. Each is one file in `.claude/agents/`.
- **A skill for house rules**: the nine segments, tone for non-experts, and "every number must trace to the report". Claude loads it whenever it writes a script.
- **A hook as a hard gate**: a PostToolUse or Stop hook that runs a number-tracing check (like `claimcheck` from `repo_scan.md`) and blocks when a script contains an untraced number. Hooks are code, not instructions, so they enforce rules the model could otherwise skip.
- **The GitHub Action later**, for example a scheduled workflow that runs the daily research pass and opens a PR with the report.

## Caution

Installing a plugin, hook or agent from these repos lets it run commands on your machine with your permissions. Read the files before installing, and prefer copying the one agent or skill you want over installing a whole marketplace.
