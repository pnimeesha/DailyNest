# DailyNest MVP backlog

Based on `_docs/plan.md` and `_docs/architecture.md`: Django, Django templates, HTMX, Tailwind CSS, and PostgreSQL, with scheduled Django management commands. Each task is a bounded session-sized deliverable; prerequisites identify existing work rather than requiring the assignee to read previous task descriptions.

Each handoff includes this task, the repository, and the relevant source documents or policy decisions. Implement only the stated scope, add focused verification for behavior and permissions, and record larger discoveries as new tasks. Product choices pending in task 2 are not implicitly approved; rewards, points, streaks, and leaderboards remain outside the MVP.

## 1. Set up an empty Django project with a passing test

Goal: Establish a runnable empty project and a passing smoke test.
Description: Create an empty Django project using compatible Python and Django versions based on `_docs/architecture.md`, with pinned dependencies and environment-based configuration. Add one test that verifies the project loads without product features or applying application migrations, and document setup, server startup, and the test command.

## 2. Record MVP behavior decisions

Goal: Resolve the product policies needed for later tasks.
Description: Use `_docs/plan.md` and `_docs/architecture.md` to record agreed owner permissions, chore edit/archive rules, invitation redemption, recurrence/timezone behavior, due-soon windows, reminder timing/channels/recipients, and fairness attribution in `_docs/decisions.md`. Keep this session to policy documentation and mark unanswered choices as pending rather than silently accepting defaults; later tasks depend only on their relevant agreed policies.

## 3. Define the email-based user model

Goal: Establish email identity before the initial database migrations.
Description: Prerequisite: the empty Django project from task 1. Create the `accounts` custom user model, configure it before running initial migrations, define email normalization/uniqueness, and add model tests plus maintenance-admin registration; household roles remain separate.

## 4. Configure PostgreSQL for development and tests

Goal: Make database setup reproducible on a clean machine.
Description: Prerequisites: the project and custom user model from tasks 1 and 3. Configure a supported PostgreSQL release through environment variables, supply a secret-free environment example and local setup instructions, and verify initial migrations and tests against PostgreSQL.

## 5. Add automated project checks

Goal: Detect migration and test failures on each change.
Description: Prerequisite: PostgreSQL setup from task 4. Add a CI workflow that installs pinned dependencies, starts a fresh PostgreSQL test database, checks for missing migrations, and runs the existing tests; document the local equivalent.

## 6. Configure the Tailwind CSS build

Goal: Produce reproducible static styling assets.
Description: Prerequisite: the Django project from task 1. Pin compatible Tailwind and build-tool versions using `_docs/architecture.md`, configure template scanning and static output, and document development and production build commands without adding product screens.

## 7. Add the shared responsive page shell

Goal: Give later screens reusable accessible layout and form styling.
Description: Prerequisite: the CSS build from task 6. Add base Django templates with navigation, messages, form errors, and empty states, then verify a placeholder page on mobile and desktop with keyboard navigation.

## 8. Establish HTMX progressive enhancement

Goal: Let routes return full pages or reusable fragments consistently.
Description: Prerequisite: the page shell from task 7. Pin the chosen HTMX release, add CSRF handling and a demonstration form that works with ordinary POST/redirect and HTMX fragment responses, and document the response convention for later features.

## 9. Add email-and-password registration

Goal: Let a new member create an account.
Description: Prerequisites: the user model and page shell from tasks 3 and 7. Add a Django registration form with password validation and duplicate-email handling, and test valid creation, invalid passwords, and duplicate accounts.

## 10. Add login, logout, and protected-page access

Goal: Authenticate members through Django sessions.
Description: Prerequisites: the user model and page shell from tasks 3 and 7. Add email/password login, POST logout, and reusable protected-page behavior with safe return URLs, and test valid/invalid credentials and unauthenticated access.

## 11. Add password recovery

Goal: Let a member reset a forgotten password.
Description: Prerequisites: authentication and page templates from tasks 7 and 10. Add Django password-reset request and confirmation flows with configurable email and a local mail backend, and test expiry/invalid tokens while avoiding disclosure of account existence.

## 12. Model households and memberships

Goal: Represent household membership and optional owner mode.
Description: Prerequisites: the user model from task 3 and relevant policies from task 2. Add `households` models for household timezone, owner-mode setting, and unique user/household membership with role and active status, preserving memberships referenced by history.

## 13. Implement household permission helpers

Goal: Provide reusable server-side household isolation and capabilities.
Description: Prerequisites: household models from task 12 and permission decisions from task 2. Add membership-scoped resource queries and capability checks accepting actor and household explicitly, and test outsiders, ordinary members, owners, and owner mode enabled/disabled.

## 14. Add household creation

Goal: Let a signed-in person create a household atomically.
Description: Prerequisites: authentication, templates, and household helpers from tasks 7, 10, and 13. Add a household creation form that creates the household and creator membership in one transaction, validates timezone input, and redirects to a household-scoped landing page.

## 15. Add household settings and member listing

Goal: Show household members and allow authorized configuration changes.
Description: Prerequisites: household creation and permission helpers from tasks 13 and 14. Add member listing and authorized timezone/owner-mode settings forms using policies from task 2, and test unauthorized updates and household-scoped navigation.

## 16. Issue and revoke invitations

Goal: Let authorized members share a secure household invitation.
Description: Prerequisites: household helpers and templates from tasks 7 and 13 plus invitation policy from task 2. Add invitation storage, expiry, token/code generation, and issuance/revocation controls, storing token digests and testing invalid or expired invitations.

## 17. Accept invitations after authentication

Goal: Join the invited household without duplicate membership.
Description: Prerequisites: registration/login and invitation storage from tasks 9, 10, and 16. Preserve the invitation through authentication, then redeem it through a CSRF-protected POST with transactional expiry/revocation/redemption checks, and test repeated or simultaneous acceptance.

## 18. Model chore definitions and occurrences

Goal: Separate chore configuration from individual scheduled work.
Description: Prerequisites: household models and policies from tasks 12 and 2. Add `chores` definitions and occurrences with standard fields, UTC deadlines plus local scheduling information, membership-based assignment, snapshots, cancellation/archive state, schedule/deadline revisions, and unique occurrence keys.

## 19. Add chore detail and active-list screens

Goal: Let members inspect household-scoped chore occurrences.
Description: Prerequisites: chore models, permission helpers, and templates from tasks 18, 13, and 7. Add a basic active list and occurrence detail screen with assignment and deadline, excluding cancelled work and preparing to exclude completed work, and test cross-household identifier access.

## 20. Create one-time chores

Goal: Create a chore definition and one occurrence together.
Description: Prerequisites: chore models and screens from tasks 18 and 19. Add a validated creation form for title, description, deadline, and optional assignee, creating both records atomically and checking that assignees belong to the household.

## 21. Edit active chore details

Goal: Let authorized members update open work without rewriting history.
Description: Prerequisites: one-time creation and permissions from tasks 20 and 13 plus edit policy from task 2. Add editing for open occurrence details and definition defaults, apply deadline revision rules, and reject edits to preserved completed snapshots.

## 22. Archive chores and cancel future work

Goal: Retire chores without deleting historical records.
Description: Prerequisites: chore models and permission helpers from tasks 18 and 13 plus archive policy from task 2. Add definition archiving and occurrence cancellation according to that policy, ensuring active lists exclude retired work and recurrence queries skip archived definitions.

## 23. Implement atomic chore claiming

Goal: Allow only one member to claim an available occurrence.
Description: Prerequisites: chore models, detail screens, and permissions from tasks 18, 19, and 13. Add a claim service and POST/HTMX action using a conditional update or lock, and test simultaneous claims, assigned/cancelled work, and outsider requests against PostgreSQL.

## 24. Implement authorized assignment

Goal: Let an authorized member assign open work to another household member.
Description: Prerequisites: chore detail screens and permission helpers from tasks 19 and 13 plus assignment policy from task 2. Add an assignment service/form with transactional state checks and same-household assignee validation, and test denied actions and cancelled/completed work where completion exists.

## 25. Record chore completion and attribution

Goal: Let any household member complete an occurrence exactly once in the database.
Description: Prerequisites: chore models and detail screens from tasks 18 and 19 plus fairness-credit policy from task 2. Add the unique completion record and action storing timestamp, actor, and credited performer, hide completed work from active lists, and test completion by a non-assignee and repeated/concurrent requests.

## 26. Add comments to occurrences

Goal: Support household discussion on active and completed chores.
Description: Prerequisites: occurrence detail screens and membership checks from tasks 19 and 13. Add comment storage and a posting form with author and timestamp, render member text safely, and test access control and posting/viewing on completed work.

## 27. Calculate recurring deadlines

Goal: Provide a deterministic recurrence service for accepted patterns.
Description: Prerequisites: chore definitions from task 18 and recurrence/timezone policy from task 2. Implement recurrence calculation without generating database records, and test supported patterns, month boundaries, daylight-saving transitions, and missed-occurrence rules with fixed time.

## 28. Generate recurring occurrences

Goal: Create upcoming work safely across repeated runs.
Description: Prerequisites: recurrence calculation and chore models from tasks 27 and 18. Implement `generate_occurrences` using shared services, a documented horizon, definition locking, and occurrence-key uniqueness, and test reruns, concurrent runs, archived definitions, and catch-up.

## 29. Add recurring chore creation

Goal: Let members configure a new recurring series.
Description: Prerequisites: one-time forms, recurrence calculation, and generation from tasks 20, 27, and 28. Extend creation with only the accepted recurrence patterns, explain household timezone behavior, and use the generation service to create initial occurrences.

## 30. Reconcile recurring schedule edits

Goal: Apply series changes without duplicating work or changing completion history.
Description: Prerequisites: recurring creation, completion, and editing from tasks 29, 25, and 21 plus series-edit policy from task 2. Add schedule revision and reconciliation for already-generated open occurrences, and test deadline changes, old reminder revision invalidation hooks, and unchanged completed snapshots.

## 31. Define household custom fields

Goal: Let households configure optional extra chore information.
Description: Prerequisites: household permissions and settings from tasks 13 and 15. Add custom-field definitions with a documented small type set and authorized configuration forms, preserving stable keys and retiring definitions instead of deleting history.

## 32. Collect and preserve custom-field values

Goal: Add validated household-specific data to chores.
Description: Prerequisites: custom definitions and chore forms/detail screens from tasks 31, 20, and 21. Add validated values to creation/editing and display, snapshot labels and values on occurrences, and test foreign-household fields, invalid types, and historical preservation after field retirement.

## 33. Add due-soon and overdue dashboard sections

Goal: Show open work grouped by urgency.
Description: Prerequisites: chore models, completion, and permissions from tasks 18, 25, and 13 plus due-soon policy from task 2. Add `dashboard` queries and sections using household-local boundary rules, links to occurrences, and empty states, and test deadline boundaries and exclusion of completed/cancelled work.

## 34. Add member-assignment dashboard sections

Goal: Show each member's responsibilities and available chores.
Description: Prerequisites: the dashboard and assignment data from tasks 33 and 18. Add per-member grouping, a signed-in member filter, and an unassigned section with existing action links, and verify household isolation and empty groups.

## 35. Add the calendar view

Goal: Make household deadlines browsable by month.
Description: Prerequisites: dashboard, templates, and occurrence data from tasks 33, 7, and 18. Add previous/next month navigation with household-timezone queries and links to details, and provide a narrow-screen list alternative with clearly labelled completion state.

## 36. Add completion history

Goal: Review completed work using preserved occurrence details.
Description: Prerequisites: completion records and occurrence snapshots from tasks 25 and 18. Add a paginated history screen with date/credited-member filters, showing completion actor and performer, and test that later definition edits leave the displayed history unchanged.

## 37. Add fairness statistics

Goal: Show each member's share of completed chore counts.
Description: Prerequisites: completion records and dashboard shell from tasks 25 and 33 plus attribution/denominator policy from task 2. Aggregate by credited performer over a selectable date range, show counts and percentages, and handle zero completions and uncredited work without implying effort weighting or rewards.

## 38. Model and select reminder deliveries

Goal: Persist due-soon and overdue reminder events without sending them.
Description: Prerequisites: chore state and deadline revisions from tasks 18, 25, and 30 plus reminder policy from task 2. Add `notifications` delivery records uniquely keyed by occurrence, recipient, channel, stage, and deadline revision, and test repeated selection, changed deadlines, completed/cancelled work, and recipient validity.

## 39. Implement reminder delivery and retries

Goal: Deliver through one accepted channel with recoverable failure handling.
Description: Prerequisites: delivery records from task 38 and a selected channel/provider under task 2. Implement one channel adapter with a local substitute, short transactional leases, eligibility rechecks, retry timing, and provider idempotency where available, and test success, failure, and abandoned-work recovery; create a separate task per additional accepted channel.

## 40. Add the reminder-processing command

Goal: Expose reminder selection and delivery as a scheduled entry point.
Description: Prerequisites: reminder selection and delivery services from tasks 38 and 39. Implement `process_reminders` with bounded batches, injectable current time, run summaries, and meaningful failure exit codes, and test repeated command runs without sending real external messages.

## 41. Add HTMX actions to chore workflows

Goal: Enhance existing actions without changing permission or business rules.
Description: Prerequisites: HTMX conventions and chore actions from tasks 8 and 20–26. Add fragment responses for claim, assignment, completion, and comment forms using existing services, and verify CSRF, error feedback, refreshed dashboard state, and ordinary form fallback.

## 42. Prepare staging deployment configuration

Goal: Make the Django release runnable on a selected host.
Description: Prerequisites: the project, database, CSS build, and accepted hosting choice from tasks 1, 4, and 6. Configure environment-driven production settings, static assets, HTTPS/proxy handling, secrets, migrations, and deployment checks, then document and smoke-test a staging deployment without production release.

## 43. Schedule recurring jobs in staging

Goal: Run recurrence and reminders independently of web traffic.
Description: Prerequisites: staging deployment and both commands from tasks 42, 28, and 40. Configure host-managed schedules from the same release, record intervals and overlap behavior, and verify successful invocation and recovery after a missed run using local/test delivery settings.

## 44. Add operational visibility and database recovery instructions

Goal: Make web/job failures and data recovery observable.
Description: Prerequisites: staging deployment and scheduled commands from tasks 42 and 43. Add web/database health checks and job failure/stale-delivery visibility without logging secrets or invitation tokens, and document the host's database backup process with a verified restore to a disposable database.

## 45. Add authentication and invitation throttling

Goal: Limit repeated attempts on account and invitation entry points.
Description: Prerequisites: authentication, invitation acceptance, and staging host choice from tasks 10, 17, and 42. Configure application or hosting throttling for login, registration, password recovery, and invitation redemption, and verify limits give usable feedback without exposing account existence.

## 46. Validate the two-member MVP journey

Goal: Confirm the agreed household workflow works end to end.
Description: Prerequisites: all accepted MVP feature tasks and staging setup, including tasks 17, 29–40, and 43. Add a focused two-member smoke journey covering invitations, one-time/recurring work, claiming/assignment, non-assignee completion, comments, custom fields, dashboards, history, fairness, and reminders with a test channel; report concrete failures without expanding this task into fixing every issue.

## 47. Review mobile usability and accessibility

Goal: Find interface problems before release.
Description: Prerequisites: the completed household screens and HTMX actions through task 41. Inspect narrow/wide layouts, keyboard navigation, labels, focus after fragment updates, validation feedback, and crowded calendar dates, fixing small presentation issues and recording larger issues as separately scoped tasks.
