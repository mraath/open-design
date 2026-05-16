# Open Design — Setup Notes

## What this repo is

Open-source alternative to Anthropic's Claude Design. Local-first, web-deployable, BYOK at every layer.
Cloned from https://github.com/nexu-io/open-design on 2026-05-11.

## How I plan to use it

**Workflow:** Ask Open Design to research best-performing sites doing X, generate 3 template options, pick one, export it, and use it as a starting point in a real project.

Example prompt:
> "Generate 3 landing page templates for X, referencing the best-performing sites doing X."

Then export as HTML or ZIP and drop into the target project.

## Integration approach: Option B (Standalone + Export)

**Decision:** Run Open Design as a standalone local tool. No MCP wiring into other repos for now.

**Why:**
- The workflow is a one-time handoff per project (research → generate → pick → export), not ongoing design-token sync.
- MCP shines when you're iterating on a live design system alongside code — not needed for template generation.
- Keeps it simple: no daemon dependency in other repos, no "must be running" requirement.

## Upgrade path to MCP (Option A) — when needed

If I later want Claude Code in another repo to query Open Design files mid-session (e.g. read live design tokens, components, or artifacts without exporting), the upgrade is one command:

1. Start Open Design: `pnpm tools-dev run web`
2. Go to Settings → MCP Server in the UI
3. Copy the `claude mcp add-json` one-liner it generates
4. Paste in terminal, restart Claude Code
5. Done — any repo's Claude Code session can now call `search_files`, `get_file`, `get_artifact`

Nothing in the current standalone setup blocks this upgrade.

## Running it locally

**Requirements:** Node ~24, pnpm 10.33.x

```powershell
cd C:\Personal\open-design
corepack enable
pnpm install
pnpm tools-dev run web
```

Open the URL printed on startup.

## Key concepts

| Thing | What it is |
|---|---|
| Skills | Folder under `skills/` — defines what kind of artifact the agent produces (landing page, dashboard, deck, etc.) |
| Design Systems | `DESIGN.md` per brand under `design-systems/` — colors, typography, spacing, components |
| Daemon | Local Express server that spawns the agent CLI and manages projects/conversations in SQLite |
| `.od/` | Runtime data folder (gitignored) — SQLite DB + per-project artifact folders |

## Useful commands

```powershell
pnpm tools-dev run web        # start daemon + web (foreground)
pnpm tools-dev start web      # start in background
pnpm tools-dev stop           # stop all
pnpm tools-dev logs           # view logs
pnpm tools-dev status         # check what's running
```

## Notes on agent detection

On startup the daemon scans PATH for agent CLIs. Claude Code (`claude`) will be auto-detected.
No config needed — it just picks it up.

## MORE

Add:
- scroll animations
- responsive polish
- micro-interactions
