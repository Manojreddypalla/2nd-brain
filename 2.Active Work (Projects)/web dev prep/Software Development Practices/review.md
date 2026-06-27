## 1. SDLC (Software Development Life Cycle)

### Full Form

**SDLC = Software Development Life Cycle**

### What is SDLC?

SDLC is the complete process of developing software from planning to maintenance.

### Phases of SDLC

```
Planning    ↓Requirement Analysis    ↓System Design    ↓Development    ↓Testing    ↓Deployment    ↓Maintenance
```

### Explanation

### Planning

- Define project goals
- Estimate cost and time
- Identify resources

### Requirement Analysis

- Gather client requirements
- Functional Requirements
- Non-Functional Requirements

### Design

- Database Design
- UI Design
- System Architecture

### Development

- Write source code
- Implement features

### Testing

- Find bugs
- Verify functionality

### Deployment

- Release software to users

### Maintenance

- Fix bugs
- Add new features
- Improve performance

---

# 2. What is Agile?

Agile is a software development methodology that focuses on:

- Iterative development
- Customer feedback
- Team collaboration
- Frequent software releases

Instead of building the whole project at once,

```
Small Feature      ↓Feedback      ↓Improve      ↓Repeat
```

---

## Agile Principles

- Deliver software frequently
- Welcome changing requirements
- Continuous customer feedback
- Team collaboration
- Working software over documentation

---

# 3. Scrum

### What is Scrum?

Scrum is the most popular Agile framework.

It organizes work into **Sprints**.

---

# 4. Sprint ⭐⭐⭐⭐⭐

A Sprint is a fixed development cycle.

Usually

```
1–4 Weeks
```

Most companies use

```
2 Weeks
```

Goal

Deliver a working feature.

---

# 5. Scrum Roles

## Product Owner

Responsible for

- Product vision
- Requirements
- Priorities

---

## Scrum Master

Responsible for

- Managing Scrum process
- Removing blockers
- Helping the team

---

## Development Team

Responsible for

- Designing
- Coding
- Testing
- Delivering software

---

# 6. Product Backlog

Complete list of project features.

Example

```
LoginPaymentSearchNotificationsProfile
```

---

# 7. Sprint Backlog

Tasks selected for the current Sprint.

Example

Sprint 1

```
LoginRegisterForgot Password
```

---

# 8. User Story

A feature written from the user's perspective.

Format

```
As a <user>I want <feature>So that <benefit>
```

Example

```
As a customerI want to reset my passwordSo that I can access my account.
```

---

# 9. Sprint Planning

Meeting before Sprint starts.

Purpose

Choose tasks for the Sprint.

---

# 10. Daily Stand-up

Short daily meeting.

Usually

```
15 Minutes
```

Everyone answers

1. What did I do yesterday?
2. What will I do today?
3. Any blockers?

---

# 11. Sprint Review

Held after Sprint completion.

Purpose

- Demonstrate completed work
- Collect feedback

---

# 12. Sprint Retrospective

Held after Sprint Review.

Discuss

- What went well?
- What didn't?
- Improvements for next Sprint

---

# Scrum Workflow

```
Product Backlog        ↓Sprint Planning        ↓Sprint Backlog        ↓Development        ↓Daily Stand-up        ↓Testing        ↓Sprint Review        ↓Sprint Retrospective        ↓Next Sprint
```

---

# 13. Kanban

Another Agile methodology.

Instead of Sprints, work is visualized on a board.

```
To Do↓In Progress↓Testing↓Done
```

Popular Tools

- Jira
- Trello
- GitHub Projects

---

# 14. Code Review ⭐⭐⭐⭐⭐

Before code is merged,

another developer reviews it.

Purpose

- Find bugs
- Improve readability
- Improve maintainability
- Maintain coding standards
- Share knowledge

---

# 15. Pull Request (PR)

### Full Form

**PR = Pull Request**

A request to merge one branch into another.

Workflow

```
Create Branch↓Write Code↓Commit↓Push↓Pull Request↓Review↓Merge
```

---

# 16. What Reviewers Check

- Correctness
- Readability
- Naming conventions
- Code style
- Performance
- Security
- Edge cases
- Tests

---

# 17. Writing Clean Code

Follow

- Meaningful variable names
- Small functions
- Reusable code
- Proper indentation
- Consistent formatting
- Minimal comments (comment **why**, not **what**)
- Avoid duplicate code

---

# 18. DRY Principle

### Full Form

**DRY = Don't Repeat Yourself**

Bad

```
let tax1 = price1 * 0.18;let tax2 = price2 * 0.18;
```

Good

```
function calculateTax(price){    return price * 0.18;}
```

---

# 19. KISS Principle

### Full Form

**KISS = Keep It Simple, Stupid**

Write the simplest solution that works.

Avoid unnecessary complexity.

---

# 20. YAGNI Principle

### Full Form

**YAGNI = You Aren't Gonna Need It**

Don't build features until they are actually needed.

---

# 21. Unit Testing

Tests one small unit of code.

Example

```
add(2,3)
```

Expected

```
5
```

Frameworks

- Jest
- Vitest
- Mocha

---

# 22. Integration Testing

Tests multiple modules working together.

Example

```
React↓Express API↓MongoDB
```

Ensures all components communicate correctly.

---

# 23. Continuous Feedback

Agile promotes continuous improvement.

```
Develop↓Test↓Review↓Improve↓Repeat
```

---

# 24. Common Agile Tools

- Jira ⭐⭐⭐⭐⭐
- Trello ⭐⭐⭐⭐☆
- GitHub Projects ⭐⭐⭐⭐☆
- Azure DevOps ⭐⭐⭐☆☆

---

# Important Differences

## Agile vs Waterfall

|Agile|Waterfall|
|---|---|
|Iterative|Sequential|
|Flexible|Rigid|
|Frequent releases|One final release|
|Continuous feedback|Feedback at the end|

---

## Product Backlog vs Sprint Backlog

|Product Backlog|Sprint Backlog|
|---|---|
|All project tasks|Tasks for current Sprint|

---

## Sprint Review vs Sprint Retrospective

|Sprint Review|Sprint Retrospective|
|---|---|
|Review product|Improve process|

---

## Unit Testing vs Integration Testing

|Unit Test|Integration Test|
|---|---|
|Tests one module|Tests multiple modules|

---

# Full Forms

- SDLC → Software Development Life Cycle
- Agile → _(Not an acronym)_
- Scrum → _(Not an acronym)_
- PR → Pull Request
- DRY → Don't Repeat Yourself
- KISS → Keep It Simple, Stupid
- YAGNI → You Aren't Gonna Need It

---

# Frequently Asked Interview Questions

1. What is SDLC?
2. Explain the phases of SDLC.
3. What is Agile?
4. What is Scrum?
5. What is a Sprint?
6. What is a Product Backlog?
7. What is a Sprint Backlog?
8. What happens in a Daily Stand-up?
9. What is a Code Review?
10. What is a Pull Request?
11. What is the DRY principle?
12. What is KISS?
13. Difference between Agile and Waterfall?
14. Difference between Unit Testing and Integration Testing?

---

# Revision Checklist ✅

- [ ]  SDLC
- [ ]  Agile
- [ ]  Scrum
- [ ]  Sprint
- [ ]  Scrum Roles
- [ ]  Product Backlog
- [ ]  Sprint Backlog
- [ ]  User Stories
- [ ]  Sprint Planning
- [ ]  Daily Stand-up
- [ ]  Sprint Review
- [ ]  Sprint Retrospective
- [ ]  Kanban
- [ ]  Code Review
- [ ]  Pull Request
- [ ]  Clean Code
- [ ]  DRY
- [ ]  KISS
- [ ]  YAGNI
- [ ]  Unit Testing
- [ ]  Integration Testing
- [ ]  Agile Tools