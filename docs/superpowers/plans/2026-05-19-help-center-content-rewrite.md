# Help Center Content Rewrite Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite every content page in the Mintlify help center (`elimubora-docs`) from the product source-of-truth docs, fixing known inaccuracies and gaps, while leaving the meta files and navigation untouched.

**Architecture:** Per-page verify-then-write. For each page, read the matching source doc under `~/Code/elimubora/docs/`, check its `Last verified` SHA, crosscheck the codebase under `~/Code/elimubora` only when a SHA is stale or a guardian-facing claim appears, then rewrite the `.mdx` from the locked page skeleton (Records table, verb-led workflow groups, Statuses and lifecycle, How records relate, Reports and analytics, What guardians see, FAQs). One commit per page on the `docs/content-rewrite` branch. `mint broken-links` runs before every commit.

**Tech Stack:** Mintlify (MDX), Lucide icons, Mermaid for diagrams, `mint` CLI for validation.

---

## Reference

- **Design spec:** `docs/superpowers/specs/2026-05-19-help-center-content-rewrite-design.md` (committed at `3ca6b8f` on `docs/content-rewrite`).
- **Source-of-truth docs:** `~/Code/elimubora/docs/project-overview.md`, `~/Code/elimubora/docs/conventions.md`, `~/Code/elimubora/docs/modules/*.md`.
- **Product codebase:** `~/Code/elimubora`. Latest HEAD at plan time: `2cebfe0`. Guardian-portal merge: `253e249`.
- **Doc conventions:** `AGENTS.md`, `CONTRIBUTING.md`.
- **Style rule (AGENTS.md line 69):** No em-dashes, no emojis, no informal language.

## Execution order (overrides task numbering)

Tasks below are content-grouped by file and keep their original numbers, but they execute in the order below. Two pattern-setters are reviewed closely before any other module page is written.

| Order | Task | Page | Notes |
|---|---|---|---|
| 1 | Task 0 | (pre-flight) | |
| 2 | Task 8 | `modules/finance` | Primary pattern-setter. Pause for close review after commit. |
| 3 | Task 2 | `modules/users` | Secondary pattern-setter. Pause for close review after commit. |
| 4 | Task 1 | `modules/attendance` | Normal review. |
| 5 | Task 3 | `modules/curriculum` | |
| 6 | Task 4 | `modules/academic-years` | |
| 7 | Task 5 | `modules/assessments` | |
| 8 | Task 6 | `modules/timetable` | |
| 9 | Task 7 | `modules/events` | |
| 10 | Task 9 | `modules/inventory` | |
| 11 | Task 10 | `modules/sports` | |
| 12 | Task 11 | `modules/clubs` | |
| 13 | Task 12 | `modules/activity-log` | |
| 14 | Task 13 | `introduction` | |
| 15 | Task 14 | `quickstart` | |
| 16 | Task 15 | `roles-permissions` | |
| 17 | Task 16 | `staff/overview` | |
| 18 | Task 17 | `guardians/overview` | |
| 19 | Task 18 | `settings/overview` | |
| 20 | Task 19 | open PR | |

The "Pause for review" step originally on Task 1 (attendance) moves to Task 8 (finance) and Task 2 (users).

## Locked page skeleton (applies to every module page)

````text
---
title: "[Module name as shown in the sidebar]"
description: "[One sentence stating the user-visible purpose]"
icon: "[lucide-icon-name]"
---

[One short paragraph: what the module does and who uses it.]

[Optional-module note (only on optional modules): a Mintlify Note
component stating that the module is optional and can be enabled or
disabled under Settings.]

## Records you'll work with
| Record | What it is |
|---|---|
| [Record] | [Plain-English definition] |

## What you can do
[CardGroup of anchor cards linking to workflow groups below. Include only
when the page has three or more groups.]

## [Workflow group, named by area of work]

### [Verb-led workflow name]
[Prerequisite callout (only when the workflow has a real dependency): a
Mintlify Note component naming the dependency and linking to where it is
set up. Real dependencies include an integration being configured, a
setting being on, a permission being granted, or another record already
existing. Skip the callout when the prerequisite is satisfied
automatically.]

[Lead with the goal. Steps block. Screenshot placeholder. Name what each
step changes in the system.]

[Insert screenshot: specific page, modal, or form with named fields and
visible state]

## Statuses and lifecycle

### [Record name] statuses
| Status | What it means | Who can act | What they can do |

[Mermaid stateDiagram-v2 block here: every transition, labelled with the
actor and the action.]

## How records relate
[Plain-prose bullets describing user-visible relationships. No erDiagram.
No database table or column names. Skip the section if relationships add
nothing the workflows have not already conveyed.]

## Reports and analytics
[Each report: name, filters available, intended audience, where it lives
in the panel.]

## What guardians see
[Read-only paragraph. List actions hidden from the Guardian role. Skip
the section only when the module exposes nothing to guardians.]

## FAQs and troubleshooting
[AccordionGroup of frequent questions and answers.]
````

**Style rules baked into every page:**

- Active voice, second person.
- Match product UI labels exactly.
- No em-dashes, no emojis, no informal language.
- Headings at `##` and `###` only. Page H1 comes from frontmatter `title`.
- Screenshot placeholders follow `CONTRIBUTING.md`: name the page, the fields or controls, and the state worth showing. Vague placeholders such as `[Insert screenshot: the page]` are rejected.
- Statuses are exhaustive. Transitions are labelled with the actor and the action. Reversibility is stated.
- **Prerequisite callouts.** Every workflow that depends on prior setup, an integration, a setting, or another record existing opens with a `<Note>` callout that names the dependency and links to where it is set up. Skip only when the prerequisite is satisfied automatically.
- **Diagrams.** Use `stateDiagram-v2` for status lifecycles. Do not include `erDiagram`s on any module page. Describe relationships in plain prose when they help understanding.
- **No developer-level detail.** Do not name database tables, columns, migration files, classes, services, jobs, observers, enums, command strings, or methods. Do not use code-style permission identifiers; describe permissions in plain English. The source-of-truth docs under `~/Code/elimubora/docs/` are internal company IP and only guide understanding; they are not copy-pasted.

## Pre-write check for every task

1. Read the matching source doc under `~/Code/elimubora/docs/modules/<name>.md`.
2. Read its `Last verified` SHA. If older than `253e249` (guardian-portal merge) and the page touches guardian behaviour, crosscheck the relevant Filament resources and policies in `~/Code/elimubora`.
3. For every status the page documents, open the matching enum in `~/Code/elimubora/app/Enums/<EnumName>.php` and confirm the cases.
4. For every form field, action, or label the page documents, confirm it in the Filament resource (`~/Code/elimubora/app/Filament/Resources/...`).
5. Read the current `.mdx` file in this repo before overwriting (the `Write` tool requires this).

## Commit message template for every task

```
docs(<page-slug>): rewrite from source doc

Source: ~/Code/elimubora/docs/modules/<name>.md (verified <sha>)
Codebase crosscheck: <yes/no, summary>
Folds in: <known-issue fixes if any>

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
```

---

## Task 0: Pre-flight

**Files:** none.

- [ ] **Step 1: Confirm branch**

```bash
git rev-parse --abbrev-ref HEAD
```
Expected: `docs/content-rewrite`.

- [ ] **Step 2: Confirm working tree is clean**

```bash
git status --porcelain
```
Expected: empty output.

- [ ] **Step 3: Confirm mint CLI is installed**

```bash
which mint && mint --version
```
Expected: a path and a version string. If missing, install with `npm i -g mint` and re-run.

- [ ] **Step 4: Read the spec**

Read `docs/superpowers/specs/2026-05-19-help-center-content-rewrite-design.md` end-to-end so the executor knows the page skeleton, the workflow inventory, and the known-issue map.

- [ ] **Step 5: Confirm source docs are present**

```bash
ls ~/Code/elimubora/docs/modules/
```
Expected: `academic-years.md  academics.md  activity-log.md  attendance.md  clubs.md  curriculum.md  events.md  finance.md  identity.md  inventory.md  sports.md  timetable.md` and a `.gitkeep`.

---

## Task 1: Rewrite `modules/attendance.mdx` (pattern-setter)

**Files:**
- Modify: `modules/attendance.mdx`
- Source: `~/Code/elimubora/docs/modules/attendance.md` (verified `c4e60f6`; fresh)

**Records to cover (Records you'll work with table):**

| Record | Plain-English meaning |
|---|---|
| Register | A daily classroom register for a stream, per session (morning or afternoon). |
| Attendance | One student's presence record on one register. |
| AttendanceFollowUp | A staff follow-up opened against a student's absence. |

**Workflow groups and workflows:**

- **Daily registers:** Generate today's registers, Mark a register (morning or afternoon), Confirm a register, Edit a marked register, View a register's history.
- **Follow-up on absences:** Open a follow-up, Add notes to a follow-up, Resolve a follow-up, View a student's absence history.
- **Guardian notifications:** How absence and late notifications reach guardians, Resend a notification.

**Statuses to document with `stateDiagram-v2`:**

- Register lifecycle. Read enum cases from `~/Code/elimubora/app/Enums/RegisterStatus.php` (confirm exact name in the source doc) and transitions from `attendance.md` Status and Workflows sections.
- Follow-up lifecycle. Read enum cases from the matching `AttendanceFollowUpStatus` enum.

**What guardians see (read-only):** Children's attendance history, absence and late notifications. No mark/edit actions are exposed to the Guardian role.

**Reports and analytics:** Read the `attendance.md` "Reports / exports" section and document each report by name, filters, and audience.

**FAQs to include:** Address at least: "What if a class teacher is absent and no one marks the register?", "Can a register be edited after it is confirmed?", "Why didn't a guardian receive an absence notification?", "How are late arrivals counted?". Answers come from `attendance.md`; if not covered, omit rather than guess.

**Screenshots (specific placeholders):**

- `[Insert screenshot: Registers list for today, showing stream name, session (morning/afternoon), and a status column with badges (Pending, In Progress, Marked, Confirmed)]`
- `[Insert screenshot: Mark register form with the student roster, attendance status radio per student (Present, Absent, Late), and the Save button]`
- `[Insert screenshot: Follow-up record showing the linked student, the absence date, notes timeline, and the Resolve action]`
- `[Insert screenshot: Activity-log entry for a follow-up resolution showing causer, subject, and timestamp]`

- [ ] **Step 1: Read source and crosscheck**

Read `~/Code/elimubora/docs/modules/attendance.md` in full. Confirm `Last verified: c4e60f6`. Open `~/Code/elimubora/app/Filament/Resources/Attendance/` (find the resource directory) to confirm action names and field labels. Open `~/Code/elimubora/app/Enums/` for the Register and AttendanceFollowUp status enums and read every case.

- [ ] **Step 2: Read the current page**

```
Read modules/attendance.mdx
```
This is required before overwriting with `Write`.

- [ ] **Step 3: Write the new page**

Overwrite `modules/attendance.mdx` using the page skeleton above. Cover every record, every workflow group, every workflow, every status, the guardian view, the reports section, and the FAQs as enumerated. Include the four screenshot placeholders.

Frontmatter:

```yaml
---
title: "Attendance"
description: "Mark daily registers and follow up on absences."
icon: "clipboard-list"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```
Expected: zero broken links, or only pre-existing ones in pages not yet rewritten.

- [ ] **Step 5: Commit**

```bash
git add modules/attendance.mdx
git commit -m "docs(attendance): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/attendance.md (verified c4e60f6)
Codebase crosscheck: yes (Filament resource actions, status enums)
Pattern-setter page for the help-center rewrite.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

- [ ] **Step 6: Pause for review**

Hand off for close review. The page sets the pattern for the remaining 11 module pages. If the review changes the skeleton (for example, the shape of the Records table or the placement of the guardian view), update the spec and propagate the change before proceeding.

---

## Task 2: Rewrite `modules/users.mdx`

**Files:**
- Modify: `modules/users.mdx`
- Source: `~/Code/elimubora/docs/modules/identity.md` (verified `bb871a9`; stale, 71 commits behind HEAD)
- Codebase: full crosscheck required.

**Records:** Student, Teacher, Staff, Guardian, StudentGuardian (link), User (tenant account), Role.

**Workflow groups:**

- **Sign in to your panel:** First sign-in for staff, Password reset, Locked accounts.
- **Students:** Admit a student, Edit a student profile, Bulk-import students, Transfer a student between streams, Promote students at year-end, Deactivate or archive a student.
- **Teachers and staff:** Onboard a teacher, Onboard a non-teaching staff member, Edit a profile, Assign roles to a staff account, Deactivate a staff account.
- **Guardians:** Register a guardian, Link a guardian to one or more students, Update guardian contacts, Set the primary guardian, Unlink a guardian.

**Statuses:** Confirm against `identity.md` and the codebase. Likely a small set (active/inactive on user accounts, admission type on students). Render any with a `stateDiagram-v2`.

**Relationships:** `erDiagram` showing Student to Guardian via StudentGuardian, Student to GradeStream, User to Role.

**Guardian view:** Guardians see their own children's profiles. The Guardian role is locked and read-only. Students do not log in.

**Reports and analytics:** From `identity.md` reports section. If absent in the source doc, omit.

**FAQs to include:** "Why can't a teacher see a student?" (role-scoped queries), "What happens when I deactivate a student?", "Can a guardian be linked to children in different schools?" (no; tenancy isolates).

**Screenshots:**

- `[Insert screenshot: Student admission form showing admission number, full name, date of birth, grade stream, guardian linker, and the Save button]`
- `[Insert screenshot: Guardian record showing linked children (names, admission numbers), primary-guardian toggle, and contact fields]`
- `[Insert screenshot: Roles page with the role list, the "Add custom role" button, and the locked Super Admin and Guardian rows]`

- [ ] **Step 1: Read source and crosscheck**

Read `identity.md` in full. Read `~/Code/elimubora/app/Filament/Resources/Students/`, `Teachers/`, `Staff/`, `Guardians/`. Read `~/Code/elimubora/app/Models/Student.php`, `Guardian.php`, `User.php`. Check post-`bb871a9` commits affecting Identity: `git log --oneline bb871a9..HEAD -- app/Filament/Resources/Students app/Filament/Resources/Guardians app/Models/Student.php app/Models/Guardian.php app/Policies/StudentPolicy.php app/Policies/GuardianPolicy.php` from `~/Code/elimubora`. Note any behaviour the source doc misses.

- [ ] **Step 2: Read the current page**

```
Read modules/users.mdx
```

- [ ] **Step 3: Write the new page**

Overwrite `modules/users.mdx` using the page skeleton. Cover every record, workflow, status, relationship, the guardian view, reports, and FAQs as enumerated. Frontmatter:

```yaml
---
title: "Users & Identity"
description: "Manage students, teachers, non-teaching staff, and guardian accounts."
icon: "users"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/users.mdx
git commit -m "docs(users): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/identity.md (verified bb871a9, stale)
Codebase crosscheck: yes (Filament resources for Students, Teachers, Staff,
Guardians; models; post-bb871a9 commit review)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 3: Rewrite `modules/curriculum.mdx`

**Files:**
- Modify: `modules/curriculum.mdx`
- Source: `~/Code/elimubora/docs/modules/curriculum.md` (verified `9074bf7`)

**Records:** Curriculum, CurriculumStage, GradeLevel, GradeStream, Subject, Pathway, GradingScale.

**Workflow groups:**

- **Set up the curriculum:** Choose a curriculum (CBC or 8-4-4), Add stages, Add grade levels, Add streams per grade level.
- **Subjects and pathways:** Add a subject, Assign a subject to a grade level, Create a senior school pathway, Place a student into a pathway.
- **Grading scales:** Define a grading scale, Apply a grading scale to a subject or grade.
- **Cleanup:** Force-delete an empty stage.

**Statuses:** Per the source doc, curriculum entities likely have no lifecycle statuses (only soft deletes). If a `CurriculumType` enum is documented as categorical only, mention it under "Records you'll work with" rather than as a status diagram.

**Relationships:** `erDiagram` showing Curriculum to Stage to GradeLevel to GradeStream, Subject to GradeLevel (and Pathway), Student to Pathway.

**Guardian view:** Curriculum context is not directly visible to guardians, but it determines the structure of their child's report card and timetable. Note this briefly.

**FAQs:** "Can a school run both CBC and 8-4-4?", "How do I rename a stream mid-year?", "What happens to students when a stage is force-deleted?"

**Screenshots:**

- `[Insert screenshot: Curriculum setup wizard with curriculum selector (CBC, 8-4-4), stages list, and Add stage button]`
- `[Insert screenshot: Subject configuration showing grade-level assignment toggles and the assigned grading scale]`
- `[Insert screenshot: Pathway placement form for a Form 4 student with the available pathways listed]`

- [ ] **Step 1: Read source and crosscheck**

Read `curriculum.md`. Confirm enum and resource names in `~/Code/elimubora/app/Enums/CurriculumType.php` and `~/Code/elimubora/app/Filament/Resources/Curriculum*`.

- [ ] **Step 2: Read the current page**

```
Read modules/curriculum.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Curriculum & Structure"
description: "Set up the curriculum, grade levels, streams, subjects, pathways, and grading scales."
icon: "book-open"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/curriculum.mdx
git commit -m "docs(curriculum): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/curriculum.md (verified 9074bf7)
Codebase crosscheck: yes (CurriculumType enum, Filament curriculum resources)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 4: Rewrite `modules/academic-years.mdx`

**Files:**
- Modify: `modules/academic-years.mdx`
- Source: `~/Code/elimubora/docs/modules/academic-years.md` (verified `9074bf7`)

**Records:** AcademicYear, Term.

**Workflow groups:**

- **Set up the year:** Create an academic year, Define terms, Set start and end dates.
- **Run the year:** Activate a year, Activate a term, Close a term, Close a year, Read the current year and term anywhere in the system.

**Statuses with `stateDiagram-v2`:** Read enum cases from `~/Code/elimubora/app/Enums/AcademicYearStatus.php` and `~/Code/elimubora/app/Enums/TermStatus.php`. Document every case and transition (Upcoming, Active, Closed and equivalents).

**Relationships:** Brief: AcademicYear has many Term; Term scopes Assessments, Invoices, Attendance Registers, etc.

**Guardian view:** Not directly. The current year and term context surfaces on every guardian-visible record.

**FAQs:** "What happens to in-flight invoices when a term closes?", "Can two terms be active at once?", "How is the current term determined for daily operations?"

**Screenshots:**

- `[Insert screenshot: Academic years list showing year names, statuses (Upcoming, Active, Closed), and the Activate action]`
- `[Insert screenshot: Term form with start date, end date, and the Set as current term toggle]`

- [ ] **Step 1: Read source and crosscheck**

Read `academic-years.md`. Read `~/Code/elimubora/app/Enums/AcademicYearStatus.php` and `~/Code/elimubora/app/Enums/TermStatus.php`. Read the AcademicYear and Term resources.

- [ ] **Step 2: Read the current page**

```
Read modules/academic-years.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Academic Years & Terms"
description: "Set up academic years and terms; manage their lifecycle."
icon: "calendar"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/academic-years.mdx
git commit -m "docs(academic-years): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/academic-years.md (verified 9074bf7)
Codebase crosscheck: yes (AcademicYearStatus, TermStatus enums; resources)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 5: Rewrite `modules/assessments.mdx`

**Files:**
- Modify: `modules/assessments.mdx`
- Source: `~/Code/elimubora/docs/modules/academics.md` (verified `9074bf7`)

**Records:** Assessment, StudentGrade, ReportCard.

**Workflow groups:**

- **Assessments:** Create an assessment (CAT, end-term, project), Configure the rubric, Open for entry, Close for entry.
- **Grades:** Enter grades for a subject, Bulk-import grades, Edit a grade before publish, View student performance.
- **Report cards:** Generate report cards for a term, Add class teacher remarks, Publish report cards, Download a report card PDF, Republish after a correction.

**Statuses with `stateDiagram-v2`:** `AssessmentStatus` (cases from `~/Code/elimubora/app/Enums/AssessmentStatus.php`) and ReportCard publish lifecycle (read the source doc; if the lifecycle is observer-driven rather than enum-backed, document the states present).

**Relationships:** `erDiagram` showing Assessment to StudentGrade to Student, Assessment to Subject to GradeLevel, ReportCard to Student and Term.

**Guardian view:** Published assessment results and published report cards for the guardian's own children. Mark/edit actions are hidden from the Guardian role. Unpublished items are not visible.

**FAQs:** "Can a teacher edit a grade after publish?", "What if a student is added to a stream after grades are entered?", "Can a report card be unpublished after a correction?"

**Screenshots:**

- `[Insert screenshot: Assessment list filtered by Term, with columns for Title, Subject, Status (Draft, Open, Closed, Published), and the Open for Entry action]`
- `[Insert screenshot: Grade entry table for one subject with student rows, score input, and the rubric reference panel]`
- `[Insert screenshot: Report card preview showing subject scores, class teacher remarks block, and the Publish action]`

- [ ] **Step 1: Read source and crosscheck**

Read `academics.md`. Read `AssessmentStatus.php` and any `ReportCardStatus.php` or report card observer. Read `~/Code/elimubora/app/Filament/Resources/Assessments` and ReportCard resources.

- [ ] **Step 2: Read the current page**

```
Read modules/assessments.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Assessments & Report Cards"
description: "Create assessments, enter grades, and generate term-end report cards."
icon: "graduation-cap"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/assessments.mdx
git commit -m "docs(assessments): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/academics.md (verified 9074bf7)
Codebase crosscheck: yes (AssessmentStatus enum, ReportCard lifecycle,
Filament resources)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 6: Rewrite `modules/timetable.mdx`

**Files:**
- Modify: `modules/timetable.mdx`
- Source: `~/Code/elimubora/docs/modules/timetable.md` (verified `9074bf7`)

**Records:** Timetable, Lesson, SchoolDayPeriod.

**Workflow groups:**

- **Configure school days:** Define school day periods, Set the weekly structure.
- **Build a timetable:** Create a teacher's timetable, Add lessons, Renew a timetable for the next term, View a class timetable.
- **Run lessons:** Expand recurring lessons, Cancel a lesson, Substitute a lesson.

**Statuses:** Document any timetable or lesson lifecycle states (likely simple: Draft, Active, Archived). Confirm against the codebase.

**Guardian view:** State explicitly that the Timetable resource is hidden from the Guardian role per commit `4fc6fd4` (`block TimetableResource for guardians`). Guardians do not see timetables in the panel.

**FAQs:** "How do I substitute a teacher for one lesson?", "What happens to a timetable when a term ends?", "Why does a recurring lesson not appear on a specific date?"

**Screenshots:**

- `[Insert screenshot: Weekly timetable grid for a stream, with days across and periods down, lessons coloured by subject]`
- `[Insert screenshot: School day periods setup with period number, start time, end time, and type (Lesson, Break, Assembly)]`
- `[Insert screenshot: Substitute lesson modal showing the original teacher, the date, the replacement teacher dropdown, and the Save action]`

- [ ] **Step 1: Read source and crosscheck**

Read `timetable.md`. Read the Timetable and Lesson resources. Read the guardian-blocking change at commit `4fc6fd4` in `~/Code/elimubora`.

- [ ] **Step 2: Read the current page**

```
Read modules/timetable.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Timetable & Lessons"
description: "Build weekly timetables and run daily lessons."
icon: "calendar-clock"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/timetable.mdx
git commit -m "docs(timetable): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/timetable.md (verified 9074bf7)
Codebase crosscheck: yes (Timetable, Lesson resources; guardian block at 4fc6fd4)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 7: Rewrite `modules/events.mdx`

**Files:**
- Modify: `modules/events.mdx`
- Source: `~/Code/elimubora/docs/modules/events.md` (verified `9074bf7`)

**Records:** Event, EventAttendee, EventAttachment.

**Workflow groups:**

- **Plan events:** Create an event, Add attachments, Set the event type, Set start and end times.
- **Invite attendees:** Resolve recipients (school-wide, grade, stream, individual), Send invitations.
- **Reminders:** Configure event reminders, Send a manual reminder.
- **Term markers:** Read system-generated term markers on the calendar.

**Statuses:** Document any Event or Invitation status with `stateDiagram-v2`. Confirm enums in `~/Code/elimubora/app/Enums/EventType.php` and any `EventStatus.php`.

**Guardian view:** Guardians see events their child is invited to (read-only). Action buttons such as Send invitation are hidden from the Guardian role.

**FAQs:** "How do I cancel an event after invitations are sent?", "Why didn't a guardian receive an event invitation?", "What is a term marker?"

**Screenshots:**

- `[Insert screenshot: Event calendar view showing colour-coded event types and a selected event card]`
- `[Insert screenshot: Event creation form with title, type, start, end, recipient resolver (school-wide, grade, stream, individual), and attachments]`
- `[Insert screenshot: Reminder configuration showing the trigger time relative to event start and the recipient channels]`

- [ ] **Step 1: Read source and crosscheck**

Read `events.md`. Read `~/Code/elimubora/app/Enums/EventType.php`. Read the Event resource for action names.

- [ ] **Step 2: Read the current page**

```
Read modules/events.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Events"
description: "Plan school events, invite attendees, and send reminders."
icon: "calendar-check"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/events.mdx
git commit -m "docs(events): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/events.md (verified 9074bf7)
Codebase crosscheck: yes (EventType enum, Event resource)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 8: Rewrite `modules/finance.mdx`

**Files:**
- Modify: `modules/finance.mdx`
- Source: `~/Code/elimubora/docs/modules/finance.md` (verified `c4e60f6`; fresh)

**Records:** Billable, BillableVariant, Discount, Invoice, InvoiceLine, InvoicePayment, Earmark, Wallet, MpesaPaymentIntent, PaymentIntegration.

**Workflow groups:**

- **Set up fees:** Create a billable, Add a billable variant, Define a discount, Choose a pricing model.
- **Invoices:** Generate termly invoices, Edit an invoice line, Cancel an invoice, View an invoice's balance and history.
- **Payments:** Record a cash or bank payment, Trigger an M-Pesa STK push, Reconcile an M-Pesa Paybill (C2B) deposit, Refund a payment, Apply a credit note.
- **Wallets:** Top up a wallet, Spend from a wallet (canteen flow), Read the wallet ledger.
- **Earmarks:** Lock funds with an earmark, Release earmarked funds, Read the earmark ledger.
- **Integrations:** Configure M-Pesa, Configure a payment integration.

**Statuses with `stateDiagram-v2`:** `InvoiceStatus`, `PaymentStatus`, `EarmarkStatus`, `QuotationStatus`. Cases come from `~/Code/elimubora/app/Enums/{InvoiceStatus,PaymentStatus,EarmarkStatus,QuotationStatus}.php`.

**Categorical enums to mention (not diagrams):** `EarmarkTransactionType`, `PaymentMethod`, `PaymentProvider`, `DiscountType`, `PricingModel`, `RefundDestination`, `SponsorType`. Surface these inline where they are user-visible (for example, when creating a payment, list `PaymentMethod` options).

**Relationships:** `erDiagram` for Invoice to Student, Invoice has many InvoiceLine, Invoice has many InvoicePayment, Earmark belongs to Wallet, Wallet belongs to Student, MpesaPaymentIntent links to InvoicePayment.

**Guardian view:** Read-only access to children's invoices, payments, wallets, earmarks. The following actions are hidden from the Guardian role (verify against commits `186ca6a`, `2b20f46`, `9461913`): Record Payment, Lock Funds, Release Funds, Wallet Top-Up, Refund. M-Pesa STK push prompts originate from a school action; the guardian receives them on their phone.

**Reports and analytics:** From `finance.md` "Receipts / reports" section. Document the Balances and Aging report and any others by name, filters, audience.

**FAQs:** "How do I handle a partial scholarship?" (credit note vs discount), "What happens when a guardian overpays?", "How do refunds reach a guardian?", "Why did an M-Pesa payment not apply to the right invoice?"

**Screenshots:**

- `[Insert screenshot: Billables list with columns Name, Pricing Model, Default Amount, Status; the New billable button]`
- `[Insert screenshot: Generate Invoices wizard showing Term selector, Grade filter, dry-run preview, and Generate action]`
- `[Insert screenshot: Invoice detail with line items, payment list, balance summary, and the Pay with M-Pesa, Record Payment, and Refund actions]`
- `[Insert screenshot: Wallet ledger for one student showing top-ups, spend entries, earmark holds, and current balance]`
- `[Insert screenshot: Earmark detail showing locked amount, source wallet, intended use, and the Release action]`

- [ ] **Step 1: Read source and crosscheck**

Read `finance.md` in full. Read each status enum listed above. Read `~/Code/elimubora/app/Filament/Resources/` for Invoice, InvoicePayment, Billable, Earmark, Wallet resources. Confirm the action names and the hidden-from-guardian commits.

- [ ] **Step 2: Read the current page**

```
Read modules/finance.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Finance & Fee Management"
description: "Manage fees, invoices, M-Pesa payments, refunds, wallets, and earmarks."
icon: "wallet"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/finance.mdx
git commit -m "docs(finance): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/finance.md (verified c4e60f6)
Codebase crosscheck: yes (status enums, Filament resources, guardian action
hides at 186ca6a, 2b20f46, 9461913)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 9: Rewrite `modules/inventory.mdx`

**Files:**
- Modify: `modules/inventory.mdx`
- Source: `~/Code/elimubora/docs/modules/inventory.md` (verified `9074bf7`)

**Records:** Product, Inventory, InventoryMovement, Supplier, Purchase, PurchaseItem, StockRequisition, LowStockAlert.

**Workflow groups:**

- **Catalogue:** Add a product, Edit a product, Categorise products.
- **Suppliers:** Add a supplier, Edit a supplier, Review a supplier's purchase history.
- **Purchases:** Create a purchase, Receive a purchase into stock, Cancel a purchase.
- **Stock:** View stock levels, Adjust stock, View movement history.
- **Requisitions:** Submit a stock requisition, Approve or reject a requisition, Issue stock against an approved requisition, Mark a requisition received.
- **Low-stock alerts:** Review alerts, Resolve an alert.

**Statuses with `stateDiagram-v2`:** `PurchaseStatus`, `RequisitionStatus`, `AlertStatus`. Cases from `~/Code/elimubora/app/Enums/{PurchaseStatus,RequisitionStatus,AlertStatus}.php`.

**Categorical enums to mention inline:** `ProductCategory`, `MovementType`, `RequisitionReason`, `RequisitionPriority`.

**Relationships:** `erDiagram` showing Product to Inventory to InventoryMovement, Purchase has many PurchaseItem, StockRequisition relates to Product and Requester.

**Guardian view:** Confirm against the codebase. Commit `c4e60f6` mentions "hide assigned items and attendance note actions from guardians", which may include items issued to a student (resources assigned). Verify and document either "Guardians can see items assigned to their children (read-only)" or "Guardians do not see inventory."

**Reports:** Low-stock report, Stock movement report, Purchase history. Names confirmed against `inventory.md`.

**FAQs:** "How do I correct a wrong stock receipt?", "Can a requisition be edited after submission?", "Why didn't a low-stock alert fire?"

**Screenshots:**

- `[Insert screenshot: Products list with columns Name, Category, On Hand, Reorder Level, Status; the New product button]`
- `[Insert screenshot: Purchase detail showing supplier, line items (product, qty, unit cost), status (Draft, Ordered, Received), and the Receive action]`
- `[Insert screenshot: Stock requisition with requester, items, priority (Low, Normal, High), reason, status (Pending, Approved, Issued, Received, Rejected), and the Approve and Reject actions]`
- `[Insert screenshot: Low-stock alert list with product, current quantity, reorder threshold, and the Resolve action]`

- [ ] **Step 1: Read source and crosscheck**

Read `inventory.md`. Read the three status enums. Read the inventory resources. Verify guardian access to assigned items by checking the codebase for `c4e60f6` and any AssignedItem resource policy.

- [ ] **Step 2: Read the current page**

```
Read modules/inventory.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Inventory & Procurement"
description: "Manage products, suppliers, purchases, stock movements, and requisitions."
icon: "package"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/inventory.mdx
git commit -m "docs(inventory): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/inventory.md (verified 9074bf7)
Codebase crosscheck: yes (status enums, Filament resources, guardian access
verification at c4e60f6)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 10: Rewrite `modules/sports.mdx`

**Files:**
- Modify: `modules/sports.mdx`
- Source: `~/Code/elimubora/docs/modules/sports.md` (verified `9074bf7`)

**Records:** Sport, Team, TeamMember, Fixture, Competition, MatchResult, IndividualResult, Award, ExternalSchool, House, HousePointEntry.

**Workflow groups:**

- **Sports setup:** Add a sport, Add a house, Add an external school.
- **Teams:** Create a team, Select the roster, Edit a team.
- **Competitions and fixtures:** Create a competition, Schedule a fixture, Generate a knockout bracket.
- **Results:** Record a team result, Record an individual result, Award trophies.
- **Inter-house points:** Read the automatic sweep on result publish, Record a manual house-points entry, Read house standings.

**Statuses:** Render `stateDiagram-v2` for each lifecycle enum (Team, Fixture, Competition) confirmed against `~/Code/elimubora/app/Enums/` and the `sports.md` "Lifecycle enums" section.

**Categorical enums:** Mentioned inline where user-visible.

**Relationships:** `erDiagram` for Team to TeamMember to Student, Fixture to Team, Competition has many Fixture, MatchResult to Fixture, HousePointEntry to House.

**Guardian view:** Children's team membership, fixtures, results, and awards (read-only). Recording actions are hidden from the Guardian role.

**Reports:** Inter-house standings, results history, team rosters.

**FAQs:** "How do I correct a recorded score?", "What happens to a fixture when a team is disbanded?", "How are inter-house points calculated?"

**Screenshots:**

- `[Insert screenshot: Team detail showing sport, roster (students with positions), captain, coach, and status]`
- `[Insert screenshot: Fixture form with date, home team, away team (or external school), venue, and competition selector]`
- `[Insert screenshot: Result entry for a knockout fixture showing scores, scorers list, and the Publish action]`
- `[Insert screenshot: Inter-house standings widget with house names, total points, and recent entries]`

- [ ] **Step 1: Read source and crosscheck**

Read `sports.md`. Read each lifecycle enum. Read the Sports cluster of Filament resources.

- [ ] **Step 2: Read the current page**

```
Read modules/sports.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Sports & Games"
description: "Manage sports, teams, fixtures, results, and inter-house points."
icon: "trophy"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/sports.mdx
git commit -m "docs(sports): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/sports.md (verified 9074bf7)
Codebase crosscheck: yes (lifecycle enums, sports resources)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 11: Rewrite `modules/clubs.mdx`

**Files:**
- Modify: `modules/clubs.mdx`
- Source: `~/Code/elimubora/docs/modules/clubs.md` (verified `9074bf7`)

**Records:** Club, ClubMembership, ClubMeeting, MeetingAttendance, ClubActivity.

**Workflow groups:**

- **Set up clubs:** Create a club, Set the patron and student leaders, Set the membership policy.
- **Membership:** Add a member, Remove a member.
- **Meetings:** Schedule a meeting, Take attendance, Edit attendance.
- **Activities:** Log a club activity, See it mirrored on the calendar.
- **Reminders:** Configure meeting reminders.

**Statuses:** `MeetingAttendanceStatus` rendered as a `stateDiagram-v2`. Confirm cases in `~/Code/elimubora/app/Enums/MeetingAttendanceStatus.php`.

**Categorical enums to mention inline:** `ClubType`, `MeetingType`, `ClubActivityType`, `ClubPatronRole`, `ClubLeaderRole`.

**Relationships:** `erDiagram` showing Club to ClubMembership to Student, ClubMeeting to MeetingAttendance, ClubActivity to Club.

**Guardian view:** Children's club memberships, meetings, attendance, and activities (read-only). Recording actions hidden.

**FAQs:** "How do I close a club mid-term?", "Can a student belong to more than one club?", "What if a meeting was cancelled but attendance was already recorded?"

**Screenshots:**

- `[Insert screenshot: Club detail with patron, student leaders, type, membership list, and meetings tab]`
- `[Insert screenshot: Meeting attendance roster with status radio (Present, Absent, Excused, Late) per student and the Save action]`
- `[Insert screenshot: Club activity entry showing activity type, date, description, and the calendar mirror toggle]`

- [ ] **Step 1: Read source and crosscheck**

Read `clubs.md`. Read `MeetingAttendanceStatus` enum. Read the Clubs resource cluster.

- [ ] **Step 2: Read the current page**

```
Read modules/clubs.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Clubs & Societies"
description: "Run clubs, schedule meetings, and log activities."
icon: "users-round"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/clubs.mdx
git commit -m "docs(clubs): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/clubs.md (verified 9074bf7)
Codebase crosscheck: yes (MeetingAttendanceStatus enum, clubs resources)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 12: Rewrite `modules/activity-log.mdx`

**Files:**
- Modify: `modules/activity-log.mdx`
- Source: `~/Code/elimubora/docs/modules/activity-log.md` (verified `9074bf7`)

**Records:** Activity entry.

**Note up front:** Activity Log is technically optional in `App\Enums\Module` but functionally always on. Disabling it only hides the UI viewer. Writes still happen across the platform. This is the platform-wide audit carve-out.

**Workflow groups:**

- **Audit trail:** Browse the audit log, Filter by subject, causer, or event, Read an entry's full payload.
- **Module-gated jobs:** Read `module_gate` entries that the system writes when a job is suppressed.

**Statuses:** None.

**Relationships:** Brief: every audit entry references a subject (any model), a causer (a user), and stores `properties` (a JSON diff of changes).

**Guardian view:** None. Activity Log is admin-only.

**FAQs:** "Can I delete an audit entry?" (no), "How long is the audit trail retained?" (per retention pruning rules in `activity-log.md`), "Why do I see entries created by 'System'?"

**Screenshots:**

- `[Insert screenshot: Activity Log list with columns Date, Causer, Event, Subject, Description; filter chips for Causer, Event type]`
- `[Insert screenshot: Audit entry detail showing the changes JSON payload and the linked subject record]`
- `[Insert screenshot: A module_gate entry showing the suppressed job class, module name, and tenant context]`

- [ ] **Step 1: Read source and crosscheck**

Read `activity-log.md`. Skim `~/Code/elimubora/app/Filament/Resources/ActivityLog*` and the carve-out behaviour in `~/Code/elimubora/app/Services/ModuleService.php`.

- [ ] **Step 2: Read the current page**

```
Read modules/activity-log.mdx
```

- [ ] **Step 3: Write the new page**

Frontmatter:

```yaml
---
title: "Activity Log"
description: "Browse the audit trail of every recorded change in your school's data."
icon: "history"
---
```

- [ ] **Step 4: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 5: Commit**

```bash
git add modules/activity-log.mdx
git commit -m "docs(activity-log): rewrite module page from source doc

Source: ~/Code/elimubora/docs/modules/activity-log.md (verified 9074bf7)
Codebase crosscheck: yes (ModuleService carve-out, ActivityLog resource)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 13: Rewrite `introduction.mdx`

**Files:**
- Modify: `introduction.mdx`
- Sources: `~/Code/elimubora/docs/project-overview.md` (verified `c4e60f6`).

**Outline:**

- One paragraph stating what Elimu Bora is: multi-tenant school ERP for Kenyan schools running CBC or 8-4-4, accessed by school staff through a subdomain panel.
- `## Who uses Elimu Bora`: a single section (the duplicate from the current page is removed). Lists school leadership, teaching staff, non-teaching staff, and guardians (logged-in, read-only). States explicitly that students do not log in.
- `## How the platform is organised`: tenant subdomain, the tenant Filament panel, optional modules.
- `## Modules at a glance`: a `CardGroup` linking each of the 12 module pages by their docs.json paths.
- `## Next steps`: a `CardGroup` of two cards linking to `/quickstart` and `/roles-permissions`.

**Known-issue fixes folded in:**

- Remove the duplicate `## Who uses Elimu Bora` section.
- Repoint broken `/roles/Overview` to `/roles-permissions`.
- Repoint broken `/guardians/Overview` to `/guardians/overview`.
- Repoint broken `/roles-and-access` to `/roles-permissions`.
- Drop the broken `/resources/links` card. The global navbar exposes Contact Us.
- Remove the false student-login claim.

**Screenshots:**

- `[Insert screenshot: Tenant panel home page after sign-in showing top navigation, the dashboard widgets, and the module sidebar]`

- [ ] **Step 1: Read source and current page**

Read `~/Code/elimubora/docs/project-overview.md` (in particular the "What it is" section's guardian and student paragraph). Read `introduction.mdx`.

- [ ] **Step 2: Write the new page**

Frontmatter:

```yaml
---
title: "Introduction"
description: "Welcome to Elimu Bora — the school management platform for CBC and 8-4-4 schools."
icon: "notebook-pen"
---
```

The page must contain exactly one `## Who uses Elimu Bora` heading and must not link to `/roles/Overview`, `/guardians/Overview`, `/roles-and-access`, or `/resources/links`.

- [ ] **Step 3: Run broken-links check**

```bash
mint broken-links
```
Expected: zero broken links on `introduction.mdx`.

- [ ] **Step 4: Commit**

```bash
git add introduction.mdx
git commit -m "docs(introduction): rewrite from product overview

Source: ~/Code/elimubora/docs/project-overview.md (verified c4e60f6)
Folds in: removed duplicate \"Who uses\" section, fixed 4 broken internal
links, removed false student-login claim.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 14: Rewrite `quickstart.mdx`

**Files:**
- Modify: `quickstart.mdx`
- Sources: `~/Code/elimubora/docs/project-overview.md`, `conventions.md`, and the relevant module docs.

**Outline (each step a `Step` in a `Steps` block):**

1. Sign in to your tenant subdomain with the Super Admin account from the welcome email.
2. Configure the school identity in Settings: name, logo, contacts, primary colour.
3. Choose the curriculum and build grade levels, streams, and subjects (cross-link to `/modules/curriculum`).
4. Create the academic year and terms; activate the current term (cross-link to `/modules/academic-years`).
5. Add staff and assign roles (cross-link to `/modules/users` and `/roles-permissions`).
6. Add students and link guardians (cross-link to `/modules/users`).
7. If Finance is enabled, set up billables and generate the first term's invoices (cross-link to `/modules/finance`).
8. Start daily operations: begin marking registers and run lessons (cross-link to `/modules/attendance` and `/modules/timetable`).

**Verification required before writing:** Confirm in `~/Code/elimubora` whether MFA is implemented on user accounts, and confirm the exact path to modules toggling in Settings. Drop any claim that fails verification.

**Screenshots:**

- `[Insert screenshot: Settings page showing School profile fields (name, logo upload, contact email, contact phone, primary colour picker)]`
- `[Insert screenshot: Module toggles in Settings showing optional modules with on/off switches]`

- [ ] **Step 1: Read sources and current page**

Read `project-overview.md`, the Settings code in `~/Code/elimubora/app/Filament/Pages/`, and `quickstart.mdx`.

- [ ] **Step 2: Write the new page**

Frontmatter:

```yaml
---
title: "Quick Start"
description: "Set up your school on Elimu Bora in one sitting."
icon: "rocket"
---
```

Drop any unverified claim (for example, MFA if not present; exact Settings menu paths if they do not match).

- [ ] **Step 3: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 4: Commit**

```bash
git add quickstart.mdx
git commit -m "docs(quickstart): rewrite from product setup flow

Sources: ~/Code/elimubora/docs/project-overview.md, conventions.md, module docs
Codebase crosscheck: yes (Settings panel, modules toggling)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 15: Rewrite `roles-permissions.mdx`

**Files:**
- Modify: `roles-permissions.mdx`
- Sources: `~/Code/elimubora/docs/conventions.md` (Authorization section), `~/Code/elimubora/app/Enums/Role.php`, `~/Code/elimubora/app/Enums/Permission.php`, `~/Code/elimubora/app/Enums/PermissionGroup.php`.

**Outline:**

- Concept: permission-based RBAC via `spatie/laravel-permission`. Permissions are the source of truth; roles bundle permissions.
- `## The pre-seeded roles`: list every case in `Role.php` (17 expected) with the intended user and a one-line summary of the default permission scope (from `Role::defaultPermissions()`).
- `## Locked roles`: Super Admin (full access) and Guardian (read-only). State they cannot be edited or deleted in the UI.
- `## Permission groups`: the 14 cases of `PermissionGroup` (Identity, AcademicStructure, Curriculum, Academics, Attendance, Finance, Inventory, Sports, Clubs, Events, Library, Health, Boarding, Settings). Note Library, Health, and Boarding exist for future modules.
- `## What you can do`:
  - Create a custom role.
  - Edit a role's permissions.
  - Assign one or more roles to a user.
  - Combine roles (additive permission model).
- `## Role-scoped visibility`: teachers see only their assigned stream's students; guardians see only their own children.
- `## FAQs and troubleshooting`: rewrite the "cannot create custom roles" answer; "Why can a teacher not see a student?"; "What happens when a user has two roles with overlapping permissions?"

**Known-issue fixes folded in:**

- Replace the false "cannot create custom roles" FAQ.
- Repoint broken `/modules/Users#staff` to `/staff/overview`.
- Remove any false student-login claim.

**Screenshots:**

- `[Insert screenshot: Roles list with the 17 pre-seeded roles, the locked badges on Super Admin and Guardian, and the New role button]`
- `[Insert screenshot: Role edit form showing the permission picker grouped by PermissionGroup (Identity, Curriculum, Finance, etc.) with checkboxes]`

- [ ] **Step 1: Read sources and current page**

Read the Authorization section of `conventions.md`. Read `~/Code/elimubora/app/Enums/Role.php`, `Permission.php`, `PermissionGroup.php`. Read `~/Code/elimubora/app/Filament/Resources/Roles/RoleResource.php`. Read `roles-permissions.mdx`.

- [ ] **Step 2: Write the new page**

Frontmatter:

```yaml
---
title: "Roles & Permissions"
description: "How access is controlled in your school's panel: roles, permissions, and custom roles."
icon: "shield-check"
---
```

- [ ] **Step 3: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 4: Commit**

```bash
git add roles-permissions.mdx
git commit -m "docs(roles-permissions): rewrite from authorization conventions

Sources: ~/Code/elimubora/docs/conventions.md (Authorization), Role/Permission/
PermissionGroup enums, RoleResource
Folds in: corrected \"custom roles\" FAQ, fixed broken /modules/Users#staff link,
removed false student-login claim.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 16: Rewrite `staff/overview.mdx`

**Files:**
- Modify: `staff/overview.mdx` (currently a stub)
- Sources: `~/Code/elimubora/app/Enums/Role.php`, `~/Code/elimubora/app/Enums/PermissionGroup.php`, all module docs.

**Outline:**

- Lead paragraph: this page maps staff roles to the work they do in the system, linking out to the module pages where the work happens.
- `## Teaching staff`:
  - Class teacher: marks daily registers, adds report card remarks, manages classroom-level events. Links to `/modules/attendance`, `/modules/assessments`, `/modules/events`.
  - Subject teacher: enters assessment grades, runs lessons. Links to `/modules/assessments`, `/modules/timetable`.
  - Head of department: subject oversight, departmental assessments. Links to `/modules/assessments`.
- `## Non-teaching staff`:
  - Bursar: fees, invoices, payments, refunds. Links to `/modules/finance`.
  - Registrar: admissions and student records. Links to `/modules/users`.
  - HR: staff records. Links to `/modules/users`.
  - Storekeeper: inventory and procurement. Links to `/modules/inventory`.
  - Librarian: library workflows. Library is a permission group; its module is not yet built. State this explicitly.
  - Nurse: health workflows. Health is a permission group; its module is not yet built. State this explicitly.
  - Matron: boarding workflows. Boarding is a permission group; its module is not yet built. State this explicitly.
- `## How role assignment works`: cross-link to `/roles-permissions`.
- `## First sign-in`: link to `/quickstart`.

**Screenshots:**

- `[Insert screenshot: Staff member profile showing assigned roles (chips) and the scope summary]`

- [ ] **Step 1: Read sources and current page**

Read `Role.php`, `PermissionGroup.php`, and skim each module doc for the staff roles that operate it. Read `staff/overview.mdx`.

- [ ] **Step 2: Write the new page**

Frontmatter:

```yaml
---
title: "Teaching & Non-teaching Staff"
description: "How teaching and non-teaching staff use Elimu Bora day to day."
icon: "briefcase"
---
```

- [ ] **Step 3: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 4: Commit**

```bash
git add staff/overview.mdx
git commit -m "docs(staff-overview): replace stub with full content

Sources: Role.php, PermissionGroup.php, module docs
Covers teaching staff (class teacher, subject teacher, HOD) and non-teaching
staff (bursar, registrar, HR, storekeeper, librarian, nurse, matron) with
cross-links to module pages. Notes Library, Health, and Boarding as
permission groups without modules yet.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 17: Rewrite `guardians/overview.mdx`

**Files:**
- Modify: `guardians/overview.mdx`
- Sources: `~/Code/elimubora/docs/project-overview.md` (the authoritative guardian paragraph), the guardian-portal commits in `~/Code/elimubora` between `816d8b2` and `253e249`.

**Outline:**

- Lead paragraph: guardians log in to the tenant Filament panel under the locked Guardian role. Access is read-only and scoped to their own children's records. Students do not log in.
- `## Getting access`: the school provisions a guardian account, first sign-in, password reset.
- `## What you can see`: children's profiles, attendance, published report cards, invoices, payments, wallets, earmarks, events the child is invited to, club and sports memberships and results.
- `## What you cannot do`: no edits, no recording payments, no top-ups, no wallet operations, no role or settings access. The school records payments. M-Pesa STK push prompts originate from a school action and arrive on the guardian's phone.
- `## Notifications`: M-Pesa receipts, attendance and late alerts, report card publish notices arrive regardless of whether the guardian signs in.
- `## Multiple children`: the panel scopes lists to all linked children at once; document the actual UI behaviour (no child-switcher widget unless the codebase shows one).
- `## FAQs and troubleshooting`: corrections to the current FAQ. Drop the false "Pay Now", "child selector", "mobile app" claims unless verified.

**Verification required before writing:**

- Confirm the panel layout for guardians (single combined list per resource, or a child selector). Inspect `~/Code/elimubora/app/Traits/HasRoleScopedQueries.php` and the Filament guardian dashboard widgets.
- Confirm the password reset flow exists; if it does not, drop the reset card and state the school resets the password.

**Screenshots:**

- `[Insert screenshot: Guardian dashboard showing the children's summary widgets (one card per child with name, grade stream, recent attendance, fee balance)]`
- `[Insert screenshot: Invoice list as a guardian sees it, with no action buttons (Record Payment, Refund are hidden)]`

- [ ] **Step 1: Read sources and current page**

Read the guardian paragraph in `project-overview.md`. Read `HasRoleScopedQueries.php` and the guardian-portal feature commits. Read `guardians/overview.mdx`.

- [ ] **Step 2: Write the new page**

Frontmatter:

```yaml
---
title: "Parents & Guardians"
description: "How parents and guardians sign in to view their children's records."
icon: "user-round-check"
---
```

- [ ] **Step 3: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 4: Commit**

```bash
git add guardians/overview.mdx
git commit -m "docs(guardians): rewrite to reflect read-only locked role

Source: ~/Code/elimubora/docs/project-overview.md (verified c4e60f6)
Codebase crosscheck: yes (guardian-portal commits 816d8b2..253e249,
HasRoleScopedQueries, guardian dashboard widgets)
Removes false claims about guardian-initiated payments, child-selector
widget, and mobile app.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 18: Rewrite `settings/overview.mdx`

**Files:**
- Modify: `settings/overview.mdx`
- Sources: `~/Code/elimubora/app/Filament/Pages/` (settings pages), `~/Code/elimubora/app/Services/ModuleService.php`, and any settings-related sections in `conventions.md`.

**Outline:**

- Lead paragraph: where school-level configuration lives.
- `## School profile`: name, logo, contacts, primary colour. Note what each affects (report cards, invoices, panel branding).
- `## Modules`: enable or disable optional modules. Cross-link to each module page for what it does. State that core modules (Identity, Curriculum, Academic Years) cannot be disabled, and that Activity Log writes continue even when the module is disabled.
- `## Integrations`: M-Pesa, mail, anything else surfaced in the settings panel. Cross-link to the Finance page for M-Pesa workflow details.
- `## Roles and permissions`: cross-link to `/roles-permissions`.
- `## FAQs and troubleshooting`: "Why is a module I enabled still missing from the sidebar?", "What happens to existing data when I disable a module?"

**Verification required:** Confirm every settings page in the Filament panel by reading `~/Code/elimubora/app/Filament/Pages/` and any cluster pages. Drop any claim that does not match the UI.

**Screenshots:**

- `[Insert screenshot: Settings landing page showing the available sections (School profile, Modules, Integrations, Roles)]`
- `[Insert screenshot: Modules toggle page with each optional module as a row, status switch, and a short description]`

- [ ] **Step 1: Read sources and current page**

Read `~/Code/elimubora/app/Filament/Pages/`, `ModuleService.php`, `conventions.md` (Module gating section), and `settings/overview.mdx`.

- [ ] **Step 2: Write the new page**

Frontmatter:

```yaml
---
title: "Settings"
description: "School-level configuration: profile, modules, integrations, and roles."
icon: "settings"
---
```

- [ ] **Step 3: Run broken-links check**

```bash
mint broken-links
```

- [ ] **Step 4: Commit**

```bash
git add settings/overview.mdx
git commit -m "docs(settings-overview): rewrite from Filament settings pages

Sources: ~/Code/elimubora/app/Filament/Pages, ModuleService,
conventions.md (Module gating)

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Task 19: Open the pull request

**Files:** none.

- [ ] **Step 1: Push the branch**

```bash
git push -u origin docs/content-rewrite
```

- [ ] **Step 2: Open the PR**

```bash
gh pr create --base main --head docs/content-rewrite \
  --title "docs: rewrite help center content from product source-of-truth" \
  --body "$(cat <<'EOF'
Rewrites every content page in the help center from the product source-of-truth docs (~/Code/elimubora/docs) with codebase crosschecks. Meta files and docs.json navigation are unchanged.

## What changed
- 12 module pages rewritten with the locked page skeleton (Records, verb-led workflows, statuses with stateDiagram-v2, relationships, reports, guardian view, FAQs).
- 6 Get Started and Settings pages rewritten or expanded.
- Fixed: 5 broken internal links; duplicate "Who uses" section; staff/overview stub; false student-login claims; false "cannot create custom roles" FAQ.
- modules/attendance was reviewed first as the pattern-setter.

## Design and plan
- Spec: docs/superpowers/specs/2026-05-19-help-center-content-rewrite-design.md
- Plan: docs/superpowers/plans/2026-05-19-help-center-content-rewrite.md

## Verification
- mint broken-links ran clean before every commit.
- Status enums and Filament resource labels were verified against ~/Code/elimubora at each module.

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

- [ ] **Step 3: Confirm PR is open**

```bash
gh pr view --json url,number,state
```
Expected: state `OPEN`, URL printed.

---

## Self-review

- **Spec coverage:** Every spec section maps to a task. Module pages: Tasks 1-12. Get Started and Settings: Tasks 13-18. Branching and PR: Tasks 0 and 19. Known-issue map: folded into Tasks 13 (broken links, duplicate, student-login claim), 15 (custom-roles FAQ, broken link, student-login claim), 16 (stub), 17 (guardian inaccuracies). Verification workflow: documented in the "Pre-write check for every task" section and in each task's Step 1.
- **Placeholder scan:** No `TBD` or `TODO`. Workflow lists and screenshot descriptions are specific. The one place the engineer makes a runtime decision is "drop any claim that fails verification" in Tasks 14, 17, 18; the criterion (the codebase) is explicit.
- **Type consistency:** Status enum names (`InvoiceStatus`, `PaymentStatus`, `EarmarkStatus`, `QuotationStatus`, `AssessmentStatus`, `AcademicYearStatus`, `TermStatus`, `PurchaseStatus`, `RequisitionStatus`, `AlertStatus`, `MeetingAttendanceStatus`) match the source-doc enum names. Commit hashes (`bb871a9`, `9074bf7`, `c4e60f6`, `253e249`, `4fc6fd4`, `186ca6a`, `2b20f46`, `9461913`, `816d8b2`) are taken from the product git log. Page slugs match `docs.json` exactly.
