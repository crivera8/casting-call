# Changelog

All changes below are to `casting-call.html`, the build that keeps the live web search.

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
