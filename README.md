# life-guide — Reduce Information Gaps

[中文文档](README.zh-CN.md)

> 🧭 Help AI see the technical route before it starts building.

`life-guide` is a general Agent Skill for technical choices, research-heavy tasks, and context-dependent decisions. It helps an AI slow down before it jumps to a tool, library, workflow, or confident recommendation.

## When AI is moving too fast

You ask:

> “I want a technical animation with particles, captions, and audio reactivity.”

A typical answer may immediately list a few tools. `life-guide` first checks what can change the route:

- 🎞️ Is this for real-time interaction or final video rendering?
- ⚙️ Will it run on the web, in a compositor, inside a 3D tool, or as a desktop app?
- 🧩 Do you need high frame rate, low latency, maximum visual quality, or repeatable renders?
- 🔎 Which versions, dependencies, and host limits could block delivery?
- 🧪 What is the smallest demo that can validate the biggest risk first?

Then it compares Canvas, WebGL, 3D, video-composition, and other relevant routes; checks primary sources; and gives you a practical next step.

## What `life-guide` changes

- 🧭 Expands the option space beyond the first tool or workflow that comes to mind.
- 🔎 Separates facts, user statements, assumptions, inferences, and unknowns.
- 📚 Traces decision-changing claims back to official documentation, rules, source code, or original data.
- ⚖️ Compares cost, time, performance, maintenance, reversibility, and opportunity cost.
- 🧪 Designs a low-cost validation step before a full build.
- 🛑 States stop conditions and what new information would change the recommendation.

Ask it:

> “Should this be Canvas, WebGL, or a 3D workflow?”<br>
> “Can my current stack support it reliably?”<br>
> “Is this animation library still worth using?”<br>
> “What should I prototype first?”

Simple question? Answer directly. Complex build? Map the route first. High-impact choice? Check the evidence and failure branches before recommending a path.

`life-guide` does not promise one perfect answer or an exhaustive list. It helps AI and people see more of the real options before committing.

The longer behavior example is in [`skills/life-guide/references/examples.md`](skills/life-guide/references/examples.md).

## Output structure since 6.1.0 (fixed four sections)

Since 6.1.0, complex and multi-option answers follow a fixed order: **data → mind map → summary → action or exploration**.

1. **Data section first**: grouped by market / tech / cost / rules; every key data point carries source, query date, scope, source strength, and whether it is a primary source. No judgments mixed into the data.
2. **Mind map after the data**: covers conclusion, key branches, evidence or unknowns, and the next step. Falls back to a plain-text tree when Mermaid cannot render.
3. **Compact summary**: judgment body within ~300 characters, at most 1–2 key numbers, each line labeled as verified / user-stated / inferred / unknown / experienced judgment.
4. **Action or exploration section last**: when the user wants to act, give a 100–200 character action section with what to do this week, cost, verification, current recommendation, switch conditions, and biggest unknown. When the user is exploring directions, give a concise exploration section with unexpanded routes, the unknowns that could change the ranking, and what to investigate next.

Two hard rules take effect at the same time:

- **Time-sensitive claims must be searched first**: prices, versions, laws, policies, platform rules, software and tools, jobs and market trends, public sentiment — verify against current sources before judging; when the latest data cannot be obtained, write "unverified as of the query date" instead of "the latest data shows".
- **Source strength in four tiers**: primary source / reliable repost / summary only / original not obtained. Secondhand material must never be written up as "verified original text"; summary-only material is a lead, not a basis for conclusions.

Simple facts, pure writing, and stable common knowledge still get a short direct answer — no forced web search, no forced diagram.

## Installation

**Copy the entire `life-guide/` directory, not only `SKILL.md`.** Reference files are loaded as needed. If they are missing, some rules cannot be reached. The standard package is in [`skills/life-guide/`](skills/life-guide/). A release ZIP is also available, with `life-guide/` as its top-level directory after extraction.

| Platform | Location or import method | How to check discovery |
|---|---|---|
| Claude Code | Project `.claude/skills/life-guide/`, or user `~/.claude/skills/life-guide/` | Check `/skills` |
| OpenAI Codex | `.agents/skills/life-guide/` | Invoke `$life-guide` explicitly |
| Gemini CLI | `.gemini/skills/life-guide/`, or `gemini skills install <path>` | Run `/skills list`; trust the workspace first and reload skills after adding |
| GitHub Copilot | Project `.github/skills/life-guide/` and supported `.claude/skills` or `.agents/skills`; user `~/.copilot/skills/` | Ask a question and observe loading; `gh skill` may also be available |
| WorkBuddy local | `life-guide/` inside the host skill directory | Check the skill list for the name and description |
| WorkBuddy hosted platform | Requires a host-specific package. See [`adapters/workbuddy/`](adapters/workbuddy/) | Follow the host's review result |
| Doubao / Coze | Content mapping, not direct installation of the standard package. See [`adapters/`](adapters/) | Not verified |

Windows example for a project-level Claude Code installation using Git Bash:

```bash
mkdir -p .claude/skills
cp -r /path/to/skills/life-guide .claude/skills/
ls .claude/skills/life-guide/SKILL.md
```

After installation, try these two boundary checks:

1. Ask “Which country is Paris in?” The skill should answer directly without asking about career goals or starting an investigation.
2. Ask “I want to make a technical animation with particles and audio reactivity.” The skill should restate what is known, identify route-changing gaps, and ask prioritized questions instead of immediately naming a tool.

If both behaviors match, discovery and boundaries are working in that host. If they do not, submit the host, version, input, and actual output according to [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Verification boundary

- **Structural checks do not prove outcome quality.** Static checks only verify package structure, references, and rule consistency.
- The current version has structural checks and limited local self-test records. Stable behavior across sessions, automatic triggering, cross-host behavior, cross-model behavior, and real-user outcomes remain unverified. Platform status is listed in [`adapters/`](adapters/), and untested items are labeled as unverified.
- Behavior depends on the model, host, loading mode, references actually read, and available tools. A same-model stability claim requires the same conditions and at least three repeated runs. A model without web access cannot verify current information.
- Historical behavior records come from 5.x batches. They used one host, one model, manual loading, and human judgment across versions, so they cannot be generalized to the current version or every host.
- Tool installation, account login, and external writes require explicit user approval. The skill does not promise that a tool is virus-free or immune from account restrictions.

## Evidence and maintenance documents

- With/without-skill comparison and boundaries: [`docs/ab-evidence.md`](docs/ab-evidence.md)
- Release audit for the current candidate: [`docs/release-audit-6.1.6.md`](docs/release-audit-6.1.6.md)
- Release plan and post-release observations: [`docs/release-plan.md`](docs/release-plan.md)
- Behavior cases and run records: [`tests/`](tests/)
- Package boundaries and maintenance rules: [`docs/architecture/package-boundaries.md`](docs/architecture/package-boundaries.md)

## Contributing

Reports about installation failures, missing triggers, unreadable reference files, or unexpected answers are welcome. Include the host, version, input, and actual output. Rule-change discipline is documented in [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Language

Chinese documentation is available in [`README.zh-CN.md`](README.zh-CN.md).

## License

MIT. See [`LICENSE`](LICENSE). The skill package includes the same license file.
