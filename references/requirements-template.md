# requirements.md template

Use this structure exactly. Omit a section only if it would be empty, and say so in one line
rather than leaving an empty heading.

````markdown
# <Product name> — Requirements Specification

| | |
|---|---|
| Version | 0.1 (draft) |
| Date | <YYYY-MM-DD> |
| Status | Draft — generated from guided interview *(Update mode: "Draft — revised in guided interview")* |
| Previous version | <n/a for a new spec; otherwise e.g. 0.1 (YYYY-MM-DD)> |
| Coverage | Core: <complete / n of m areas>; Advanced: <n of m areas reviewed / not started>; <k> open questions |

## 1. Introduction
### 1.1 Purpose
<2–3 sentences: what is being built and the problem it solves.>
### 1.2 Goals
- <Outcome the product must achieve>
### 1.3 Non-goals
- <Things deliberately not being attempted>

## 2. Glossary
| Term | Definition |
|---|---|
| <System name> | The system specified in this document. |

## 3. Users and roles
| Role | Description | Key permissions |
|---|---|---|

## 4. Scope
### 4.1 In scope
| ID | Feature area | Core sub-features | Advanced sub-features |
|---|---|---|---|
| F1 | <Area> | F1.1 <name>, F1.2 <name> | F1.3 <name> |

Core = without it the solution cannot work (the minimal product). Advanced = builds on
the core. An area appears here if any of its sub-features was accepted.
### 4.2 Out of scope
| Item | Tier | Reason |
|---|---|---|
| <Item> | Core / Advanced | Rejected / Deferred to later release / Not selected / Not applicable |
### 4.3 Not yet reviewed
Advanced candidates the interview didn't reach. They are suggestions only — not
requirements, not decisions. Omit this subsection when the advanced round was completed.
- <Area or sub-feature> — <one-line description>

## 5. Functional requirements

### F1 <Feature area name>
<One sentence on what this area covers.>

#### F1.1 <Sub-feature name>
**REQ-F1.1-01 — <short title>**
- **User story:** As a <role>, I want <capability>, so that <benefit>.
- **Tier:** Core | Advanced
- **Priority:** Must | Should | Could
- **Source:** Confirmed | User-added | Assumed | Imported
- **Acceptance criteria:**
  1. WHEN <trigger>, THE <system> SHALL <response>.
  2. IF <unwanted condition>, THEN THE <system> SHALL <response>.
- **Notes:** <user's verbatim wording, rules, values — optional>
- **Legacy ID:** <original ID from an imported foreign document — only in Update mode>

(repeat per requirement; a sub-feature may have several requirements)

## 6. Non-functional requirements
Group by category; each item uses EARS.

### 6.1 Performance
**NFR-PERF-01** — THE <system> SHALL <measurable response>. *(Source: …)*
### 6.2 Security
### 6.3 Privacy and compliance
### 6.4 Availability and reliability
### 6.5 Usability and accessibility
### 6.6 Compatibility and platforms
### 6.7 Observability

## 7. External interfaces and integrations
| System | Direction | Purpose |
|---|---|---|

## 8. Constraints
- <Technology, budget, regulatory, timeline constraints the user stated>

## 9. Assumptions
| ID | Assumption | Affects |
|---|---|---|
| A-01 | <default applied without explicit confirmation> | REQ-… |

## 10. Open questions
| ID | Question | Area | Why it matters |
|---|---|---|---|
| Q-01 | <unexplored or unresolved item> | F4 | <what can't be specified until answered> |

## 11. Traceability
| Requirement | Feature | Sub-feature | Tier | Source | Last changed |
|---|---|---|---|---|---|
| REQ-F1.1-01 | F1 | F1.1 | Core | Confirmed | 0.1 |

## 12. Change history
Omit in a first version (write "Initial version." instead). In Update mode, add a new
entry at the top for each revision; never edit older entries.

### Version <new> — <YYYY-MM-DD>
| Change | ID | Summary |
|---|---|---|
| Added | REQ-F7.1-01 | <one line> |
| Modified | REQ-F2.1-03 | Lockout threshold 5 → 3 failed attempts (user decision) |
| Withdrawn | REQ-F4.2-02 | <one line> — <reason> |
| Resolved | Q-03 | <answer, and the requirement it became> |
| Confirmed | A-02 | Assumption confirmed by the user; Source now Confirmed |

### Withdrawn requirements
| ID | Withdrawn in | Last text | Reason |
|---|---|---|---|
````

## Priority guidance

Default priority from the tier: **Core → Must**, **Advanced → Should**, unless the user
signalled otherwise ("nice to have" → Could, "important but not for launch" → Should; an
Advanced item the user insists on for launch → Must). A Core item is Must by definition —
if the user says it can wait, that is a sign it isn't Core; re-tier it rather than giving
it a lower priority. Don't ask the user to prioritise
every item one by one; if priorities matter to them, offer one prioritisation turn near the
end of the interview.

## Updating an existing spec

- Section numbers and headings stay fixed so versions can be compared side by side.
- Resolved open questions keep their Q-ID retired in section 10 history (remove the row
  from the table; the Change history records where it went). New open questions take the
  next free Q number; the same append-only rule applies to A- and NFR- IDs.
- When a baseline without tiers gets them (user-approved), log it as one Change history
  row ("Tier classification added — n Core, m Advanced") rather than one row per requirement.
- Sub-feature and requirement order follows ID order, so new items appear at the end of
  their area rather than being slotted in by topic.
