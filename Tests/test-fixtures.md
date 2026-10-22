# BulletMark Test Fixtures

This file contains test cases that implementations should handle correctly.

## Test Case 1: Basic Item Types

### Input
```markdown
- [ ] Incomplete task
- [/] In progress task
- [x] Completed task
- [<] Backlogged task
- [>] Migrated task
- [o] Event
- [-] Cancelled task
-     Regular note
=     Reflection
```

### Expected Parsing
- 9 items total
- 7 task items with different statuses
- 1 regular note item
- 1 reflection item

## Test Case 2: Inline Markers

### Input
```markdown
- [ ] Task with project +ProjectName and tag @Context
- [ ] Multiple tags @Tag1 @Tag2 @Tag3
```

### Expected Parsing
- Item 1: projects=['ProjectName'], tags=['Context']
- Item 2: tags=['Tag1', 'Tag2', 'Tag3'] (multiple @Tags are valid)

### Invalid Examples (must be rejected or flagged)
```markdown
- [ ] Task with +Proj1 +Proj2 multiple projects
- [ ] +StartsWithProject text after
- [ ] @StartsWithTag text after
```
- Item 1: invalid — only one +Project per item; first project may be extracted, second ignored or error raised
- Item 2: invalid — item text must not begin with a +Project marker
- Item 3: invalid — item text must not begin with a @Tag marker

## Test Case 3: Due Dates

### Input
```markdown
- [ ] Task with due date
    due: 2026-02-15
- [ ] Another task
    due: 2026-12-31
- [ ] Task with specific deadline time
    due: 2026-02-20 09:00
- [ ] Task with afternoon deadline
    due: 2026-03-01 17:30
```

### Expected Parsing
- Item 1: due_date='2026-02-15', due_time=None
- Item 2: due_date='2026-12-31', due_time=None
- Item 3: due_date='2026-02-20', due_time='09:00'
- Item 4: due_date='2026-03-01', due_time='17:30'

## Test Case 4: Completion Dates

### Input
```markdown
- [x] 2026-02-05 Completed task
- [x] 2026-01-30 Another completed task with +Project
```

### Expected Parsing
- Item 1: status='completed', completion_date='2026-02-05'
- Item 2: status='completed', completion_date='2026-01-30', projects=['Project']

## Test Case 5: Events with Date/Time

### Input
```markdown
- [o] 2026-02-10 14:00: *Team meeting*
- [o] 2026-02-11 All-day event
- [o] 15:30: *Meeting today*
- [-] 16:00: *Cancelled call*
```

### Expected Parsing
- Item 1: type='event', date='2026-02-10', time='14:00', text='Team meeting'
- Item 2: type='event', date='2026-02-11', text='All-day event'
- Item 3: type='event', time='15:30', text='Meeting today' (date implicit from context)
- Item 4: status='cancelled', time='16:00', text='Cancelled call' (cancelled events use [-])

## Test Case 6: Hierarchical Items

### Input
```markdown
- [ ] Parent task +Project
    - This is a note
    - Another note
    - [ ] Subtask one
    - [ ] Subtask two
        - Note on subtask
```

### Expected Parsing
```
- Task (level 0)
  - Note (level 1)
  - Note (level 1)
  - Task (level 1)
  - Task (level 1)
    - Note (level 2)
```

## Test Case 7: Headers

### Input
```markdown
# 2026-02-05 Wednesday

## Morning

- [ ] Task one

## Afternoon

- [ ] Task two
```

### Expected Parsing
- 3 headers (1 × h1, 2 × h2)
- 2 tasks
- Tasks associated with their nearest preceding header

## Test Case 8: Mixed Content

### Input
```markdown
# Daily Log

- [x] 2026-02-05 Morning task +Work
- [o] 14:00: *Afternoon meeting* @Team
- [ ] Evening task +Personal
    due: 2026-02-06
    - Note about task
    - https://example.com
- [/] In progress work +Project
    - Making good progress
    - Need to finish tomorrow

= Good day overall
```

### Expected Parsing
- 1 completed task with date and project
- 1 event with time and tag
- 1 incomplete task with project, due date, and 2 child notes
- 1 in-progress task with project and 2 child notes
- 1 reflection

## Test Case 9: Invalid Indentation

### Input
```markdown
- [ ] Task
  - Invalid: only 2 spaces
    - Valid: 4 spaces from parent
```

### Expected Behavior
Parsers should handle invalid indentation gracefully (either ignore or normalize)

## Test Case 10: Edge Cases

### Input
```markdown
- [ ] Task with +Project123 and @Tag-Name
- [ ] due: 2026-02-05
```

### Expected Parsing
- Item 1: projects=['Project123'], tags=['Tag-Name'] (alphanumeric + hyphens)
- Item 2: no due date — `due:` is only metadata on its own indented line; inline it is plain text

## Test Case 11: Unicode and Special Characters

### Input
```markdown
- [ ] Task with emoji 🎯 +Project
- [ ] Task with åäö characters @Context
- [ ] Task with "quotes" and 'apostrophes'
```

### Expected Parsing
All items should parse correctly, preserving unicode characters

## Test Case 12: Empty Lines and Whitespace

### Input
```markdown
- [ ] Task one

- [ ] Task two

    - Note with leading blank line
```

### Expected Parsing
Empty lines between items should be allowed and ignored

## Test Case 13: Malformed Dates

### Input
```markdown
- [ ] Task
    due: 2026-13-45
- [ ] Task with invalid time
    due: 2026-02-15 25:99
- [ ] Task with invalid defer date
    defer: 2026-02-30
- [x] 2026-99-99 Invalid completion date
- [o] 2026-02-30 25:00: Invalid event
```

### Expected Behavior
Parsers should handle invalid dates and times gracefully (reject or flag as invalid)

## Test Case 14: Complex Nesting

### Input
```markdown
- [ ] Level 0 task
    - Level 1 note
    - [ ] Level 1 task
        - Level 2 note
        - [ ] Level 2 task
            - Level 3 note
```

### Expected Parsing
```
- Task (level 0)
  - Note (level 1)
  - Task (level 1)
    - Note (level 2)
    - Task (level 2)
      - Note (level 3)
```

## Test Case 15: Project and Tag Extraction

### Input
```markdown
- [ ] Start +Project middle @Tag end
- [ ] Text+NotAProject @NotATag text
- [ ] Task with +Valid-Project and @Valid-Tag-Name
```

### Expected Parsing
- Item 1: projects=['Project'], tags=['Tag']
- Item 2: projects=[], tags=[] (no space before +/@ means not a marker)
- Item 3: projects=['Valid-Project'], tags=['Valid-Tag-Name']

### Invalid Examples (must be rejected or flagged)
```markdown
- [ ] +Front @tag text after
- [ ] @Context text content
```
- Item 1: invalid — item text begins with a +Project marker
- Item 2: invalid — item text begins with a @Tag marker

## Test Case 16: Defer Dates

### Input
```markdown
- [ ] Task that becomes actionable later
    defer: 2026-11-15
- [ ] Task with both dates
    defer: 2026-02-10
    due: 2026-02-20
- [ ] Task with deferred time
    defer: 2026-02-10 09:00
```

### Expected Parsing
- Item 1: defer_date='2026-11-15'
- Item 2: defer_date='2026-02-10', due_date='2026-02-20'
- Item 3: invalid — `defer:` takes a date only, no time component

## Test Case 17: Section Rules

### Input
```markdown
# 2026-02-05 Wednesday

- [ ] Morning task

---

- [ ] Afternoon task
```

### Expected Parsing
- 1 header, 2 tasks, 1 section rule
- Section rules (`---`) are structural only — parsers must not attach semantics or hierarchy to them

## Test Case 18: Other Markdown

### Input
```markdown
# Notes

- [ ] Task one

> A blockquote worth keeping

| Col A | Col B |
|-------|-------|
| 1     | 2     |

- [ ] Task two
```

### Expected Parsing
- 2 tasks and 1 header parse normally
- Blockquotes, tables, code blocks and other non-BulletMark markdown are opaque: preserved verbatim, never interpreted
- Opaque content does not affect item parsing or hierarchy
