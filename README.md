# quick_brain

[![skills.sh](https://skills.sh/b/frankberliner/quick_brain)](https://skills.sh/frankberliner/quick_brain)

A standalone, lightweight fork of the original `brainstorming` skill from the [Superpowers](https://github.com/obra/superpowers) skill collection by [Jesse Vincent (@obra)](https://github.com/obra).

## Install

```bash
npx skills add frankberliner/quick_brain
```

That's it — the CLI auto-detects your agent (Claude Code, Codex, Cursor, Amp, OpenCode, and 50+ others) and installs `quickbrain` along with all bundled resources.

## What is quick_brain?

`quick_brain` is a set of skills that guide an AI agent through:

1. **Brainstorming** an idea with the user via in-depth questioning (`SKILL.md` → `quickbrain`)
2. **Writing a detailed implementation plan** that faithfully captures the brainstorming discussion (`writing-plans.md` → `quickbrain-writing-plans`)
3. **Executing the plan** either via subagents (`subagent-driven-development.md`) or inline (`executing-plans.md`)

It also includes the original **Visual Companion** — a browser-based mockup tool — for moments where a visual is clearer than text (`visual-companion.md` + `scripts/`).

## Why this fork?

The original `brainstorming` skill is excellent but is designed around a heavyweight workflow:

- It writes a separate **spec document** before any plan
- It dispatches a **subagent review loop** to validate that spec
- It requires an additional **user review gate** on the spec
- Only then does it transition into `writing-plans`

For small or focused changes, this workflow consumes a lot of tokens and adds friction. In practice, the spec-then-review chain also occasionally produced plans that drifted from what was actually discussed — either missing things, or adding things that were never agreed on.

`quick_brain` was forked to address this with a deliberately lighter design:

- **No spec document.** The conversation itself is the spec.
- **No spec review loop.** No extra subagent dispatch between brainstorming and planning.
- **More clarifying questions.** Depth of understanding moves from the spec/review phase up into the conversation phase, where it belongs.
- **Adaptive presentation.** Simple topics skip the design-presentation step; complex ones still get section-by-section approval.
- **A strict plan-fidelity rule.** When the plan is written, it MUST capture everything discussed — and MUST NOT add anything that wasn't.
- **Three explicit transition options.** After brainstorming, the user chooses: (1) write the plan, (2) code directly, or (3) keep discussing.

## Differences from the original brainstorming skill

| Feature | Original `brainstorming` | `quick_brain` |
|---|---|---|
| `HARD-GATE` requiring design before any code | ✅ Yes | ❌ Removed |
| "Anti-Pattern: Too Simple" section | ✅ Yes | ❌ Removed |
| Write spec doc to `docs/superpowers/specs/...` | ✅ Yes | ❌ Removed |
| Dispatch `spec-document-reviewer` subagent | ✅ Yes | ❌ Removed |
| User review gate on the spec | ✅ Yes | ❌ Removed |
| Number of clarifying questions | Moderate | **More — depth is the point** |
| "Propose 2-3 approaches" step | Always | **Adaptive — only when multiple reasonable approaches exist** |
| Present design in sections | Always | **Adaptive — skipped for simple topics** |
| Visual Companion "must be its own message" rule | Strict | Softened, just be clear |
| Final transition | Auto-invokes `writing-plans` | **User picks: (1) plan, (2) code, (3) keep talking** |
| Plan must capture everything from the conversation | Implicit | **Explicit, strict rule** in the plan skill |
| External skill dependencies (`superpowers:*`) | Yes | **None — fully standalone** |
| Skill names | `brainstorming`, `writing-plans`, ... | `quickbrain`, `quickbrain-writing-plans`, `quickbrain-executing-plans`, `quickbrain-subagent-driven-development` |

## File structure

```
quick_brain/
├── README.md                                 ← you are here
├── LICENSE                                   ← MIT
├── SKILL.md                                  ← main brainstorming skill (quickbrain)
├── visual-companion.md                       ← detailed guide for the browser companion
├── writing-plans.md                          ← writes the implementation plan
├── subagent-driven-development.md            ← plan execution via fresh subagents per task
├── executing-plans.md                        ← inline plan execution (no subagents)
└── scripts/                                  ← visual companion browser server
    ├── frame-template.html
    ├── helper.js
    ├── server.cjs
    ├── start-server.sh
    └── stop-server.sh
```

All `.md` files reference each other by **relative path** within this directory. There are no external skill dependencies. If you copy or move the `quick_brain/` folder, everything keeps working.

## Manual installation (alternative)

If you prefer not to use the `skills` CLI:

1. Clone or download this repository:
   ```bash
   git clone https://github.com/frankberliner/quick_brain.git
   ```
2. Copy the contents into your agent's skills directory. For example:
   - **Amp:** `~/.config/agents/skills/quick_brain/`
   - **Claude Code:** `~/.claude/skills/quick_brain/`
   - **Codex / Cursor / OpenCode / …:** see [the supported-agents table in the skills CLI README](https://github.com/vercel-labs/skills#supported-agents)
3. Make the visual-companion server scripts executable if they aren't already:
   ```bash
   chmod +x quick_brain/scripts/start-server.sh quick_brain/scripts/stop-server.sh
   ```

## Usage

Trigger `quickbrain` the same way you would trigger any skill — typically by asking your agent to brainstorm, design, or plan a feature. The agent will:

1. Explore the project context
2. Offer the visual companion if upcoming questions will be visual
3. Ask **many** clarifying questions, one at a time
4. Propose alternative approaches only if more than one exists
5. Present a design only if the topic is complex
6. Offer you three choices: write the plan, code directly, or keep discussing

If you pick **"write the plan"**, the agent invokes `quickbrain-writing-plans`, which produces a detailed, step-by-step plan saved under `docs/plans/`. That plan is then executable via either `quickbrain-subagent-driven-development` or `quickbrain-executing-plans`.

## Credits / Attribution

This fork is based on the `brainstorming`, `writing-plans`, `executing-plans`, and `subagent-driven-development` skills from the original **[Superpowers](https://github.com/obra/superpowers)** skill collection by [Jesse Vincent (@obra)](https://github.com/obra) and the wider Superpowers contributor community.

All original design, prose, the Visual Companion server, and the underlying workflow concepts are their work. `quick_brain` only re-shapes that workflow for a lighter, lower-token use case — the credit for the ideas belongs to the original authors.

If you find this fork useful, please also check out and support the original project:
- **Repository:** https://github.com/obra/superpowers
- **Author's blog post:** https://blog.fsck.com/2025/10/09/superpowers/
- **Sponsor Jesse:** https://github.com/sponsors/obra

## License

MIT — same as the upstream Superpowers project. See [`LICENSE`](./LICENSE).
