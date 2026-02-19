# BulletMark Specification

**A lightweight, human-readable markup format for tasks and notes.**

BulletMark is a personal specification I use for managing my tasks and notes in plain text. It extends standard markdown with intuitive syntax for task management while remainingcompletely readable in any text editor. If you find it useful too, that's great!

## What is Bullet Mark?

BulletMark is a specification for organizing tasks, evens, and notes using enhanced markdown syntax. It combines the simplicity of bullet journaling with the power of structured markup.

```markdown
# 2026-02-04 Wednesday

- [o] 09:00: *Team standup*
- [ ] (A) Review pull requests +CodeReview
    due: 2026-02-04
    - https://github.com/org/repo/pull/123
- [/] Write documentation +Docs
    - Making good progress
- [>] Get feedback from design team @Design
- [x] 2026-02-04 Fix login bug

= Productive morning!
```

## Key Features

- **Multiple Item Types**: Tasks, events, notes, and reflections
- **Organization**: Projects (+Tag) and context (@Tag)
- **Dates**: Due dates, completion dates, and event scheduling
- **Priority Levels**: (A), (B), ..., (Z) for task prioritization
- **Hierarchical**: Nested items with four-space indentation
- **Plan Text**: Works with any text editor
- **Markdown Compatible**: Valid markdown that renders everywhere

## Quick Start

### Item Types

```markdown
- [ ] Incomplete task
- [/] In progress
- [x] Completed task
- [<] Mitigated task, move to backlog
- [>] Delegated/waiting task, move to next day
- [o] Event or time-block
- [-] Cancelled

- Regular note
= Reflection/feeling
```

### Inline Markers

```markdown
- [ ] Task with due date
    due: 2026-02-10
- [x] 2026-02-05 Completed task, with completion date
- [o] 20026-02-06 14:00: *Meeting*
```

## Documentation

See [Spec.md](./Spec.md) for the complete specification including: 
- Detailed syntax rules
- Parsing guidelines
- Complete examples
- Implementation notes

## Implementations

BulletMark implementations across different platforms:

- Yet to come

## Examples

Check the [Examples/](./Examples/) directory for
- Daily planning workflows
- Project tracking templates
- Meeting notes formats
- Common use cases

## Testing 

The [Test/](./Tests/) directory contains shared test fixtures in markdown format that implementations can use to verify compliance with the specification. 

## Contributing

BulletMark is a personal spec that evolves based on my workflow needs. That said, I welcome: 
- Bug reports and clarifications
- Test fixtures and examples
- Non-breaking additions that fit naturally

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details. Breaking changes are unlikely to be accepted, but I'm happy to discuss ideas.

## Design Philosophy

1. **Human-First**: Optimized for reading and writing by humans
2. **Plain Text**: No special tools required
3. **Minimal Syntax**: Small, focused feature set
4. **Unambiguous**: Clear parsing rules
5. **Standard Markdown**: Valid markdown everywhere

## License

This specification is released into the public domain. See [LICENSE](./LICENSE) for details.

## Version 

Current version: **1.0.0**

See [Spec.md](./Spec.md) for version history and changelog.
