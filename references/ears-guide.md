# EARS — Easy Approach to Requirements Syntax

EARS gives every requirement a fixed shape so it is unambiguous and directly testable.
Keywords are written in capitals. `<system>` is the name of the system (or component) being
specified — use the same name throughout the spec.

## The five patterns

| Pattern | Use when | Template |
|---|---|---|
| Ubiquitous | Always true, no trigger | THE `<system>` SHALL `<response>` |
| Event-driven | A trigger causes a response | WHEN `<trigger>`, THE `<system>` SHALL `<response>` |
| State-driven | Applies while a state holds | WHILE `<state>`, THE `<system>` SHALL `<response>` |
| Unwanted behaviour | Errors, failures, abuse, invalid input | IF `<unwanted condition>`, THEN THE `<system>` SHALL `<response>` |
| Optional feature | Applies only when a feature/config is present | WHERE `<feature is included>`, THE `<system>` SHALL `<response>` |

Patterns can be combined, in the order WHERE → WHILE → WHEN/IF:

> WHILE the user is offline, WHEN the user saves a note, THE app SHALL store the note locally
> and mark it "pending sync".

## Rules

- **One SHALL per criterion.** Two behaviours → two criteria.
- **Measurable.** Replace vague words with values: "quickly" → "within 2 seconds at the
  95th percentile"; "large files" → "files larger than 25 MB".
- **Observable response.** The response must be something a test can check (a message
  shown, a record stored, a status code returned, an email sent) — not an intention.
- **Active trigger.** Triggers are events ("WHEN the user submits the form"), not vague
  conditions ("WHEN appropriate").
- **Cover the unhappy path.** For each event-driven criterion, ask what can go wrong and add
  an IF/THEN criterion: invalid input, missing permission, timeout, duplicate, external
  service down.
- **No design.** Say what the system does, not how it is built ("SHALL store the note",
  not "SHALL INSERT into the notes table").
- **Consistent vocabulary.** Terms used in criteria must match the Glossary.

## Examples

Weak → Strong

- "The system should send reminders." →
  WHEN a task is 24 hours from its due date, THE system SHALL send a reminder email to the
  task's assignee.
- "Handle bad passwords." →
  IF a user enters an incorrect password 5 times within 15 minutes, THEN THE system SHALL
  lock the account for 15 minutes and notify the account owner by email.
- "Admins can export data." →
  WHERE the data export feature is enabled, WHEN an admin requests an export, THE system
  SHALL generate a CSV of all records visible to that admin within 60 seconds.
- "Works offline." →
  WHILE the device has no network connection, THE app SHALL allow the user to view all
  previously synced notes.
