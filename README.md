# Casting Call

A companion tool to Vanessa Conzon & Allie Feldberg's working paper on AI, gender, and "doing gender" in organizations. It walks you through assessing how an existing AI product gets gendered (Path A) or designing one deliberately (Path B), and ships a full methodology page covering where each of its claims comes from.

## Which file is which

| File | What it is |
|---|---|
| `index.html` | **The build. Start here.** 2,689 lines, ~162 KB. Keeps the live web search. |
| `old/index.html` | An **earlier, much smaller build** (1,394 lines, ~68 KB). Predates the methodology page, the per-field auto-classification, the source-link verification, and the disclosure-page additions. Retired, kept for comparison. Nothing should be edited here. |

**This file used to be called `casting-call.html`.** It was renamed so that GitHub Pages serves the current build: Pages serves `index.html` from the repo root, and that slot was occupied by the old build, so <https://vanecon.github.io/casting-call/> was quietly serving a version from before the methodology page and the source-link verification existed. Both builds carry the same `<title>`, so there was nothing on the page to tell you which one you had.

There is now exactly one build in this repo. If you have a local clone or an open branch that still has `casting-call.html`, that is the same file under its old name.

See `CHANGELOG.md` for what changed in the current build and why.

## What "the code" is

`index.html` is the whole thing. One file, with the CSS and JavaScript embedded in it. There is no separate source, no build step, no framework, no package manager, and no dependencies to install. Nothing is fetched from a CDN — no external scripts or stylesheets at all. The only outbound references are citation URLs and two calls to Anthropic's API.

So there isn't a hidden layer of "code Claude used to make the agent" sitting somewhere else. The file *is* the artifact. Open it in a browser and it runs.

## Rebuilding it in your own Claude account

1. Start a new chat in your Claude account.
2. **Upload `index.html` as an attachment** rather than pasting it. At roughly 40,000 tokens, pasting eats a large share of the context window before you've asked for anything.
3. Ask Claude to open it as an artifact. It will render, and you can edit from there in conversation.
4. When you're done, download the updated file and commit it.

Note that an artifact published through Claude Code cannot reach `api.anthropic.com` directly — it has to go through the artifact runtime's `sample` capability instead, which has no web search. That variant is a separate file and is not in this repo; this repo holds the build that keeps real web search.

## Editing notes

**Ask for targeted edits, not rewrites.** With a single file this size, "rewrite it with X changed" invites silent regressions in parts nobody was looking at. Naming the section to change keeps the diff small and reviewable.

**Check the diff before committing.** `git diff` on a single HTML file is readable and is the fastest way to catch an edit that touched more than intended.

**Useful landmarks.** The `<style>` block holds all the design tokens as CSS variables at the top, so colors and type can be changed in one place. The coding logic lives in a handful of small functions: `tallyMarkers`, `placementFromTally`, `computeScore`, `behaviorCoding`, `alignmentLabel`, and `pathwayFor`. Changing how a signal is scored means changing those, and the scoring explanation in the methodology page should be updated to match — they're currently consistent and it would be easy to let them drift.

**Badge labels are separate from badge logic.** `PATHWAY_INFO[...].tag` is the internal key — it sets the CSS class and is what the `info.tag === 'amplify'` comparisons test. `tagLabel` is what the user actually reads. Change the label; leave the key alone.

## The API caveat (worth deciding before hosting)

Two features call `https://api.anthropic.com/v1/messages`:

- `runSearch` — the "Have Claude look it up" search in Path A, Option A. Declares the `web_search_20250305` server tool.
- `classifyField` — the per-field "Claude judges how this reads" helper in the manual coding sheet. No web search; a small text-only call.

**Neither call carries an API key.** They work because the Claude.ai artifact sandbox injects authentication at runtime.

| Where it's hosted | Manual coding | Claude-powered search & auto-classify |
|---|---|---|
| Artifact inside Claude.ai | Works | Works |
| GitHub Pages or any static host | Works | **Fails** |
| Opened as a local file | Works | **Fails** |

Everything a user types in by hand — the full coding sheet, the scoring, the placement, the design studio, the methodology page — is pure client-side JavaScript and works anywhere. Only the two API-backed features depend on the sandbox.

If the hosted version needs working search, it requires your own API key routed through a small server-side proxy. **Do not put a key in this file**: it's client-side, so the key would be visible to anyone who views source, and committing it to a public repo would leak it. If search can live only in the Claude-hosted version, no proxy is needed and the repo copy simply degrades to manual mode.

## How source links behave

Search mode used to show whatever URLs the model wrote into its JSON. Those are prose the model generates, so some were reconstructed from memory and 404'd (`ncwit.org/resource/btn/` was one; the real page is `ncwit.org/resource/thefacts/`).

The current build harvests the URLs the web search *actually returned* — from `web_search_tool_result` blocks and text-block citations — and only links a source if its URL matches one of those, compared after normalisation. A source whose URL isn't in that set keeps its title as plain text and loses its link.

Consequence worth knowing: **in any environment where the search can't run, every source link degrades to plain text.** That is deliberate. The allowlist is empty, so nothing can be vouched for, and the tool shows titles rather than links it can't stand behind.

## Known rough edges

- The search declares `web_search_20250305` and uses `claude-sonnet-4-6`. That model also supports the newer `web_search_20260209` variant, which adds dynamic filtering. Left as-is so far — a deliberate upgrade, not drift, when someone wants it.
- Per-field source links (`designer_source`, `org_diversity_source`, `adaptive_source`) are verified against the same allowlist, but they aren't deduplicated against the "Pulled from" list, so one article can appear in both places.
- Query parameters aren't sorted during URL normalisation, so the same article with params in a different order won't collapse. Rare for article URLs.

## Avoiding divergence

Three people editing a single 2,700-line file in three different Claude accounts will produce conflicting versions quickly, and this file has no module boundaries to merge along. Worth agreeing up front that this repo is the source of truth, that each round of edits starts from a fresh pull, and that changes go back as commits rather than as re-uploaded whole files.

The `index.html` / `casting-call.html` split in this repo was that problem already happening once. It is resolved: there is one build, at `index.html`, and the retired one is parked under `old/` where it cannot be mistaken for current.

## Contact

- Vanessa Conzon, Boston College — conzon@bc.edu
- Allie Feldberg, Harvard Business School — afeldberg@hbs.edu
