# Changelog

All changes below are to `index.html`, the build that keeps the live web search. It was
called `casting-call.html` until 2026-10-06; see the entry for that date.

## 2026-10-12

Two sets of edits to `index.html`. For the submission build, the authors' names,
affiliations and contact details are removed from everything a reader of the tool
sees. Names in code comments and in the git history were left alone on purpose.
The reasoning and the restore instructions are in `decision-rules-log.md` (R18).
Separately, the voice rule changed; see the last section (R10).

### Authors' names and contact details removed from the visible page

- **Page title:** the browser-tab title no longer ends with the authors' surnames.
- **Byline:** the "By ..." line under the page title is removed.
- **Notice:** the "Questions? Email ..." line at the end of "Before the curtain
  rises" is removed.
- **Footer:** the attribution sentence is removed. The remaining sentence now says
  "Claude's own interpretation of academic theorizing on AI and "doing gender" in
  organizations" where it said "that research", so it still reads correctly
  without the sentence before it.
- **Essay:** the "Questions or comments" section (the invitation to send feedback,
  with each author's affiliation and email) is removed in full.
- **Design section intro:** the clause that credited the model to the authors is
  removed, and "You'll" now starts the sentence.

### Deliberately kept

- **The inline citation and the reference-list entry** for the working paper. They
  name the authors, and the decision was to keep them.
- **Code comments** in the script that name the authors, and the git history.

### Restoring the "Questions or comments" section

An exact copy is kept outside the repo, with a note on where it goes back in. The
script and styles for it are still in the page, so pasting the block back is all
that is needed.

### Path corrected

The 2026-10-09 entry pointed to `docs/decision-rules-log.md`. The log is in the
repo root, so the path now reads `decision-rules-log.md`.

### Voice: the default voice is used when there is one

This is separate from the anonymization above. The voice rule used to code every
product with more than one voice as "varies". It now says: if the product has one
default voice, use it for the voice code and say that other voices exist; if there
is no single default (the user chooses at setup, it differs by country or
language, or it changed), record "varies". It changed in two places, the lookup
prompt and the manual-mode voice classifier, and nowhere else: the model,
temperature, token limit, search settings and scoring are untouched (checked by
comparing the requests the page sends before and after).

Siri still comes out "varies", which is accurate: reports say users choose a voice
at setup, with none preselected. Alexa is now coded on its default voice
(feminine), which raises its marker score from 33 or 50 to 75.

A check of Siri and Alexa, five lookups each, behaved as intended: Siri was
"varies" in all five, and Alexa was feminine in all five with "other voices
available" in the wording. The final result held in 9 of 10 lookups; the exception
was an adaptiveness reading of "unclear". The word "genuinely" appeared 0 times.

A second set, Bixby and Gemini (five lookups each), found no single default for
either: Bixby was "varies" in all five (its voices differ by language and region)
and Gemini was "varies" in all five (the user selects a voice at setup), so their
results did not change from before. A last check on two products with no voice,
GitHub Copilot and DeepSeek (three lookups each), left the voice code unchanged:
"none" in every run, with wording that says there is no voice. Scores for any
product with a clear default, such as Alexa, are no longer comparable with earlier
measurements.

## 2026-10-09

Edits to `index.html`. The reasoning, the test results and the open questions
behind them are in `decision-rules-log.md`.

### Path A, Option A: one general web search

`runSearch` makes one request with one `web_search_20250305` call at
`temperature: 0`. The query is the product name, its maker and the words
"AI assistant". Claude is told the product is an AI assistant, agent or
chatbot, so a name shared with something else resolves to the AI (Alexa, not
alexa.com), and that names and pasted links are not case-sensitive.

Removed, because tests showed no stability benefit and the requirement caused
most of the failures: the rule that the search must land on the product's
official site, the confirmation search for the site Claude named, the retry
when that site was rejected, the official-site link, and the "no official site,
try again" message. A second pass that read the official site's own pages was
tried first and dropped: the same results at about three times the input
tokens.

### Behaviors, voice and source tags

- **At most three tasks**, the most prominent first, in the order the source
  gives them, with no two covering the same activity. The page also cuts any
  list to three, so the limit holds even if Claude returns more.
- **A voice that varies is recorded as varying.** If a product offers more than
  one voice, or its voice varies by language, region, device or user choice,
  the result is code `none`, worded "Varies", instead of one pick. It scores as
  neutral. Siri had been returned as both "higher-pitched" and "deliberately
  androgynous" on different runs.
- **Every answer carries a tag**: Other source, Claude's general knowledge, or
  Source not specified. It is Claude's own report of where the answer came from,
  and the page cannot verify it.
- **A line above the answers says results can vary** from one search to the
  next, because they depend on which pages the search brings back.

### "Genuinely" in the gender-coding output

The word is gone from the output that says how feminine or masculine something
is, in two ways.

- **Printed text, four places.** The "mixed" label changed from "Genuinely
  spans both" to "Spans both" in `TASK_CODE_LABELS` (the result chip) and in the
  task dropdown of each path, Path A's and Path B's. The worked example's reason
  now begins "Spans admin/care-adjacent and technical/analytical work". The tone
  dropdowns read "Blends both depending on context" and never had the word.
- **The prompts that write the explanations.** Removed from the two
  `CLASSIFY_PROMPTS` entries that had it (task and tone), and `classifyField`
  and the lookup prompt now each carry "Do not use the word 'genuinely' in your
  explanations. State the observation plainly." It is not an output filter:
  deleting the word afterwards can leave a broken sentence.

This reverses the 2026-10-06 decision to leave "Genuinely spans both". The
reason given then, that changing it on one side would re-open the drift between
the two paths, no longer applies: both paths' task dropdowns and the shared
label table changed together, so they still match.

A live check (three lookups and eight classifier calls) found the word 0 times
in Claude's text, the results panel, the dropdowns and the labels. That is a
small sample, and the old version was not run live for comparison.

Deliberately left: the five explanatory notes that still use the word, including
"These associations are genuinely contestable..." in the copy shared between
the paths. They are page text, not the output that rates how feminine or
masculine something is.

### Failure notice on the manual sheet

When a lookup fails, the page still opens the manual sheet (Option B) as before
but now says why, at the top of the sheet: in plain words, what happened and what
to do next, with the technical message underneath as "Details". Before, the
reason was written into the search panel, which is hidden once the manual sheet
opens, so a usage limit, a dropped connection and an unreadable answer all
looked the same. `explainSearchFailure` and `showSearchFailureNote` hold it.

It is provisional. Every piece is marked `[search-failure-note]`, and deleting
each marked line or block restores the previous page exactly (checked by diff).

### One retry on a network error

If the main search fails with a network error, `fetchWithRetry` waits about a
second and tries once more. An HTTP error (for example 400 or 429) is not
retried, because the service did answer.

### Methodology text corrected

It said a keyword check turns Claude's note into the dropdown choice. The code
stopped doing that: Claude returns each selection directly, with a reason and a
source tag. The sentence now says so.

### Stability measured, not changed

Eight products were measured, five runs each, while this was built. The final
result (aligned, misaligned, unclear) held for six and varied for Claude and
GitHub Copilot, where one borderline reading tips a neutral placement into a
directional one. The causes that remain are pronouns coded "unknown" when usage
is mixed, how tone and ambiguous tasks are coded, the name "Claude", and
adaptiveness (the prompt gives Claude no definition of it). Each lookup costs
about 21,000 input tokens, one call and one search. These are research decisions,
listed in the decision log; none were made in this change.

### Deliberately not changed

- **Scoring.** `tallyMarkers`, `placementFromTally`, `computeScore`,
  `behaviorCoding`, `alignmentLabel` and `pathwayFor` are untouched.
- **Where it runs.** Still a claude.ai artifact, per the README. The two API
  calls still need the artifact sandbox; hosting the file elsewhere still leaves
  Option B working and Option A failing, now with a notice that says so.

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
