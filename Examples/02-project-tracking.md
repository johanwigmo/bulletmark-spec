# BulletMark Examples - Project Tracking

This example demonstrates how to use BulletMark for project and sprint management.

## Example 1: Sprint Planning

```markdown
# Sprint 5 - Mobile App Redesign +MobileRedesign

## Sprint Goal
Implement new navigation system and update home screen

## In Progress

- [/] Build bottom navigation component +Frontend
    - Basic structure complete
    - Adding animations
    - Testing on different screen sizes
    due: 2026-02-08

- [/] Design home screen widgets +Design
    - Weather widget done
    - Calendar widget in progress
    - News feed widget pending

## To Do

- [ ] (A) Implement new navigation system +Frontend
    due: 2026-02-10
    - https://figma.com/file/nav-designs
    - Use React Navigation 6
    - Add gesture support

- [ ] (B) Update home screen layout +Frontend
    - Integrate widgets
    - Add customization options
    - Performance optimization

- [ ] (C) Write unit tests for navigation +Testing
    - Test all navigation flows
    - Test deep linking
    - Test state persistence

## Blocked

- [>] Get icon assets from design team @Design
    - Requested 2026-02-03
    - Blocking home screen work

- [>] API endpoint for widget data @Backend
    - Backend team working on it
    - Expected 2026-02-09

## Completed

- [x] 2026-02-05 Research navigation libraries +Research
- [x] 2026-02-06 Create navigation prototype +Frontend
- [x] 2026-02-07 Design review and approval @Design

## Moved to Backlog

- [<] Add dark mode support +Frontend
    - Lower priority for this sprint
    - Will revisit in Sprint 6

- [<] Accessibility improvements +A11y
    - Important but not blocking
    - Plan for dedicated sprint

## Notes

- Team velocity: ~15 story points per sprint
- Consider breaking up large tasks for better tracking
- Design team capacity limited this sprint

= Team morale good, making solid progress
```

## Example 2: Feature Development

```markdown
# Feature: User Authentication +AuthFeature

## Overview
Implement secure authentication system with OAuth2 support

## Phase 1: Research & Planning

- [x] 2026-01-15 Research authentication options +Research
    - Evaluated Auth0, Firebase, custom solution
    - Decided on OAuth2 with JWT tokens
    
- [x] 2026-01-18 Design authentication flow @Design
    - Created user journey maps
    - Designed login/signup screens
    
- [x] 2026-01-20 Technical architecture review @Team
    - Approved OAuth2 approach
    - Defined API contracts

## Phase 2: Implementation

- [x] 2026-01-25 Set up OAuth2 server +Backend
- [x] 2026-01-28 Implement JWT token generation +Backend
- [/] Build login UI +Frontend
    - Form validation complete
    - Adding social login buttons
    - Need to integrate with backend
    
- [ ] (A) Implement token refresh mechanism +Backend
    due: 2026-02-10
    - Automatic refresh before expiry
    - Handle refresh failures
    
- [ ] (A) Add social login providers +Backend
    due: 2026-02-12
    - Google
    - GitHub
    - Apple

## Phase 3: Security & Testing

- [ ] (A) Security audit +Security
    due: 2026-02-15
    - Penetration testing
    - Code review
    - Dependency audit
    
- [ ] Write comprehensive tests +Testing
    - Unit tests for auth logic
    - Integration tests for flows
    - End-to-end tests
    
- [>] Get security team approval @Security
    - Scheduled review for 2026-02-16

## Phase 4: Documentation & Launch

- [ ] Write user documentation +Docs
    - Login/signup guide
    - Password reset process
    - Account security tips
    
- [ ] Write developer documentation +Docs
    - API reference
    - Integration guide
    - Security best practices
    
- [ ] (A) Prepare launch checklist +Launch
    due: 2026-02-20
    - Monitoring setup
    - Rollback plan
    - User communication

## Technical Debt

- [<] Migrate from old session system
    - Plan gradual migration
    - Support both systems temporarily
    
- [<] Improve error messages
    - User-friendly messages
    - Better debugging info

## Risks & Issues

- Performance concerns with token validation
    - Consider caching strategies
    - Load testing needed
    
- Third-party provider dependencies
    - Have fallback plan
    - Monitor provider status

= Complex feature but good progress so far
```

## Example 3: Bug Tracking

```markdown
# Bug Tracking - Week of 2026-02-05 +BugFixes

## Critical (P0)

- [ ] (A) Fix login failure on iOS 17 +Mobile
    due: 2026-02-06
    - https://github.com/org/app/issues/234
    - Affects 15% of iOS users
    - Root cause identified: OAuth redirect issue
    - Working on fix

## High Priority (P1)

- [/] Fix data sync issues +Backend
    due: 2026-02-08
    - https://github.com/org/app/issues/229
    - Race condition in sync logic
    - Fix implemented, testing in progress
    
- [ ] (B) Resolve image upload errors +Storage
    due: 2026-02-09
    - https://github.com/org/app/issues/241
    - Only affects large images
    - Need to implement chunked upload

## Medium Priority (P2)

- [ ] Fix typos in error messages +UI
    - https://github.com/org/app/issues/238
    - Low impact but poor user experience
    
- [ ] Improve loading state UI +Frontend
    - https://github.com/org/app/issues/245
    - Currently confusing to users

## Low Priority (P3)

- [<] Optimize database queries +Performance
    - https://github.com/org/app/issues/201
    - Not urgent, moving to performance sprint
    
- [<] Update deprecated dependencies +Maintenance
    - https://github.com/org/app/issues/189
    - Schedule for next maintenance window

## Resolved This Week

- [x] 2026-02-05 Fixed navigation crash on Android +Mobile
    - https://github.com/org/app/issues/232
    
- [x] 2026-02-06 Resolved memory leak in image cache +Performance
    - https://github.com/org/app/issues/227
    
- [x] 2026-02-07 Fixed incorrect date formatting +UI
    - https://github.com/org/app/issues/240

## Won't Fix

- [-] Support for IE 11 +Frontend
    - https://github.com/org/app/issues/156
    - Product decision: drop IE 11 support
    
- [-] Add feature X in legacy view
    - https://github.com/org/app/issues/178
    - Legacy view being deprecated

= Good bug resolution rate this week
```

## Example 4: Release Planning

```markdown
# Release 2.0 Planning +Release2.0

## Release Date
Target: 2026-02-28

## Must Have (Blocking Release)

- [ ] (A) Complete authentication overhaul +AuthFeature
    due: 2026-02-20
    - [x] 2026-02-10 Backend implementation
    - [/] Frontend integration
    - [ ] Security audit
    - [ ] Documentation
    
- [ ] (A) Fix all P0/P1 bugs +BugFixes
    due: 2026-02-18
    - [x] 2026-02-05 iOS login issue
    - [/] Data sync issues
    - [ ] Image upload errors
    
- [ ] (A) Performance improvements +Performance
    due: 2026-02-22
    - [ ] Reduce app startup time
    - [ ] Optimize API response times
    - [ ] Minimize bundle size

## Nice to Have (Not Blocking)

- [ ] (B) Dark mode support +Feature
    - Design complete
    - Implementation ready
    - Can ship in 2.1 if needed
    
- [ ] (B) New onboarding flow +UX
    - Improves first-time experience
    - Can defer if timeline tight
    
- [<] Advanced search features +Feature
    - Moving to 2.1 release
    - Not critical for launch

## Release Tasks

- [ ] (A) Update changelog +Docs
    due: 2026-02-25
    
- [ ] (A) Prepare release notes +Marketing
    due: 2026-02-25
    
- [ ] (A) Submit to app stores +Deploy
    due: 2026-02-26
    - iOS App Store
    - Google Play Store
    
- [ ] (A) Prepare rollback plan +Deploy
    due: 2026-02-24
    
- [ ] Schedule release announcement +Marketing
    due: 2026-02-28

## Testing & QA

- [/] Beta testing program +QA
    - 50 beta users enrolled
    - Collecting feedback
    - No critical issues reported
    
- [ ] (A) Final QA pass +QA
    due: 2026-02-23
    - Regression testing
    - Cross-platform testing
    - Performance testing
    
- [>] Security review +Security
    - Scheduled for 2026-02-21
    - Waiting on security team availability

## Marketing & Communication

- [ ] Create announcement blog post +Marketing
- [ ] Prepare social media content +Marketing
- [ ] Update website with new features +Marketing
- [ ] Email existing users about update +Marketing

## Post-Release

- [ ] Monitor error rates +Support
- [ ] Track adoption metrics +Analytics
- [ ] Gather user feedback +Support
- [ ] Plan 2.0.1 patch if needed +Planning

= Ambitious timeline but achievable with current progress
```

## Key Patterns

### Milestone Tracking
```markdown
# Milestone: API v2 Launch +APIv2

## Week 1
- [x] 2026-02-01 Design API contracts
- [x] 2026-02-03 Set up new endpoints

## Week 2
- [/] Implement core functionality
- [ ] Write documentation

## Week 3
- [ ] Testing and bug fixes
- [ ] Launch preparation
```

### Dependency Management
```markdown
- [x] 2026-02-05 Complete database migration +Backend
- [/] Update API to use new schema +Backend
    - Depends on migration above
- [ ] Update frontend to use new API +Frontend
    - Depends on API update
```

### Risk Tracking
```markdown
## Risks
- [>] Waiting on third-party API approval
    - High risk if delayed
    - Fallback: use alternative provider
    - Mitigation: start approval process early
```
