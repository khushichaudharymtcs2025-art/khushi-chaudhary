# Assessment Test: Software Tester (QA)

**Duration:** 01:15 Hrs  
**Full Name:** Khushi Choudhary  
**Email ID:** khushi.chaudhary.mtcs.2025@miet.ac.in
**Contact Number:** 7496001532________________________

---

## Application Under Test

**Task Management Application**

| Feature | Description |
|---------|-------------|
| Registration | New users can create an account |
| Login | Registered users can sign in |
| Create Task | Logged-in users can add new tasks |
| View Task List | Logged-in users can see all their tasks |
| Edit Task | Logged-in users can modify existing tasks |
| Delete Task | Logged-in users can remove tasks |
| Data Storage | Tasks stored in database and displayed in a list |

---

# Task 1: Test Case Design

## 1. Registration

### Test Scenarios

| Scenario ID | Scenario Description | Type |
|-------------|---------------------|------|
| REG-S01 | User registers with valid details | Positive |
| REG-S02 | User registers with invalid or missing data | Negative |
| REG-S03 | User registers with boundary or unusual input values | Edge |

### Test Cases

| TC ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|---------------|------------|-----------|-----------------|------|
| REG-001 | Successful registration with valid credentials | Registration page is accessible; email not already registered | 1. Open registration page<br>2. Enter valid name, email, password, confirm password<br>3. Click Register | Name: Khushi Choudhary<br>Email: khushi@test.com<br>Password: Test@123<br>Confirm: Test@123 | Account created successfully; user redirected to login or dashboard; success message displayed | Positive |
| REG-002 | Registration with already registered email | Email already exists in system | 1. Open registration page<br>2. Enter details with existing email<br>3. Click Register | Email: existing@test.com | Registration blocked; error message: "Email already registered" | Negative |
| REG-003 | Registration with empty mandatory fields | Registration page open | 1. Leave all fields blank<br>2. Click Register | All fields empty | Form not submitted; inline validation errors shown for required fields | Negative |
| REG-004 | Registration with invalid email format | Registration page open | 1. Enter invalid email formats<br>2. Click Register | Email: khushi@, khushi.com, @test.com | Validation error: "Invalid email format" | Negative |
| REG-005 | Registration with weak password | Registration page open | 1. Enter valid email and weak password<br>2. Click Register | Password: 123, abc, password | Password policy error displayed (min length, uppercase, number, special char as per rules) | Negative |
| REG-006 | Password and confirm password mismatch | Registration page open | 1. Enter different values in password fields<br>2. Click Register | Password: Test@123<br>Confirm: Test@456 | Error: "Passwords do not match" | Negative |
| REG-007 | Registration with SQL injection in email field | Registration page open | 1. Enter SQL injection string in email<br>2. Click Register | Email: `' OR '1'='1` | Input rejected or sanitized; no DB error; registration fails safely | Edge |
| REG-008 | Registration with XSS in name field | Registration page open | 1. Enter script tag in name field<br>2. Complete registration | Name: `<script>alert('XSS')</script>` | Script not executed; input sanitized or rejected | Edge |
| REG-009 | Registration with maximum length inputs | Registration page open | 1. Enter values at max allowed length<br>2. Click Register | Name/Email/Password at boundary limits | Registration succeeds if within limits; clear error if exceeded | Edge |
| REG-010 | Registration with leading/trailing spaces | Registration page open | 1. Enter email/password with spaces<br>2. Click Register | Email: ` khushi@test.com ` | Spaces trimmed or validation error shown; no duplicate account due to whitespace | Edge |

---

## 2. Login

### Test Scenarios

| Scenario ID | Scenario Description | Type |
|-------------|---------------------|------|
| LOG-S01 | User logs in with valid credentials | Positive |
| LOG-S02 | User fails login with invalid credentials | Negative |
| LOG-S03 | Login behavior under edge/session conditions | Edge |

### Test Cases

| TC ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|---------------|------------|-----------|-----------------|------|
| LOG-001 | Successful login with valid credentials | User registered and active | 1. Open login page<br>2. Enter valid email and password<br>3. Click Login | Email: khushi@test.com<br>Password: Test@123 | User logged in; redirected to task list/dashboard; session created | Positive |
| LOG-002 | Login with incorrect password | Valid user exists | 1. Enter correct email, wrong password<br>2. Click Login | Password: WrongPass@99 | Login denied; generic error: "Invalid email or password" | Negative |
| LOG-003 | Login with unregistered email | Email not in system | 1. Enter unregistered email<br>2. Click Login | Email: unknown@test.com | Login denied; same generic error (no user enumeration) | Negative |
| LOG-004 | Login with empty email or password | Login page open | 1. Leave one or both fields empty<br>2. Click Login | Empty fields | Validation errors shown; login not attempted | Negative |
| LOG-005 | Login with invalid email format | Login page open | 1. Enter malformed email<br>2. Click Login | Email: notanemail | Format validation error displayed | Negative |
| LOG-006 | Multiple failed login attempts | Valid user exists | 1. Enter wrong password 5+ times<br>2. Try correct password | Wrong password repeated | Account temporarily locked or CAPTCHA shown after threshold | Edge |
| LOG-007 | Login session persistence after browser refresh | User logged in | 1. Login successfully<br>2. Refresh page | Valid session | User remains logged in; task list accessible | Positive |
| LOG-008 | Access task page without login | User not authenticated | 1. Navigate directly to task URL | N/A | Redirect to login page; tasks not visible | Negative |
| LOG-009 | Login with copy-paste password containing hidden characters | User registered | 1. Paste password with trailing newline/space<br>2. Click Login | Password with hidden char | Login fails or trims input consistently | Edge |
| LOG-010 | Logout and re-login | User logged in | 1. Click Logout<br>2. Login again with valid credentials | Valid credentials | Session cleared on logout; successful re-login | Positive |

---

## 3. Task CRUD Operations

### Test Scenarios

| Scenario ID | Scenario Description | Type |
|-------------|---------------------|------|
| CRUD-S01 | Create, read, update, delete tasks successfully | Positive |
| CRUD-S02 | CRUD operations fail with invalid/unauthorized input | Negative |
| CRUD-S03 | CRUD behavior at limits and concurrent conditions | Edge |

### Test Cases — Create

| TC ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|---------------|------------|-----------|-----------------|------|
| TASK-C01 | Create task with valid title and description | User logged in | 1. Click Create Task<br>2. Enter title and description<br>3. Save | Title: Complete QA Assessment<br>Desc: Write test cases | Task created; appears in task list; saved in DB | Positive |
| TASK-C02 | Create task with only mandatory title | User logged in | 1. Enter title only<br>2. Leave description empty<br>3. Save | Title: Buy groceries | Task created if description optional | Positive |
| TASK-C03 | Create task with empty title | User logged in | 1. Leave title blank<br>2. Click Save | Title: empty | Validation error; task not created | Negative |
| TASK-C04 | Create task with very long title/description | User logged in | 1. Enter max+1 characters<br>2. Save | 500+ char title | Error at limit or truncation per spec | Edge |

### Test Cases — Read (View List)

| TC ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|---------------|------------|-----------|-----------------|------|
| TASK-R01 | View task list with existing tasks | User logged in; tasks exist | 1. Navigate to task list | N/A | All user's tasks displayed with correct title, description, date | Positive |
| TASK-R02 | View task list when no tasks exist | User logged in; no tasks | 1. Open task list | N/A | Empty state message shown: "No tasks yet" | Positive |
| TASK-R03 | View another user's task via direct URL | User A logged in; User B has tasks | 1. Access User B's task ID in URL | Task ID belonging to another user | Access denied or 404; task not shown | Negative |

### Test Cases — Update (Edit)

| TC ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|---------------|------------|-----------|-----------------|------|
| TASK-U01 | Edit task title and description | Task exists | 1. Open task<br>2. Edit fields<br>3. Save | Updated title/desc | Changes saved; list reflects updates | Positive |
| TASK-U02 | Edit task with empty title | Task exists | 1. Clear title<br>2. Save | Empty title | Validation error; changes not saved | Negative |
| TASK-U03 | Cancel edit without saving | Task exists | 1. Modify fields<br>2. Click Cancel | Modified data | Original data unchanged | Positive |
| TASK-U04 | Edit deleted task (stale page) | Task deleted in another tab | 1. Open edit form<br>2. Delete task elsewhere<br>3. Save | N/A | Error: task not found; no corrupt data | Edge |

### Test Cases — Delete

| TC ID | Test Case | Preconditions | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|---------------|------------|-----------|-----------------|------|
| TASK-D01 | Delete task with confirmation | Task exists | 1. Click Delete<br>2. Confirm | Valid task | Task removed from list and DB | Positive |
| TASK-D02 | Cancel delete operation | Task exists | 1. Click Delete<br>2. Cancel confirmation | N/A | Task remains in list | Positive |
| TASK-D03 | Delete non-existent task | User logged in | 1. Send delete request for invalid ID | Invalid task ID | Graceful error; no system crash | Negative |
| TASK-D04 | Delete same task twice | Task exists | 1. Delete task<br>2. Retry delete on same ID | Same task ID | Second attempt returns not found; no error crash | Edge |

---

## 4. Input Validation & Error Handling

### Test Scenarios

| Scenario ID | Scenario Description | Type |
|-------------|---------------------|------|
| VAL-S01 | System accepts valid input across all forms | Positive |
| VAL-S02 | System rejects invalid/malicious input | Negative |
| VAL-S03 | System handles boundary and unexpected conditions | Edge |

### Test Cases

| TC ID | Test Case | Module | Test Steps | Test Data | Expected Result | Type |
|-------|-----------|--------|------------|-----------|-----------------|------|
| VAL-001 | Special characters in task title | Create Task | Enter symbols and unicode | Title: `Task #1 — 50% done ✓` | Accepted if allowed; displayed correctly in list | Positive |
| VAL-002 | HTML/script tags in task description | Create Task | Enter HTML/script | `<b>Important</b><script>alert(1)</script>` | Tags escaped/sanitized; no script execution | Negative |
| VAL-003 | SQL injection in search/filter (if present) | Task List | Enter SQL in search | `' DROP TABLE tasks;--` | Query sanitized; no DB impact | Negative |
| VAL-004 | Duplicate task title allowed or blocked | Create Task | Create two tasks with same title | Same title twice | Behavior per spec (either allowed or duplicate warning) | Edge |
| VAL-005 | Network failure during save | Create/Edit Task | Disconnect network; click Save | N/A | User-friendly error; data not lost if possible; retry option | Edge |
| VAL-006 | Server error (500) handling | Any form | Simulate server failure | N/A | Generic error message; no stack trace exposed to user | Negative |
| VAL-007 | Invalid date format in due date (if present) | Create Task | Enter invalid date | 32/13/2025, abc | Validation error with clear message | Negative |
| VAL-008 | Whitespace-only task title | Create Task | Enter spaces only | `"     "` | Treated as empty; validation error | Negative |
| VAL-009 | Rapid double-click on Save/Register/Login | All forms | Double-click submit button | N/A | Single record/action created; no duplicates | Edge |
| VAL-010 | Browser back button after form submission | Create Task | Submit task; press browser Back | N/A | No duplicate submission; appropriate page state | Edge |

---

# Task 2: Bug Identification (Static Analysis)

Based on the application description and typical risks in task management apps nearing production, the following potential bugs and risk areas are identified **without executing the application**.

| # | Bug Description | Severity | Reason / Impact |
|---|----------------|----------|-----------------|
| 1 | **Session not invalidated on logout** — User clicks Logout but session token remains valid | **Critical** | Attacker or shared-device user can reuse session to access tasks. Breaks authentication security before production. |
| 2 | **Users can view/edit/delete other users' tasks via direct URL manipulation** — Task IDs are sequential and not authorization-checked | **Critical** | Data breach: User A can access User B's private tasks by guessing `/tasks/101`. Violates data isolation. |
| 3 | **Passwords stored in plain text or weak hashing** — No evidence of secure password storage | **Critical** | Database compromise exposes all user credentials. Legal/compliance risk and reputational damage. |
| 4 | **No input sanitization on task title/description** — Stored XSS possible | **Major** | Malicious script stored in task description executes when another user views the list. Can steal session cookies. |
| 5 | **Registration allows duplicate accounts with email case variations** — `User@test.com` vs `user@test.com` | **Major** | Same person creates multiple accounts; login confusion; data fragmentation across accounts. |
| 6 | **Task list not refreshed after create/edit/delete** — UI shows stale data until manual refresh | **Major** | User believes action failed and repeats create/delete, causing duplicates or confusion. Poor UX near release. |
| 7 | **No confirmation dialog before task deletion** — Delete is immediate on single click | **Major** | Accidental deletion of important tasks with no recovery option. Data loss and user frustration. |
| 8 | **Missing rate limiting on login API** — Unlimited login attempts allowed | **Major** | Brute-force attack possible on user accounts. Security vulnerability for production deployment. |
| 9 | **Empty or whitespace-only task titles accepted** — Validation not enforced server-side | **Minor** | Task list fills with blank entries; hard to identify tasks; indicates weak server-side validation. |
| 10 | **No pagination on task list** — All tasks loaded at once from database | **Minor** | Performance degrades as task count grows; slow page load and possible timeout for power users. Scalability risk post-launch. |

---

## Summary

| Area | Test Cases Written | Coverage |
|------|-------------------|----------|
| Registration | 10 | Positive, negative, edge |
| Login | 10 | Positive, negative, edge |
| Task CRUD | 14 | Create, Read, Update, Delete |
| Input Validation & Error Handling | 10 | Positive, negative, edge |
| **Total Test Cases** | **44** | — |
| **Bugs/Risks Identified** | **10** | Critical: 3, Major: 5, Minor: 2 |

---

*Note: This assessment focuses on structured thinking, clear documentation, and practical QA coverage for a task management application approaching production release.*
