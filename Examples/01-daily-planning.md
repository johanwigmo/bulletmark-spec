# BulletMark Examples - Daily Planning

This example demonstrates how to use BulletMark for daily task management and planning.

## Example 1: Basic Daily Log

```markdown
# 2026-02-05 Wednesday

- [o] 09:00: Team standup @Work
- [ ] Review pull requests +CodeReview
- [/] Write API documentation +Docs
    - Completed authentication section
    - Working on error handling
- [x] 2026-02-05 Fixed bug in login flow +BugFix
- [ ] Buy groceries @Errands
    - Milk
    - Eggs
    - Bread
```

## Example 2: Daily Planning with Priorities

```markdown
# 2026-02-06 Thursday

## Work

- [ ] (A) Finish quarterly report +Reports
    due: 2026-02-07
    - Gather metrics from Q4
    - Review with manager
- [ ] (B) Update documentation +Docs
- [o] 14:00: Client meeting +ProjectX
- [>] Get design feedback from Sarah @Design
    - Sent mockups on 2026-02-04

## Personal

- [ ] (A) Schedule dentist appointment @Personal
    due: 2026-02-08
- [ ] Exercise @Health
- [<] Research vacation destinations @Personal
    - Not urgent, moving to backlog

= Feeling more organized with priorities set
```

## Example 3: Work from Home Day

```markdown
# 2026-02-07 Friday

## Morning Block (9:00 - 12:00)

- [o] 09:00: Deep work session +Feature
- [ ] (A) Implement authentication flow +AuthRefactor
    due: 2026-02-07
    - Add OAuth2 support
    - Test with different providers
    - Update error messages
- [/] Write unit tests +Testing
    - Completed auth tests
    - Need to add integration tests

## Afternoon Block (13:00 - 17:00)

- [o] 13:00: Lunch break
- [o] 14:30: Sprint planning @Team
- [ ] (B) Code review +CodeReview
- [ ] Update project board +Admin
- [-] Refactor old API
    - Not needed after architecture discussion

## Notes

- Team decided to postpone API refactor
- New authentication approach looks promising
- Need to follow up with Sarah on designs

= Good progress on auth feature, feeling confident about deadline
```

## Example 4: Multi-Project Day

```markdown
# 2026-02-08 Saturday

## Client A +ClientA

- [x] 2026-02-08 Deployed hotfix to production
- [ ] Schedule follow-up call
    due: 2026-02-10
- [>] Waiting on client approval for Phase 2

## Client B +ClientB

- [/] Building new dashboard feature
    - UI mockups complete
    - Backend API in progress
    - Need to integrate charts library
- [ ] (A) Fix reported bug in export function
    due: 2026-02-09
    - https://github.com/clientb/app/issues/42

## Personal Projects +SideProject

- [ ] Update portfolio website @Personal
    - Add recent projects
    - Refresh design
- [ ] Write blog post about authentication @Writing
    - Draft outline done
    - Research best practices

## Admin

- [ ] Send invoices @Business
- [ ] Backup important files @Maintenance

= Balanced day across multiple projects
```

## Example 5: Meeting-Heavy Day

```markdown
# 2026-02-09 Sunday

- [o] 09:00: 1:1 with manager @Work
    - Discussed Q1 goals
    - Career development planning
    - New project assignment

- [o] 10:30: Architecture review @Team
    - Reviewed database schema changes
    - Decided on migration strategy
    - Action items assigned

- [ ] (A) Prepare presentation for stakeholders +ProjectX
    due: 2026-02-10
    - Create slide deck
    - Add metrics and charts
    - Practice timing

- [o] 14:00: Stakeholder demo @ProjectX
- [o] 15:30: Team retrospective @Team

- [x] 2026-02-09 Sent follow-up emails from meetings

= Lots of meetings but productive discussions
```

## Key Patterns

### Time Blocking
```markdown
- [o] 09:00: Deep work block
- [o] 12:00: Lunch
- [o] 14:00: Meetings
```

### Task Dependencies
```markdown
- [x] 2026-02-05 Research options +Project
- [/] Make decision based on research +Project
- [ ] Implement chosen solution +Project
    - Depends on decision above
```

### Daily Reflections
```markdown
= Productive morning, struggled in afternoon
= Need to schedule more focused work time
= Happy with progress on main project
```

### Context Switching
```markdown
## Focus Time
- [o] 09:00: Work on +ProjectA @DeepWork

## Communication Time  
- [o] 14:00: Respond to emails @Communication
- [o] 15:00: Team sync @Communication
```
