# Changelog

All changes below are to `index.html`, the build that keeps the live web search. It was
called `casting-call.html` until 2026-10-06; see the entry for that date.

## 2026-10-06

### Path B: coaching register removed

Twelve edits. The pattern swept for, rather than only the two instances
reported: second-person coaching and therapeutic register -- breezy
imperatives ("lock this in first", "pick these on purpose"), instructions
about how to feel ("Be honest about the stereotype, even if you don't like
it", "worth sitting with", "with eyes open"), sincerity intensifiers
("genuinely", "real freedom here"), knowing jargon ("as specced"), and a
closing moral ("Whatever you land on... is most of what designing on purpose
means in practice"). Each is replaced by a statement of what the field is
for or what the result means.

Two findings beyond the copy:

- **The section A hint was doing real work** and the replacement keeps it.
  "Be honest about the stereotype, even if you don't like it" exists because
  an aspirational answer here breaks the tool -- the marker guidance keys off
  this field. It now reads "This records how the work is conventionally
  coded, not how it ought to be," which says the same thing without
  presuming the reader is reluctant.

- **The shared explainer had already drifted.** The "How are these
  categorized" block is duplicated in both paths, and the two copies had
  diverged: Path A read "the one most worth double-checking", Path B "the
  one most worth thinking through deliberately". Path B's was both the
  Claude-ism and the drifted copy, so converging it on Path A's wording
  fixed both. The one difference that remains between the copies is
  deliberate -- Path A ends with an instruction about its notes field, which
  Path B has no equivalent of.

Deliberately left: "Genuinely spans both" and "These associations are
genuinely contestable" both appear in copy shared with Path A, so changing
them on one side only would re-open the drift just closed. Path A's own
"Worth sitting with" heading is also untouched, being out of scope.

### "Feminine-coded" -> "stereotypically feminine-coded" throughout

All 60 occurrences, both cases and both genders: 21 lowercase and 9
capitalized for each of feminine and masculine. The one instance that already
read "stereotypically feminine-coded" was left alone rather than doubled.

Verified that none of these strings is load-bearing. The internal codes are
`fem`, `masc`, `mixed` and `neutral`, and every occurrence changed was a
display label, an `<option>`'s text, prose, or a comment -- the `value="fem"` /
`value="masc"` attributes and all `'fem'`/`'masc'` comparisons are untouched
and were counted before and after to confirm it.

Two consequences worth knowing:

- **The gauge labels are now long**: "Strongly stereotypically
  feminine-coded", "Leans stereotypically masculine-coded". Grammatical but
  wordy for a badge. Left consistent with everything else on the grounds that
  these are the most assertive-sounding strings in the tool and therefore the
  ones that most need the hedge; easy to shorten if they read badly in use.
- **The classifier prompts changed too**, which is a behaviour change rather
  than a copy change. `CLASSIFY_PROMPTS` now asks Claude to judge whether
  something reads as *stereotypically* masculine- or feminine-coded. The
  return contract is untouched -- still `fem`/`masc`/`mixed`/`neutral` -- so
  nothing downstream shifts, but the framing handed to the model now matches
  the framing shown to the reader.

### Masthead

- **Added a standfirst paragraph** above "Every AI assistant has a director...",
  stating plainly what the tool is for: reflecting on the gender-typing of the
  AI tools researchers use, plus the feature for taking a gender-conscious
  approach to designing their own. The page previously opened straight into the
  framing line, which says what the subject is but not what the tool does.
  Styled as a second `.dek`, so it stacks with the existing one.

### The deployment target is a claude.ai artifact

Settled, and recorded in `README.md` so it is not re-litigated. The two
Claude-backed features — `runSearch` and `classifyField` — send nothing but
`Content-Type: application/json`. No `x-api-key`, no `anthropic-version`, no
`anthropic-dangerous-direct-browser-access`. A real browser call to Anthropic
needs all three, so a request shaped like this cannot succeed anywhere that
forwards it as written; it works only where something **intercepts** the call
to that hostname and rewrites it, which is what the claude.ai artifact sandbox
does.

That makes a public URL and the sandbox the same slot: hosting the file is
exactly what turns the interception off. The GitHub Pages copy is therefore a
preview of the current build in manual mode, not the working tool.

**Added a sign-in notice** as the first line of "Before the curtain rises":
"You'll need to be signed in to Claude to use this tool." Thea & Drew has
carried this sentence since 2026-10-05 and Casting Call did not, which matters
more here — an unauthenticated viewer gets no warning, just a search that
reports it couldn't complete and source links that quietly render as plain
text.

### The hosted site was serving the wrong build

GitHub Pages is enabled on this repo and live at
<https://vanecon.github.io/casting-call/>. Pages serves `index.html` from the
repo root, and that slot held the **old** 68 KB build — so the public URL had
been quietly serving a version that predates the methodology page, the
per-field auto-classification, the source-link verification, and the
disclosure-page additions. Measured before the fix: the root returned 67,727
bytes, byte-identical to the old build, while the current build sat unvisited
at `/casting-call.html`.

What made this worth fixing rather than documenting: **both builds carry the
identical `<title>`** — "Casting Call — Assessing How an AI Gets Gendered |
Conzon & Feldberg". Nothing on the page distinguished them, so anyone sent the
link would have had no way to notice they were reading the superseded tool.

The fix is a rename, not a redirect or a second copy:

- `casting-call.html` → `index.html`. The current build now *is* the page
  Pages serves, at the clean root URL, with no redirect hop.
- The old build → `old/index.html`, parked where it cannot be mistaken for
  current and still reachable at `/old/` for comparison.

A redirect page or a duplicated file would both have worked, and both were
rejected: a duplicate is the divergence problem this repo already had once,
and a redirect leaves two names for one file. There is now exactly one build.

Safe to rename because the file references its own name nowhere and loads no
relative assets — every `src`/`href` in it is either an absolute citation URL
or built at runtime from a search result. Nothing to repoint.

`README.md` updated throughout, including the "upload this file to Claude"
instructions, which named the old filename.

---

## 2026-10-01

### Disclosure page ("Before the curtain rises")

- **Added a model-training opt-out paragraph.** Points to Settings → Privacy → "Help Improve our AI models" in Claude.ai or the mobile app, notes that opting out stops future training use but doesn't remove data already used, and links Anthropic's own instructions at `privacy.claude.com`.
- **Added "A note on bias."** Flags that the tool can gravitate toward familiar methods, frameworks, and academic traditions, and that work outside that mainstream — non-Western contexts, understudied populations, less conventional methods — should weigh its input accordingly. Placed second, next to "Claude makes mistakes," so the two limitation notes sit together before the page turns to data handling.
- **Added a contact line** for Vanessa Conzon and Allie Feldberg, matching the wording and placement the Thea & Drew tool uses on its own landing page.

### Contact addresses: no more `mailto:`

- **Converted both `mailto:` links in the essay's contact block to selectable plain text**, with a copy-to-clipboard button beside each. `mailto:` does nothing for many viewers of a published artifact, and the page has no way to detect that it failed.
- The copy handler calls `navigator.clipboard.writeText` inside the click handler, catches rejection, and falls back to range-selecting the address so it can be copied by hand. It never reports that mail was sent, because nothing here sends mail.
- Addresses are `.addr` spans: monospace, `user-select: all`, so one click selects a whole address. They work standalone — if every copy button failed, the addresses are still readable and selectable.

### Path B (Design your own AI agent)

- **Fixed three reversed direction words** in the marker callout: it told the reader to pick markers "below" when `#markerCallout` is the last element in that fieldset and all five marker controls render above it.
- **Rewrote the section D instruction.** Was: *"If your goal is to not reaffirm the stereotype, pick nonadaptive."* Now: *"To push against the stereotype, pick 'No — its manner should stay consistent.'"* The old text stacked three negations and named `nonadaptive`, which is the `<option>`'s **value** attribute, not the label the reader sees. Option labels, order, and values are unchanged.
- **Added a gloss** defining *adaptive* and *nonadaptive* in terms of the answer choices, placed before either term is used later in the same callout.

### Plain language in the results

- **Badge `ATTENUATE` → `WEAKEN`** and **`AMPLIFY` → `REINFORCE`**. "Reinforce" matches the pathway prose directly beneath the badge, which already said "tends to reinforce the existing stereotype."
- Implemented by adding a `tagLabel` display field read as `(info.tagLabel || info.tag)`. The internal `tag` values are untouched, so the CSS classes (`.tag.amplify`, `.tag.attenuate`) and the `info.tag === 'amplify'` comparisons that drive the Path B verdict all still work.
- **Essay:** "The *amplifies vs. attenuates* language is a hypothesis" → "*reinforces vs. weakens*".

### Source links: deduplication

Testing found runs returning 8 links with 0 unique URLs, and 10 links with 1 unique — the same article cited repeatedly under different display labels.

- Added `normalizeUrl`, which lowercases the host, strips `www.`, strips a trailing `/amp` or `/amp/`, removes tracking parameters (`utm_*`, `fbclid`, `gclid`, `ref`), drops an empty query string and trailing slash, and ignores the fragment. Unparseable input falls back to a cheap textual normalisation rather than throwing.
- Added `dedupeSources`, which keys on the normalised URL and **never** on the label. When two entries collapse it keeps the longer label and the non-AMP original for the href.
- The href is always a URL that actually came back; the normalised string exists only to compare.

### Source links: verification against the search (the dead-link fix)

Reported symptom: `https://ncwit.org/resource/btn/` 404s, and appeared in some runs of the same query but not others. Diagnosis: the app read only `type === 'text'` blocks from the API response and discarded the `web_search_tool_result` blocks — so every URL shown was one the model had typed into its JSON, informed by the search but not taken from it. Some were reconstructed from memory. The real page is `ncwit.org/resource/thefacts/`: right organisation, invented path.

- Added `harvestSearchUrls`, which collects the URLs the search actually returned, from `web_search_tool_result` blocks and from text-block `citations`. It branches on `Array.isArray(block.content)` because a server-tool error arrives as a normal HTTP 200 with `.content` as a single error object rather than an array, and does not throw.
- Added `verifySources`: dedupes as before, then keeps a URL only if it's in that allowlist. An unmatched entry keeps its title and loses its link, so attribution survives without a dead link behind it.
- `validSource` now gates the per-field source links (`designer_source`, `org_diversity_source`, `adaptive_source`) on the same allowlist.
- The allowlist resets at the start of each run, so a previous search can never vouch for the current one.
- Side effect: closes a gap where a model-supplied `javascript:` URL could have reached an `href`, since `escapeAttr` only escapes quotes. Such a URL can never appear in search results, so it can't pass verification.

### Deliberately not changed

- **Web search is retained.** This build keeps the real `web_search_20250305` server tool and both `api.anthropic.com` calls, so the search works when hosted somewhere that can authenticate them.
- **The full page title is retained:** "Casting Call — Assessing How an AI Gets Gendered | Conzon & Feldberg".
- How sources are requested, selected, or counted. The prompt still asks for up to 2; only what gets rendered was filtered.
- Option labels, option order, and all scoring logic.
