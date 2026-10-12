# Casting Call: decision rules log

**Scope:** Path A ("Bring an AI on stage"), Option A ("Have Claude look it up"); since Oct 12 also the design studio (Path B), blank readings, and the wording of the essay and notices.
**Started:** Oct 8, 2026. **Last updated:** Oct 12, 2026. Rules are numbered in the order they were decided, so the list in force skips the superseded ones, which are kept further down as history.

---

## Where we landed (as of Oct 12, 2026)

**How Option A works now**
1. The person types a product name or link.
2. The page sends one request to Claude with one general web search (the query is the product name, its maker, and the words "AI assistant"), at temperature 0.
3. Claude returns a code and a short reason for each marker (name, pronouns, voice, imagery) and for behavior (up to three tasks, plus tone), along with designer, organizational-diversity and adaptiveness fields. It also says whether each answer came from a page the search returned or from its general knowledge. For voice, one default voice is coded and the others are noted; if there is no single default, the voice is "varies" (R10).
4. The results panel shows each answer with a tag (*Other source*, *Claude's general knowledge*, or *Source not specified*) and opens with a warning that results can vary between searches.
5. The person can edit any answer before revealing the placement. If the connection drops, the page retries once. If a lookup fails, the page opens the manual sheet and says why (R16, provisional).

**How the design studio works now (Path B)**
1. The person types the job and a working name.
2. Claude fills in how the work typically gets coded and how the name reads, each with a short reason, using the same reading step as Path A's manual mode.
3. Claude proposes one avatar for the job and says how it reads, with no steering toward or away from the job's stereotype. "Propose another" asks for a different one.
4. The readings Claude fills in start blank, in both paths, and a blank counts as the old default, so scoring and results match the original. Every reading can be changed, and the page says up front that results can vary (R19, R21).
5. Pronouns, voice, manner and adaptiveness are the person's own decisions, and the verdict logic is the original.

**What is no longer there:** any requirement that the product has an official website; finding, confirming or linking that site; the retry when a named site failed checking; the "no official site, try again" message; and the second pass that read the official site's own pages (R13, R14).

**What the tests showed**
- Reading the official site's pages (ON vs OFF, Siri and Alexa): the same verdicts either way, at about three times the input tokens. Removed.
- Aiming the search at the official site (check ON vs OFF, Siri and Alexa, 12 lookups): the final result was aligned-adaptive in all 12. ON was a little steadier on the raw numbers, partly because it leaned on general knowledge. No stability benefit was shown, and the requirement caused most of the failures. Removed.
- Where the instability comes from: pronouns flipping between "object" and "unknown" (moves the marker score by one reading), which tasks Claude picks and how it codes them (moves the behavior score), and occasionally the adaptiveness reading (changes the pathway).
- Re-measure of the rebuilt page (Siri 5, Alexa 5): the final result was aligned-adaptive in all 10. Every marker-score difference traced to one cause, pronouns coded "unknown" in 6 of 10 runs. Behavior varied mostly through how "smart home control" (Alexa) and "app actions" (Siri) were coded. In a same-session comparison, pronouns were "unknown" in 1 of 6 runs with the official-site-aimed search and 3 of 6 without it, then 6 of 10 on the rebuilt page; this is the first sign of a stability benefit from the official-site wording (see the note under R14 and open item 19).
- Second pair (Bixby 5, Claude 5): the final result is **not steady for every product.** Bixby was "unclear" in all 5 (its placement is neutral, so alignment cannot be decided). Claude's pathway was "unclear" in 3 runs and "misaligned-nonadaptive" in 2, driven by the tone code (feminine vs mixed on near-identical wording), the name code (masculine vs neutral) and the adaptiveness reading (nonadaptive vs unclear). Siri and Alexa were steady partly because everything about them leaned feminine.
- Third pair (OpenAI Dots 5, Gemini 5): "unclear" in all 10, because every marker is neutral-coded and the placement is therefore neutral. Dots' behavior score ranged from -50 to -100 (which third task was chosen, and tone coded masculine vs mixed) without any effect on the result. Gemini was steady except for one "unclear" adaptiveness reading.
- Fourth pair (GitHub Copilot 5, DeepSeek 5): DeepSeek was "unclear" in all 5 (neutral placement). Copilot's result varied: "unclear" in 3 runs and "aligned-nonadaptive" in 2, because the logo was coded masculine in 2 runs and neutral in 3, from the model's differing memory of the logo.
- **Across all eight products tested, the final result was steady for six (Siri, Alexa, Bixby, Dots, Gemini, DeepSeek) and varied for two (Claude, Copilot).** Bixby, Dots, Gemini and DeepSeek are steady because their placement is neutral. In both products that varied, one borderline reading (Claude's name or tone, Copilot's imagery) tips a neutral placement into a directional one (open item 30).
- Design studio, live (Oct 12, 90 calls, 6 jobs, 6 names, 5 runs each): job and name readings were the same in every run; the avatar reading was the same in 29 of 30 runs; the unsteered avatar read the same as the job in 24 of 30 runs and neutral in 6. Each reading call cost about 4,500 input tokens (open item 45).
- Cost per lookup with this design, confirmed on the rebuilt page (10 lookups): 21,103 to 21,304 input tokens and 661 to 927 output tokens, one call and one search every time.

**Verified live (Oct 9, 2026):** you ran the rebuilt page in the preview (10 lookups, Siri and Alexa). All completed, one call and one search each, no errors (open item 18).

**Still open, for the research team:** the pronoun rule, anchored task coding, tone coding, adaptiveness, how one borderline reading flips a placement, and how steady is steady enough (open items 2, 10, 11, 13, 14, 21, 22, 23, 27, 29, 30); and, new since Oct 9, whether the avatar proposal should steer (42), what it should be based on (44), whether a blank tone or task should count as neutral (43), and a steadiness test of the design studio's readings (40). The published link from early on is out of date, and a published artifact cannot run live searches (open item 11).

**Companion one-pagers:** the decision list for the research team (https://claude.ai/artifact/EGrZUcwj2su9g3EuVHePWP) and a short account of where things stand and why (https://claude.ai/artifact/SkorN8hSWLWB91Rb8aDRJ8). Both were brought up to date on Oct 12. The wording changes for the authors to confirm are in `wording-review` (PDF and written) and `wording-checklist.html`.

**Files:** `index.html` is the main page, with every change in this log. `casting-call-update.zip` holds the files to push (`index.html`, `CHANGELOG.md`, `decision-rules-log.md`, `README.md`). `wording-review.md`, `wording-review.pdf` and `wording-checklist.html` are the wording list for the authors. `removed-contact-block.html` holds the removed "Questions or comments" section; it contains names and emails, so keep it out of the repo and the submission. `index-artifact.html` is a copy without web search, made for a published artifact; it contains names and is not for submission. `index-backup-with-official-site.html`, `variation-test-rebuilt.html`, `variation-test.html`, `variation-test-gate.html` and the word-check pages are earlier copies and test pages.

**Submission build:** `index.html` has had the authors' names, affiliations and emails removed from the visible page (R18). The other copies, the repo's README and `old/index.html`, the older changelog entries and the git history still contain them (open item 37).

---

## How to read this

Each rule says **what the tool does**, **why**, and **how it is enforced** (in the instructions to Claude, in the page's own code, or both). Where a rule is only an instruction to Claude, the page cannot guarantee it is followed. That is noted.

---

## Rules in force

These are the rules the main page follows. Where a line says "Revised Oct 9, 2026," the entry was reworded for the version without the official-site requirement; the earlier wording is described in that line.

### R2. Behaviors are limited to the top three

- **Rule:** At most three tasks are returned, chosen and ordered as R12 describes. The section is labeled "top three tasks."
- **Enforced by:** Instruction to Claude **and** the page itself cuts any list to three (so it holds even if Claude returns more). Applies to the lookup's search.
- **Revised Oct 9, 2026:** the earlier wording said "most central first" and mentioned the official-site search. Ordering is now as in R12, and the official-site search no longer exists (R13, R14).

### R5. The product is assumed to be an AI assistant, agent or chatbot

- **Rule:** Claude is told that this tool only assesses AI assistants, agents and chatbots, and that when a name is shared with something else the AI assistant is the product. The prompt uses Amazon's Alexa vs. `alexa.com` as its example.
- **Now also the main guard against scoring the wrong product.** The official-site check that used to catch this is removed (R14); R5 and the query wording in R8 are what is left.
- **Limit:** Names shared by *other AI products from different makers* may still be scored as the wrong product, and nothing else will catch it. The person should check that the name and description that come back match the product they meant.
- **Revised Oct 9, 2026:** the earlier limit said such names "may still need the person to paste the official link." The link is no longer used to check anything.

### R6. Capitalization does not matter

- **Rule:** "siri", "Siri" and "SIRI" are the same product. A pasted link is treated as naming the product.
- **Enforced by:** Instruction to Claude only. The page itself never treated case as meaningful; earlier variation was not caused by casing.
- **Revised Oct 9, 2026:** the earlier wording said pasted links worked in any casing and without `https://`, through the page's own link handling. That handling existed to match a pasted link against the official site (R3) and is removed with it (R14), so the page now passes the text to Claude as typed.

### R7. Temperature is 0 on every call

- **Rule:** Every request the page sends to Claude uses temperature 0: the main search and the Option B per-field classification.
- **Why:** To reduce run-to-run variation in judgment calls.
- **Limit:** This steadies how Claude judges a *given* piece of evidence. It cannot steady the evidence: a live web search can return different pages each time.
- **Revised Oct 9, 2026:** the earlier wording also listed the site-confirmation searches and the official-site search. Both are removed (R13, R14).

### R8. Fixed search-query form

- **Rule:** Claude is told to make its single search exactly the product name, then its maker, then the words "AI assistant" (for example: "Siri Apple AI assistant"), and nothing else.
- **Enforced by:** Instruction to Claude only.
- **Revised Oct 9, 2026:** the query used to end in "official site," to reach the product's own website (R1). With R1 removed (R14) it ends in "AI assistant." This is the wording the OFF side of the check ON vs OFF test used.

### R10. Voice: use the default voice when there is one; otherwise say it varies

- **Rule (revised Oct 12, 2026):** If the product has one default voice, use it for the voice code and say in the voice value that other voices exist (for example "Higher-pitched default; other voices available"). If there is no single default (the user chooses at setup, it differs by country or language, or it changed), do not pick one: the voice code is "none," the description starts with "Varies," and the label reads "Varies — no fixed voice, or user picks one." A product with no voice at all keeps the code "none," as before; that case is unchanged and was not retested in this batch.
- **Applies to:** the lookup's search and Option B's per-field check. In Option B: a description that names one default voice is judged on that voice, and a description that says there is no single default, or simply says the voice varies, is answered "none."
- **Replaces the Oct 9 rule,** which coded every product with more than one voice as "varies," whether or not it had a clear default. That rule had been added because Siri was returned as both "higher-pitched" and "deliberately androgynous" on different runs, and it held in every run afterward.
- **Why the change:** decided by the user. The Oct 9 rule treated a product with one clear default the same as one with none. Siri should still come out "varies," which is accurate: reports from 2021 say users choose a voice at setup, with none preselected, and that some countries had defaulted to a male voice, so there is no single default.
- **Scoring effect:** A product with a clear default now scores on that voice (feminine, masculine or neutral) instead of counting as a neutral zero. "None" still counts as neutral, as before.
- **Expected shifts (not yet measured):** Alexa should gain a feminine voice reading. Bixby's earlier notes said its default tends toward a higher-pitched female voice in English, so its placement may move from neutral to feminine and its result from "unclear" to aligned or misaligned. Products with no voice should not change. Earlier test numbers are not comparable for the products that shift.
- **Risk to check:** Claude might pick an outdated default (for example Siri's historical female default). The "or it changed" clause is meant to guard against that.
- **Enforced by:** Instruction to Claude only (the lookup prompt and the voice classifier prompt). Checked by capturing the requests the page sends before and after: the voice-rule sentence is the only difference in each prompt; model, temperature (0), token limit, search tool and the "genuinely" instruction are identical.
- **Main page after this change:** `index.html`, 170848 bytes, SHA-256 `81687cf3b33adf231de7f16d6d0d5f83c8b6cde339f2f54c0e45ad71920194d0`.
- **Test:** run Oct 12 in three sets: Siri and Alexa, Bixby and Gemini (5 runs each), and GitHub Copilot and DeepSeek (3 runs each). Siri, Bixby and Gemini were "varies," Alexa was coded on its default voice, and Copilot and DeepSeek stayed "none," consistently in every run. See the three "Voice rule re-measure" sections under Test results. All four cases (one default, no single default, no voice) are tested.
### R11. Warn that results can vary between searches
- **Rule:** The results panel (shown only after a real search) opens with: "Results can vary from one search to the next, because they depend on which pages the search brings back." It sits directly above the line explaining the source tags.
- **Why:** Results depend on a live web search, so the same product searched twice can return different pages and different readings. Chosen by you from four wordings (short, tag-pointing, "starting point," and a banner); the short one was picked and the tag explanation was already present.
- **Where it does not appear:** the worked example, the manual sheet, and the final placement and score view. Not added there; can be if wanted.

### R12. Task selection must be repeatable and non-overlapping

- **Rule:** Claude is told to pick the three tasks the product is most prominently presented as doing, list them in the order the source presents them (what it leads with comes first; "source" means whichever pages the search returned), use the source's own wording where possible, keep each to a few words, and never list two tasks that cover the same activity or split one activity into several entries. Applies to the lookup's search. Replaces the earlier "top three, most central first" wording.
- **Why:** In the detailed Siri test the third task changed from run to run (navigation, information lookup, writing assistance), and tasks overlapped (calls, texts and reminders appeared in two entries), which double-counted some readings. How a given task was coded was consistent; the choice of tasks was not.
- **Cost:** none (wording change only).
- **Evidence so far:** In the build for every test run since it was added, but never compared against a run without it, so its effect on stability is unproven. Task lists still differed from run to run in the check ON vs OFF test (for example, Alexa: 3 different lists in 3 runs on both sides).
- **Enforced by:** Instruction to Claude only. The page still cuts the list to three (R2).
- **Revised Oct 9, 2026:** the earlier wording applied to both the open search and the official-site read (removed, R13), and said "Not yet tested against live results," which is out of date.

### R13. The official-site read is removed; each lookup is one general web search

- **Rule:** A lookup is one general web search (query form per R8). There is no second pass that reads the official domain's own pages.
- **Revised Oct 9, 2026 (R14):** this rule originally kept a cheap step that found, confirmed and linked the official site. R14 removed that step too. What stays in force from R13 is: no second pass reading the product's own pages, and the source labels below.
- **Supersedes (the older text stays below as history, not rewritten):**
  - R9: the official-site pass, the "official site first" merge order, the "Official site" label, the pages-read list and the "why it failed" lines.
  - The official-site parts of R2, R7, R10 and R12. Those instructions still apply to the open search.
- **Source labels now:** *Other source*, *Claude's general knowledge*, *Source not specified*. The legend under "What Claude found" was reworded to match. (An earlier version of this line said the official site was still linked at the top of the results; R14 removed that.)
- **Why:** The ON vs OFF test (below). The verdicts were the same either way, and OFF cost about one third of the input tokens.
- **Declined:** Making the read optional (a button after the result, or a checkbox before the search). Judged too complicated for the benefit.
- **Known cost of this choice:** Provenance is weaker. Nearly every answer is now tagged *Other source* or *general knowledge*, and that split is Claude's own report.
- **Enforced by:** Page code (second pass deleted) and a reworded instruction to Claude ("fill the fields from pages your search returned, then from general knowledge, and say which in markers_basis").
- **Reversible:** A backup with the read still in place is `variation-test-backup-with-official-read.html`.
- **Scope of the edit:** made in the variation-test copy first; applied to the main page in the Oct 9, 2026 rebuild (R14).

### R14. The official-site requirement, confirmation and retry are removed

- **Rule:** A lookup is one general web search (query form per R8). The tool no longer looks for, requires, confirms or links an official website, and there is no retry. If a product has no official website, it is still scored from whatever the search and Claude's knowledge provide; the tool does not refuse or ask the person to try again. No official link is shown. The note about official websites under the search box and the "try again" message are removed.
- **Supersedes:** R1, R3 and R4 (kept below as history), and the earlier "official site" query wording (R8).
- **Why:** You decided it ("it's ok to do this, the goal for it was stability only"). Two tests found no stability benefit from aiming at the official site: the read ON vs OFF test (R13) and the check ON vs OFF test. In the second, the final result was identical in all 12 lookups; the ON side was a little steadier on the raw numbers, but partly because Siri ON leaned on general knowledge. The requirement also caused most of the false "couldn't find an official webpage" failures, plus retries and extra calls.
- **Note after the re-measure (Oct 9, 2026):** the reasoning above, "no stability benefit," is weaker than it looked for markers. Pronouns were coded "unknown" in 1 of 6 runs with the official-site-aimed search and 3 of 6 without it in the same session (check ON vs OFF test), then in 6 of 10 runs on the rebuilt page. Earlier tests that aimed at the official site had it rarely (Siri 0 of 10; Alexa 1 of 12). Small numbers, and the earlier tests ran at other times, but it is the first evidence of a benefit. The final result was unaffected in every test. R14 stands for now; whether the official-site *wording* (without the check, retry or link) steadies pronouns is untested (open item 19).
- **What is lost:**
  - The wrong-product guard. R5 and the "AI assistant" query wording (R8) are what remains, and names shared with other AI products can still be scored as the wrong product.
  - The refusal for products with no website. A made-up or obscure name will get a score from general knowledge, with the unknown and none codes and the confidence note as the only signal.
  - The link at the top of the results.
- **Expect a shift:** The prompt wording changes with R1 gone. In the check ON vs OFF test, "Smart home control" was coded masculine in all 3 Alexa ON runs and neutral in all 3 OFF runs. Numbers collected with the check ON are a different baseline from numbers collected without it.
- **Status:** Decided Oct 9, 2026 and **applied to the main page the same day** (rebuilt from the variation-test copy; the previous main page is saved as `index-backup-with-official-site.html`). The OFF side of `variation-test-gate.html` is what was measured.
- **Enforced by:** Page code (the check, confirmation and retry deleted) and the reworded instruction to Claude.

### R15. Retry once if the main search fails with a network error

- **Rule:** If the main search fails with a network error, the page waits about a second and tries once more before showing an error.
- **What counts as a network error:** the request got no response at all ("Failed to fetch"). If the service answers with an error (for example an HTTP 400 or 429), the page does not retry, because the service did answer.
- **If the second try also fails:** the person sees "couldn't reach the service (the connection dropped twice) — please try again" inside the usual "Couldn't complete that search" message, and can still fill in the sheet by hand.
- **Why:** A "Failed to fetch" error was reported once in live use and its cause was never confirmed; a dropped connection looks identical.
- **Enforced by:** Page code. Applies to the main search only.
- **Status:** In the current main page and the test copies.

### R16. When a lookup fails, say why on the manual sheet (provisional)

- **Rule:** If a lookup fails for any reason, the page opens the manual sheet as before and shows a notice at the top of the sheet. The notice says in plain words what happened and what to do, with the technical message underneath as "Details" so it can be reported. It clears when a new search starts or when the person switches away from the manual sheet.
- **What it says (added after a review of the first wording, "Couldn't complete that search (Request failed (429))," which said neither what it meant nor what to do):**
  - **429:** the search service is busy or a usage limit was reached; wait a few minutes, then go back to Option A and search again; if it keeps happening, the Claude usage limit may be used up.
  - **500s (for example 529):** the service is temporarily unavailable; wait a minute and search again.
  - **400:** the request was rejected; try a different product name or link; if it keeps happening, tell whoever runs the page.
  - **401 or 403:** the page isn't allowed to use the search service, which usually means it is open outside Claude; use the sheet instead.
  - **Connection could not be reached (after the one retry):** check the internet connection and search again; if the page is open outside Claude (for example on a website), the lookup isn't available there, so use the sheet.
  - **Answer cut off, or answer unreadable:** go back to Option A and search again; that usually works.
  - **Anything else:** try again in a moment; if it keeps happening, use the sheet.
  - **Refreshing the page is not suggested:** searching again from Option A keeps what was typed, and a refresh would clear anything entered on the sheet.
- **Why:** The reason used to be written into the search panel, which is hidden once the manual sheet opens, so the person could not tell whether a usage limit, a dropped connection or a parsing problem had occurred (open item 31). The original page behaved the same way.
- **Provisional by design:** You asked for this fix "and later we can maybe take it out." Every piece is marked `[search-failure-note]` in `index.html`: one CSS block, one HTML element at the top of the manual sheet, two helper functions, and three one-line calls. Deleting every line or block carrying that marker restores the previous page exactly (checked).
- **Enforced by:** Page code only. No effect on scoring, instructions to Claude, or cost.

### R17. Explanations must not use the word "genuinely"

- **Rule:** The two prompts that write the gender-coding explanations each carry this instruction: "Do not use the word 'genuinely' in your explanations. State the observation plainly." They are (1) the lookup prompt, placed beside the gender-coding rules, and (2) the manual-mode (Option B) classifier prompt, which covers all five field types (name, voice, imagery, task, tone).
- **What was removed:** from the prompts, "genuinely" appeared in two classifier prompts and nowhere in the lookup prompt. Task prompt: "genuinely mixed/spans both" became "mixed (it spans both)." Tone prompt: "genuinely blends both" became "a blend of both." From the output the person sees: the label for the "mixed" code, "Genuinely spans both," became "Spans both" in all three places it appears (the task chip in the results panel and the task dropdown in each of Path A and Path B), and the worked example's reason now begins "Spans admin/care-adjacent and technical/analytical work." Scope, as clarified by the user: the output that says how feminine or masculine something is (the codes, their labels and the explanations), not the page's explanatory notes.
- **The four printed outputs that changed (approved by the user):**

  | # | Where it shows on screen | Before | After | Line |
  | --- | --- | --- | --- | --- |
  | 1 | "Gender coding" chip for a task rated "mixed," in the results panel | Genuinely spans both | Spans both | 1575 |
  | 2 | Path A task dropdown in the editable sheet, the "mixed" choice | Genuinely spans both | Spans both | 949 |
  | 3 | Path B (design studio) task dropdown, the "mixed" choice | Genuinely spans both | Spans both | 1079 |
  | 4 | Sample explanation under the first task in the worked example | Genuinely spans admin/care-adjacent and technical/analytical work, no single stereotype fits. | Spans admin/care-adjacent and technical/analytical work, no single stereotype fits. | 1938 |

  Voice, name, pronouns and imagery never had the word in the page's own wording, so nothing printed changed for them; for those, any use of the word came from Claude's written explanation, which the added instruction addresses. The tone chip in the results panel and both tone dropdowns (lines 970 and 1092) read "Blends both depending on context" and never had the word, so they needed no change. (An earlier version of this table called line 1079 a tone dropdown; it is Path B's task dropdown. Corrected after the repo was read.) The same label is also used in the placement breakdown shown after "Reveal"; that display was not rendered in the test.
- **By design:** no find-and-replace filter on the output, because deleting a word after the fact can leave a broken sentence. The fix is at the prompt.
- **Scope kept:** seven lines of `index.html` changed in total: the two classifier sentences, the classifier prompt assembly, the lookup prompt, the three "mixed" labels and the worked-example reason. Checked by capturing the requests the page sends before and after, for Siri, Clippy and Bixby and for each field type: the model, temperature (0), token limit, search tool and user message are identical, and the only difference in the lookup prompt is the added sentence. Scoring, task rules, search settings and the explanatory notes are untouched. No code compares against the label text.
- **Not changed (by design):** five explanatory notes that use the word (lines 754, 847, 1059, 1302, 1310). They are page text, not the output that rates how feminine or masculine something is.
- **Caveat:** naming a word in an instruction can sometimes make a model more likely to use it. The live word check will show whether that happens.
- **Live check (Oct 9, 2026), `wordcheck-after.html`, run by the user in the chat preview:** Siri, Clippy and Bixby, one lookup each, plus 8 manual classifier calls. The word (searched as "genuine," case-insensitive) appeared 0 times in Claude's lookup text, 0 times in the classifier answers, 0 times in the results panels, 0 in the dropdown choices and 0 in the result labels. Limits: a small sample (11 outputs); the old version was not run live for comparison, so the instruction cannot be credited with removing a word that was never seen in a live run; and none of the three lookups produced a task coded "mixed," so the changed "Spans both" chip was covered by the label scan and the earlier render test, not by this live run. `wordcheck-before.html` is available to run the same check on the old wording.
- **Observed in that run, outside this rule:** Siri's pronouns came back "unknown" ("Apple avoids gendered pronouns officially; usage varies"), the same flip as open items 13 and 26; and one classifier answer said "stereotypically male domain" where the others say "masculine-coded." Neither was changed.
- **Tested:** the prompts, by capturing real requests (above), and what appears on screen, by rendering a result with a "mixed" task and scanning the results panel, the dropdown choices and the worked example before and after (before: the word appeared in the task chip, the dropdowns and the example; after: nowhere). Not tested: live model output, because there is no live access from the build environment. Two test pages run the check live in the chat preview: `wordcheck-before.html` (old prompts and labels) and `wordcheck-after.html` (new). Each runs three products and the manual classifier on sample descriptions, then searches Claude's text, the results panel, the dropdown choices and the result labels for the word.
- **Main page after this change:** `index.html`, 171818 bytes, SHA-256 `5b7d7f11d338031964f5837b71e2df57725773e790cfb3ed7077847ef16a7145`.

### R18. The authors' names and contact details are removed from the visible page (submission build)

- **Rule:** For the submission, nothing a reader of the tool can see names the authors, gives their affiliations, or gives their email addresses. This covers the visible page only: names in code comments and in the git history are left alone on purpose, and the inline citation and the reference-list entry for the working paper are kept on purpose (they name the authors; that was the decision).
- **Why:** The tool is submitted without the authors' names. Decided by the user, one visible item at a time.
- **The seven changes:**

  | # | Where it shows | Before | After |
  | --- | --- | --- | --- |
  | 1 | Browser-tab title | ended with the authors' surnames | ends after "Assessing How an AI Gets Gendered" |
  | 2 | Byline under the page title | "By" and both authors' names | removed |
  | 3 | End of the "Before the curtain rises" notice | a "Questions? Email ..." line with both names and both emails | removed |
  | 4 | Footer, first sentence | an attribution to the authors, "adapted from their working paper" | removed |
  | 5 | Footer, remaining sentence | "...interpretation of that research for reflection purposes..." | "...interpretation of academic theorizing on AI and "doing gender" in organizations for reflection purposes..." (so it reads correctly without the removed sentence) |
  | 6 | Essay, "Questions or comments" section | heading, an invitation to send feedback, and each author's affiliation and email, with copy buttons | removed in full |
  | 7 | Design section intro | a clause crediting the model to the authors, then "you'll make..." | clause removed; "You'll make the same four calls..." |

- **Restoring item 6:** the exact block is saved as `removed-contact-block.html`, with a comment saying where it goes back in (inside the essay section, after the "Read the full methodology" button, before the section closes). The copy-button script and styles are still in the page. That file contains the names and emails, so keep it out of the repo and out of the submission.
- **Checks:** only the intended lines changed in each step; the page's tag structure is balanced before and after; the page loads with no errors; no email address and no "Questions or comments" text remains in the visible page; the script passes a syntax check.
- **Not changed (listed for the record, see open item 37):** the other copies of the page I made (the artifact copy, the backup and the test pages), the repo's README, `old/index.html` and older changelog entries, the git history, and the published artifact copy. They still contain the names.
- **Main page after this change:** `index.html`, 170537 bytes, SHA-256 `fefd7076f849739e6bd912370061edbae4c0cbe56260e246ae5ef87f87dce46a` (since superseded by the R10 voice-rule change; see R10 for the current checksum).
- **Enforced by:** Page edit only. No effect on scoring, instructions to Claude, or cost.

### R19. Design studio: Claude reads the job and the name, and proposes an avatar

- **Rule:** In the design studio the person types the job and a working name. Claude fills in how the work typically gets coded and how the name reads, each with a short reason, using the same reading step and prompts as Path A's manual mode (unchanged). Once the job is described, Claude proposes one avatar for it and says how it reads, as it does for the other readings: the proposal appears on its own, and a button asks for another (or tries again after a failure). **The proposal does not steer:** Claude is not told to match or avoid the job's stereotype and is not told how the job reads (the request carries the job text only). Like the job and name readings, it is whatever Claude outputs; the avatar reading is set from it and can be changed. "Propose another" asks for something clearly different from the earlier ones. Pronouns, voice, manner and adaptiveness stay the person's own decisions. Every reading can be changed, and the page says so ("You changed this reading").
- **Same scoring as Path A:** the job, name and avatar readings start blank (R21) and are scored by Path A's rules. A blank name or avatar counts as "not sure" (left out of the score) and a blank job counts as neutral, exactly as the old defaults did. Checked: the same choices, including untouched fields, give the same verdict, pathway and chips on the original design studio and on this one, for six sets of choices.
- **Path A's scoring and results are unchanged.** Checked against the original page on ten scenarios (see R21): identical text and markup. Its prompts and classifier code are identical, and the scoring functions are byte-identical. The Path A code touched is an optional hook in the reading step and the blank handling in R21.
- **Why:** decided by the user: these readings should be outputs of the tool, not inputs, and the tool should propose the avatar itself.
- **Up front about variation:** a note above the brief ("Results can vary from one request to the next...") and a line in the prediction card say the readings and the proposal can vary.
- **The avatar request:** one call, model and temperature 0 as elsewhere, no web search, 250 tokens at most. It carries the job text only, not the job's reading. It includes the "genuinely" instruction (R17), tells Claude to use no real people, existing characters or brands, and asks for a one-sentence concept, a code (masculine, feminine or neutral) and a reason of 12 words or fewer.
- **If Claude is unavailable,** the dropdowns still work by hand and the page says what failed.
- **Checks (against a stand-in for Claude, not live):** a job and a name typed each fill in a reading; the avatar proposal appears on its own once the job is described, with no click, and sets the avatar reading; tabbing through the job field again sends no new request; "Propose another" tells Claude to avoid the earlier proposals; a changed job gets a new proposal with nothing carried over; a slow answer for an old job cannot overwrite a newer one; clearing the job hides the proposal; a Claude failure shows the message and a "Try again" button that works once Claude is back; overriding a reading works; the prediction shows the proposal and the variation note; clearing returns everything to blank.
- **Live steadiness test (Oct 12, 2026):** the job, name and avatar calls were run against real Claude, 90 calls, and the job and name readings were the same in every run (see Test results). The page's flows (blank starts, the automatic proposal, clearing, overriding) have been tested only against a stand-in.
- **Main page after this change:** `index.html`, 183438 bytes, SHA-256 `7a76b88464c0b3514585cadf9f7eea659aeca1616e0f7a1e76da53115d48bb84`.
- **Open decision for the research team:** whether the avatar proposal should steer, toward or away from the job's stereotype, or stay as it is. A steering version (aimed not to match the job's stereotype) was built on Oct 12 and removed the same day: the design studio shows what Claude outputs until section D, so a steering rule is a substantive change that the research team should decide (see "Considered, not built"). Without steering, the proposal reflects Claude's own associations with the job, so it may or may not match the job's stereotype; the reading shown says which. Also open: what the proposal should be based on (open item 44).

### R20. The essay, notices and explainer boxes say who makes each reading

- **Rule:** Wording that said the person makes the readings, or that the tool contributes no information, was changed where Claude now makes them (R19). I read the whole methodology page, the main-page essay and the opening notice for passages the change touches. 14 passages changed or were added (rows 16 to 29 of the list below); the design studio rows are R19.
- **Why:** the wording would otherwise have misdescribed the tool, most importantly the methodology page's argument that "one of the things being assessed here is you," the claim of "exactly two routes in," and the opening notice's statement about the data you put in.
- **Status:** all wording below is for the authors to confirm. A standalone copy with a "Confirmed?" column is in `wording-review.md`.

#### Wording changes to confirm

| # | Where | Before | After | Why | Confirmed? |
| --- | --- | --- | --- | --- | --- |
| 1 | Design studio: Design brief, above section A (new) | (nothing) | Results can vary from one request to the next, because Claude's answers are not identical every time. Treat Claude's readings and its avatar proposal as a starting point; you can change any of them. | Be up front that results vary, as Path A does. | |
| 2 | Design studio: Job field label | How does that work typically get coded? | How that work typically gets coded (Claude's reading) | The reading is now Claude's output. | |
| 3 | Design studio: Job field hint | This records how the work is conventionally coded, not how it ought to be. | Same, plus: "Claude fills it in from what you typed above; you can change it." | Says who fills it in. | |
| 4 | Design studio: Name field label | How will that name read? | How that name reads (Claude's reading) | The reading is now Claude's output. | |
| 5 | Design studio: Name field hint (new) | (nothing) | Claude fills this in from the working name; you can change it. | Says who fills it in. | |
| 6 | Design studio: Avatar field label | Avatar or imagery | Avatar or imagery (Claude proposes one) | Claude now proposes the avatar. | |
| 7 | Design studio: Avatar field hint (new) | (nothing) | Claude fills this in from the job you typed above: it proposes an avatar and says how it reads. You can change the reading, or choose your own. | Says when the proposal appears and what it is. It does not steer. | |
| 8 | Design studio: Avatar buttons and readout (new) | (nothing) | The first proposal appears on its own once the job is described, shown as "Proposed: ...". A button "Propose another" appears after it ("Try again" if it fails). | The avatar behaves like the other readings; the button only asks for another. | |
| 9 | Design studio: Brief card, new line | (nothing) | Proposed avatar: ... | Shows the proposal in the brief. | |
| 10 | Design studio: Prediction card, new line | (nothing) | This prediction rests on Claude's readings above, which can vary from one request to the next. If a reading changes, the prediction can change with it. | The verdict depends on readings that vary. | |
| 11 | Design studio: Status and error messages (new) | (nothing) | "Claude is thinking of an avatar..."; "You changed this reading."; "Couldn't propose an avatar — choose the avatar's reading yourself." | New states of the new controls. The existing "Claude is reading this..." and "Couldn't auto-classify — set this yourself." are reused unchanged. | |
| 12 | Design studio: Intro paragraph | You'll make the same four calls a real director makes for an AI actor... | You'll make the same calls a real director makes for an AI actor... | Claude now makes some of the calls; "four" no longer fits. | |
| 13 | Design studio: "How are these categorized" box | You make each call yourself, guided by the rules below; it's a judgment call applied consistently, not a database hit. | Claude makes the call for the job, the name and the avatar, guided by the rules below, and you can change any of them. Pronouns, voice, manner and adaptiveness are your own decisions. It's a judgment call applied consistently, not a database hit. | Says who makes which call. | |
| 14 | Design studio: Callout when no job has been read | ...once you pick one, this note will tell you... | ...once one is set, this note will tell you... | The reading is often set by Claude, not picked. | |
| 15 | Design studio: Note on undecided markers | ...will firm up once you pick them. | ...will firm up once they are set. | Same reason. | |
| 16 | Path A: "How are these categorized" box, manual mode | In manual mode, you make each call, guided by the rules below. | In manual mode, you type what you know, Claude reads it using the rules below and fills in each selection with a short reason, and you can change any of them. | Path A's manual mode already filled the dropdowns through Claude; the old sentence was inaccurate. | |
| 17 | Notice and essay: Opening notice, "On the data you put in" | ...if you fill in the sheet yourself, that stays in this conversation. | ...if you fill in the sheet yourself, what you type stays in this conversation, and Claude reads some of it, such as a name or a job, to fill in how each one reads. | What you type is sent to Claude to be read, as Path A's manual mode already did. | |
| 18 | Notice and essay: Opening notice, "On where the answers come from" | ...when you code by hand the tool is partly recording your own gender associations rather than measuring the product. | ...the tool is partly recording gender associations rather than measuring the product: yours when you code by hand, and Claude's where it reads for you. | Some of the associations are now Claude's. | |
| 19 | Notice and essay: Closing line of both "How are these categorized" boxes | ...why your own judgment is part of what gets recorded here. | ...why your own judgment, and Claude's, is part of what gets recorded here. | Same. | |
| 20 | Notice and essay: Essay's link to the methodology | ...why your own judgment is part of what gets recorded | ...why your own judgment, and Claude's, is part of what gets recorded | Same. | |
| 21 | Notice and essay: Essay, "It applies the same convention every time" (added sentence) | ...so two assessments can be compared to each other even if you'd have drawn the lines somewhere else. | Same, plus: "Claude's own readings can still vary from one request to the next, so small differences between two assessments mean little." | Consistency claim now sits beside the variation notes. | |
| 22 | Notice and essay: Essay, "It shows its work" | Every call comes with the reasoning and the sources behind it... | Every call comes with the reasoning behind it (and, in search mode, the sources)... | The design studio gives reasons but no sources. | |
| 23 | Notice and essay: Essay, "One coder is one coder" | A score produced by a single person in a single sitting records that person's reading. | A score produced by a single coder, a person or Claude, in a single sitting records that coder's reading. | A coder can now be Claude. | |
| 24 | Methodology page: Section 1, opening line | There are exactly two routes in, and they behave differently. | There are three routes in, and they behave differently. | The design studio is a third route. | |
| 25 | Methodology page: Section 1, "If you fill in the sheet yourself" | ...The tool contributes no information about the product whatsoever — it only applies the coding conventions below to what you typed and does the arithmetic. | ...The tool contributes no facts about the product whatsoever — it only applies the coding conventions below to what you typed, with Claude reading each entry to fill in how it reads, and does the arithmetic. | Claude reads each entry. | |
| 26 | Methodology page: Section 1, new third subsection | (nothing) | "If you use the design studio": you describe the job and give a working name; Claude reads each using the coding conventions and fills in how it reads, with a short reason; Claude also proposes an avatar for the job and says how it reads, and a button asks for another; no web search is involved; every reading and the proposal can be changed and can differ from one request to the next. "The avatar is an idea, not a design": Claude describes a concept in words, has not made or tested an image, and the call it rests on is the weakest in the tool. | Describes the new route. | |
| 27 | Methodology page: Section 2, abstract imagery (added sentence) | ...the one to discount first when a result surprises you. | Same, plus: "The design studio's avatar proposal rests on this same call, so treat a proposal as an idea to react to, and discount it first." | The proposal rests on the weakest call. | |
| 28 | Methodology page: Section 5, opening paragraph | ...the tool is asking you to decide whether a name, a voice, or a picture reads as masculine or feminine. There's no lookup table behind that. So what comes out reflects your associations as much as it reflects any property of the product. | ...the tool is asking you to decide whether a name, a voice, or a picture reads as masculine or feminine — and where Claude reads for you, as it does for the job, the name and the avatar in the design studio, it is making that call instead, and you can change it. There's no lookup table behind either. So what comes out reflects someone's associations, yours or Claude's, as much as it reflects any property of the product. | The central argument of the section, adjusted. | |
| 29 | Methodology page: Section 5, "The practical version" (added sentences) | ...Disagreement isn't noise here. It's the most interesting thing the tool produces. | Same, plus: "In the design studio, Claude's reading is a second coder: compare it with your own and notice where you disagree." | Claude's reading as a second coder. | |

**Left unchanged on purpose**

- Design brief section E, "Who's actually making these calls?" It asks about the human director, so it still fits.
- Essay, "In design mode, it front-loads a trade-off." It does not mention the avatar proposal.
- Methodology page, section 3 callout ("record why you coded something") and the section 5 title ("One of the things being assessed here is you").
- The five explanatory notes that still use the word "genuinely" (decided Oct 9: page text, not the output).

### R21. Readings that Claude fills in start blank; scoring and results unchanged

- **Rule:** The readings Claude fills in start blank, so a default cannot be mistaken for Claude's reading. In the design studio: the job, the name and the avatar. In Path A's manual mode: name, pronouns, voice, imagery, tone and each task row. Adaptiveness, and the design studio's pronouns, voice and manner, are the person's own decisions, so they keep their "Haven't decided" and default options. "Not sure" remains an option; it means Claude could not tell, or the person is unsure.
- **Scoring and results are unchanged:** wherever the page reads one of these readings, a blank stands for that field's original default. A blank name, pronouns, voice, imagery or avatar counts as "Not sure" (left out of the score), and a blank tone, task or job counts as neutral. The results screens show the original default's label ("Not sure", "Doesn't read strongly either way"). So every score and every results screen is identical to the original.
- **Checks:** Path A, ten scenarios (an empty sheet; a name only; a name and its reading; a task typed with its reading untouched; a task set to feminine with the tone untouched, which scored +50 originally; a tone set with a task row untouched; two task rows with the second untouched; markers set with tone and task untouched; a fully filled sheet; the worked example): identical text and markup on the original and the new page. Design studio, six sets of choices: the same verdict, pathway, chips and undecided count (apart from the approved wording "once they are set"). The scoring functions, the classifier prompts and the lookup prompt are byte-identical to the original. New behavior checked: both paths start blank, Claude fills the readings in, a new task row starts blank, clearing returns to blank.
- **Trade-off to know:** a blank tone or task counts as neutral without the results saying it is a default. This is how the original worked (its default was a neutral reading), so nothing changed, but an unset reading can look unset on the form and still count as neutral. Changing it is a research decision (open item 43).
- **History:** built Oct 12, 2026 with a blank left out of the score; removed the same day because that changed Path A's scores; restored the same day with the original scoring (see Considered, not built).
- **Main page after this change:** `index.html`, 183438 bytes, SHA-256 `7a76b88464c0b3514585cadf9f7eea659aeca1616e0f7a1e76da53115d48bb84`.

---

## How each marker is judged (unchanged framework)

These are judgment calls applied consistently, not a database lookup. In Option A, Claude makes them; in Option B, the person types and Claude judges on leaving the field (pronouns are the exception: a simple text match, no judgment).

| Marker | How it is coded |
|---|---|
| Name | Common naming convention in the relevant language or culture |
| Pronouns | The pronoun the product is actually referred to by |
| Voice | Pitch of the default voice: lower reads masculine-coded, higher reads feminine-coded. If there is no single default, "varies" (see R10) |
| Imagery / avatar | Humanlike: body-shape and styling cues. Non-human: general cultural association (angular, cool, tool-like reads masculine; soft, rounded, warm, floral reads feminine). The least settled of the six |
| Tasks | Admin, care, support reads feminine-coded; technical, quantitative, strategic reads masculine-coded |
| Tone / manner | Agentic (assertive, direct) reads masculine-coded; communal (warm, deferential) reads feminine-coded |

---

---

## Rules superseded (kept as history)

Each of these was in force at one time. They are kept so the reasoning is not lost.

### R1. The search must land on the product's official webpage

> **Superseded by R14 (Oct 9, 2026):** the official-site requirement, confirmation and retry are removed. Kept here as history.

- **Rule:** A lookup only succeeds if the tool finds the product's official website (for example, https://alexa.amazon.com/). The official link is shown at the top of the results.
- **If there is no official site:** The tool stops, stays on the search box, and asks the person to try again. It does **not** fall back to the manual sheet and keeps nothing from that search.
- **Message shown:** "Claude couldn't find an official webpage for "[input]". These searches only work for products that have an official website (an older tool like Clippy, for instance, has none). Please try again with a different product name or link." If Claude had suggested a site that failed checking, the message also says which one.
- **Told up front:** A note under the search box says the search only works for products with an official website, using Clippy as the example. Current wording: "If there isn't one, Claude can't pull it up — Clippy, for example, is a real tool but has no official website."
- **Why:** Decided by you. Keeps results anchored to a real, official source.
- **Enforced by:** Page code (gate before any results are shown) plus instructions to Claude.

### R3. A claimed official site must be verified, not just claimed

> **Superseded by R14 (Oct 9, 2026):** the official-site requirement, confirmation and retry are removed. Kept here as history.

- **Rule:** Claude naming a site is not enough. The site is accepted only if one of these is true:
  1. the exact page appeared in the search results;
  2. a page on the same site (same host, a subdomain, or the parent domain) appeared in the search results; or
  3. it is on the same site as a link the person pasted.
- **If none holds:** a confirmation search is run, limited to the claimed site's own domain, then to the company's main domain if that returns nothing (example: `alexa.amazon.com`, then `amazon.com`). A result from that domain confirms the site. If both return nothing, the site is rejected.
- **Why:** Stops made-up or recalled-from-memory links from reaching the person. The fallback exists because real official sites (Siri, Alexa) often don't appear in the first search, and some (web apps like alexa.amazon.com) aren't indexed.
- **Known looseness:** The company-domain fallback confirms the *owner*, not the exact page. A made-up address under a real company's domain could pass.

### R4. One retry when the named site fails checking

> **Superseded by R14 (Oct 9, 2026):** the official-site requirement, confirmation and retry are removed. Kept here as history.

- **Rule:** If Claude's named site is rejected, the search runs once more, telling Claude which site was ruled out and that it is probably a different product with the same name. A second failure ends in the R1 message.
- **The retry changes its input (revised after Alexa failed again):** because temperature is 0 (R7), the same input gets the same answer, and the plain retry note let Claude name `alexa.com` a second time. The retry now (1) rules the rejected domain out by name, (2) tells Claude to run a specific, different search ("[product] AI assistant official site"), (3) lists the sites the first search actually returned, since the official site is often named in those pages' text, and (4) says to answer "no site found" rather than guess.
- **Why:** "Alexa" first returned `alexa.com` (Alexa Internet, a retired web-traffic site), not Amazon's assistant.
- **Limit:** Still one retry. If Claude names the same wrong site again, the lookup fails with the R1 message.

### R9. Official site first, open search fills the gaps, every answer labeled

> **Superseded by R13 and R14 (Oct 9, 2026):** the official-site read described here was removed, and so was the rest of the official-site machinery. Kept here as history.
- **Order of authority for each marker and behavior:** official site, then other web source, then Claude's general knowledge.
- **Official-site pass:** A search limited to the official domain (then the company domain if needed), up to two searches, told to use **only** what those pages say and to leave a field blank if the site doesn't state it.
- **Open search:** Fills every field the official site left blank.
- **When an official answer counts:** the field has a value **and** a code other than "unknown." Official tasks replace the open search's tasks if any are returned (still capped at three).
- **Labels shown next to each answer:** *Official site*, *Other source*, *Claude's general knowledge*, or *Source not specified*.
- **Pages read:** The official pages the search returned are listed above the answers. If none were readable, the page says so and every answer is from other sources or general knowledge.
- **Limits:**
  - A discarded official-site read (site rejected by checking) cannot be cancelled mid-request in the chat preview, so its searches are still spent; its result is ignored.
  - The split between *Other source* and *general knowledge* is **Claude's own report** of where an answer came from; the page cannot verify it. *Official site* is the more reliable label, because that search can only return that site's pages.
  - Official sites are mostly marketing pages and often won't state voice pitch or tone, so many answers will legitimately carry the non-official labels.
  - Designer, organizational-diversity and adaptiveness fields still come from the open search; the official-site pass does not cover them.
  - Labels do not carry over into the editable sheet ("Edit these answers").
- **When the official-site read fails, the page says why** (added after Siri showed "Nothing readable"): a line under the official link lists the reason per domain tried: request rejected (with the HTTP status and message), no pages returned from the domain, the search tool returned an error, pages found but the reply was unusable or cut off. If a reply is cut off partway, whatever fields were complete are still used. The official-site reply limit was raised from 1,200 to 2,500 tokens, and Claude is told to reply with only the JSON.

---

## Open items and known issues

0. **Siri's official-site read returned "Nothing readable" in live use** (Oct 8, 2026). Cause found via the new "Why:" line: "AbortSignal object could not be cloned", a bug I introduced (change 13, fixed). Not yet re-confirmed in live use after the fix. Lesson for any future change: in the chat preview, requests must carry only plain method/headers/body; no abort signals or other non-plain objects. **Resolved Oct 9, 2026:** the official-site read no longer exists (R13). The lesson stands.
1. **Nothing has been run against live searches by me.** All my tests used simulated responses. The live behavior you reported: Siri was rejected (the first search returned only third-party pages), Alexa was rejected (Claude named `alexa.com`), and Siri's voice varied between runs. Each led to a rule above. Results with the latest version have not been confirmed. Still true for the rebuilt page.
2. **The published link is out of date.** The shared link (https://claude.ai/artifact/TwmXJdHrR3XsnrXMPCE5cV) was last republished after R1 and R2. Everything from R3 onward exists only in the version presented in the chat. A published page also cannot run live web searches, so the lookup cannot work at that link. The latest version has to be tested in the chat preview or hosted elsewhere (for example, your own site with a small server in front of the API). The main page file has since been rebuilt (R14) but not republished at that link.
3. **The page's methodology text is outdated.** It says a keyword check turns Claude's note into the dropdown choice in Option A. The code no longer does that; Claude returns the code directly. Not yet corrected. **Resolved Oct 9, 2026:** the rebuilt page's text now says Claude returns each selection directly, with a reason and a source tag.
4. **Redirects are not followed.** The tool links to the address Claude named and does not check where it leads. **No longer applicable (R14):** no link is shown.
5. **Run-to-run variation is reduced, not eliminated.** See R7 and R9 limits.
6. **Path B ("Design your own AI agent") was not touched.**
7. **The R13 removal is only in the variation-test copy.** The main page still has the official-site read until the same edit is made there. **Resolved Oct 9, 2026:** the main page was rebuilt.
8. **Pronouns:** whether the she/her entries contradict the products' own style guides is unchecked. Removing the read does not settle it.
9. **Marker score treats "Varies" / "none" as a known zero,** so Alexa's marker score is capped at 50 while voice and imagery are neutral-coded.
10. **Adaptiveness** may be saying "adaptive" by default on weak evidence. See item 21.
11. **Task coding wobble** remains the main source of behavior-score variation.
12. **The test panel** was simplified to repeated lookups with cost counters; the ON/OFF comparison no longer exists in this copy.
13. **Pronouns flipping between "object" and "unknown"** changes the marker score by one reading's worth (4 of 12 runs in the ON vs OFF check test). **Update Oct 9, 2026:** in the re-measure of the rebuilt page it was the only cause of marker-score differences: 'unknown' in 6 of 10 runs (Siri 3 of 5, Alexa 3 of 5), always on wording that mixed the maker's usage with common usage (for example "it in official copy; colloquially she/her"). The score moves because 'unknown' drops out of the denominator (25 to 33 for Siri, 50 to 33 for Alexa). **Update (voice-rule test, Oct 12):** Siri's pronouns were "unknown" in 4 of 5 runs; Alexa's were "fem" in 5 of 5 (earlier "unknown" in 3 of 5).
14. **The same task can get a different code depending on the prompt wording** ("Smart home control": masculine in 3 of 3 Alexa ON runs, neutral in 3 of 3 OFF runs). **Update (voice-rule test):** "smart home control" was masculine 3 times and neutral once for Alexa.
15. **The official link can differ between runs** (three different Alexa pages in three ON runs), so it works as an identity check but is not reliably one canonical page.
16. **R14, R15 and the revised entries (R2, R5, R6, R7, R8, R10, R12) describe the intended main page.** The main page has not been rebuilt without the official-site requirement yet, so until then it still behaves as R1, R3 and R4 describe. **Resolved Oct 9, 2026:** the main page was rebuilt without the official-site requirement.
17. **The test copies no longer match the main page.** `variation-test.html` and `variation-test-gate.html` were built from earlier versions. There is no test copy of the rebuilt page yet, so its numbers have not been re-measured. **Update Oct 9, 2026:** `variation-test-rebuilt.html` now exists. Apart from the test panel and harness it is identical to the rebuilt main page (checked byte for byte), so it measures exactly what ships. Re-measure result: not yet run.
18. **The rebuilt page was run live by you on Oct 9, 2026** (10 lookups, Siri and Alexa): all completed, one call and one search each, no errors. I still have no live access myself, so I cannot run it.
19. **Untested: does the official-site-aimed query (without the check, retry or link) steady the pronoun reading?** The check ON vs OFF test changed the requirement and the query wording together, so the two cannot be separated. A clean test would keep the query and instruction ("[product] [maker] official site", prefer the maker's own pages) but drop the check, confirmation, retry and link.
20. **The test panel groups tasks by exact wording,** so near-duplicates ("systemwide app actions (drafting emails, editing photos)" and "systemwide app actions and drafting") count as different tasks and the per-task table understates coding differences.
21. **Adaptiveness has no definition in the prompt.** The page asks "Does its manner shift based on how you respond to it?" (adaptive = "it visibly adapts to feedback"), but the instruction to Claude only lists the three codes. Claude's notes use different meanings: personalizing by recognizing voices (Bixby, 5 of 5 "adaptive"), user-selectable voices or personalities (Alexa), changes in gender presentation (Claude, 4 "nonadaptive", 1 "unclear" for the same evidence). This also bears on item 10. **Update:** GitHub Copilot read "adaptive" in 2 of 5 runs because users can choose between AI models (not a manner shifting with feedback), and "nonadaptive" in 3. **Update (voice-rule test):** Alexa's adaptiveness was "unclear" once in 5 runs ("personalization noted but gender-adaptive voice not confirmed"), which changed that run's pathway to "unclear".
22. **The tone code flips between "feminine" and "mixed" on near-identical wording** (Bixby: 3 feminine, 2 mixed; Claude: 2 feminine, 3 mixed; for example "Thoughtful, nuanced, honest, safety-focused" was coded feminine in one run and "thoughtful, honest, nuanced, safety-focused" mixed in another). For Claude it decides the final result. **Update (final check):** Siri's tone was coded "mixed" once (feminine in 4), which changed its result from aligned to unclear; Claude's tone was "mixed" in all 5 runs. **Update (voice-rule second set):** Gemini's tone was coded feminine 3 times, neutral once and mixed once (earlier mixed in all 5), moving its behavior score from 25 to between 0 and 50.
23. **The name "Claude" was coded masculine in 4 runs and neutral in 1.** One neutral run changed the marker score from -25 to 0 and the placement from masculine to neutral. **Update (live run, from a screenshot):** the name "Claude" was coded "unknown" (label "Not sure / haven't checked") with the reason "Used for men and women; ambiguous in convention." By the page's own label, "ambiguous" is what the "neutral" code means ("Neutral or ambiguous"), so the code and the reason disagree. The name now has three codings across six runs: masculine (4), neutral (1), unknown (1). The instruction does not say when to use "neutral" and when "unknown." A possible fix, not made (research call, would change numbers): a name used for both men and women, or with an ambiguous convention, is "neutral"; "unknown" only when the name cannot be assessed. **Update (final check):** in the final stability check the dominant reading flipped between sessions (neutral 4, masculine 1; earlier masculine 4, neutral 1).
24. **The voice label for code "none" reads "Varies — no fixed voice, or user picks one" even for a product with no voice at all** (Claude is text-only), which is misleading on screen. Scoring is unaffected (both count as neutral).
25. **The final result is steady for Siri and Alexa (22 of 22 lookups) but not for Claude.** Siri and Alexa were easy cases (feminine name, voice that varies, feminine-leaning tasks and tone). It also varies for GitHub Copilot (item 30).
26. **"Not described" is coded two ways.** Dots' voice was "unknown" in 4 runs and "none" in 1 for the same evidence ("voice calls supported; no default voice described"). "Unknown" drops out of the score's denominator and "none" counts as a known zero. No effect on Dots (everything else was neutral), but it would move the score for a product with a feminine or masculine name. Same family as the pronoun flips (item 13).
27. **Products whose markers are all neutral-coded always get a neutral placement and an "unclear" alignment** (Bixby, Dots, Gemini), however the behavior reads. "It," a voice that varies and an abstract avatar are folded into neutral (known zeros), so the placement is "neutral," not "insufficient information." Whether that is the intended treatment is a question for the research team.
28. **Output can differ when the input appears to be the same.** Gemini's input token count was identical (21,468) in all 5 runs, which suggests the search returned the same results, yet adaptiveness read "adaptive" 4 times and "unclear" once, at temperature 0. The variation warning (R11) says results depend on which pages the search brings back; that is not the whole story. Not proven (identical token counts are an indirect sign). **Update:** DeepSeek's input was 21,764 tokens in all 5 runs, again suggesting the same search results, with different wording and one different tone coding.
29. **Imagery for a logo-only product is coded from the model's memory of the logo,** which it cannot see. GitHub Copilot's logo was described as "angular, helmet/visor" (coded masculine, 2 runs) and as "circular" or "geometric" (coded neutral, 3 runs); every imagery answer was tagged "knowledge." The code follows the description, and the description varies. **Update (no-voice check):** DeepSeek's logo was coded masculine in 1 of 3 runs ("cool-colored"), where it had been neutral in all 5 earlier runs, so the flip is not limited to Copilot.
30. **The final result sits on a threshold.** The placement rule calls a placement masculine or feminine when one side has more readings than the other, so a single borderline reading (Claude's name or tone, Copilot's imagery) moves a neutral placement to a directional one and the result flips between "unclear" and aligned or misaligned. Whether a single reading should be enough is a question for the research team. Related to items 22, 23, 27 and 29.
31. **When a lookup fails, the page switches to the manual sheet and hides the reason.** The message ("Couldn't complete that search (...)") is written into the search panel, which is hidden once the page is in manual mode, so the person sees only the manual sheet and cannot tell why (a usage limit, a dropped connection and a parsing problem all look the same). The original page behaved the same way. The test copies show the message in their results table. **Resolved Oct 9, 2026 by R16** (provisional).
32. **On Oct 9, 2026, lookups briefly fell back to the manual sheet** after heavy testing, with the reason hidden (item 31). Searches worked again later with no change to how the page searches (the only change was R16, which only displays errors). Cause not confirmed: a usage limit, a connection problem or a service problem. If it recurs, R16's notice will show the reason.
33. **Public distribution (Oct 9, 2026). Corrected after the user supplied a working example.** An earlier version of this item said a public link where visitors use their own Claude accounts "does not appear to be possible." That was wrong. The user supplied a public link in the older Publish format (`claude.ai/public/artifacts/<id>`) that they say ran searches. (Only the empty page shell could be loaded from here, so the contents were not verified.) Anthropic's help page describes this route: a published chat artifact opens for anyone without a Claude account, visitors are asked to sign in only for features that use Claude, and those features count toward their own usage limits. The limits are on which accounts can do it: the Publish button exists only for artifacts made in the previous chat experience (a free account tested on Oct 9 showed only a personal "Only you" link), and in the newer Share dialog, artifacts that use Claude cannot use "Anyone with the link." Plan: publish the rebuilt `index.html` from the account and setup that has the Publish button (new chat, attach the file, "Show this HTML file as an artifact exactly as it is," AI-powered artifacts on, then Publish), and test the link in a private window. Cautions: unpublishing is permanent for that artifact, and a third-party guide says a published link cannot be updated in place (not confirmed in Anthropic's pages). Fallbacks if no Publish button is available: a small server holding an API key (the owner pays and must guard against misuse), each visitor pasting their own API key, or a public page with Option B only. Decision pending. **Update:** the working example was made from a collaborator's Pro account, which has the Publish button. Plan: the collaborator publishes the rebuilt `index.html` (SHA-256 `abd71026ffae958ae908a572ecba6493b4766a1b456f68e87d362e4a6dfff707`) from that account. If a fresh chat shows no Publish button there, try the original chat that holds the earlier published artifact (not confirmed to work). The link is owned by the collaborator's account: if they unpublish or delete it, it stops working, so record who owns the link. **Update (published artifact, Oct 9, 2026):** a ported copy, `index-artifact.html` (SHA-256 `28622e5037e6518540a31dee178526de6b6cead7ea3da6c5dab5400ddf1ea42b`), was published as a Claude artifact at https://claude.ai/artifact/DMozabpMCHwhHmo841FyMJ. `index.html` is unchanged. A published page cannot call the Claude API or search the web, so both Claude calls (the lookup and the manual-mode classifier) were moved to the artifact's built-in "ask Claude" capability (`sample`, declared at publish), which runs on each viewer's own Claude account. What changed: lookups have no web search (answers come from Claude's general knowledge, every answer is tagged as such, and no source links are shown); temperature 0 cannot be set (the capability exposes no temperature); the one-retry on a dropped connection was removed (the platform reports its own errors); the lookup prompt now says Claude cannot search and must set all source fields to null; the failure notice understands the platform's error codes; and four passages that described a web search (the Option A card, the privacy note, the "What actually happens" note and the results legend) were reworded. Because there is no search, scores from this version are not comparable with the stability tests above, which all used a web search. Not verified: whether a visitor on a public link (as opposed to a signed-in viewer) can use the capability, and whose usage it counts against; the capability's documentation says it spends the viewer's own Claude usage after a consent prompt, and that a page opened outside Claude cannot use it. Tested only against a stand-in for the capability, not live.
35. **Static "genuinely" text (resolved for the output on Oct 9, 2026).** The label "Genuinely spans both" for the "mixed" code appeared in the task chip in the results panel (and the placement breakdown), the task dropdown in Path A (line 949) and the task dropdown in Path B (line 1079); the worked example's reason (line 1938) also used it. After the user clarified that the scope is the output that rates how feminine or masculine something is, all four were changed ("Spans both"; the example reason now starts "Spans..."), verified by rendering before and after. Five explanatory notes (lines 754, 847, 1059, 1302, 1310) still use the word and were left alone as page text, not output. Note: only task rows use this label; the tone chip and both tone dropdowns read "Blends both depending on context." Correction: this item previously called line 1079 a tone dropdown; it is Path B's task dropdown, so both paths now match, which also answers the concern in the repo's 2026-10-06 changelog entry (changing the label on one side only would reopen drift).
36. **The repo's README was out of date.** Resolved Oct 12, 2026: the file statistics (2,935 lines, about 179 KB) and the token estimate were updated, the count of Claude-backed features was corrected from two to three, and the design studio's dropdowns are now named as the part that works without Claude (12 factual edits, listed in the changelog). The README's deployment section otherwise stands.
37. **The authors' names remain in places outside the visible page of `index.html`.** Left on purpose: code comments in the script, and the git history (commit authors and email addresses, and the account handles in the repo and Pages URLs). Not addressed, because only the submitted page was in scope: the repo's `README.md` (it names both authors, their affiliations and their emails), `old/index.html`, older changelog entries, the artifact copy and the test pages built from earlier versions, and the published artifact made from the artifact copy. Kept on purpose, and they still identify the authors: the inline citation and the working-paper reference entry. If the repo or any of those copies is shared with reviewers, those matter.
38. **The 2026-10-09 changelog entry pointed to `docs/decision-rules-log.md`, but the log is in the repo root.** Corrected in the changelog to `decision-rules-log.md` on 2026-10-12.
39. **Whether Claude finds the same default voice each time: answered for the products tested.** Across 26 lookups, each of six products was coded the same way in every run, with no outdated default chosen: Siri, Bixby and Gemini as "varies," Alexa as its feminine default, Copilot and DeepSeek as "none" (no voice). Still untested: a clear-default product other than Alexa, and products where the default changed recently.
40. **The design studio's readings: steady in one live session, not yet across days.** On Oct 12 the job, name and avatar calls were run live (90 calls, 6 jobs, 6 names, 5 runs each): job and name readings were the same in every run, and the avatar reading in 29 of 30 runs (see Test results). Still untested: a different day (earlier Path A lookups flipped between sessions), harder or more borderline jobs and names, and the page's own flows against real Claude.
41. **The opening notice now says Claude reads some of what you type.** Path A's manual mode already worked this way. The wording needs the authors' check for accuracy; the opening notice's existing privacy paragraph is otherwise unchanged.
42. **Should the avatar proposal steer?** Right now it does not: Claude proposes one avatar for the job, with no instruction to match or avoid the job's stereotype. Whether it should steer, toward or away from the stereotype, is for the research team (see R19). Evidence from the live test: the unsteered proposal read the same as the job in 24 of 30 runs and neutral in the other 6, and never the opposite.
43. **Should a blank tone or task count as neutral, or be left out of the score?** Right now a blank counts as the old default (neutral for tone, tasks and the job; "not sure" for markers), so scores match the original exactly. Leaving a blank tone or task out would change Path A's scores for sheets with an untouched reading (one feminine job with the tone untouched: +50 now, +100 if left out). That is for the research team, and ties to the existing question of whether a missing reading counts as dropped or as zero (R21).
44. **What should the avatar proposal be based on?** Right now it is based on the job text alone. The job is the anchor of the design brief (section A: "the markers below are judged against this"), and a single input keeps the avatar as simple as the other readings. It does not use the name, manner, pronouns or voice. Using them would make the avatar fit the rest of the character, but it could steer by another route (matching or contrasting with a masculine or feminine name, for example) and would mean re-proposing whenever those fields change. This is for the research team to decide, together with open item 42 (see R19).
45. **Each Claude reading costs about 4,500 input tokens here, far more than the request's own size.** In the live test every job, name and avatar call reported about 4,500 input tokens (21 to 55 output), against roughly 150 to 300 tokens for the request itself. The difference is probably overhead added where the call is made (the artifact sandbox), but that is not confirmed. The same reading function serves Path A's manual mode, so each field there probably costs about the same (not measured), and a design-studio job, name and avatar costs about 13,600 input tokens. Earlier cost estimates for these calls were too low.

---

## Test results

### Variation test: Siri, 10 runs (Oct 8, 2026)
Run in the chat preview with the variation-test copy of the page, after the signal fix (change 13). All 10 runs completed; the official site's pages were read in 10 of 10; official URL was https://www.apple.com/siri/ every time.

- **Marker score: 25 in all 10 runs.** Name feminine-coded; voice, imagery and pronouns all neutral-coded. Name, voice and imagery were tagged "Official site" every run; pronouns came from general knowledge in 8 runs and the official site in 2.
- **Behavior score varied: 25 (x7), 0 (x1), 50 (x1), -50 (x1).** Task codes (in order): mixed,fem,masc (x5); neutral,fem,masc (x2); neutral,fem,neutral (x1); mixed,mixed,masc (x1); mixed,masc,masc (x1). Tone was feminine-coded in 9 runs, neutral in 1 (inferred: the -50 run).
- **Labels people read:** 8 of 10 runs "Leans feminine-coded", 1 "Neutral / balanced", 1 "Leans masculine-coded". Markers were "Leans feminine-coded" in all 10.
- **Final result (derived by applying the page's own rules to the pasted counts; not observed directly):** the page's placement is "feminine" in all 10 runs (one feminine-coded marker, none masculine). "Behavior reads as" comes from the *sum* of task and tone codes, with "mixed" and "neutral" both counting zero, and gives feminine in 8 runs, neutral in 1, masculine in 1. So the markers-vs-behavior result would be aligned in 8 runs, unclear in 1, misaligned in 1, and the pathway would follow, provided the adaptiveness reading stayed the same. The first test did not capture adaptiveness.
- **Why behaviors varied (diagnosis, not tested further):** Claude chooses which three tasks to list on each run (8 different task lists in 10 runs) and codes each task in the same step. Temperature 0 did not make runs identical.
- **Limits of the test:** one product; compares scores only, not the final placement or alignment label; a neutral reading can hide different underlying values (for example "varies" vs "androgynous" voice) because the test shows codes, not wording.
- **Step size:** with three tasks plus tone, each changed reading moves the behavior score by 25 points; with four marker inputs, a marker score moves by 25 points per reading.
- **Options discussed, none built yet:** code each task with the fixed per-field check after Claude chooses the tasks; instruct Claude to take the three most prominent tasks in the page's order; add raw wording and the final placement label to the test; rely on the variation warning (R11).

### Variation test: Alexa, 10 runs (Oct 8, 2026), first version of the test (table only)
Only the per-run table was pasted: no summary, wording, source tags or adaptiveness, so those are unknown.

- **Marker score: 25 in 9 runs, 50 in 1.** The 50 (run 1) came from the voice reading feminine that run; voice was neutral-coded in the other nine. Marker label was "Leans feminine-coded" in all 10.
- **Behavior score: 25 (x5), 0 (x3), 50 (x1), -25 (x1).** Labels: leans feminine (x6), neutral (x3), leans masculine (x1).
- **Task code patterns:** neutral,fem,neutral (x5); neutral,mixed,neutral (x2); masc,fem,neutral (x1); neutral,fem,fem (x1); neutral,masc,neutral (x1). Tone alternated between "neutral" and "mixed" (5 each); both count zero, so the flip did not move the score.
- **Official site reads:** 3 pages read in 8 runs (URL .../about), 1 page read in 2 runs (root URL). The two 1-page runs were the most unusual ones (the only marker score of 50, the only negative behavior score). Small sample; a possible link, not shown.
- **Final result (derived from the page's rules, not observed; assumes adaptiveness constant):** placement feminine in all 10; behavior reads feminine 6, neutral 3, masculine 1; so aligned 6, unclear 3, misaligned 1. Compare Siri: aligned 8, unclear 1, misaligned 1 (also derived).
- **Pattern across both products:** markers are steady, behaviors are not, and the behavior swings change the final aligned/unclear/misaligned result in 2 of 10 (Siri) and 4 of 10 (Alexa) runs.
- **Still unknown (needs the detailed test):** whether the same task gets coded differently from run to run, or Claude chooses different tasks each time.

### Variation test: Siri, detailed version (Oct 8, 2026), table only, 7 complete runs plus a cut-off 8th
The paste ended partway through run 8 (tone and tasks missing) and did not include the summary section, so source tags, task-by-task counts and pathway counts are unknown. Scores for runs 1-7 were checked against the page's math.

- **Markers: 25 in all 8 runs; placement feminine in all.** Voice read "Varies - user picks voice, pitch, ..." with code "none" in 8 of 8 (R10 working). Pronouns "it" with code "object" in 8 of 8. Imagery (animated orb, no humanlike avatar) was coded "neutral" in 6 runs and "none" in 2: the raw code flipped for the same description, with no effect on the score because both fold into neutral.
- **Behaviors: 25 (x6), 50 (x1), 75 (x1); behavior read feminine and alignment "aligned" in all 8.** Tone fem in the 7 runs visible.
- **What varies is which tasks are chosen, not how a given task is coded (7 complete runs):**
  - Slot 1 ("everyday tasks via voice (calls, texts, reminders)"): same task every run, coded mixed (x3) or neutral (x4). Both count zero, so no score effect.
  - Slot 2 (communication / messaging / calls): fem in 7 of 7.
  - Slot 3 changed task from run to run: navigation or device control (runs 1, 5), information lookup or brainstorming (runs 2, 3, 6, 7), writing assistance (run 4). Coding followed the task type: navigation and Q&A masculine-coded, writing feminine-coded. Run 4's 75 came from choosing writing as the third task, which overlaps with slot 2 and so counts feminine twice.
  - Tasks overlap in most runs (slot 1 and slot 2 both list calls, texts, reminders), so the three scored tasks are not three distinct tasks.
- **Adaptiveness flipped: adaptive in 6 runs, nonadaptive in 2 (runs 1 and 5).** This reading comes from the open search, not the official-site read, and was not captured by the first test. Because the pathway depends on it, 2 of 8 runs would route to "aligned-nonadaptive" instead of "aligned-adaptive" even though the scores agree.
- **Compared with the first Siri test (same product, same day):** this run was steadier (no negative behavior score, all aligned). Sessions differ from each other as well as runs within a session.
- **Decision made:** task choice made repeatable (R12). Adaptiveness reasoning is now captured in the test. Coding each task separately (option 1) was dropped, since a given task type was coded consistently.

### Open question: is the official-site read worth its token cost?
- **Stated goal:** keep the scores reasonably stable.
- **What the data says so far:** every test ran with the official read ON, and markers were steady while behaviors were not; the behavior swings came from which tasks Claude picked, which the official read did not prevent. We have never run it OFF, so we cannot say whether it helped stability. It does provide source tags and a pages-read list (provenance).
- **Test built:** a comparison copy of the page that alternates ON and OFF (OFF skips only the step that reads the official site's own pages; the official site is still found, confirmed and linked), and counts calls, searches and tokens per lookup. Plan: 3 lookups per side, then decide.
- **Result:** not yet run.

**Status update for "Open question: is the official-site read worth its token cost?":** Run. Result: same verdicts at about three times the cost, so the read was removed (R13).

### ON vs OFF: official-site read (Oct 9, 2026), Siri 3+3 and Alexa 6+6

"OFF" skipped only the read of the official site's pages. It still ran the general search and still found, confirmed and linked the official site.

**Cost per lookup (average)**

| | Siri ON | Siri OFF | Alexa ON | Alexa OFF |
| --- | --- | --- | --- | --- |
| API calls | 2 | 1 | 2 | 1 |
| Web searches | 3 | 1 | 3 (one run: 5) | 1 |
| Input tokens | ~61,600 | ~21,500 | ~65,900 (typical ~59,000; one run 100,318) | ~21,800 |
| Output tokens | ~1,470 | ~980 | ~1,460 | ~920 |

ON compared with OFF: Siri input tokens 2.87x, Alexa 3.02x (about 2.7x without the 100k run).

**Stability**
- **Alexa:** aligned-adaptive in all 12 runs (both sides). Marker score 50 in 10 of 12 runs; the exceptions were ON run 11 (25, pronouns coded "none") and OFF run 12 (33, pronouns "unknown").
- **Siri:** aligned-adaptive in 2 of 3 runs and unclear in 1, on both sides. ON had marker 25 every run and behavior 25, 25, 0. OFF had behavior 25 every run and marker 0, 25, 33.
- **Task lists:** Alexa ON gave 2 different lists in 6 runs (the same three tasks in 5 of 6); Alexa OFF gave 5 lists in 6 runs. Siri gave 3 lists in 3 runs on both sides.
- **Task coding still varied** even with identical wording: Alexa's "smart home helper" was coded masculine in 4 ON runs and neutral in 1, which moved behavior from 50 to 75.
- **Pronouns:** In Siri ON run 1 and Alexa ON run 11, the official read reported a style-guide statement against gendered pronouns, and pronouns were coded "none". Five of six Alexa ON runs instead said she/her from press usage ("knowledge"). Not verified against the pages. Alexa run 11 also returned "Alexa+" as the name.
- **Adaptiveness:** "adaptive" in all 18 runs (a few citing only user-selectable voices or accents). The earlier detailed Siri test had 2 of 8 runs nonadaptive.
- **Provenance:** ON tagged name, voice, imagery, tasks and tone "Official site"; OFF tagged nearly everything "knowledge" or "other".
- **Limits:** Two products, both easy cases (feminine name, "varies" voice, neutral imagery). A product whose verdict hinges on one pronoun or task call might behave differently.

### Open question: does aiming the search at the official site buy any stability? (Oct 9, 2026)
- **Prompted by:** R13 removed the official-site read, so the score now comes from a general web search plus Claude's knowledge. The goal is scores that stay as similar as possible from run to run.
- **What the official-site step still does:** an identity check (stops the wrong product being scored, for example `alexa.com`), the link shown at the top, and the "no official site, try again" gate (R1). In normal runs it adds nothing to the cost, but it also shapes the search (the query ends in "official site" and the instruction says to aim at the official page).
- **Test built:** `variation-test-gate.html`. For each product it alternates runs with the check ON (current behavior) and OFF (no official-site requirement, confirmation, retry or link; the query ends in "AI assistant"; everything else identical). Defaults: Siri and Alexa, 3 per side, so 12 lookups. The summary reports, for each score, how many runs share the most common value on each side and with both sides together, plus cost per lookup.
- **Known confound:** OFF changes two things at once, the official-site requirement and the wording of the search query, so a difference cannot be assigned to one or the other. Sample is small (3 per side per product).
- **Result (Oct 9, 2026): run, Siri 3+3 and Alexa 3+3. Decision (Oct 9, 2026): remove the official-site requirement (R14).** Details in "Official-site check ON vs OFF" below.

### Official-site check ON vs OFF (Oct 9, 2026), Siri 3+3 and Alexa 3+3, from `variation-test-gate.html`

ON = current behavior (search aimed at the official site, then confirmed and linked, with retry). OFF = no official-site requirement, confirmation, retry or link; query ends in "AI assistant". All 12 lookups completed. Scores checked against the page's math.

| Product | Check | Marker scores | Behavior scores |
| --- | --- | --- | --- |
| Siri | ON | 25, 25, 33 | 25, 25, 25 |
| Siri | OFF | 25, 33, 33 | 50, 25, 25 |
| Alexa | ON | 50, 50, 50 | 25, 25, 25 |
| Alexa | OFF | 50, 33, 50 | 50, 75, 50 |

- **Final result identical:** aligned-adaptive in all 12 lookups, both sides, both products.
- **ON was a little steadier on the numbers** (behavior identical in 6 of 6 ON runs; OFF ranged 25 to 75). Three runs per side: too few to call a difference.
- **Part of ON's steadiness may be reliance on general knowledge:** in all 3 Siri ON runs every field was tagged "knowledge" and the tasks were generic ("answer questions", "control device settings and apps"). In Siri OFF runs voice and tasks were tagged "other" and described newer features (surfacing info from messages, emails, photos; system-wide app actions). Steadier versus more current; not judged here. The tags are Claude's own report.
- **Pronouns flipped to "unknown" in 4 of 12 runs** (Siri ON run 5, Siri OFF runs 4 and 6, Alexa OFF run 10) after wording like "it / she (varies)". "Unknown" drops out of the score's denominator, so the marker score moved 25 to 33 (Siri) or 50 to 33 (Alexa). Happened on both sides.
- **Same task, different code by side:** "Smart home control" was coded masculine in all 3 Alexa ON runs and neutral in all 3 Alexa OFF runs. This accounts for most of Alexa's behavior gap (25 vs 50-75). Consistent within a side, so a shift caused by the prompt wording, not random noise.
- **Cost per lookup:** ON averaged 25,107 input and 955 output tokens (7 searches over 6 lookups); OFF 21,172 and 769 (1 search every lookup). ON was 1.19x OFF on input tokens. One ON lookup (Siri run 3) fired the confirmation call and cost 41,012 input tokens; without it, ON averaged about 21,926, roughly 3.6% above OFF.
- **Official link in ON runs:** Siri always `apple.com/siri`. Alexa gave three different pages in three runs (`alexa.amazon.com`, `alexa.amazon.com/about`, and an `amazon.com` store category page).
- **Limits:** small sample; OFF changes the official-site requirement and the query wording together, so effects cannot be separated; two easy products (feminine name, "varies" voice, neutral imagery).
- **Candidate levers named (not built, your decision):** a pronoun rule (when sources conflict, use the pronoun the product's own materials use; "unknown" only as a last resort); anchored coding for common task types (which code each type gets is a research decision).

### Re-measure of the rebuilt page (Oct 9, 2026)

- **Test:** `variation-test-rebuilt.html`. Repeated lookups of Siri and Alexa, rotating between them (Siri, Alexa, Siri, Alexa, ...), default 5 per product, so 10 lookups. Reports score and wording stability, pronoun codes and wording, how each task was coded each time it appeared, adaptiveness, the final result, and cost per lookup.
- **Compare against:** the earlier tests above. Reference points: Siri markers were 25 in 10 of 10 runs (read ON) and 25, 25, 33 / 25, 33, 33 in the check ON vs OFF test; Alexa markers were 50 in 10 of 12 runs (read ON or OFF) and 50, 50, 50 / 50, 33, 50 in the check ON vs OFF test; behavior scores varied in every earlier test; the final result was aligned-adaptive in all 12 lookups of the check ON vs OFF test.
- **Questions it answers:** whether the pronoun flips ("object" vs "unknown") and the task-coding differences ("Smart home control") still occur without the official-site wording, whether adaptiveness stays "adaptive," and whether the final result stays steady.
- **Limits:** small samples, run at a different time from the earlier tests (the search results can differ), and no baseline run without the task rules (R12), so their effect cannot be separated.
- **Result (Oct 9, 2026): run, Siri 5 and Alexa 5, rotating. All 10 completed; one call and one search each; 21,103 to 21,304 input tokens (average 21,181) and 661 to 927 output (average 746); 211,810 input and 7,462 output in total.** Scores below were checked against the page's math.

| Product | Marker scores | Behavior scores |
| --- | --- | --- |
| Siri | 25, 33, 33, 25, 33 | 50, 50, 50, 25, 50 |
| Alexa | 33, 33, 50, 50, 33 | 50, 50, 50, 75, 100 |

- **Final result identical:** aligned-adaptive in all 10 lookups (and 12 of 12 in the check ON vs OFF test). Marker label "Leans feminine-coded" in all 10; behavior label "Leans feminine-coded" in 8 and "Strongly feminine-coded" in 2 (Alexa 75 and 100).
- **Steady in all 10:** name feminine, voice "Varies" (code none), imagery neutral, tone feminine, adaptiveness "adaptive". The voice rule (R10) held again.
- **Marker differences came from pronouns alone.** Pronouns were "unknown" in 6 of 10 runs (Siri 3 of 5, Alexa 3 of 5), always on wording that mixed the maker's usage with common usage. Siri: 25 where pronouns were "object", 33 where "unknown". Alexa: 50 where "fem", 33 where "unknown". All pronoun answers were tagged "knowledge".
- **Behavior differences came from task coding and the third task.**
  - Siri: the same three tasks in 4 of 5 runs (answering questions; surfacing personal context; app actions). The one score of 25 came from "systemwide app actions and drafting" coded masculine (it was coded mixed in the other four runs).
  - Alexa: smart home control and shopping appeared in all 5 runs, but "smart home control" was coded masculine (2 runs), neutral (1), mixed (as "smart home control and reservations") and feminine (as "smart home control (lights, locks, thermostats)"). The third task varied (entertainment and meal planning; answering questions; family coordination). The 100 came from smart home coded feminine plus two other feminine tasks.
- **Task choice looks steadier than in the detailed Siri test, where the third task changed from run to run** (R12 may be helping, but there is no baseline without it).
- **Limits:** small samples; run at a different time from the earlier tests; the per-task table undercounts coding differences (open item 20).
- **What it changes:** the pronoun finding weakens the "no stability benefit" reasoning under R14 (see the note there and open item 19).


### Re-measure of the rebuilt page, second pair: Bixby 5 and Claude 5 (Oct 9, 2026)

All 10 completed; one call and one search each; 20,949 to 21,858 input tokens (average 21,424) and 519 to 650 output (average 576); 214,235 input and 5,762 output in total. Scores, alignment and pathway checked against the page's rules; all matched.

| Product | Marker scores | Behavior scores | Alignment | Pathway |
| --- | --- | --- | --- | --- |
| Bixby | 0, 0, 0, 0, 0 | 0, 0, -25, -25, 0 | unclear x5 | unclear x5 |
| Claude | -25, -25, 0, -25, -25 | 25, 0, 0, 0, 25 | misaligned x2, unclear x3 | misaligned-nonadaptive x2, unclear x3 |

- **Bixby:** name, voice, imagery and pronouns all neutral-coded, so the placement is neutral and the alignment is "unclear" in every run. Stable, but it cannot say more. The three tasks and their codes were identical in all 5 runs (device control masculine, reminders and scheduling feminine, information retrieval masculine). Behavior differed only through tone: feminine in 3 runs (score 0), mixed in 2 (score -25).
- **Claude:** pronouns "it" in all 5. Tasks and codes steady (writing and editing feminine, code and data masculine, research and documents mixed). Three things moved:
  - **Tone:** feminine in 2 runs (behavior reads feminine, so alignment is "misaligned" against a masculine placement) and mixed in 3 (behavior reads neutral, so "unclear").
  - **Name:** masculine in 4 runs, neutral in 1 (placement masculine vs neutral).
  - **Adaptiveness:** "nonadaptive" in 4, "unclear" in 1.
  Inferred from the pasted results: the two "misaligned-nonadaptive" runs are the two runs with a feminine tone; the "unclear" runs are the three with a mixed tone.
- **Held across both products:** voice "none" in all 10 (Bixby "Varies"; Claude "text-only, no voice"), imagery neutral or none, pronouns "object" in all 10 (usage was not mixed, so the "unknown" flip seen for Siri and Alexa did not occur).
- **Task coding was steady here** (0 of 12 tasks coded more than one way), unlike Alexa's "smart home control" and Siri's "app actions." The coding wobble seems to affect ambiguous task types, not clear ones.
- **Limits:** claims about adaptiveness reflect the model's own notes; "Claude" is also the model making the judgments, so that case is worth treating separately; 5 runs per product.

### Re-measure of the rebuilt page, third pair: OpenAI Dots 5 and Gemini 5 (Oct 9, 2026)

All 10 completed; one call and one search each; 21,468 to 22,638 input tokens (average 22,007) and 606 to 881 output (average 739); 220,066 input and 7,385 output in total. Scores, alignment and pathway checked against the page's rules; all matched.

| Product | Marker scores | Behavior scores | Alignment | Pathway |
| --- | --- | --- | --- | --- |
| OpenAI Dots | 0 x5 | -100, -50, -75, -50, -75 | unclear x5 | unclear x5 |
| Gemini | 0 x5 | 25 x5 | unclear x5 | unclear x5 |

- **Why "unclear" every time:** all markers neutral-coded for both products (neutral name, pronouns "it," abstract avatar, voice varies or not described), so the placement is neutral and alignment cannot be decided (item 27). The final result is steady but says little.
- **Dots (a new product; every answer was tagged "Other source"):** behavior varied through (a) the third task, "drafting documents and software fixes" (masculine, 2 runs) vs "managing projects and updating information" (mixed, 2 runs) vs "research and document drafting" (mixed, 1 run), and (b) tone, masculine in 2 runs and mixed in 3 on near-identical framing ("capable colleague/friend rather than deferential helper" vs "trusted colleague or personal friend"). The first task ("background task execution across 4,000+ apps," masculine) appeared in all 5 runs; 5 different task lists in 5 runs by wording. Voice was "unknown" in 4 runs and "none" in 1 (item 26). Pronouns "it or they (plural dots)" coded "object" in all 5.
- **Gemini:** tasks (mixed, feminine, mixed) and tone (mixed) identical in all 5 runs; marker 0 and behavior 25 in all 5. Only adaptiveness varied: "adaptive" in 4 (users choose a voice, connect Google apps) and "unclear" in 1 ("voice and tone can be adjusted but persona gender is not user-set"). Input tokens were 21,468 in all 5 runs (item 28). Same split as Claude's adaptiveness (item 21).
- **Limits:** 5 runs per product; two products; the neutral placement makes the final result insensitive to behavior.

### Re-measure of the rebuilt page, fourth pair: GitHub Copilot 5 and DeepSeek 5 (Oct 9, 2026)

All 10 completed; one call and one search each; 21,764 to 22,416 input tokens (average 22,064) and 567 to 821 output (average 657); 220,636 input and 6,570 output in total. Scores, alignment and pathway checked against the page's rules; all matched. No changes were made to the page or rules (pending the research team).

| Product | Marker scores | Behavior scores | Alignment | Pathway |
| --- | --- | --- | --- | --- |
| GitHub Copilot | 0, 0, -25, -25, 0 | -100 x5 | unclear x3, aligned x2 | unclear x3, aligned-nonadaptive x2 |
| DeepSeek | 0 x5 | -50 x4, -25 x1 | unclear x5 | unclear x5 |

- **GitHub Copilot:** tasks and codes steady (all masculine; behavior -100 in all 5), tone masculine in all 5. Name neutral, voice "none" (text-based, no voice), pronouns "it" in all 5. Imagery was the only marker that moved: masculine in runs 5 and 7 ("stylized dark helmet/visor icon, angular, cool-toned"; "angular lines, cool blue/purple tones"), neutral in runs 1, 3 and 9 ("circular, dark-themed"; "stylized geometric shape"). With a masculine imagery reading the marker score is -25, the placement is masculine, and with all-masculine behavior the alignment is "aligned" (pathway aligned-nonadaptive in both, as adaptiveness was "nonadaptive"); with neutral imagery the placement is neutral and the alignment "unclear." Adaptiveness: "adaptive" in 2 (users can choose between AI models), "nonadaptive" in 3 (item 21). All imagery answers tagged "knowledge" (item 29).
- **DeepSeek:** name neutral, voice none, imagery neutral (blue whale / abstract logo in all 5), pronouns "it" in all 5, adaptiveness "unclear" in all 5 ("no evidence found"). Tasks steady (coding masculine, content creation feminine, math and reasoning masculine; once "research and reasoning"). Tone masculine in 4 runs and mixed in 1 (the run where "described as helpful and clear" was added), which accounts for the single -25. Input tokens were 21,764 in all 5 runs (item 28).
- **Limits:** 5 runs per product; two products; imagery and adaptiveness findings rest on the model's own descriptions and notes.

### Final stability check on the final page: Siri 5 and Claude 5 (Oct 9, 2026)

Run on `variation-test-rebuilt.html` (identical to the final `index.html` apart from the test panel; SHA-256 of `index.html` begins `5b7d7f11`). All 10 lookups completed; one call and one search each; 20,991 to 21,346 input tokens (average 21,137) and 491 to 809 output (average 603); 211,366 input and 6,034 output in total. Scores, alignment and pathway checked against the page's rules; all matched. The word "genuine" appeared 0 times in Claude's text and 0 times on screen across the 10 lookups.

| Product | Marker scores | Behavior scores | Alignment | Pathway |
| --- | --- | --- | --- | --- |
| Siri | 33, 25, 33, 25, 33 | 50, 50, 25, 50, 0 | aligned x4, unclear x1 | aligned-adaptive x4, unclear x1 |
| Claude | 0, -25, 0, 0, 0 | 0 x5 | unclear x5 | unclear x5 |

- **Siri vs. the earlier Siri test on the rebuilt page:** the marker split is identical (33 in 3 runs, 25 in 2) and comes from pronouns coded "unknown" in 3 of 5 runs, as before. The final result was aligned-adaptive in 4 of 5 (5 of 5 before). The one "unclear" (run 9) had tone coded "mixed" instead of feminine and the third task coded masculine, so the behavior score was 0. "Systemwide app actions" was coded neutral (once), mixed (twice) and masculine (twice) depending on its wording; the test page groups tasks by exact wording, so its "0 of 7 coded more than one way" understates this (open item 20).
- **Claude vs. the earlier Claude test:** the dominant readings flipped between sessions. Name: neutral 4, masculine 1 (earlier masculine 4, neutral 1; one live run returned "unknown"). Tone: mixed 5 (earlier feminine 2, mixed 3). Result: "unclear" 5 (earlier unclear 3, misaligned-nonadaptive 2). Adaptiveness: unclear 3, nonadaptive 2 (earlier nonadaptive 4, unclear 1); it did not change the result here because the placement was neutral. The steadier result should not be read as the cause being fixed: each outcome still turns on one borderline reading (items 22, 23, 21).
- **Overall:** the final result held in 9 of 10 lookups; the remaining variation comes from the causes already on the research decision list (pronouns "unknown," tone coding, the name "Claude," adaptiveness). No regression was seen from the Oct 9 changes (failure notice, "genuinely" instruction and labels): same cost, no errors, same marker split for Siri.
- **Limits:** 5 runs per product; two products; run in one session, so differences between sessions are not covered by one run.

### Voice rule re-measure (Oct 12, 2026)

- **Test:** `variation-test-rebuilt.html`, Siri and Alexa, 5 runs each (10 lookups), rotating. It reports the voice code and wording per product, the scores, the final result, the word check and the cost.
- **Compare against:** Siri's earlier voice reading was "none" (varies) in every run, with markers of 25 or 33 and aligned-adaptive in 4 of 5 to 5 of 5 runs. Alexa's was "none" in every run, with markers of 33 or 50, behavior 50 to 100, and aligned-adaptive in all runs.
- **What to look for:** Siri should stay "none." Alexa should now be coded on its default voice (feminine), which would raise its marker score. The wording should say other voices exist. Watch for a different default being chosen from run to run, and for an outdated default.
- **Result (Oct 12, 2026): run, Siri 5 and Alexa 5, rotating. All 10 completed; one call and one search each; 21,171 to 21,303 input tokens (average 21,268) and 607 to 837 output (average 722); 212,676 input and 7,215 output in total.** Scores, alignment and pathway checked against the page's rules; all matched. The word "genuine" appeared 0 times in Claude's text and on screen.

| Product | Voice code and wording | Marker scores | Behavior scores | Final result |
| --- | --- | --- | --- | --- |
| Siri | "none" x5: "Varies — user selects voice at setup; multiple voices across genders and accents available" | 33 x4, 25 x1 | 50 x4, 25 x1 | aligned-adaptive x5 |
| Alexa | feminine x5: "Higher-pitched, warm female-presenting default; other voices available" | 75 x5 | 50 x3, 75 x2 | aligned-adaptive x4, unclear x1 |

- **The rule behaved as intended on both sides.** Siri stayed "varies" in all 5 runs and the wording matches the real situation (the user chooses at setup); no outdated female default was picked. Alexa was coded on its default voice in all 5 runs, and every wording said other voices exist. No different default was chosen from run to run.
- **Alexa's marker score rose from 33 or 50 to 75, as expected.** Part of the steadier result is not the voice rule: Alexa's pronouns were "fem" in all 5 runs this time (earlier tests had "unknown" in 3 of 5), and the voice rule does not affect pronouns. The rule explains the rise, not the steadiness.
- **Siri's marker split is still the pronoun reading:** "unknown" in 4 of 5 runs (score 33), "object" in 1 (score 25).
- **The one result that was not aligned-adaptive** was Alexa run 8, where adaptiveness read "unclear" ("Personalization noted but gender-adaptive voice not confirmed"); see open item 21.
- **Alexa's "smart home control" was coded masculine 3 times and neutral once** (and neutral as "smart home control and reservations"), the same wobble as before (open item 14).
- **Source of the default:** Alexa's voice was tagged "knowledge" in all 5 runs, so its default voice came from Claude's general knowledge, not from a page the search returned. Siri's voice was tagged "other" in 3 runs and "knowledge" in 2.
- **Not tested:** Bixby (default probably feminine, so its placement may move from neutral to feminine), products with several selectable voices (for example Gemini), products with no voice, and whether an outdated default is picked for some other product.
- **Limits:** two products, 5 runs each, one session.


### Voice rule re-measure, second set: Bixby 5 and Gemini 5 (Oct 12, 2026)

- **Why these two:** each tests a case the first set did not. Bixby's earlier notes said its default tends toward a higher-pitched female voice in English but that voices vary by language and region, so it sits on the line between "one default" and "differs by language." Gemini offers selectable voices. Both are the untested cases listed under open item 39. Products with no voice are still untested.
- **Test:** `variation-test-rebuilt.html`, Bixby and Gemini, 5 runs each (10 lookups), rotating; voice code and wording shown per product.
- **Earlier baselines (voice read "none" under the Oct 9 rule):** Bixby, 5 runs: voice "none" in all 5; marker 0 in all 5 (every marker neutral-coded); behavior 0, 0, -25, -25, 0; final result "unclear" in all 5. Gemini, 5 runs: voice "none" in all 5 ("multiple voice options across languages and devices; Gemini Live offers selectable voices"); marker 0 in all 5; behavior 25 in all 5; final result "unclear" in all 5.
- **What could change:** if Claude finds one default for Bixby (feminine), its marker score becomes 25 and its placement feminine; its behavior reads neutral or masculine, so its result would move from "unclear" to "unclear" (neutral behavior) or "misaligned" (masculine behavior). If it says the voice differs by language, nothing changes. Gemini should stay "none" unless Claude finds one default.
- **What to look for:** the same voice code in every run; no outdated default; wording that says other voices exist when a default is coded.
- **Result (Oct 12, 2026): run, Bixby 5 and Gemini 5, rotating. All 10 completed; one call and one search each; 21,096 to 21,927 input tokens (average 21,464) and 591 to 807 output (average 687); 214,636 input and 6,871 output in total.** Scores, alignment and pathway checked against the page's rules; all matched. The word "genuine" appeared 0 times in Claude's text and on screen.

| Product | Voice code and wording | Marker scores | Behavior scores | Final result |
| --- | --- | --- | --- | --- |
| Bixby | "none" x5: "Varies - differs by language and region; multiple voice options available" (3), "...user picks voice at setup..." (1), "...no single fixed default" (1) | 0 x5 | -25 x4, 0 x1 | unclear x5 |
| Gemini | "none" x5: "Varies — user selects voice at setup; multiple voices offered..." | 0 x5 | 50 x3, 25 x1, 0 x1 | unclear x5 |

- **Claude found no single default for either product,** so neither was coded on a voice. For Bixby it judged that voices differ by language and region (once: the user picks at setup), although earlier notes had said its English default tends toward a higher-pitched female voice. The shift predicted for Bixby (placement from neutral to feminine) did not happen: marker 0 and "unclear" in all 5 runs, the same as before the rule changed. Gemini likewise stayed "none" in all 5 runs, as before. So far the "one default" branch has fired only for Alexa.
- **Consistent within each product:** the same voice code in all 5 runs, no outdated default, and the wording gave the reason it varies.
- **Gemini's behavior score changed from the earlier test (25 in all 5 runs) to 50 x3, 25 x1, 0 x1.** The cause is the tone code, not the voice rule: feminine in 3 runs, neutral in 1 and mixed in 1, where earlier it was mixed in all 5 (open item 22). It cannot change the result, because both products' placements are neutral.
- **Adaptiveness was "adaptive" in all 10 runs.**
- **Still untested:** a product with no voice, and a product with a clear default other than Alexa. Limits: two products, 5 runs each, one session.
- **Across both sets (20 lookups):** the voice code was the same in every run for each of the four products, Siri and Bixby and Gemini as "varies" and Alexa as its feminine default. The only products whose results changed because of the rule are the ones with a clear default (so far, Alexa).


### Voice rule re-measure, no-voice check: GitHub Copilot 3 and DeepSeek 3 (Oct 12, 2026)

- **Why:** the last untested case under open item 39. The rule's first clause ("if the product has one default voice...") should not apply to a product with no voice, which should keep the code "none."
- **Test:** `variation-test-rebuilt.html`, GitHub Copilot and DeepSeek, 3 runs each (6 lookups), rotating.
- **Earlier baselines (5 runs each, before the voice rule changed):** GitHub Copilot: voice "none" in all 5 ("no voice interface; text/code suggestions only"); markers 0 x3 and -25 x2 (imagery coded masculine in 2 runs); behavior -100 in all 5; final result "unclear" x3 and "aligned-nonadaptive" x2. DeepSeek: voice "none" in all 5 ("text-only chatbot, no voice output"); marker 0 in all 5; behavior -50 x4 and -25 x1; final result "unclear" in all 5.
- **What to look for:** the voice code stays "none" in every run, is not "unknown," and no voice is invented; the wording says there is no voice.
- **Result (Oct 12, 2026): run, GitHub Copilot 3 and DeepSeek 3, rotating. All 6 completed; one call and one search each; 22,006 to 22,723 input tokens (average 22,371) and 557 to 800 output (average 664); 134,225 input and 3,982 output in total.** Scores, alignment and pathway checked against the page's rules; all matched. The word "genuine" appeared 0 times in Claude's text and on screen.

| Product | Voice code and wording | Marker scores | Behavior scores | Final result |
| --- | --- | --- | --- | --- |
| GitHub Copilot | "none" x3: "No voice output; text-only coding assistant" / "No voice interface; text-only assistant" | 0 x2, -25 x1 | -100 x3 | unclear x2, aligned-nonadaptive x1 |
| DeepSeek | "none" x3: "No voice output; text-based chat only" (2), "...text-only assistant" (1) | 0 x2, -25 x1 | -25 x3 | unclear x3 |

- **The voice rule did not affect the no-voice case.** The code was "none" in all 6 runs, never "unknown," and no voice was invented; every wording said the product has no voice. This matches the earlier baselines for both products.
- **GitHub Copilot matched its baseline:** behavior -100 in all 3 runs; the marker moved to -25 in 1 run because its logo was coded masculine ("dark helmet/visor shape"), which also made that run "aligned-nonadaptive" instead of "unclear" (open item 29).
- **DeepSeek differed from its baseline in two ways, neither from the voice rule.** Its logo was coded masculine in 1 of 3 runs ("blue whale logo; abstract, cool-colored, non-human"), where it was neutral in all 5 earlier runs, moving the marker from 0 to -25 (open item 29). And its third task was "document reading and Q&A" (neutral), where earlier it was "math and logical reasoning" (masculine), so the behavior score was -25 in all 3 runs instead of mostly -50. The pathway stayed "unclear" in all 3 because adaptiveness was "unclear" every time.
- **Limits:** two products, 3 runs each, one session.
- **All four voice cases are now tested:** one default (Alexa: coded on the default), no single default (Siri, Bixby, Gemini: "varies"), and no voice (GitHub Copilot, DeepSeek: "none"), 26 lookups in all, with the voice code the same in every run for each product.


### Design studio steadiness test, live (Oct 12, 2026)

- **Test:** `design-studio-steadiness-test.html`, run in the chat preview against real Claude: 6 jobs, 6 names, 5 runs each, using the page's own reading and avatar functions at temperature 0. Job readings 30 calls, name readings 30, avatar proposals 30. **All 90 completed; none failed or needed a retry.** The word "genuine" appeared 0 times.

| What was tested | Same answer in every run | Detail |
| --- | --- | --- |
| Job readings | 6 of 6 jobs | scheduling and note-taking: feminine; financial modeling and analysis: masculine; customer support: feminine; smart home control: masculine; writing and editing content: feminine; generating code and visualizing data: masculine |
| Name readings | 6 of 6 names | Marcus: masculine; Sophia: feminine; Alex: neutral; Nova: feminine; Claude: neutral; Atlas: masculine |
| Avatar readings | 5 of 6 jobs (29 of 30 runs) | the exception was customer support: feminine in 4 runs, neutral in 1 |
| Avatar descriptions | identical in every run for 2 of 6 jobs | the others varied in wording: 2 distinct descriptions for scheduling and for code and data, 3 for financial modeling and for smart home control |

- **Steadier than Path A's lookups, but not a like-for-like comparison.** "Smart home control" read masculine in all 5 runs, where the Path A lookups coded it four different ways. "Claude" read neutral in all 5 runs, where the lookups gave masculine and neutral on different runs. These calls read one short typed field with a small prompt and no search, so far less can vary.
- **Without steering, the avatar mostly mirrored the job.** The avatar read the same as the job in 24 of 30 runs and neutral in the other 6 (customer support once, and writing and editing content in all 5, a fountain pen with "no strong gender pull"). It never read as the opposite. Feminine jobs got soft, rounded, warm imagery and masculine jobs got angular, cool, circuit or geometric imagery.
- **Caution on the avatar readings.** The prompt tells Claude how imagery reads (angular and cool-toned read masculine; soft, rounded and warm read feminine) and then asks it to propose an avatar and say how it reads. The proposal and its reading come from the same convention, so the reading is steady partly because Claude is reading its own design by the rule it was given. That steadiness is not evidence that the reading is right.
- **Proposals vary in wording even at temperature 0:** in 4 of 6 jobs the same job returned 2 or 3 different descriptions across 5 runs, although the most common one often repeated (scheduling returned the same description in 4 of 5 runs).
- **Cost, and a correction.** 408,575 input and 3,039 output tokens for 90 calls: about 4,500 input tokens per call (job 4,495; name 4,500; avatar 4,624) and 21 to 55 output. My estimate before the run was about 18,000 input in total (150 to 300 per call, from the size of the request). The real figure was about 20 times higher. See open item 45.
- **Limits:** one session; short, unambiguous inputs; 6 jobs and 6 names; 5 runs. It does not show steadiness across days, and it does not test the page's own flows against real Claude.

---

## Considered, not built

- A search limited **only** to the official domain, with no open-search fallback (judged too strict: most markers would come back "unknown").
- A switch letting the person choose "official site only" vs. "official site plus other sources."
- Running each judgment twice and flagging markers where the runs disagree.
- Remembering results per product so repeated searches return the same answer.
- Forcing the search query into a fixed form in code (R8 is an instruction only).
- Scoring only the default voice for products with several voices (R10 chose "varies" instead).
- Starting again from the original `index.html` (declined Oct 9, 2026): the original lacks nine changes that are being kept (temperature 0, the three-task limit, the voice rule, the task rules, the capitalization instruction, the variation warning, the source labels, the dropped-connection retry, and the AI-assistant assumption), so rebuilding from the test copy was less risky.
- Keeping the official-site check only as an identity guard and link, with no stability claim (declined; R14).
- Candidate levers for steadier scores, not built and undecided: a pronoun rule (when sources conflict, use the pronoun the product's own materials use; "unknown" only as a last resort), and anchored coding for common task types (which code each type gets is a research decision).
- Testing the official-site-aimed query without the check, retry or link, to see whether it steadies pronouns (open item 19). Not built.
- Defining adaptiveness in the instruction to Claude using the page's own question ("Does its manner shift based on how you respond to it?"), and adding definitions for the "mixed" and "neutral" tone codes. Not built; would change the numbers.

### Token-efficiency ideas (added Oct 9, 2026; undecided)

Not measured. Most of the ~21,500 input tokens per lookup is search results fed back to Claude, so these shift cost by amounts not yet known.

- Cap searches in the API call (`max_uses: 1`); today "at most one search" is only a prompt instruction, and ON runs exceeded it.
- Skip the confirmation call when the first search already backs up the named site.
- Cap or drop the retry, which repeats a full search.
- Shorten the JSON Claude returns (~920 output tokens): drop per-field reasons, the org-diversity and demographics fields, and the extra sources list.
- Cache results by product name within a session.
- Use a smaller model for Option B's per-field classification (needs a check that it works in this environment).
- Run fewer test lookups while tuning.

---

- **An avatar proposal aimed not to match the job's stereotype (built Oct 12, 2026, removed the same day).** Claude was told to propose imagery that would not reaffirm the job's stereotype (feminine-coded job: neutral or masculine-coded imagery; masculine-coded job: neutral or feminine-coded imagery; mixed or unmapped job: abstract, neutral imagery). It was removed because it was a steering rule chosen without the research team: in the design studio, sections A to C display what Claude outputs, and advice begins later (the callout in section C and the guidance after the prediction are the original authors'). The proposal now carries no steering. Whether it should steer is open item 42.
- **Readings that start blank and are left out of the score (tried Oct 12, 2026, removed the same day, then restored with the original scoring).** The first version left a blank out of the score. That changed Path A's scoring for an untouched tone or task reading (one feminine job with the tone untouched scored +50 originally and +100 with the change). The user decided not to change the original author's scoring, so it was removed. It was restored the same day with a blank counting exactly as the old default did, so scoring and results are unchanged (R21). Whether a blank should instead be left out is open item 43.

---

## Change history

| # | Change | Rule |
|---|---|---|
| 1 | Published the original page unchanged | n/a |
| 2 | Official-site requirement, "try again" message, scope note, top-three behaviors | R1, R2 |
| 3 | Site verification against search results | R3 |
| 4 | Case handling for pasted links and names | R6 |
| 5 | Confirmation search for sites missing from first results (Siri) | R3 |
| 6 | Temperature 0 on all calls; fixed query form | R7, R8 |
| 7 | Parent-domain fallback; failure message names the suggested site (Alexa) | R3, R1 |
| 8 | AI-assistant assumption and one automatic retry (alexa.com) | R4, R5 |
| 9 | Official-site pass, merge order, source labels, pages-read list | R9 |
| 10 | "Voice varies" rule | R10 |
| 11 | Official-site read runs in parallel with the confirmation check; one automatic retry if the connection drops. (A "cancel the read if the site is rejected" feature was also added here; see 13, it broke the read.) | R3, R9 |
| 12 | Official-site read reports why it failed; reply limit raised to 2,500 tokens | R9 |
| 13 | **Bug fix:** the cancellation signal added in 11 made every official-site read fail in the chat preview ("AbortSignal object could not be cloned"), so Siri showed all answers from general knowledge. Signal removed from the request. A rejected site's read now can't be cancelled mid-request; it is only flagged so it starts nothing further and its result is ignored. | R9 |
| 14 | Stronger retry input after Alexa failed again; scope-note wording edited ("but has no official website") | R4, R1 |
| 15 | Added the variation warning to the results panel | R11 |
| 16 | Variation test copy built; first 10-run Siri results recorded (see Test results) | R11 |
| 17 | Variation test copy rebuilt with per-run wording, raw codes, task-by-task coding consistency, adaptiveness and the final alignment/pathway | R11 |
| 18 | Alexa 10-run results recorded (first test version) | R11 |
| 19 | Detailed Siri test results recorded (partial paste) | R11 |
| 20 | Task-selection rule (R12); comparison test copy with ON/OFF switch and token counters built | R12 |
| 21 | ON vs OFF results recorded (Siri 3+3, Alexa 6+6) | R13 |
| 22 | Official-site read removed from the variation-test copy; source labels, legend and instruction to Claude reworded; test panel reduced to repeated lookups with cost counters; backup saved | R13 |
| 23 | Token-efficiency ideas listed (undecided) | n/a |
| 24 | Comparison test copy built: official-site check ON vs OFF, Siri and Alexa, with an agreement measure and cost counters | R13 |
| 25 | ON vs OFF official-site check results recorded (Siri 3+3, Alexa 3+3); decision pending | R13 |
| 26 | Rules R2, R5, R6, R7, R8, R10, R12 rewritten for the version without the official-site requirement; R14 (requirement removed) and R15 (network retry) added; R1, R3, R4 marked superseded | R14, R15 |
| 27 | Main page rebuilt from the variation-test copy without the official-site requirement, confirmation, retry, link and scope note; methodology text corrected; previous main page saved as `index-backup-with-official-site.html` | R14 |
| 28 | Documentation restructured: "Where we landed" added, rules split into in force and superseded, open items updated, short summary added at the end | n/a |
| 29 | Stability re-measure test copy of the rebuilt page built (`variation-test-rebuilt.html`) | R14 |
| 30 | Re-measure results recorded (Siri 5, Alexa 5): final result steady, marker differences traced to pronouns, behavior differences to task coding; note added under R14 | R14 |
| 31 | Second re-measure recorded (Bixby 5, Claude 5): final result not steady for Claude; adaptiveness and tone coding identified as unstable; open items 21 to 25 added | R14 |
| 32 | Third re-measure recorded (OpenAI Dots 5, Gemini 5): final result steady but structurally "unclear"; open items 26 to 28 added. No changes to the page or rules, pending the research team | n/a |
| 33 | Fourth re-measure recorded (GitHub Copilot 5, DeepSeek 5): final result varies for Copilot (imagery from memory of the logo); open items 29 and 30 added; summary across eight products updated. No changes to the page or rules | n/a |
| 34 | Two one-pagers written: the research team decision list (https://claude.ai/artifact/EGrZUcwj2su9g3EuVHePWP) and where things stand and why (https://claude.ai/artifact/SkorN8hSWLWB91Rb8aDRJ8). No changes to the page or rules | n/a |
| 35 | Found that a failed lookup hides its error message behind the manual sheet (open item 31); no change made | n/a |
| 36 | Fix made: a failed lookup now shows its reason at the top of the manual sheet (R16, provisional, marked for easy removal); test copy regenerated to match the main page | R16 |
| 37 | Recorded that lookups fell back to the manual sheet briefly and then worked again; cause unconfirmed (open item 32) | n/a |
| 38 | Failure notice (R16) reworded: plain-language reason, what to do next, technical message kept underneath as "Details"; still removable by deleting the marked blocks (checked) | R16 |
| 39 | Public distribution options researched; item 33 corrected after the user supplied a working public Publish link (legacy route) | n/a |
| 40 | Live example recorded: the name "Claude" coded "unknown" while the reason says ambiguous (item 23 updated); no change to the page | n/a |
| 41 | R17: the instruction not to use "genuinely" added to the lookup and classifier prompts; the word removed from two classifier prompts; before and after word-check test pages built; static label text left for a decision (item 35) | R17 |
| 42 | R17 extended to the output labels: "Genuinely spans both" became "Spans both" in the task chip and both paths' task dropdowns, and the worked-example reason was reworded; word-check test pages now also scan the screen and labels | R17 |
| 43 | Upload set prepared for GitHub: `index.html` (SHA-256 `5b7d7f11d338031964f5837b71e2df57725773e790cfb3ed7077847ef16a7145`) and this log; commit text written; user approved the R17 output changes as made | R17 |
| 44 | Live word check recorded: 0 occurrences across 3 lookups, 8 classifier calls, the results panels, dropdowns and result labels | R17 |
| 45 | Final stability check recorded (Siri 5, Claude 5): final result steady in 9 of 10 lookups, 0 occurrences of "genuine," no regression; page unchanged | n/a |
| 46 | Ported copy published as a Claude artifact (`index-artifact.html`): Claude calls moved to the artifact's ask-Claude capability; no web search, no temperature setting, no network retry; copy describing the search reworded; `index.html` unchanged | n/a |
| 47 | A changelog entry for the user's repo was first drafted without access to the repo; superseded by row 48 | n/a |
| 48 | Repo github.com/crivera8/casting-call read (public, read-only clone). Its `index.html` is byte-identical to the final file (SHA-256 `5b7d7f11...`), so it was already uploaded. A `CHANGELOG.md` entry for 2026-10-09 was written in the repo's own style and inserted above 2026-10-06 in a copy (110 lines added, none removed). Found and corrected an error in this log: line 1079 is Path B's task dropdown, not a tone dropdown. No push was made (no GitHub connection) | n/a |
| 49 | Anonymization of the visible page for the submission, one item at a time (title, byline, contact line, footer, "Questions or comments" section, design-intro clause); restore file saved; R18 added | R18 |
| 50 | Changelog entry for 2026-10-12 added to the repo's `CHANGELOG.md` (written without names); path to the decision log corrected from `docs/` to the repo root | R18 |
| 51 | Voice rule revised (R10): use the default voice when there is one, otherwise "varies"; both prompts changed (verified by diffing the requests); stability test page rebuilt with voice detail and Siri and Alexa as defaults; test pending | R10 |
| 52 | Voice rule re-measure recorded (Siri 5, Alexa 5): Siri stayed "varies", Alexa coded on its default voice, final result steady in 9 of 10 lookups, 0 occurrences of "genuine"; voice-rule section added to the 2026-10-12 changelog entry | R10 |
| 53 | Second voice-rule test set up (Bixby 5, Gemini 5) before pushing the update; test pending | R10 |
| 54 | Second voice-rule set recorded (Bixby 5, Gemini 5): both "varies" in every run, results unchanged from before the rule; 20 lookups across both sets consistent; changelog updated | R10 |
| 55 | No-voice check set up (GitHub Copilot 3, DeepSeek 3) before pushing; test pending | R10 |
| 56 | No-voice check recorded (GitHub Copilot 3, DeepSeek 3): "none" in all 6 runs, rule unaffected; all four voice cases tested (26 lookups); changelog updated | R10 |
| 57 | Design studio: Claude reads the job and the name and proposes an avatar; variation notes added; tested against a stand-in; one click-loss bug found and fixed | R19 |
| 58 | Blank-start readings built for both paths, then removed the same day because they changed Path A's scoring; Path A returned to the original (identical on nine cases); the design studio uses Path A's defaults and scoring | R19 |
| 59 | Essay, notices and explainer boxes reworded where they said the person makes the readings; methodology page gets a third route; intro "four calls" becomes "calls" | R20 |
| 60 | Full before/after wording list written for the authors to confirm (`wording-review.md`) | R20 |
| 61 | Rendered copies of the wording list made for the authors: a PDF (`wording-review.pdf`) and a single-file HTML checklist (`wording-checklist.html`). In the checklist, ticks and comments save in the reader's own browser, and "Download my answers" or "Copy my answers" returns them; nothing is sent anywhere automatically | R20 |
| 62 | Avatar proposal made unsteered: the instruction to avoid the job's stereotype and the job's reading were removed from the request, and the wording that described the steering was removed from the page, the wording list and the log. The steering version is recorded under Considered, not built; whether the proposal should steer is open item 42 | R19 |
| 63 | Blank starts restored in both paths with the original scoring (a blank counts exactly as the old default did). Path A identical to the original in text and markup on ten scenarios; scoring functions and prompts byte-identical; the design studio gives the same verdict, pathway and chips on six sets of choices | R21 |
| 64 | Avatar proposal now appears on its own once the job is described, like the other readings. The first-proposal button was removed; a "Propose another" button appears after a proposal and "Try again" after a failure. A changed job gets a fresh proposal, and a slow answer for an old job cannot overwrite a newer one. Wording rows 7, 8, 11 and 26 updated; the older note about a click-loss bug no longer applies | R19 |
| 65 | Open item 44 added: the avatar proposal is based on the job text alone; whether it should also use the name, manner, pronouns or voice is left to the research team | R19 |
| 66 | README corrected (statistics; Claude-backed features counted as three; the design studio's dropdowns named as the part that works anywhere). The research-team decision list and the where-things-stand note were updated to Oct 12, and the log's overview was brought up to date | n/a |
| 67 | Design studio steadiness test run live (90 calls) and recorded: job and name readings steady in every run; avatar reading steady in 29 of 30; unsteered avatar read the same as the job in 24 of 30 runs; each reading call cost about 4,500 input tokens, about 20 times my estimate (open items 40, 42, 45) | R19 |


---

## Summary of the decision rules

- One lookup is one general web search ("[product] [maker] AI assistant"), with no official-site requirement, check, link or retry (R8, R14).
- The product is assumed to be an AI assistant, agent or chatbot, and names are not case-sensitive (R5, R6).
- Temperature is 0 on every call (R7).
- At most three tasks: the most prominent first, no overlaps (R2, R12).
- Voice: if the product has one default voice, code it and say others exist; if there is no single default, say "varies" (it scores as neutral) (R10).
- Every answer is tagged *Other source*, *Claude's general knowledge* or *Source not specified*, and the results warn that scores can vary between searches (R11, R13).
- If the main search fails with a network error, retry once after about a second (R15).
- If a lookup fails, the manual sheet opens and says why (R16, provisional; can be removed).
- Explanations don't use the word "genuinely" (R17).
- For the submission, the authors' names, affiliations and emails are removed from the visible page (R18).
- In the design studio, Claude reads the job and the name and proposes an avatar, with Path A's scoring; the readings Claude fills in start blank in both paths and count exactly as the old defaults did, so Path A's scoring and results are unchanged; the essay and notices say who makes each reading (R19 to R21).
- Markers and behaviors are judgment calls made by Claude, not a database lookup.
