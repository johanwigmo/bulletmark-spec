# BulletMark Specification

**Version:** 1.0.0
**Last Updated:** 2026-02-19

BulletMark is a lightweight, human-readable markup format for task management and notes. It extends standard markdown with intuitive syntax for tasks, events, and metadata while remaining completely readable as plain text. 

## Design Principles

1. **Human-First**: Readable and writable without tooling
2. **Plain Text**: Compatible with any text editor or markdown viewer
3. **Minimal Syntax**: Small learning curve, focused feature set
4. **Unambiguous**: Clear parsing rules without special cases
5. **Standard Markdown**: Valid markdown that renders sensibly everywhere

## Core Syntax

### Item Types

BulletMark defines several item types, each with a specific marker: 

```
#Header                                 Standard markdown header
- [ ] Incomplete task                   Task not yet started
- [/] In progress                       Task currently being worked on
- [x] Completed task                    Task finished
- [<] Mitigated task, moved to backlog  Task deprioritized
- [>] Delegated task                    Tasked waiting on someone else
- [o] Event/time block                  Calendar event or time allocation
- [-] Cancelled task/event              Task/event no longer needed
- Regular note                          Standard bullet point
= Reflection/feeling                    Personal reflection or feeling
```

### Inline Markers

BulletMark supports inline markers for organization and context:

```
+ProjectName    Project tag (e.g., +AuthRefactor)
@Tag            Context or tag (e.g., @Home, @Work, @Design)
(A)             Priority level, A-Z (placed at start of text, after checkbox
```

**Naming Rules**
- Project and tag names are case-sensitive
- Must start with a letter
- Can contain letters, numbers, and hyphens
- No spaces (use PascalCase, CamelCase or hyphen-separation)

**Placement Rules**
- An item's text must not begin with a `+Project` or `@Tag` marker — they must follow actual text content
- Each item may contain at most one `+Project` marker
- Multiple `@Tag` markers per item are allowed

**Priority Placement:**
```
- [ ] (A) High priority task +Project @Tag
- [/] (B) Medium priority work in progress
```

### Dates and Times

#### Due Dates

Due dates use a special `due:` metadata format:

```
- [ ] Task description
    due: 2026-01-25
- [ ] Task with specific deadline time
    due: 2026-01-25 14:00
```

**Format:** `due: YYYY-MM-DD [HH:MM]`
**Placement:** On its own indented line below the task
**Notes:** This is the ONLY `key:value` format in BulletMark. The time component is optional; when omitted the due date applies to the whole day.

#### Completion Dates

Completed tasks can include a completion date at the start: 

```
- [x] 2026-01-23 Fixed bug in login flow
```

**Format:** `- [x] YYYY-MM-DD Task description`

#### Events with Date/Time

Events can include date and/or time:

```
- [o] 2026-01-23 14:00: Team sync meeting
- [o] 2026-01-23 All-day event
- [o] 14:00: Event today
```

**Format:** `- [o] [YYYY-MM-DD] [HH:MM]: Event description`
**Note:** Event namne can be emphasized with asterisks for better visibility

### Indentation and Hierrchy

**Rule:** Every level of indentation consists of exactly **four spaces** (not tabs).

#### Child Items

Child items provide additional context, notes, or sub-tasks:

```
- [ ] Main task +Project
    - This is a note about the task
    - https://example.com/reference
    - Another detail to remember
    - [ ] Subtask one
    - [ ] Subtask two
        - Note on subtask two
```

#### Prohibited Formats

Do NOT use `key:value` formats other than `due:`:

```
# WRONG: 
- [ ] Task
    note: Some information
    url: https://example

# CORRECT:
- [ ] Task
    - Some information
    - https://example.com
```

## Complete Examples

### Daily Planning

```markdown
# 2026-02-05 Wednesday

## Morning

- [o] 09:00: Standup meeting @Work
- [ ] (A) Review critical PRs +CodeReview
    due: 2026-02-05
    - https://github.com/org/repo/pull/123
    - https://github.com/org/repo/pull/124
- [/] Write API documentation +Docs
    - Completed endpoints section
    - Still need authentication chapter

## Afternoon

- [o] 14:00: Design review with Sarah @Design
- [ ] (B) Refactor authentication flow +AuthRefactor
    - Consider OAuth2 integration
    - Check backwards compatibility
- [>] Get legal approval for TOS changes @Legal
    - Sent to legal team on 2026-02-03

## Personal

- [ ] Schedule dentist appointment @Personal
- [ ] Buy groceries @Errands
    - Milk
    - Eggs
    - Coffee

= Feeling good about progress on the documentation today
```

### Project Tracking

```markdown
# Project: Website Redesign +WebsiteRedesign

## Sprint 3

- [x] 2026-02-01 Set up design system +Frontend
- [x] 2026-02-03 Implement navigation component +Frontend
- [/] Build responsive layouts +Frontend
    - Desktop done
    - Working on tablet breakpoints
    - Mobile pending
- [ ] (A) Add accessibility features +Frontend @A11y
    due: 2026-02-07
    - ARIA labels
    - Keyboard navigation
    - Screen reader testing
- [<] Optimize image loading +Performance
    - Lower priority than accessibility
    - Moving to next sprint

## Blocked/Waiting

- [>] Get brand guidelines from marketing @Marketing
    - Requested 2026-01-28
    - Follow up needed
- [-] Implement carousel component
    - Decided against after UX review
```

### Mixed Content

```markdown
# 2026-02-05 Wednesday

- [x] 2026-02-05 Morning workout @Personal
- [o] 10:00: Client call +ProjectX @Work
- [ ] (A) Finish quarterly report +Reports
    due: 2026-02-06
    - Gather metrics from analytics
    - https://dashboard.example.com
    - Review with manager before submitting
- [/] Learn Rust +Learning
    - Completed chapters 1-3
    - Working through ownership concepts
    - https://doc.rust-lang.org/book/

= Realized I need to delegate more tasks to focus on high-priority items

- Meeting notes from client call
    - Client happy with progress
    - Requested two new features
    - Budget discussion next week
```

## Parsing Rules

### Line Types

A parser should recognize lines as: 

1. **Headers:** Lines starting with one or more `#` characters
2. **Task itemsa:** Lines starting with `- [X]` where X is one of : ` `, `/`, `x`, `<`, `>`, `o`, `-`
3. **Regular bullets:** Lines starting with `- ` (without checkox)
4. **Reflections:** Lines starting with `= `
5. **Due dates:** Indented lines matching pattern `due: YYYY-MM-DD [HH:MM]`
6. **Regular markdown:** Everything else

### Date Parsing

- **ISO 8601 format:** `YYYY-MM-DD`
- **Time format:** `HH:MM` in 24-hour format
- Times without dates implicityly refer to the current or most recent date header

### Marker Extraction

- **Projects:** Extract the first token matching `+[A-Za-z][A-Za-z0-9-]*`; only one +Project is valid per item
- **Tags:** Extract all tokens matching `@[A-Za-z][A-Za-z0-9-]*`; multiple tags per item are allowed
- **Priority:** Extract `([A-Z])` at the start of task text (after checkbox)
- **Item text** must not begin with a `+Project` or `@Tag` token; markers appearing as the very first token of the text content are invalid

### Indentation

- Count leading spaces
- Must be divisible by 4 to be valid indentation
- Item at level N is child of the nearest preceding item T level N-1

## Implementation Notes

### Recommended Features

While not part of the core spec, implementations may want to provide: 

- Task filtering by project, tag, status, or priority
- Due date calculations and overdue warnings
- Task statistics and completion rates
- Export to other formats (JSON, CSV)

### Rendering Considerations

BulletMark should render reasonably in any markdown viewer: 

- Task checkoxes rendre as standard markdown checkboxes
- In-progress `[/]` renders as `[/]` (clear enough)
- Delegated `[>]` and backlog `[<]` render as directional indicators
- Event marker `[o]` suggests a circle/dot (calendar event)
- Cancelled marker `[-]` shows strikethrough intent
- Reflections with `k` appear as emphasized lines

### Write-back and Round-tripping

When an implementation writes BulletMark back to a file (after parsing, editing, or inserting), it should follow these guidelines.

#### Preserve What You Don't Own

- **Unrecognized lines** (e.g. `-missing-space` or unknown syntax) must be kept verbatim — never silently discarded
- **Empty lines** must be preserved in their relative positions; they carry no semantic meaning but may be intentional for readability
- **Only lines the implementation created or explicitly edited** should change; everything else stays exactly as found

#### Insertion Position

When inserting a new item between two existing items, place the new line immediately before the first line of the next recognized item. Unrecognized lines and empty lines between those two items remain where they are.

```
- This is valid
                       ← empty line (stays)
-this is unrecognized  ← preserved verbatim (stays)
- [ ] New item         ← inserted here, just before the next recognized item
- This is also valid
```

#### New Items Must Be Valid

Tooling must always write syntactically valid BulletMark when creating new items. Implementations should never produce lines that would be classified as unrecognized by a parser.

### Extension Points

Implementations may exted BulletMark with:

- Additional item types (with different markers)
- Custom metadata fields (with clear namespacing)
- Integration with external tools ocalendars, issue trackres)

**Guideline:** Extensions should degrade gracefully in basic markdown viewers.

## Version History

BulletMark uses semantic versioning with three tiers:

- **X.0.0 — Major:** Breaking changes to parsing or syntax
- **x.X.0 — Minor:** Additions or bug fixes that should not break existing parsers
- **x.x.X — Patch:** Clarifications only; no change to actual parsing behaviour

### 1.0.0 (2026-02-19)
- Initial specification
- Core item types and markers
- Date and time formats
- Indentation rules
- Examples and parsing guidelines
