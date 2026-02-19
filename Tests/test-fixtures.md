# BulletMark Test Fixtures

This file contains test cases that implementations should handle correctly.

## Test Case 1: Basic Item Types

### Input
```markdown
- [ ] Incomplete task
- [/] In progress task
- [x] Completed task
- [<] Backlog task
- [>] Delegated task
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
- [ ] (A) Priority task +Project
- [ ] (B) Medium priority with @Tag
```

### Expected Parsing
- Item 1: projects=['ProjectName'], tags=['Context']
- Item 2: tags=['Tag1', 'Tag2', 'Tag3'] (multiple @Tags are valid)
- Item 3: priority='A', projects=['Project']
- Item 4: priority='B', tags=['Tag']

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
- [o] 2026-02-10 14:00: Team meeting
- [o] 2026-02-11 All-day event
- [o] 15:30: Meeting today
```

### Expected Parsing
- Item 1: type='event', date='2026-02-10', time='14:00', text='Team meeting'
- Item 2: type='event', date='2026-02-11', text='All-day event'
- Item 3: type='event', time='15:30', text='Meeting today'

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
- [o] 14:00: Afternoon meeting @Team
- [ ] (A) Evening task +Personal
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
- 1 incomplete task with priority, project, due date, and 2 child notes
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
- [ ] Task with (A) priority at start
- [ ] Task with priority at end (B)
- [ ] Task with multiple (A) (B) priorities
- [ ] due: 2026-02-05
```

### Expected Parsing
- Item 1: projects=['Project123'], tags=['Tag-Name'] (alphanumeric + hyphens)
- Item 2: priority='A' (at start after checkbox)
- Item 3: priority=None (not at start)
- Item 4: priority='A' (first one wins)
- Item 5: Should handle gracefully (due date not indented)

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

## Test Case 16: Priority Parsing

### Input
```markdown
- [ ] (A) Valid priority at start
- [ ] Text (A) priority in middle
- [ ] (X) Invalid priority letter
- [ ](A) No space after checkbox
- [ ] (A) (B) Multiple priorities
```

### Expected Parsing
- Item 1: priority='A'
- Item 2: priority=None (not at start)
- Item 3: priority=None (invalid letter)
- Item 4: priority=None or 'A' (implementation dependent)
- Item 5: priority='A' (first one wins)
