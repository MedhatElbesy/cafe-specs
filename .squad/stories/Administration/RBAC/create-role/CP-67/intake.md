# Functional Specification — CP-67: Create Role

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-67 |
| Story Title | Create Role |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** create a new role,  
**So that** I can group permissions and give admin users the access they need.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Roles → Create Role

The Platform Admin selects **Create Role** from the Roles section.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to the Roles section.
3. The Create Role form is accessible to the Platform Admin.
4. Only the Platform Admin is authorized to create roles.

## 5. Fields and Validation

### 5.1 Role Name

| Attribute | Specification |
|---|---|
| Field Type | Single-line text input |
| Mandatory | Yes |
| Maximum Length | 100 characters |
| Allowed Characters | Arabic letters, English uppercase/lowercase letters, numbers, and spaces |
| Uniqueness | Mandatory; case-insensitive |
| Leading Spaces | Automatically removed |
| Internal Spaces | Multiple consecutive spaces are automatically converted to one |
| Trailing Spaces | A maximum of one trailing space is retained |
| Validation Timing | Real-time input validation and validation upon Create |
| Duplicate Validation | Upon clicking Create only |

**Validation Behavior:**

1. An empty Role Name is invalid.
2. A Role Name containing only spaces is treated as empty.
3. All leading spaces are removed automatically.
4. Consecutive internal spaces are automatically normalized to one space.
5. Excess trailing spaces are removed, retaining a maximum of one trailing space.
6. Space normalization occurs before the 100-character limit is applied.
7. The system prevents direct entry of disallowed characters.
8. If pasted text contains disallowed characters, the text is temporarily accepted and an invalid-character error is displayed.
9. If the pasted Role Name exceeds 100 characters after space normalization, only the first 100 characters are accepted.
10. Role Names that differ only in letter case or trailing spaces are considered duplicates.
11. Duplicate-name validation occurs only when the Platform Admin clicks Create.
12. If the Role Name contains invalid characters, the invalid-character error takes precedence over duplicate-name validation.

### 5.2 Description

| Attribute | Specification |
|---|---|
| Field Type | Multi-line text input |
| Mandatory | No |
| Maximum Length | 1000 characters |
| Allowed Content | Any text, including letters, numbers, punctuation, symbols, and line breaks |
| Validation | Maximum length enforcement |

**Validation Behavior:**

1. The Description may be left empty.
2. The system prevents the entry of more than 1000 characters.
3. If the Platform Admin pastes text exceeding the available character limit, only the characters that fit within the 1000-character limit are accepted.
4. The Description does not require uniqueness.

## 6. Business Rules

**BR-01 — Role Creation Authorization**

Only the Platform Admin is authorized to create a role.

**BR-02 — Mandatory Role Name**

A Role Name must be provided before a role can be created. Empty and spaces-only values are invalid.

**BR-03 — Allowed Role Name Characters**

Role Names accept Arabic letters, English letters, numbers, and spaces only.

**BR-04 — Role Name Length**

The Role Name must not exceed 100 characters after space normalization.

**BR-05 — Role Name Space Normalization**

The system shall automatically remove leading spaces, convert consecutive internal spaces to a single space, and retain no more than one trailing space.

**BR-06 — Role Name Uniqueness**

Each Role Name must be unique. Uniqueness validation is case-insensitive and ignores trailing-space differences.

**BR-07 — Role Name Input Validation**

The system shall validate Role Name input in real time and upon Create. Direct entry of invalid characters shall be prevented.

**BR-08 — Pasted Invalid Characters**

Pasted Role Names containing invalid characters shall be temporarily accepted with a validation error. Role creation shall be prevented until the invalid characters are corrected.

**BR-09 — Role Name Paste Length**

When pasted Role Name text exceeds 100 characters after space normalization, only the first 100 characters shall be accepted.

**BR-10 — Duplicate Validation Timing**

The system shall check Role Name uniqueness when the Platform Admin clicks Create.

**BR-11 — Validation Priority**

Invalid-character validation shall take precedence over duplicate-name validation. After invalid characters are corrected, uniqueness shall be checked when Create is clicked.

**BR-12 — Description Optionality**

The Description field is optional and may contain any text.

**BR-13 — Description Length**

The Description shall not exceed 1000 characters. Input exceeding the limit shall be prevented, and pasted text shall be restricted to the available character limit.

**BR-14 — Role Creation Without Permissions**

A successfully created role shall initially have no permissions assigned.

**BR-15 — Permission Assignment Dependency**

Permissions shall be assigned through the separate user story **CP-75 — Assign Permission To Role**.

**BR-16 — Successful Role Creation**

A role shall be created only when all applicable field validations pass and saving succeeds.

**BR-17 — Cancel Behavior**

Selecting Cancel shall discard all entered data and return the Platform Admin to the Roles List without displaying a confirmation dialog.

**BR-18 — Back Behavior**

Selecting Back with no unsaved data shall return the Platform Admin to the Roles List.

If unsaved data exists, the system shall display a confirmation dialog before leaving the form.

**BR-19 — Validation Failure**

When field validation fails, the system shall not create the role. The Platform Admin shall remain on the Create Role form with the entered data available for correction.

**BR-20 — Saving Failure**

If saving fails, the system shall display an error dialog. After the Platform Admin selects OK, the system shall return to the Roles List without retaining the entered data.

**BR-21 — Success Confirmation**

After successful role creation, the system shall return to the Roles List, display the newly created role, and show the success message.

## 7. Acceptance Criteria

### AC-01 — Access Create Role

**Given** the user is logged in as a Platform Admin,  
**When** the user navigates to Roles & Permissions → Roles and selects Create Role,  
**Then** the system displays the Create Role form containing:

- Role Name (mandatory)
- Description (optional)
- Create
- Cancel
- Back

### AC-02 — Create Role Successfully

**Given** the Platform Admin is on the Create Role form,  
**And** the Role Name is valid and unique,  
**When** the Platform Admin clicks Create,  
**And** saving succeeds,  
**Then** the system shall:

1. Create the new role.
2. Create the role without assigned permissions.
3. Return to the Roles List.
4. Display the newly created role.
5. Display the message `Role created successfully`.

### AC-03 — Empty Role Name

**Given** the Role Name field is empty,  
**When** the Platform Admin clicks Create,  
**Then**:

1. The role shall not be created.
2. The Platform Admin shall remain on the Create Role form.
3. The message `Please fill required field` shall appear under Role Name.

### AC-04 — Spaces-only Role Name

**Given** the Platform Admin enters only spaces in Role Name,  
**When** validation occurs,  
**Then**:

1. The value shall be treated as empty.
2. The message `Please fill required field` shall appear.
3. The role shall not be created.

### AC-05 — Invalid Characters During Direct Entry

**Given** the Platform Admin is entering a Role Name,  
**When** the Platform Admin attempts to type a disallowed character, such as `@`, `#`, or `!`,  
**Then** the system shall prevent that character from being entered.

### AC-06 — Invalid Characters Through Paste

**Given** the Platform Admin pastes a Role Name containing invalid characters,  
**When** the pasted value is evaluated,  
**Then**:

1. The pasted value shall be temporarily accepted.
2. The system shall display the invalid-character error.
3. The Platform Admin shall be able to correct the value.

**And when** the Platform Admin clicks Create while invalid characters remain,  
**Then**:

1. The role shall not be created.
2. The Platform Admin shall remain on the form.
3. The entered data shall be retained.
4. The invalid-character error shall remain available for correction.

**Error Message:**

`Role name must contain only Arabic or English letters, numbers and spaces`

*Note: This message reflects the confirmed language requirements. The original Jira message requires a corresponding wording update.*

### AC-07 — Case-insensitive Duplicate Role Name

**Given** a role named `Admin` already exists,  
**When** the Platform Admin enters `admin` and clicks Create,  
**Then**:

1. The role shall not be created.
2. The message `Role name already exists` shall appear.
3. The Platform Admin shall remain on the Create Role form.

### AC-08 — Duplicate Name with Trailing Spaces

**Given** a role named `Admin` already exists,  
**When** the Platform Admin enters `Admin ` and clicks Create,  
**Then** the system shall treat the name as a duplicate and display:

`Role name already exists`

### AC-09 — Leading Spaces

**Given** the Platform Admin enters a Role Name containing leading spaces,  
**When** the input is processed,  
**Then** the system shall automatically remove all leading spaces.

### AC-10 — Internal Spaces

**Given** the Platform Admin enters consecutive spaces between words,  
**When** the input is processed,  
**Then** the system shall automatically convert the consecutive spaces into a single space.

### AC-11 — Trailing Spaces

**Given** the Platform Admin enters multiple spaces at the end of a Role Name,  
**When** the input is processed,  
**Then** the system shall retain a maximum of one trailing space.

### AC-12 — Role Name Maximum Length

**Given** the Platform Admin enters a Role Name,  
**When** the normalized value reaches 100 characters,  
**Then** the system shall prevent additional characters from being entered.

### AC-13 — Role Name Paste Exceeding Maximum Length

**Given** the Platform Admin pastes a Role Name that exceeds 100 characters after space normalization,  
**When** the pasted text is processed,  
**Then** the system shall accept only the first 100 characters of the normalized value.

### AC-14 — Description Is Optional

**Given** the Platform Admin enters a valid, unique Role Name,  
**And** leaves Description empty,  
**When** the Platform Admin clicks Create,  
**And** saving succeeds,  
**Then** the role shall be created successfully.

### AC-15 — Description Maximum Length

**Given** the Platform Admin enters text in Description,  
**When** the field reaches 1000 characters,  
**Then** the system shall prevent additional characters from being entered.

### AC-16 — Description Paste Exceeding Maximum Length

**Given** the Platform Admin pastes text exceeding the available Description character limit,  
**When** the pasted text is processed,  
**Then** only the characters that fit within the 1000-character limit shall be accepted.

### AC-17 — Cancel Role Creation

**Given** the Platform Admin is on the Create Role form,  
**And** has entered data,  
**When** the Platform Admin clicks Cancel,  
**Then**:

1. The entered data shall be discarded.
2. No role shall be created.
3. The system shall return to the Roles List.
4. No confirmation dialog shall appear.

### AC-18 — Back Without Unsaved Changes

**Given** the Platform Admin is on the Create Role form,  
**And** no data has been entered,  
**When** the Platform Admin clicks Back,  
**Then** the system shall return to the Roles List without displaying a confirmation dialog.

### AC-19 — Back With Unsaved Changes

**Given** the Platform Admin has entered unsaved data,  
**When** the Platform Admin clicks Back,  
**Then** the system shall display:

`You have unsaved changes. Are you sure you want to leave?`

The dialog shall provide:

- Leave
- Stay

### AC-20 — Confirm Leaving the Form

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Leave,  
**Then**:

1. The entered data shall be discarded.
2. No role shall be created.
3. The system shall return to the Roles List.

### AC-21 — Stay on the Form

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Stay,  
**Then**:

1. The dialog shall close.
2. The Platform Admin shall remain on the Create Role form.
3. The entered data shall be retained.

### AC-22 — Saving Failure

**Given** the Platform Admin enters valid role information,  
**When** the Platform Admin clicks Create,  
**And** saving fails,  
**Then**:

1. The role shall not be created.
2. The system shall display an error dialog containing:

   `Something went wrong. Please try again.`

3. The dialog shall provide an OK action.
4. After the Platform Admin clicks OK, the system shall return to the Roles List.
5. The previously entered data shall not be retained.

### AC-23 — Invalid Characters Take Priority Over Duplicate Validation

**Given** the Platform Admin has entered a Role Name containing invalid characters,  
**And** the name would otherwise conflict with an existing Role Name,  
**When** the Platform Admin clicks Create,  
**Then**:

1. The system shall display the invalid-character error first.
2. The role shall not be created.
3. The Platform Admin shall remain on the form.

**When** the Platform Admin corrects the invalid characters and clicks Create again,  
**Then** the system shall check uniqueness and display `Role name already exists` if the corrected name is a duplicate.

### AC-24 — Create Role Without Permissions

**Given** the Platform Admin successfully creates a new role,  
**When** role creation is completed,  
**Then** the newly created role shall have no permissions assigned.

Permission assignment shall be handled separately through CP-75.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | `Admin` when no equivalent role exists | Accepted if all validations pass |
| EX-02 | `admin` when `Admin` exists | Duplicate error |
| EX-03 | `ADMIN ` when `Admin` exists | Duplicate error |
| EX-04 | Leading spaces before `Manager` | Leading spaces automatically removed |
| EX-05 | `Sales  Manager` | Normalized to `Sales Manager` |
| EX-06 | `Sales Manager   ` | Excess trailing spaces removed; one retained |
| EX-07 | Empty Role Name | Required-field error |
| EX-08 | Spaces-only Role Name | Required-field error |
| EX-09 | Directly type `@` into Role Name | Character entry prevented |
| EX-10 | Paste `Sales@Manager` | Temporarily accepted; invalid-character error displayed; creation blocked until corrected |
| EX-11 | Paste a normalized Role Name of 120 characters | First 100 characters accepted |
| EX-12 | Input of 105 characters normalized to 98 | Accepted with respect to length |
| EX-13 | Arabic Role Name `مدير النظام` | Accepted if unique and otherwise valid |
| EX-14 | Description left empty | Role creation allowed if other validations pass |
| EX-15 | Description of exactly 1000 characters | Accepted |
| EX-16 | Paste 1200 characters into an empty Description | First 1000 characters accepted |
| EX-17 | Cancel after entering data | Data discarded; return to Roles List |
| EX-18 | Back with unsaved data, then Stay | Dialog closes; entered data retained |
| EX-19 | Back with unsaved data, then Leave | Data discarded; return to Roles List |
| EX-20 | Successful role creation | Role appears in Roles List with success message and no permissions |
| EX-21 | Saving failure | Error dialog shown; OK returns to Roles List without retaining entered data |

## 9. Attachments and Related Requirements

### 9.1 Attachments

- Create Role wireframe — supplied for review.
- No Jira attachments were listed in the original story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-67 | Create Role | Current User Story |
| CP-75 | Assign Permission To Role | Related story responsible for assigning permissions after role creation |

### 9.3 Dependencies

- Role creation is restricted to the Platform Admin.
- The Roles List is the destination after successful creation, Cancel, Back/Leave, and acknowledged saving failure.
- The role must be created without permissions; permission assignment belongs to CP-75.

## 10. Out of Scope

The following activities are not included in CP-67:

1. Assigning permissions to a role — covered by CP-75.
2. Editing existing roles.
3. Deleting roles.
4. Assigning roles to admin users.
5. Creating or editing permissions.
6. Managing user accounts.
7. Technical implementation details, APIs, database schemas, and system architecture.

---

