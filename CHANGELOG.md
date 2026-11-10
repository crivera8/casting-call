# Changelog

All changes below are to `index.html`. Until 2026-10-28 it was the build that kept the live
web search; since then it is the Bedrock build, which answers from Claude's general knowledge
(see that entry). It was called `casting-call.html` until 2026-10-06.

## 2026-10-29 (third change): "stays in this browser"

In the opening notice's "On the data you put in", "what you type stays in this conversation"
is now "what you type stays in this browser". The bold data line also gained "and the figures produced" ("…your responses, conversations, and the figures produced…"). The page no longer runs inside a Claude
conversation, so "conversation" no longer described where the text stays. It was the only
use of the phrase on the page. Wording only. `index.html` after this change: 177435 bytes,
SHA-256 `25a4ea9cd22bd5b0c611b92011163a7c6f5ef1943579cb4dd6176a4f65f91148`.

## 2026-10-29 (second change): data line restored at the top of the opening notice

A bold paragraph was added as the first paragraph of "Before the curtain rises", above
"Claude built this, and Claude makes mistakes.":

> **Information you enter into these tools (e.g., your responses, conversations, and the figures produced) may be saved. What you enter is processed by Anthropic's Claude.**

It is the data-handling part of the paragraph removed on Oct 28 (R23), without the sign-in
sentence. Wording only; nothing else changed. The
reasoning is in `decision-rules-log.md` (R23, revised). `index.html` after this change:
177414 bytes, SHA-256 `f1da389e556bc41b34417e54aff8e19d4f1ec0d7d25016bcbebf71f0b3516632`.

## 2026-10-29: say when Claude is busy in the design studio and manual readings (CC-27)

When the endpoint answered 429 (busy), the readings said only "Couldn't auto-classify —
set this yourself." and the avatar "Couldn't propose an avatar…", with nothing saying the
service was busy or that waiting would help. Tracker item CC-27, from the live checks on the
hosted site. The reasoning is in `decision-rules-log.md` (R26).

### Messages

- A reading (manual mode, and the design studio's job and name) that gets a busy reply now
  says: "Claude is busy right now. Wait a minute, then click into the box and out again, or
  set this yourself." With no answer at all: "Couldn't reach Claude. Check your connection,
  then click into the box and out again, or set this yourself." Anything else keeps
  "Couldn't auto-classify — set this yourself."
- The avatar proposal, on a busy reply: "Claude is busy right now — wait a minute, then press
  Try again." With no answer at all: "Couldn't reach Claude. Check your connection, then
  press Try again." (the connection line is an addition to CC-27, for consistency with the
  readings). Anything else keeps the old message. The Try again button shows in every case.

### Spending less of an unknown rate limit

The endpoint's limit is not known and may count per IP address, so everyone on one network
could share one allowance. The readings and the avatar now go through one shared function
(`postToClaude`) that:

- sends them one at a time, in the order asked, instead of up to three at once;
- retries a busy reply once: after the wait the service gives in `Retry-After`, if that is
  10 seconds or less, or after 4 to 6 seconds if it gives none (the random part keeps several
  people's retries from landing together); no retry if `Retry-After` is longer;
- after a second busy reply (or a long `Retry-After`), sends none of these calls for 30
  seconds, or for the `Retry-After` time, up to 2 minutes. A reading asked for in that time
  shows the busy message at once without sending anything.

The Option A lookup is separate and unchanged (it already explained a 429). Prompts, output
limits, scoring and page text are unchanged.

### Checks

Against a simulated endpoint in a headless browser (16 checks, all passed): a busy reply with
no `Retry-After` is retried once 4 to 6 seconds later and the reading then shows; a
`Retry-After: 1` is honoured; two busy replies show the busy message after exactly two
requests, and clicking in and out during the cool-down sends nothing; `Retry-After: 60`
sends no retry; a dropped connection and an error 500 show the right messages; a busy avatar
shows the busy message and Try again, which works after the wait; CC-27's re-test (entries
sent about a second apart against a server allowing 6 calls per 10 seconds) shows the busy
message, and after waiting, clicking in and out again works; never more than one of these
calls in flight; the Option A lookup still runs during a cool-down; no script errors. The
CC-26 checks were re-run on this file and still pass. Not checked: the real endpoint's
limit, and whether it sends `Retry-After`. `index.html` after this change: 177243 bytes,
SHA-256 `a491dfbcbb8ffe08d36d3b608667dd09ed8886209704fb2012290607e3c9618b`.

## 2026-10-28 (third change): lookups use Claude's general knowledge through the Bedrock endpoint (CC-26)

Bedrock has no web search tool, so Option A ("Have Claude look it up") now answers from
Claude's general knowledge, and all three calls to Claude go to the project's own endpoint
instead of Anthropic's API. Tracker item CC-26; the reasoning is in `decision-rules-log.md`
(R25).

### Calls to Claude

- The field reading (manual mode and the design studio's job and name), the avatar
  proposal and the lookup now go to `/api/claude/casting-call`, set once in a marked IT
  block (`CLAUDE_ENDPOINT`) above `classifyField`. Same Anthropic Messages format as before.
- `model` and `temperature` were removed from all three request bodies (the server sets
  Claude Sonnet 4.6 at temperature 0), and the `web_search` tool was removed from the lookup.
- Output limits unchanged: 150, 250 and 2,200 tokens.
- If the endpoint is set to a `PLACEHOLDER_` value or left empty, nothing is sent and the
  lookup says the page isn't connected to Claude yet (the same check as Thea & Drew).

### The lookup's instructions

- "Do at most ONE web search…" became "Answer from your general knowledge only, then respond
  immediately — a complete, valid JSON object matters more than exhaustive detail."
- Removed: the fixed search-query sentence (R8), the "from whatever pages your search
  returned… say which in markers_basis" wording, and the fields `designer_source`,
  `org_diversity_source`, `general_tech_demographics_source`, `adaptive_source`,
  `markers_basis` and `sources`.
- Added: "Answer only from what you reliably know. Where you are not sure, use the 'none',
  'unclear' or 'unknown' codes instead of guessing. Give no links."; `confidence_note` now
  also says what may be out of date; a new first key, `recognized` ("Set recognized to true
  only if you know this specific product well enough to describe it. If you don't, set it to
  false, leave every other field out, and do not guess from the name.").
- Organization diversity now uses 'technical-specific' or 'company-wide' only if Claude
  specifically knows the figure is published, 'removed' only if it knows publication stopped,
  and 'none' otherwise. Tasks follow "the order the product usually presents them" in "the
  product's own wording." The closing line no longer says "after searching."
- Kept, answered from general knowledge: designer credit, organization diversity (with
  country and general demographics) and adaptiveness.

### Code removed

The source-link state (`lastSearchSources`, `verifiedUrls`, the four per-field source
variables), `harvestSearchUrls`, `verifySources`, the URL-cleaning block, `validSource`,
`sourceLink`, `setMarkerSources`, `SRC_LABELS`, `srcTag`, the now-unused `escapeAttr`, the
"Pulled from" line, the per-field source links on the results cards, the source tag on each
results row, the empty `searchSources` box and the `.src-tag` and `.search-sources` styles.
`lastSearchConfidence` (the "How sure is this" line) is kept.

### Products Claude doesn't recognize

If the answer's `recognized` is not `true`, the page fills nothing in and shows no scores. It
opens the manual sheet with: "Claude doesn't recognize "[name]", so it has nothing reliable
to fill in. Fill in the sheet below with what you know." An answer cut off after
`"recognized": true` still keeps the flag, so a partial answer is still shown with its
heads-up note. An empty or unreadable reply (the Poke case in CC-22) gets the "couldn't be
read" notice.

### Messages and page text

Every status, error and failure notice now says "lookup" or "try again" instead of
"search" or "search again", per the CC-26 tables; the internal message keeps the words
"could not parse" that the notice matcher looks for. The Option A card, box label,
placeholder, button ("Look it up"), results introduction, results legend and the worked
example's Anthropic note were reworded per CC-26. New "How this works" note at the top of the
Option A panel (knowledge through about August 2025, no internet search, live search planned).

### Methodology and essay text

Reworded per CC-26: "It shows its work" (no sources), the "reads the marketing" bullet (now
"works from memory, not from the product"), the methodology link's first topic, "What
actually happens", "It has never seen or heard the product", "Check the details before you
trust them" (Walters & Wilder citation kept) and "The pool it draws from is itself skewed".
One more passage, found by the leftover search and not in CC-26's tables: the "How are these
categorized" box above the sheet said Claude used "whatever the web search turns up" and
tagged each answer by source; it now reads "When Claude looks a product up, it makes the same
call from its general knowledge and returns each selection directly with a short reason."

### Checks

Against a simulated endpoint in a headless browser (33 checks, all passed): one request per
lookup to `/api/claude/casting-call`, with no `model`, `temperature` or `tools` and
`max_tokens` 2,200; the new rules in the prompt and no search or link-field wording; a
recognized product shows results with no source tags or links, the designer,
organization-diversity, general-demographics and adaptiveness rows still show, and Reveal
works with no "Pulled from" line; `recognized: false` and a missing flag both open the manual
sheet with the not-recognized note and no scores; a cut-off answer with the flag shows partial
results with the heads-up note; a cut-off answer with nothing usable, an unreadable answer
and an empty reply each show the right notice; HTTP 400, 403, 404, 429 and 500 and being
offline (two attempts) each show the right notice; the manual-mode reading sends
`max_tokens` 150 and the design studio's avatar 250, neither with `model` or `temperature`;
the worked example loads; nothing is sent to `api.anthropic.com`; no script errors; with a
placeholder endpoint, nothing is sent and the not-connected notice shows. The script passes
a syntax check. Not checked: a real call through the Bedrock endpoint, and the Siri, Alexa,
Stan, Poke and made-up-name lookups on real Claude, which wait on IT's endpoint.
`index.html` after this change: 173509 bytes, SHA-256 `45cc85a54f39eb53dea83f922bf794ac3e66d5caa495718f9fba6e789a7db251`.

## 2026-10-28 (second change): working-paper citation anonymized

The working paper is now cited without the authors' names, and one phrase that
identified them was removed. Wording and code comments only. The reasoning is in
`decision-rules-log.md` (R24).

| # | Where | Before | After |
| --- | --- | --- | --- |
| 1 | Essay, "What this doesn't tell you" | (the authors' surnames, working paper) | (Authors, working paper) |
| 2 | Methodology page, References | the authors' surnames and initials (working paper). Director, actor, audience: ... | Authors (working paper). Director, actor, audience: ... (title unchanged) |
| 3 | Code comment above the alignment labels | Per (the authors' surnames): markers place an actor... | Per the working paper: markers place an actor... |
| 4 | Code comment on the placement card | ...per (the authors' surnames)' distinction between... | ...per the working paper's distinction between... |
| 5 | Methodology page, "Were these articles actually read?" | ...that is a reasonable place to stand, since it is the authors' own field and their own paper. | ...that is a reasonable place to stand. |

Row 5 was removed because, paired with the citation, it identified the authors.

### Checks

Diff against `b642f01`: five lines changed, nothing else. No prompt, scoring, verdict
or style lines touched; the two comments are not sent to Claude. A search of
`index.html` for the authors' names, other team members' names, their institutions and
email domains finds none. Still containing the names, outside `index.html`: `README.md`,
`old/index.html`, three older entries in this changelog, and the git history (open
item 37). `index.html` after this change: 182447 bytes, SHA-256 `81c5c4bf45691d0fb4b920aa65a7218ed655e76bb05251258dadbcdc8f7f85db`.

## 2026-10-28

The opening notice ("Before the curtain rises") was shortened, and its data paragraph
now describes lookups as running on Claude's general knowledge under university
agreements. Wording only. The reasoning is in `decision-rules-log.md` (R23).

### Opening notice

- **Removed:** the bold first paragraph, "You'll need to be signed in to Claude to use
  this tool. Information you enter into these tools (e.g., your responses,
  conversations, and the figures produced) may be saved. What you enter is processed
  by Anthropic's Claude." Its second and third sentences were added in the Oct 27
  upload (commit `ebb6a4b`, "the Bedrock build that Valerii sent"), which had no
  changelog entry; that upload changed only this one line.
- **Removed:** the "On opting out of model training" paragraph, including its
  "Anthropic's instructions" link to privacy.claude.com.
- **Rewritten:** the second half of "On the data you put in." The first sentence is
  unchanged. The sentences saying Claude runs a web search through Anthropic's API, that
  the usual handling for any Claude conversation applies, and pointing to Anthropic's
  privacy policy were replaced with: "If you ask Claude to look a product up, it's
  handled under university agreements for AI services and isn't used to train models."

### Not changed yet, by decision

Other text that still describes a web search was left for later: the Option A card
("Claude searches for public details"), the results legend and its "Other source" tag,
the methodology section "If you ask Claude to look it up" ("at most one web search"),
and the essay bullet "In search mode it reads the marketing, not the product." The
code is also unchanged: the lookup still requests a web search directly from Anthropic's
API, and the Option B readings and the design studio still call that API directly, so
all three fail in a published artifact. The scoring, prompts and verdict logic are
untouched.

### Checks

Diff against `ebb6a4b`: one line changed and five removed, all inside the notice; no
script, style or prompt lines touched. Published as a private artifact for review.
`index.html` after this change: 182540 bytes, SHA-256 `d240e5ed9ead570f488e218a62229200befe7f93fe2fcacc60725d498aaa7d53`.

## 2026-10-13

Path A's labels and notes no longer make statements about what designers intended
(tracker item CC-13, confirmed Oct 8). The reasoning, the tests and the decision on an
instruction to Claude are in `decision-rules-log.md` (R22).

### Path A: no statements about designers' intent

The authors' decision is that the tool cannot infer what designers intended, so it
must not write "designed to," "intended to," "chosen to," "aims to," "deliberately"
and the like, in any wording. A baseline on real Claude (20 lookups: Siri, Alexa,
Bixby and Gemini, five runs each) found none in Claude's text and 108 occurrences in
the page's own text, in all 20 lookups. So the fix is to the page's own wording:

- The voice dropdown "Deliberately androgynous" is now "Androgynous," the imagery
  dropdown "Abstract / deliberately neutral" is now "Abstract / neutral," and the
  results label table says the same.
- The score note now reads "0 if neutral/androgynous."
- The voice classifier prompt (manual mode) no longer says "deliberately
  androgynous."

Left as they are, by decision: Path B (its two dropdown labels and its requests), the
"unclear" outcome paragraph (also shown by Path B), and the essay and notice text. The
scoring functions, the verdict logic and the shared outcome text are unchanged.

### An instruction to Claude was tried and dropped

A sentence telling Claude to describe only what can be observed and to make no
statements about designers' intent was built and tested, then removed before
shipping. It fixed nothing we saw (Claude's text had no intent wording in 60
lookups) and it shifted scores: in a 10-run control on Alexa, adaptiveness read
"unclear" in 7 of 10 runs with the instruction against 1 of 10 without, which changes
the pathway, and some technical tasks were coded neutral instead of masculine (5 of 14
runs against 0 of 19). That conflicts with leaving the original scoring alone. The
sentence's text and the results are in the log.

### Checks

Against a stand-in for Claude: what Claude is sent by the lookup and by the manual-mode
name, imagery, tone and task readings is byte-identical to before; the voice request
differs only by the removed word; Path B's requests are byte-identical; the same
answers for Siri, Alexa, Bixby and Gemini give identical scores, verdicts and markup
apart from the changed wording; Path A manual mode (ten scenarios) and Path B (six
sets of choices) are identical. On real Claude (20 lookups, the same four products): no intent wording in
Claude's text; the page's own wording fell from 108 occurrences in all 20 lookups
to 20 in 10 (the "unclear" paragraph only); the final results were identical to the
baseline in all 20 lookups; and Alexa's adaptiveness read "adaptive" in 5 of 5. The
one-word voice prompt change has not been tested live.

### A correction to earlier statements

Earlier entries said Path A's prompts were "byte-identical to the original." That was
true only against the version just before the Path B work. The scoring functions are
byte-identical to the Oct 6 version and still are. The lookup prompt, the classifier
prompts and the label tables already differed from Oct 6 because of earlier approved
items (the voice rule, the "genuinely" instruction, the three-task cap, the official
site removal). After this item they match the Oct 12 version except for the voice
classifier prompt (one word) and the labels.

## 2026-10-12

Three sets of edits to `index.html`. For the submission build, the authors' names,
affiliations and contact details are removed from everything a reader of the tool
sees. Names in code comments and in the git history were left alone on purpose
(R18). The voice rule changed (R10). And the design studio changed: Claude now reads
the job and the name and proposes an avatar, and the essay and notices say who makes
each reading (R19, R20). Path A's scoring and display are unchanged. The README was
also corrected (last section of this entry). The reasoning is in
`decision-rules-log.md`.

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

### Design studio: Claude reads the job and the name, and proposes an avatar

In the design studio the person types the job and a working name, and Claude fills
in how the work typically gets coded and how the name reads, each with a short
reason. It reuses the reading step that Path A's manual mode already had, so the
prompts for those two readings are unchanged. Claude also proposes one avatar for
the job, with no instruction to match or avoid the job's stereotype, and says how
it reads. The proposal appears on its own once the job is described, like the other
readings, and a "Propose another" button asks for something clearly different (or
"Try again" after a failure). Pronouns,
voice, manner and adaptiveness stay the person's own decisions, every reading can
be changed, and the dropdowns still work by hand if Claude is unavailable.

The readings Claude fills in start blank, here (the job, the name and the avatar) and
in Path A's manual mode (name, pronouns, voice, imagery, tone and each task row), so
a default cannot be mistaken for Claude's reading. A blank counts exactly as the old
default did: a blank name, pronouns, voice, imagery or avatar is "not sure" and left
out of the score, and a blank tone, task or job counts as neutral. Path A's scoring
and every results screen are unchanged from the original: ten scenarios give
identical text and markup, the scoring functions are byte-identical to the Oct 6 version, the prompts were identical to the version before this work, and
the design studio gives the same verdict, pathway and chips for six sets of choices.
A blank tone or task still counts as neutral without the results saying so, as it did
originally (open item 43). The only Path A code touched is an optional hook in the
reading step and the blank handling.

The page says up front that results can vary from one request to the next, above
the brief and again in the prediction card.

The page's flows were tested against a stand-in for Claude. The reading and
proposal calls were then run live (90 calls): the job and name readings were the
same in every run, and the avatar reading in 29 of 30 (open item 40). The avatar
proposal does not steer; whether it should, toward or away from the job's
stereotype, is for the research team to decide. Without steering, the proposal read
the same as the job in 24 of 30 runs and neutral in the other 6.

### Essay, notices and explainer boxes say who makes each reading

Wording that said the person makes the readings, or that the tool contributes no
information, was changed where Claude now makes them. The methodology page now has
three routes in, with a short section on the design studio, and its argument that
"one of the things being assessed here is you" now covers Claude's readings too.
The opening notice now says Claude reads some of what you type (Path A's manual mode
already did), the essay's consistency claim now sits beside the variation notes, and
Path A's "How are these categorized" box now describes manual mode accurately. The
design studio intro says "the same calls" where it said "the same four calls".

The full before and after text for every changed passage is in
`decision-rules-log.md` (R20) and in `wording-review.md`, for the authors to confirm.

### README brought up to date

Twelve factual corrections, with no change to the README's decisions or contact
details: the size of `index.html` (2,935 lines, about 179 KB) and the token estimate;
the number of calls to Anthropic's API (three, adding the design studio's
`proposeAvatar`, with `classifyField` now also used for the design studio's job and
name); and the statement that the design studio works anywhere, which now applies to
its dropdowns only, because Claude's readings and avatar proposal need the artifact
sandbox.

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
