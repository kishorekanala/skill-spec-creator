---
name: "spec-creator"
description: Turns a product idea into a requirements spec (requirements.md with features, sub-features, user stories and EARS acceptance criteria) via a guided, one-question-at-a-time interview in two levels — first the minimal Core features without which the solution cannot work, then, if the user chooses, Advanced features — suggesting items to accept, reject or refine, with the option to generate the spec at any point. Also reads an existing spec (its own requirements.md or any PRD / requirements doc) and continues the interview to fill core gaps, add features, modify or remove requirements and resolve open questions, producing a new version with stable IDs and a change history. Use whenever the user wants spec-driven development, to write, extend or revise a spec / requirements / PRD, define an MVP or must-have feature set, flesh out or scope an idea, or says "help me spec this", "I want to build X, what should it do", "update the requirements" — even without the word "spec".
---

# Spec Creator

Guide the user from a rough idea to a precise, testable **requirements spec** through a
conversational interview. You do the heavy lifting of proposing features; the user decides.
The output is a single `requirements.md` — no design or task breakdown.

The skill runs in one of two modes, chosen in Phase 0:

- **New spec** — start from an idea (Phase 1 → Phase 2A Core → Core checkpoint →
  Phase 2B Advanced → Phase 3).
- **Update spec** — start from an existing specification the user provides, rebuild the
  decision log from it, and continue the interview on additions and modifications
  (Phase U → Phase 2 → Phase 3 with the update rules).

Four principles shape everything below:

1. **Core first, then advanced.** The interview first settles the *minimal* set of
   features and sub-features without which the solution cannot do its job at all. Only
   after the user has worked through that list is the richer, advanced layer offered —
   and it's the user's choice whether to continue. A user who stops after the core still
   has a coherent, buildable spec; a user who keeps going builds on a foundation they
   have already agreed. Mixing "must exist" with "would be nice" in one pass buries the
   essentials and makes the first pass feel endless.
2. **One decision point at a time.** Each turn asks about one feature (or one slice of a
   feature) and ends by letting the user choose to continue or generate the spec now. A
   wall of twenty questions gets skimmed; a single focused choice gets a real answer.
3. **The spec only records what the user decided.** Suggestions the user never saw or
   never answered are not requirements. When the user jumps to the spec early, unexplored
   areas become "Open questions" (core) or "Not yet reviewed" (advanced), not invented
   requirements. A spec full of plausible guesses is worse than a short honest one,
   because a coding agent will build the guesses.
4. **An existing spec is the baseline, not a draft to rewrite.** When updating, everything
   the user didn't touch stays as it was — same wording, same IDs, same Source. Downstream
   work (designs, tasks, tests, tickets) already points at those IDs and sentences; silently
   rephrasing or renumbering them breaks that traceability even when the new text is "better".

## Phase 0 — Choose the mode

Before anything else, check whether an existing specification is in play. Signals:

- The user attached a file or pasted text that reads as a spec: a `requirements.md`, a PRD,
  an SRS, a `.docx`/`.pdf`/Google Doc/Notion page of requirements.
- The user names or points to a path (e.g. `specs/billing/requirements.md`), or says
  "update / extend / continue / add to my spec".
- You're in a project folder and `specs/*/requirements.md` or `specs/*/.spec-session.md`
  already exists for the idea being discussed.

If there is one, use **Update spec** mode (Phase U). If the user describes a new idea and
also mentions an unrelated spec, ask one short question rather than guessing. If a
`.spec-session.md` exists but no `requirements.md`, that's an unfinished interview — resume
it as described under "Keeping the decision log" instead.

Read the file with whatever tool suits its format (plain read for Markdown/text; the
relevant document skill or connector for `.docx`, `.pdf`, Google Docs, Notion). If the user
mentions a spec you can't find or open, ask for it — don't reconstruct it from memory.

## Phase U — Load an existing spec (Update mode)

### U1. Classify the input

- **Native** — a `requirements.md` in this skill's structure (numbered sections, `F1`/`F1.1`
  features, `REQ-F1.1-01` IDs, EARS criteria, a Source per requirement). Load it directly.
- **Foreign** — any other requirements document. Import it: map its content onto this
  skill's structure (features, sub-features, requirements, NFRs, out of scope, open
  questions). Keep the document's own IDs in a "Legacy ID" note on each requirement so
  people can find their way back, and mark imported requirements `Source: Imported`.
  Where an imported statement isn't in EARS form or isn't testable, keep the original
  wording in Notes and record the gap as an open question rather than inventing values to
  make it testable — that would be putting words in the author's mouth.

### U2. Rebuild the decision log from the spec

Populate the same decision log the interview uses (see "Keeping the decision log"):

- **Confirmed summary** ← Introduction / Purpose
- **Accepted** ← every functional and non-functional requirement, with its existing ID,
  wording, Source and Tier (if the spec has tiers) — marked *baseline* so you can tell it
  apart from this session's changes
- **Rejected / Deferred** ← Out of scope, with reasons
- **Assumptions** ← the Assumptions section
- **Not yet explored** ← Open questions and the "Not yet reviewed" list, plus feature-map
  areas the spec never covered
- **Baseline version** ← the version number and date from the header (0.1 if absent)

Then privately draft the tiered feature map as in Phase 1, overlaying what the spec already
has, so you can see the gaps. Look at **core gaps first** — a core sub-feature the spec is
missing means the product as specified can't do its job, which outranks every
nice-to-have — then advanced gaps, requirements with no unhappy path, vague criteria, and
assumptions nobody confirmed.

If the file has an accompanying `.spec-session.md`, read it too — it may hold rejected
suggestions and notes that never made it into the spec.

### U3. Show where the spec stands, then ask what to work on

Restate the product in two or three lines (as in Phase 1) together with a compact status:
version, number of feature areas and requirements, open questions, assumptions, and the
two or three most significant gaps you noticed (core gaps named first). For a foreign spec,
also say in one line how it was mapped and anything that didn't fit. Confirm the summary
the same way Phase 1 does — if the user corrects it, the correction is itself a change for
the change history.

If the user already said what they want to change ("add offline mode", "drop SMS
notifications"), go straight to that. Otherwise ask one question (multi-select with a
clickable tool; numbered list in text), recommending the first option when core gaps exist:

- **Fill core gaps** — work through missing essential sub-features, one per turn
  (include only if there are any)
- **Add advanced features** — propose feature areas or sub-features the spec doesn't have
- **Resolve open questions / modify requirements** — work through open questions, or
  change values, rules, priorities, or remove items
- **Review assumptions** — confirm or correct each assumed item

Then continue in the Phase 2 loop: core gaps use the Phase 2A rules, additions use the
Phase 2B rules. Skip topics the spec already settled (don't re-ask users & roles or scope
unless the user wants to).

If the baseline spec has no Tier labels, don't retrofit them silently — that would change
every requirement. Offer it once, as a line in the status ("Tag existing requirements as
Core/Advanced? I'd tag 14 as Core, 9 as Advanced"). If the user agrees, it's logged as one
change-history entry; if not, only new items get a Tier.

### Modifying and removing in Update mode

- To modify, show the requirement's current text and offer concrete alternatives, the same
  way suggestions work: "REQ-F2.1-03 currently locks the account after 5 failed attempts:
  keep 5 / change to 3 / change to 10 / remove the lockout".
- A removed requirement is not deleted from history: record it as *withdrawn*. Its ID is
  retired and never reused, because something downstream may still reference it.
- Changing a requirement's meaning makes its Source `Confirmed` (the user just decided it).
  Rewording it for clarity without changing meaning is still a change — only do it when the
  user agrees, and log it.
- When a change ripples (removing Payments leaves Refunds dangling; adding Offline mode
  implies sync), raise the affected requirements as the next suggestion rather than
  editing them silently. Withdrawing a Core requirement deserves one sentence saying which
  part of the core journey it leaves uncovered, so the user decides knowingly.

## Phase 1 — Capture the idea (New mode)

Read the user's input and restate it in two or three lines: what is being built, for whom,
and the core problem it solves. Where something wasn't stated, fill it with your best
inference rather than asking — the confirmation step below is where the user corrects it.
Make the **primary user's main journey** explicit in the summary (e.g. "a customer finds a
home cook nearby, orders a meal and pays; the cook accepts and marks it ready") — it is the
yardstick for what counts as Core.

**Confirm the summary before going further.** Everything downstream (the feature map, every
suggestion, the spec's Purpose section) is built on this summary, so a misunderstanding here
is expensive to discover later. Show the summary and ask for confirmation:

- With a clickable question tool: one single-select question, header "Summary", with
  options such as "Looks right — continue" and "Needs changes" (describing that they can
  type what to change). If one part is genuinely ambiguous — e.g. you can't tell whether it's
  a consumer app or an internal tool — offer your two or three readings as options instead,
  plus "Looks right — continue" if one reading is already in the summary. The user can always
  type corrections via "Other".
- Without one: end with a line like
  `Does this capture it? Reply "yes" to continue, or tell me what to change.`

If the user asks for changes, apply them, show the revised summary, and ask again. Repeat
until they confirm — keep each revision to the same two or three lines, and don't move to
the interview until they have. Record the confirmed summary in the decision log; it becomes
the spec's Purpose section.

### Draft a tiered feature map

Once confirmed, privately draft a **feature map**: the top-level feature areas (F1, F2, …)
this kind of system needs, each with candidate sub-features, and give every sub-feature a
**tier**:

- **Core** — without it the solution is not possible: the primary user cannot complete
  the main journey from the summary end to end, or the product cannot legally or safely
  operate at all (e.g. identity verification for a lending app). Test each candidate by
  removing it: if the main journey still works, however clumsily, it is not Core.
- **Advanced** — everything else: conveniences, efficiency, scale, polish, secondary
  users, analytics, integrations, most cross-cutting concerns. Many are valuable; none is
  required for the solution to exist.

Keep the Core list genuinely minimal — typically 3–6 feature areas and roughly 8–15
sub-features, fewer for a simple tool. When in doubt, tier it Advanced: the user will see
it at the next level anyway, whereas a padded core list defeats the point of asking for
the essentials first. A feature area is Core if it has at least one Core sub-feature; its
other sub-features stay Advanced and come back in Phase 2B (so "Orders" can be Core with
"Place order" Core and "Reorder previous order" Advanced).

Draw on the idea itself first for both tiers, then on `references/feature-catalog.md` for
areas people forget — mostly Advanced, though some become Core for a particular product
(sign-in is Core when data is private to each user; not for a single-user local tool).

Aim for granularity: a sub-feature should be small enough that it maps to a handful of
acceptance criteria. "User management" is a feature area; "Invite user by email",
"Deactivate user", "Change a user's role" are sub-features. If a sub-feature would need
more than ~6 criteria, split it.

*Example — a marketplace for home-cooked meals.* Core: cook profile & menu listing,
browse/search cooks near me, place an order, pay for the order, cook accepts/rejects,
order status to customer. Advanced: ratings & reviews, scheduled orders, promo codes,
cook payouts dashboard, dietary filters, chat, loyalty, analytics.

## Phase 2 — The interview loop

The interview runs in two levels with a checkpoint between them. Both levels use the same
turn shape (below): one suggestion at a time, then a what-next choice.

### Phase 2A — Core specification (level 1)

Open the level with one line that sets expectations, e.g. "First, the essentials — the
minimum features this product can't work without. We'll look at advanced features after."

Order of topics (skip any the idea makes irrelevant):

1. **Primary users & roles** — only the roles the main journey needs. Admins, analysts,
   support staff and the like wait for level 2 unless the core journey can't run without
   them.
2. **Core feature areas** — present the Core areas from the map (multi-select, framed as
   "these are what I believe the product can't work without"). The user may deselect one;
   if that leaves the main journey broken, say so in one sentence and let them decide —
   e.g. payment handled outside the product is a legitimate answer, recorded as a
   constraint. Deselected areas go to Out of scope; nothing is dropped silently.
3. **Core sub-features, area by area** — for each confirmed Core area, most central to the
   main journey first, propose its **Core sub-features only**. Hold the area's Advanced
   sub-features back for level 2; don't mention them yet. After the user picks, follow up
   only on rules without which the sub-feature can't be built or tested (what "done" means,
   a limit or validation that shapes the data, what happens on the essential failure path
   such as payment declined) — leave fine-tuning for level 2.
4. **Essential constraints** — one turn, only if the idea implies any: the target platform,
   a regulation that gates launch, a mandatory integration. Other non-functional topics wait.

If the user volunteers something that is clearly Advanced during this level ("also add
loyalty points"), accept it — their free text is authoritative — tag it Advanced, and say
in a few words you'll pick it up in the advanced round unless they want it now.

When every Core area has been covered, go to the checkpoint.

### Core checkpoint

This turn is the boundary between the two levels. Show a compact recap of the core spec —
feature areas with their accepted sub-features, one line each, plus counts (e.g. "Core: 5
areas, 12 sub-features, 2 open questions") — then one single-select question, header
"Next level":

- **"Continue with advanced features"** (recommended) — go to Phase 2B. The description
  should say how many advanced candidates are waiting (e.g. "~20 suggestions across 8 areas,
  one at a time; you can stop whenever").
- **"Generate core spec now"** — go to Phase 3 with the core only.
- **"Adjust the core"** — revisit a core decision, then return to this checkpoint.

In plain text: the recap, then
`Reply: advanced · spec · adjust`.

Why this matters: a user who only needs an MVP spec gets a complete one here, and a user
who continues knows exactly what foundation the advanced features sit on.

### Phase 2B — Advanced features (level 2)

Open with one line, e.g. "Now the advanced layer — each of these builds on the core you've
set. Pick what you want; skip the rest."

Order of topics (skip any the idea makes irrelevant):

1. **Additional roles** — secondary users and administrative roles, if any.
2. **Advanced scope** — which advanced feature areas are worth exploring (multi-select, up
   to 4 per turn, split across turns if needed). Include "enhancements to core areas" as
   well as new areas. Unselected areas go to Out of scope as "Not selected" — confirm
   rather than silently drop them.
3. **Advanced sub-features, area by area** — first the held-back Advanced sub-features of
   the Core areas (the user already knows those areas, so these are quick wins), then the
   new advanced areas, highest value first. Follow up on rules and edge cases where a real
   decision exists.
4. **Cross-cutting / non-functional** — performance, security, privacy/compliance,
   availability, accessibility, localisation, platforms, integrations, observability.
5. **Wrap-up** — anything missed, an optional single prioritisation turn if priorities
   matter to the user, then generate.

### Shape of every turn

Each turn has two parts: a **suggestion question** and a **what-next question**.

Open with one short line of context: which level and where you are (e.g. "Core · Feature 2
of 5 — Ordering" or "Advanced · Feature 3 of 7 — Notifications (part 1 of 2)") and, if
useful, one sentence on why these suggestions matter. Keep it brief — the options carry the
content.

**When a clickable question tool is available** (e.g. `AskUserQuestion`), use it with two
questions in one call:

- **Q1 — the suggestion.** `multiSelect: true` for sub-feature picks, single-select for
  either/or decisions. The tool allows 2–4 options per question, so present at most 4
  sub-features at a time; when a feature has more, split it across consecutive turns
  ("part 1 of 2"). Each option: a short label and a one-line description that states the
  concrete behaviour, so the user knows exactly what they're agreeing to. Put the options
  you'd recommend first. The user can always type their own answer via "Other" — treat
  free text as authoritative, including new sub-features you didn't suggest.
- **Q2 — what next.** Single-select, header "Next", with options such as:
  - "Next suggestion" — move on
  - "Go deeper on this" — ask about rules/edge cases of what was just picked
    (include only when there's something worth going deeper on)
  - "Generate spec now" — jump to Phase 3
  - "Review decisions" — show the decision log so far, then continue

**When no such tool exists**, write the same thing as plain text: a numbered list of
suggestions (`1.` … `n.`, one line each), then a line such as
`Reply with numbers to keep (e.g. "1, 3"), your own edits, or: next · deeper · spec · review`.
Plain-text turns may offer up to ~6 suggestions since there's no tool limit.

The checkpoint turn is the one exception: it has a single question (the level choice)
instead of the suggestion + what-next pair.

### Keeping the decision log

Maintain a running log so nothing is lost and the spec can be generated at any moment:

- **Stage** — Core, Checkpoint or Advanced, so a resumed session knows which level it's in
- **Confirmed summary** — the two-to-three-line idea statement the user approved,
  including the main journey
- **Accepted** — feature / sub-feature with its **Tier**, and any rules or values the user gave
- **Rejected** — explicitly declined suggestions (useful for "Out of scope")
- **Deferred** — "later", "v2", "not now"
- **User notes** — free-text additions, verbatim where precise wording matters
- **Not yet explored** — feature-map items the interview hasn't reached, with their tier

In Update mode the log also tracks, for this session only:

- **Modified** — baseline ID, old text, new text, and the user's reason if they gave one
- **Withdrawn** — baseline IDs the user removed, with reason
- **Added** — new items (they also appear under Accepted)

If you have a filesystem, also persist the log to `specs/<slug>/.spec-session.md` after each
answer, so a long interview survives a context reset and can be resumed. On resume, read it,
summarise where you left off in one line (including the level), and continue with the next
unexplored item.

### Interview judgement

- Adapt the map as answers arrive: if the user rejects "Payments", drop its dependants
  (refunds, invoices); if they add "Offline mode", add its implications (sync, conflict
  resolution) to the map at the tier that fits.
- A user's answer can move an item between tiers — "we can't launch without bulk upload"
  makes it Core for this product. Respect it; the tier describes this product, not
  products in general.
- Don't ask what the user already answered, and don't ask about things with an obvious
  industry default — record the default as an assumption instead.
- Offer concrete values when a number matters ("Session timeout: 15 min / 30 min / 8 h /
  never") rather than asking an open question.
- Keep momentum. If the user answers tersely, keep turns short too.

## Phase 3 — Generate the spec

Triggered by "Generate spec now" (or the equivalent in text), "Generate core spec now" at
the checkpoint, or when the map is exhausted.

1. Briefly confirm what will be marked as open (one line: "Core is complete; 7 advanced
   areas weren't reviewed — I'll list them as candidates, not requirements"), then write
   the spec without waiting unless the user objects.
2. Read `references/requirements-template.md` for the exact structure and
   `references/ears-guide.md` for writing acceptance criteria. Every accepted sub-feature
   becomes a requirement with a user story and EARS acceptance criteria.
3. Mark each requirement's **Source**: `Confirmed` (user chose it), `User-added` (came from
   their free text) or `Assumed` (a default you applied that they didn't contradict); in
   Update mode, requirements that came from a foreign document keep `Imported`. Assumed
   items should be few, and each one must appear in the Assumptions section too.
4. Mark each requirement's **Tier** (`Core` / `Advanced`) and default its Priority from it:
   Core → Must; Advanced → Should, or Could if the user called it a nice-to-have.
5. Unexplored items go where they belong: an unexplored **Core** item is a gap in the
   essential product and goes to Open questions; unexplored **Advanced** candidates are
   listed by name only under "Not yet reviewed" (section 4.3) — they are suggestions, not
   requirements and not questions.
6. Save as `specs/<slug>/requirements.md` when working in a project folder; otherwise save
   `requirements.md` and hand the file to the user with whatever file-delivery tool is
   available. Then give a one-paragraph summary: counts per tier (features, requirements),
   open questions, and the single most important open question.
7. Offer the natural next step: if only the core was done, offer to continue with the
   advanced layer; otherwise offer to continue on the open questions. Either way,
   regenerate the spec from the decision log rather than patching by hand, so IDs and
   traceability stay consistent. The regenerated spec follows the Update-mode rules below
   against the version you just wrote.

### Additional rules in Update mode

The updated spec is the baseline plus this session's decisions — nothing else changes.

1. **Carry baseline requirements over verbatim.** Untouched requirements keep their exact
   wording, ID, priority, Tier and Source. Don't "improve" them in passing; if you notice a
   weakness, it belongs in Open questions or a suggestion to the user.
2. **IDs are append-only.** New feature areas take the next free number (`F7` after `F6`),
   new sub-features the next free number within their area, new requirements the next free
   sequence number. Withdrawn IDs are never reused. Never renumber to close gaps — IDs are
   not grouped by tier, so a later Core addition simply takes the next free number.
3. **Bump the version** by one minor step (0.1 → 0.2, 1.2 → 1.3). Move to the next major
   (1.0, 2.0) only when the user says this revision is a baseline or release. Update the
   date, status ("Draft — revised in guided interview") and coverage line.
4. **Add a Change history entry** (section 12 of the template) listing every Added,
   Modified and Withdrawn item with its ID, and every open question resolved or assumption
   confirmed. Withdrawn requirements move to the "Withdrawn requirements" table with their
   last text, so the history stays readable.
5. **Tag changes in the traceability table** with the version they changed in, so a reader
   can find this revision's delta without diffing.
6. **Don't overwrite the only copy.** In a project folder, save the previous file as
   `specs/<slug>/requirements.v<old-version>.md` before writing the new
   `requirements.md` (skip if version control clearly already tracks it — say so). For an
   attached or linked file, deliver the updated file under the same name, and in its
   original format if you edited a `.docx` or similar with its skill; for a foreign spec
   that was converted, deliver the new `requirements.md` and say the original is untouched.
7. In the closing summary, lead with the delta: "v0.2: 4 added (1 core, 3 advanced),
   2 modified, 1 withdrawn, 3 open questions resolved, 2 remain" — then the single most
   important remaining question.

## Quality bar for the spec

Before handing it over, check:

- The Core tier alone describes a product whose main journey works end to end; every Core
  requirement passes the removal test, and nothing merely convenient is tagged Core.
- Every acceptance criterion follows an EARS pattern, has exactly one "SHALL", and is
  testable — no "fast", "user-friendly", "appropriate", "etc." without a measurable value.
- Every requirement traces to a feature and a user decision; nothing unconfirmed is
  presented as confirmed.
- IDs are stable and hierarchical (`F2`, `F2.3`, `REQ-F2.3-01`, `NFR-SEC-01`).
- Failure and edge behaviour is covered for each requirement where it applies (use the
  IF/THEN "unwanted behaviour" pattern).
- Rejected and deferred items appear under Out of scope; unexplored core items under Open
  questions; unexplored advanced candidates under Not yet reviewed.
- In Update mode: every baseline requirement is either carried over unchanged or listed in
  the Change history as modified or withdrawn — nothing disappears or changes silently —
  and no existing ID was renumbered or reused.
