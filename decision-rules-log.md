# Casting Call: decision rules log

**Scope:** Path A ("Bring an AI on stage"), Option A ("Have Claude look it up").
**Started:** Oct 8, 2026. **Last updated:** Oct 9, 2026. Rules are numbered in the order they were decided, so the list in force skips the superseded ones, which are kept further down as history.

---

## Where we landed (as of Oct 9, 2026)

**How Option A works now**
1. The person types a product name or link.
2. The page sends one request to Claude with one general web search (the query is the product name, its maker, and the words "AI assistant"), at temperature 0.
3. Claude returns a code and a short reason for each marker (name, pronouns, voice, imagery) and for behavior (up to three tasks, plus tone), along with designer, organizational-diversity and adaptiveness fields. It also says whether each answer came from a page the search returned or from its general knowledge.
4. The results panel shows each answer with a tag (*Other source*, *Claude's general knowledge*, or *Source not specified*) and opens with a warning that results can vary between searches.
5. The person can edit any answer before revealing the placement. If the connection drops, the page retries once. If a lookup fails, the page opens the manual sheet and says why (R16, provisional).

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
- Cost per lookup with this design, confirmed on the rebuilt page (10 lookups): 21,103 to 21,304 input tokens and 661 to 927 output tokens, one call and one search every time.

**Verified live (Oct 9, 2026):** you ran the rebuilt page in the preview (10 lookups, Siri and Alexa). All completed, one call and one search each, no errors (open item 18).

**Still open, your call:** a pronoun rule, anchored task coding, whether adaptiveness defaults to "adaptive" on weak evidence, and the published link being out of date (open items 2, 10, 11, 13, 14). Also: the prompt gives Claude no definition of adaptiveness, and the tone code flips between feminine and mixed (open items 21 and 22).

**Companion one-pagers:** the decision list for the research team (https://claude.ai/artifact/EGrZUcwj2su9g3EuVHePWP) and a short account of where things stand and why (https://claude.ai/artifact/SkorN8hSWLWB91Rb8aDRJ8).

**Files:** `index.html` is the rebuilt main page. `index-backup-with-official-site.html` is the previous main page. `variation-test-rebuilt.html` is the rebuilt page plus a test panel, for re-measuring stability. `variation-test.html` and `variation-test-gate.html` are test copies built from earlier versions.

**Submission build:** `index.html` has had the authors' names, affiliations and emails removed from the visible page (R18), so it differs from the older copies and from the version in the repo until it is pushed.

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

### R10. If a product's voice varies, say so

- **Rule:** If a product offers more than one voice, or its voice varies by language, region, device or user choice, the tool does **not** pick one. The voice code is "none," the description starts with "Varies," and the label reads "Varies — no fixed voice, or user picks one." The default voice may be mentioned in the description.
- **Applies to:** the lookup's search and Option B's per-field check (which previously could only answer masculine, feminine or neutral).
- **Why:** Siri was returned as both "higher-pitched" and "deliberately androgynous" on different runs, because it has several voices and the instructions never said what to do.
- **Scoring effect:** The page's placement scoring treats this code as **neutral**. A varying voice adds no feminine or masculine point.
- **Enforced by:** Instruction to Claude only.
- **Revised Oct 9, 2026:** the earlier wording also listed the official-site pass, which is removed (R13).

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
- **Main page after this change:** `index.html`, 170537 bytes, SHA-256 `fefd7076f849739e6bd912370061edbae4c0cbe56260e246ae5ef87f87dce46a`.
- **Enforced by:** Page edit only. No effect on scoring, instructions to Claude, or cost.

---

## How each marker is judged (unchanged framework)

These are judgment calls applied consistently, not a database lookup. In Option A, Claude makes them; in Option B, the person types and Claude judges on leaving the field (pronouns are the exception: a simple text match, no judgment).

| Marker | How it is coded |
|---|---|
| Name | Common naming convention in the relevant language or culture |
| Pronouns | The pronoun the product is actually referred to by |
| Voice | Pitch: lower reads masculine-coded, higher reads feminine-coded (see R10 for varying voices) |
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
13. **Pronouns flipping between "object" and "unknown"** changes the marker score by one reading's worth (4 of 12 runs in the ON vs OFF check test). **Update Oct 9, 2026:** in the re-measure of the rebuilt page it was the only cause of marker-score differences: 'unknown' in 6 of 10 runs (Siri 3 of 5, Alexa 3 of 5), always on wording that mixed the maker's usage with common usage (for example "it in official copy; colloquially she/her"). The score moves because 'unknown' drops out of the denominator (25 to 33 for Siri, 50 to 33 for Alexa).
14. **The same task can get a different code depending on the prompt wording** ("Smart home control": masculine in 3 of 3 Alexa ON runs, neutral in 3 of 3 OFF runs).
15. **The official link can differ between runs** (three different Alexa pages in three ON runs), so it works as an identity check but is not reliably one canonical page.
16. **R14, R15 and the revised entries (R2, R5, R6, R7, R8, R10, R12) describe the intended main page.** The main page has not been rebuilt without the official-site requirement yet, so until then it still behaves as R1, R3 and R4 describe. **Resolved Oct 9, 2026:** the main page was rebuilt without the official-site requirement.
17. **The test copies no longer match the main page.** `variation-test.html` and `variation-test-gate.html` were built from earlier versions. There is no test copy of the rebuilt page yet, so its numbers have not been re-measured. **Update Oct 9, 2026:** `variation-test-rebuilt.html` now exists. Apart from the test panel and harness it is identical to the rebuilt main page (checked byte for byte), so it measures exactly what ships. Re-measure result: not yet run.
18. **The rebuilt page was run live by you on Oct 9, 2026** (10 lookups, Siri and Alexa): all completed, one call and one search each, no errors. I still have no live access myself, so I cannot run it.
19. **Untested: does the official-site-aimed query (without the check, retry or link) steady the pronoun reading?** The check ON vs OFF test changed the requirement and the query wording together, so the two cannot be separated. A clean test would keep the query and instruction ("[product] [maker] official site", prefer the maker's own pages) but drop the check, confirmation, retry and link.
20. **The test panel groups tasks by exact wording,** so near-duplicates ("systemwide app actions (drafting emails, editing photos)" and "systemwide app actions and drafting") count as different tasks and the per-task table understates coding differences.
21. **Adaptiveness has no definition in the prompt.** The page asks "Does its manner shift based on how you respond to it?" (adaptive = "it visibly adapts to feedback"), but the instruction to Claude only lists the three codes. Claude's notes use different meanings: personalizing by recognizing voices (Bixby, 5 of 5 "adaptive"), user-selectable voices or personalities (Alexa), changes in gender presentation (Claude, 4 "nonadaptive", 1 "unclear" for the same evidence). This also bears on item 10. **Update:** GitHub Copilot read "adaptive" in 2 of 5 runs because users can choose between AI models (not a manner shifting with feedback), and "nonadaptive" in 3.
22. **The tone code flips between "feminine" and "mixed" on near-identical wording** (Bixby: 3 feminine, 2 mixed; Claude: 2 feminine, 3 mixed; for example "Thoughtful, nuanced, honest, safety-focused" was coded feminine in one run and "thoughtful, honest, nuanced, safety-focused" mixed in another). For Claude it decides the final result. **Update (final check):** Siri's tone was coded "mixed" once (feminine in 4), which changed its result from aligned to unclear; Claude's tone was "mixed" in all 5 runs.
23. **The name "Claude" was coded masculine in 4 runs and neutral in 1.** One neutral run changed the marker score from -25 to 0 and the placement from masculine to neutral. **Update (live run, from a screenshot):** the name "Claude" was coded "unknown" (label "Not sure / haven't checked") with the reason "Used for men and women; ambiguous in convention." By the page's own label, "ambiguous" is what the "neutral" code means ("Neutral or ambiguous"), so the code and the reason disagree. The name now has three codings across six runs: masculine (4), neutral (1), unknown (1). The instruction does not say when to use "neutral" and when "unknown." A possible fix, not made (research call, would change numbers): a name used for both men and women, or with an ambiguous convention, is "neutral"; "unknown" only when the name cannot be assessed. **Update (final check):** in the final stability check the dominant reading flipped between sessions (neutral 4, masculine 1; earlier masculine 4, neutral 1).
24. **The voice label for code "none" reads "Varies — no fixed voice, or user picks one" even for a product with no voice at all** (Claude is text-only), which is misleading on screen. Scoring is unaffected (both count as neutral).
25. **The final result is steady for Siri and Alexa (22 of 22 lookups) but not for Claude.** Siri and Alexa were easy cases (feminine name, voice that varies, feminine-leaning tasks and tone). It also varies for GitHub Copilot (item 30).
26. **"Not described" is coded two ways.** Dots' voice was "unknown" in 4 runs and "none" in 1 for the same evidence ("voice calls supported; no default voice described"). "Unknown" drops out of the score's denominator and "none" counts as a known zero. No effect on Dots (everything else was neutral), but it would move the score for a product with a feminine or masculine name. Same family as the pronoun flips (item 13).
27. **Products whose markers are all neutral-coded always get a neutral placement and an "unclear" alignment** (Bixby, Dots, Gemini), however the behavior reads. "It," a voice that varies and an abstract avatar are folded into neutral (known zeros), so the placement is "neutral," not "insufficient information." Whether that is the intended treatment is a question for the research team.
28. **Output can differ when the input appears to be the same.** Gemini's input token count was identical (21,468) in all 5 runs, which suggests the search returned the same results, yet adaptiveness read "adaptive" 4 times and "unclear" once, at temperature 0. The variation warning (R11) says results depend on which pages the search brings back; that is not the whole story. Not proven (identical token counts are an indirect sign). **Update:** DeepSeek's input was 21,764 tokens in all 5 runs, again suggesting the same search results, with different wording and one different tone coding.
29. **Imagery for a logo-only product is coded from the model's memory of the logo,** which it cannot see. GitHub Copilot's logo was described as "angular, helmet/visor" (coded masculine, 2 runs) and as "circular" or "geometric" (coded neutral, 3 runs); every imagery answer was tagged "knowledge." The code follows the description, and the description varies.
30. **The final result sits on a threshold.** The placement rule calls a placement masculine or feminine when one side has more readings than the other, so a single borderline reading (Claude's name or tone, Copilot's imagery) moves a neutral placement to a directional one and the result flips between "unclear" and aligned or misaligned. Whether a single reading should be enough is a question for the research team. Related to items 22, 23, 27 and 29.
31. **When a lookup fails, the page switches to the manual sheet and hides the reason.** The message ("Couldn't complete that search (...)") is written into the search panel, which is hidden once the page is in manual mode, so the person sees only the manual sheet and cannot tell why (a usage limit, a dropped connection and a parsing problem all look the same). The original page behaved the same way. The test copies show the message in their results table. **Resolved Oct 9, 2026 by R16** (provisional).
32. **On Oct 9, 2026, lookups briefly fell back to the manual sheet** after heavy testing, with the reason hidden (item 31). Searches worked again later with no change to how the page searches (the only change was R16, which only displays errors). Cause not confirmed: a usage limit, a connection problem or a service problem. If it recurs, R16's notice will show the reason.
33. **Public distribution (Oct 9, 2026). Corrected after the user supplied a working example.** An earlier version of this item said a public link where visitors use their own Claude accounts "does not appear to be possible." That was wrong. The user supplied a public link in the older Publish format (`claude.ai/public/artifacts/<id>`) that they say ran searches. (Only the empty page shell could be loaded from here, so the contents were not verified.) Anthropic's help page describes this route: a published chat artifact opens for anyone without a Claude account, visitors are asked to sign in only for features that use Claude, and those features count toward their own usage limits. The limits are on which accounts can do it: the Publish button exists only for artifacts made in the previous chat experience (a free account tested on Oct 9 showed only a personal "Only you" link), and in the newer Share dialog, artifacts that use Claude cannot use "Anyone with the link." Plan: publish the rebuilt `index.html` from the account and setup that has the Publish button (new chat, attach the file, "Show this HTML file as an artifact exactly as it is," AI-powered artifacts on, then Publish), and test the link in a private window. Cautions: unpublishing is permanent for that artifact, and a third-party guide says a published link cannot be updated in place (not confirmed in Anthropic's pages). Fallbacks if no Publish button is available: a small server holding an API key (the owner pays and must guard against misuse), each visitor pasting their own API key, or a public page with Option B only. Decision pending. **Update:** the working example was made from a collaborator's Pro account, which has the Publish button. Plan: the collaborator publishes the rebuilt `index.html` (SHA-256 `abd71026ffae958ae908a572ecba6493b4766a1b456f68e87d362e4a6dfff707`) from that account. If a fresh chat shows no Publish button there, try the original chat that holds the earlier published artifact (not confirmed to work). The link is owned by the collaborator's account: if they unpublish or delete it, it stops working, so record who owns the link. **Update (published artifact, Oct 9, 2026):** a ported copy, `index-artifact.html` (SHA-256 `28622e5037e6518540a31dee178526de6b6cead7ea3da6c5dab5400ddf1ea42b`), was published as a Claude artifact at https://claude.ai/artifact/DMozabpMCHwhHmo841FyMJ. `index.html` is unchanged. A published page cannot call the Claude API or search the web, so both Claude calls (the lookup and the manual-mode classifier) were moved to the artifact's built-in "ask Claude" capability (`sample`, declared at publish), which runs on each viewer's own Claude account. What changed: lookups have no web search (answers come from Claude's general knowledge, every answer is tagged as such, and no source links are shown); temperature 0 cannot be set (the capability exposes no temperature); the one-retry on a dropped connection was removed (the platform reports its own errors); the lookup prompt now says Claude cannot search and must set all source fields to null; the failure notice understands the platform's error codes; and four passages that described a web search (the Option A card, the privacy note, the "What actually happens" note and the results legend) were reworded. Because there is no search, scores from this version are not comparable with the stability tests above, which all used a web search. Not verified: whether a visitor on a public link (as opposed to a signed-in viewer) can use the capability, and whose usage it counts against; the capability's documentation says it spends the viewer's own Claude usage after a consent prompt, and that a page opened outside Claude cannot use it. Tested only against a stand-in for the capability, not live.
35. **Static "genuinely" text (resolved for the output on Oct 9, 2026).** The label "Genuinely spans both" for the "mixed" code appeared in the task chip in the results panel (and the placement breakdown), the task dropdown in Path A (line 949) and the task dropdown in Path B (line 1079); the worked example's reason (line 1938) also used it. After the user clarified that the scope is the output that rates how feminine or masculine something is, all four were changed ("Spans both"; the example reason now starts "Spans..."), verified by rendering before and after. Five explanatory notes (lines 754, 847, 1059, 1302, 1310) still use the word and were left alone as page text, not output. Note: only task rows use this label; the tone chip and both tone dropdowns read "Blends both depending on context." Correction: this item previously called line 1079 a tone dropdown; it is Path B's task dropdown, so both paths now match, which also answers the concern in the repo's 2026-10-06 changelog entry (changing the label on one side only would reopen drift).
36. **The repo's README has stale file statistics.** It says `index.html` is "2,689 lines, ~162 KB"; the file is now 2,797 lines and 171,818 bytes. Not changed. The README's deployment section (the tool runs as a claude.ai artifact; GitHub Pages is a manual-mode preview) matches what was found on Oct 9 about publishing and search.
37. **The authors' names remain in places outside the visible page of `index.html`.** Left on purpose: code comments in the script, and the git history (commit authors and email addresses, and the account handles in the repo and Pages URLs). Not addressed, because only the submitted page was in scope: the repo's `README.md` (it names both authors, their affiliations and their emails), `old/index.html`, older changelog entries, the artifact copy and the test pages built from earlier versions, and the published artifact made from the artifact copy. Kept on purpose, and they still identify the authors: the inline citation and the working-paper reference entry. If the repo or any of those copies is shared with reviewers, those matter.
38. **The 2026-10-09 changelog entry pointed to `docs/decision-rules-log.md`, but the log is in the repo root.** Corrected in the changelog to `decision-rules-log.md` on 2026-10-12.

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


---

## Summary of the decision rules

- One lookup is one general web search ("[product] [maker] AI assistant"), with no official-site requirement, check, link or retry (R8, R14).
- The product is assumed to be an AI assistant, agent or chatbot, and names are not case-sensitive (R5, R6).
- Temperature is 0 on every call (R7).
- At most three tasks: the most prominent first, no overlaps (R2, R12).
- If a product's voice varies, say "varies" (it scores as neutral) (R10).
- Every answer is tagged *Other source*, *Claude's general knowledge* or *Source not specified*, and the results warn that scores can vary between searches (R11, R13).
- If the main search fails with a network error, retry once after about a second (R15).
- If a lookup fails, the manual sheet opens and says why (R16, provisional; can be removed).
- Explanations don't use the word "genuinely" (R17).
- For the submission, the authors' names, affiliations and emails are removed from the visible page (R18).
- Markers and behaviors are judgment calls made by Claude, not a database lookup.
