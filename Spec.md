# BulletMark Specification

**Version:** 2.0.0
**Last Updated:** 2026-10-22

BulletMark is a lightweight, human-readable markup format for task management and notes. It extends standard markdown with intuitive syntax for tasks, events, and metadata while remaining completely readable as plain text.

## Design Principles

1. **Human-First**: Readable and writable without tooling
2. **Plain Text**: Compatible with any text editor or markdown viewer
3. **Minimal Syntax**: Small learning curve, focused feature set
4. **Unambiguous**: Clear parsing rules without special cases
5. **Standard Markdown**: Valid markdown that renders sensibly everywhere

## Item Types

```
#Header                     Standard markdown header
---                         Section divider (structural, no semantics)
- [ ] Incomplete task       Task not yet started
- [/] In progress           Task currently being worked on
- [x] Completed task        Task finished
- [-] Cancelled             Task/event no longer needed — marked, never deleted
- [>] Migrated              Task pushed forward to a later date (log marker)
- [<] Backlogged            Task returned to a backlog (log marker)
- [o] Event                 Calendar event or time block
- Regular note              Standard bullet point
= Reflection                Personal reflection or feeling
```

**States vs. log markers:** `[ ]` `[/]` `[x]` `[-]` are persistent states. `[>]` and `[<]` record a movement decision at a point in time (e.g. in a daily log); the task itself reappears as `[ ]` wherever it lands.

## Markers

```
+ProjectName    Project tag (at most one per item)
@Tag            Context or tag (multiple allowed)
```

- Names are case-sensitive, start with a letter, and contain only letters, numbers, and hyphens (no spaces)
- An item's text must not begin with a `+Project` or `@Tag` marker; markers follow actual text content

## Dates and Times

Dates use ISO 8601 (`YYYY-MM-DD`), times 24-hour `HH:MM`.

### Completion Dates

Completed tasks carry their completion date at the start:

```
- [x] 2026-01-23 Fixed bug in login flow
```

### Metadata: `due:` and `defer:`

Two `key:value` pairs exist, each on its own indented line below the task:

```
- [ ] Task with a hard deadline
    due: 2026-01-25
- [ ] Task with a deadline time
    due: 2026-01-25 14:00
- [ ] Task that becomes actionable later
    defer: 2026-11-15
```

- `due:` — hard deadline; time component optional
- `defer:` — task is not actionable before this date; date only

These are the ONLY `key:value` formats in BulletMark. All other `key:value` lines are prohibited:

```
# WRONG:
- [ ] Task
    note: Some information

# CORRECT:
- [ ] Task
    - Some information
```

### Events

```
- [o] 09:00: *Team sync*
- [o] 2026-01-25 14:00: *Conference talk*
- [o] 2026-02-01 All-day event
```

Event names are emphasized with asterisks. Date and time are both optional: an optional leading date, an optional `HH:MM:` time, then the name. When the date is omitted it's implicit from context (e.g. the enclosing daily note or date header); time-only is the common form in daily logs.

A cancelled event flips its marker to `[-]`, same as tasks:

```
- [-] 09:15: *Cancelled meeting*
```

## Indentation and Hierarchy

Every level of indentation is exactly **four spaces** (not tabs). Child items provide context, notes, or sub-tasks:

```
- [ ] Main task +Project
    - A note about the task
    - https://example.com/reference
    - [ ] Subtask one
        - Note on the subtask
```

## Complete Example

```markdown
# 2026-02-05 Wednesday

= Feeling good about progress this week

- [o] 09:00: *Standup*
- [-] 09:30: *Cancelled sync*
- [x] 2026-02-05 Morning workout
- [/] Write API documentation +Docs @Work
    - Endpoints section done
    - Authentication chapter remaining
- [ ] Review PRs +CodeReview
    due: 2026-02-06
    - https://github.com/org/repo/pull/123
- [ ] Re-verify staging deploy
    defer: 2026-02-09
- [>] Call Sebbe
- [<] Optimize image loading +Performance
    - Lower priority than accessibility, back to backlog
- [-] Implement carousel component
    - Decided against after UX review
```

## Parsing Rules

### Line Types

1. **Headers:** lines starting with one or more `#`
2. **Section rules:** lines of three or more hyphens (`---`)
3. **Tasks:** lines starting with `- [X]` where X is one of ` `, `/`, `x`, `-`, `>`, `<`, `o`
4. **Notes:** lines starting with `- ` (without checkbox)
5. **Reflections:** lines starting with `= `
6. **Metadata:** indented lines matching `due: YYYY-MM-DD [HH:MM]` or `defer: YYYY-MM-DD`
7. **Other markdown:** everything else (opaque — see below)

### Other Markdown

Beyond the line types above, documents may freely contain any other markdown — blockquotes (`>`), tables, code blocks, footnotes. Parsers treat it as opaque content: preserved verbatim, never interpreted. Rendering or understanding the rest of markdown is an implementation choice, not a requirement.

Long-form content is out of scope: BulletMark is the rapid-logging layer; extended writing lives in separate documents, linked where needed.

### Marker Extraction

- **Projects:** first token matching `\+[A-Za-z][A-Za-z0-9-]*`; at most one per item
- **Tags:** all tokens matching `@[A-Za-z][A-Za-z0-9-]*`
- Item text must not begin with a `+Project` or `@Tag` token

### Indentation

- Count leading spaces; must be divisible by 4
- An item at level N is a child of the nearest preceding item at level N-1

## Write-back and Round-tripping

When an implementation writes BulletMark back to a file:

**Preserve what you don't own**
- Unrecognized lines (e.g. `-missing-space`) are kept verbatim — never silently discarded
- Empty lines are preserved in their relative positions
- Only lines the implementation created or explicitly edited may change

**Insertion position**

Place a new item immediately before the first line of the next recognized item. Unrecognized and empty lines between items stay where they are:

```
- This is valid
                        ← empty line (stays)
-this is unrecognized   ← preserved verbatim (stays)
- [ ] New item          ← inserted here
- This is also valid
```

**New items must be valid**

Tooling must never produce lines a parser would classify as unrecognized.

**No anchor**

If there is no next recognized item, new items are appended at the end of the file. Implementations that hide unrecognized lines in their UI should still make clear those lines exist — they are part of the document.

## Version History

BulletMark uses semantic versioning:

- **X.0.0 — Major:** breaking changes to parsing or syntax
- **x.X.0 — Minor:** additions that don't break existing parsers
- **x.x.X — Patch:** clarifications only

### 2.0.0 (2026-10-22)

- Removed `(A)` priority markers
- Redefined `[>]` as migrated and `[<]` as backlogged (previously delegated/mitigated); clarified both as log markers, not persistent states
- Added `defer:` metadata; `due:` and `defer:` are now the only two `key:value` formats
- Events: emphasized names; date and time both optional (time-only is the common daily-log form); cancelled events use `[-]` like tasks
- Added `---` section rules as a recognized structural line type
- Added "Other Markdown" parsing note: non-BulletMark markdown is allowed in documents but opaque to parsers; long-form journaling ruled out of scope
- Restructured and compressed

### 1.0.0 (2026-02-19)

- Initial specification
