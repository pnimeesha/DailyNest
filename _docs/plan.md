# Shared Household Chores Tool — Scope

## Product concept

A responsive web app for a single household of 2–4 people to coordinate shared chores.

## Agreed requirements

### Households and members

- A household is created by one person.
- Members join through an invitation link or code.
- Each member signs in with an email address and password.
- An optional owner/admin role is supported. Households may choose whether to use it.

### Chores

- Support both recurring chores and one-time chores.
- Members can claim available chores themselves.
- A member with appropriate control can assign a chore to someone else.
- Standard chore fields:
  - Title
  - Description
  - Due date
  - Recurrence schedule
  - Assignee
- Households can add optional custom fields.

### Completion

- Anyone can mark a chore complete, including the assigned person.
- Completed chores remain in history and are hidden from active views.
- Members can leave updates, questions, and comments on chores.

### Dashboard and reminders

The main dashboard includes separate sections for:

- Chores due soon
- Overdue chores
- A calendar view
- Each person’s assigned chores

The app sends reminders before chores are due and again when they become overdue.

### History and fairness

- Keep completion history.
- Show each person’s share of completed chores to support fairness tracking.

## Initial MVP boundary

The first version should focus on household creation, invitations, authentication, chore creation/editing, claiming and assignment, recurrence, completion, comments, reminders, dashboard views, history, and fairness statistics.

## Not yet decided

- Whether to include rewards, points, streaks, or leaderboards.
- Exact admin permissions.
- Reminder delivery channels (in-app, email, push, or a combination).
- Exact recurrence patterns and timezone behavior.
- Whether members can edit or delete chores created by others.

