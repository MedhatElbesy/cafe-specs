# Functional Specification — CP-69: Edit Role

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-69 |
| Story Title | Edit Role |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** edit an existing role,  
**So that** I can keep its name and description accurate.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Roles → Edit

The Platform Admin selects **Edit** for an existing role from the Roles section.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to the Roles section.
3. An existing role has been selected for editing.
4. Only the Platform Admin is authorized to edit roles.
5. The selected role must be available for the Edit Role form to open.

## 5. Fields and Validation

The Edit Role form displays the existing Role Name and Description as pre-filled, editable fields.

### 5.1 Role Name

| Attribute | Specification |
|---|---|
| Field Type | Single-line text input |
| Mandatory | Yes |
| Initial Value | Pre-filled with the existing Role Name |
| Maximum Length | 100 characters |
| Allowed Characters | Arabic letters, English uppercase/lowercase letters, numbers, and spaces |
| Uniqueness | Mandatory; case-insensitive; excludes the role being edited |
| Leading Spaces | Automatically removed |
| Internal Spaces | Multiple consecutive spaces are automatically converted to one |
| Trailing Spaces | A maximum of one trailing space is retained |
| Validation Timing | Real-time input validation and validation upon Save |
| Duplicate Validation | Upon clicking Save only |

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
10. Role Names that differ only in letter case or trailing spaces are considered duplicates when they belong to different roles.
11. The role being edited is excluded from duplicate-name validation.
12. Duplicate-name validation occurs only when the Platform Admin clicks Save.
13. If the Role Name contains invalid characters, the invalid-character error takes precedence over duplicate-name validation.
14. Change detection compares the Role Name after space normalization. A normalized value matching the original Role Name is not considered a change.

### 5.2 Description

| Attribute | Specification |
|---|---|
| Field Type | Multi-line text input |
| Mandatory | No |
| Initial Value | Pre-filled with the existing Description |
| Maximum Length | 1000 characters |
| Allowed Content | Any text, including letters, numbers, punctuation, symbols, and line breaks |
| Validation | Maximum length enforcement |

**Validation Behavior:**

1. The Description may be left empty.
2. The system prevents the entry of more than 1000 characters.
3. If the Platform Admin pastes text exceeding the available character limit, only the characters that fit within the 1000-character limit are accepted.
4. The Description does not require uniqueness.
5. Changing Description alone is sufficient to enable Save.
6. If Description is restored to its original value and no other actual changes exist, Save becomes disabled.

## 6. Business Rules

**BR-01 — Role Editing Authorization**

Only the Platform Admin is authorized to edit an existing role.

**BR-02 — Pre-filled Role Data**

The Edit Role form shall display the selected role's current Role Name and Description as pre-filled, editable fields.

**BR-03 — Mandatory Role Name**

A Role Name must be provided before role changes can be saved. Empty and spaces-only values are invalid.

**BR-04 — Allowed Role Name Characters**

Role Names accept Arabic letters, English letters, numbers, and spaces only.

**BR-05 — Role Name Length**

The Role Name must not exceed 100 characters after space normalization.

**BR-06 — Role Name Space Normalization**

The system shall automatically remove leading spaces, convert consecutive internal spaces to a single space, and retain no more than one trailing space.

**BR-07 — Role Name Uniqueness**

Each Role Name must be unique among other existing roles. Uniqueness validation is case-insensitive and ignores trailing-space differences. The role being edited shall not be considered a duplicate of itself.

**BR-08 — Role Name Input Validation**

The system shall validate Role Name input in real time and upon Save. Direct entry of invalid characters shall be prevented.

**BR-09 — Pasted Invalid Characters**

Pasted Role Names containing invalid characters shall be temporarily accepted with a validation error. Saving shall be prevented until the invalid characters are corrected.

**BR-10 — Role Name Paste Length**

When pasted Role Name text exceeds 100 characters after space normalization, only the first 100 characters shall be accepted.

**BR-11 — Duplicate Validation Timing**

The system shall check Role Name uniqueness against other existing roles when the Platform Admin clicks Save.

**BR-12 — Validation Priority**

Invalid-character validation shall take precedence over duplicate-name validation. After invalid characters are corrected, uniqueness shall be checked when Save is clicked.

**BR-13 — Description Optionality**

The Description field is optional and may contain any text.

**BR-14 — Description Length**

The Description shall not exceed 1000 characters. Input exceeding the limit shall be prevented, and pasted text shall be restricted to the available character limit.

**BR-15 — Save Button Initial State**

The Save button shall be disabled when the Edit Role form is opened and no changes have been made.

**BR-16 — Save Button Activation**

The Save button shall become enabled when an actual change is made to Role Name, Description, or both.

**BR-17 — Save Button After Reverting Changes**

If all modified field values are restored to their original values, Save shall become disabled again.

**BR-18 — Change Detection After Space Normalization**

Role Name change detection shall compare normalized values. Space normalization alone shall not count as a change when the resulting value matches the original Role Name.

**BR-19 — Description-only Changes**

Changing Description without changing Role Name shall be sufficient to enable Save.

**BR-20 — Save Button With Invalid Data**

Save shall remain enabled when actual changes exist, even if the modified Role Name contains invalid pasted characters. Clicking Save shall trigger validation and prevent invalid changes from being saved.

**BR-21 — Successful Role Update**

Role changes shall be saved only when all applicable field validations pass, the role remains available, and saving succeeds.

**BR-22 — Existing Permissions Preservation**

Editing Role Name or Description shall not modify the role's existing permissions, even when the Role Name is completely changed.

**BR-23 — Cancel Behavior**

Selecting Cancel shall discard all unsaved changes and return the Platform Admin to the Roles List without displaying a confirmation dialog.

**BR-24 — Back Behavior**

Selecting Back with no actual unsaved changes shall return the Platform Admin directly to the Roles List.

If actual unsaved changes exist, the system shall display a confirmation dialog before leaving the form.

**BR-25 — Validation Failure**

When field validation fails, the system shall not save the changes. The Platform Admin shall remain on the Edit Role form with the entered data available for correction.

**BR-26 — General Saving Failure**

If saving fails for a reason other than role unavailability, the system shall display an error dialog containing `Something went wrong. Please try again.`

After the Platform Admin selects OK, the system shall remain on the Edit Role form with the entered changes retained.

**BR-27 — Role Unavailable During Save**

If the selected role has been deleted or becomes unavailable for any other reason before changes are saved, the system shall prevent saving and display an error dialog containing `Role is no longer available.`

After the Platform Admin selects OK, the system shall return to the Roles List.

**BR-28 — Role Unavailable Before Editing**

If the selected role has been deleted or becomes unavailable before the Edit Role form opens, the system shall prevent opening the form and display an error dialog containing `Role is no longer available.`

After the Platform Admin selects OK, the system shall remain on the Roles List.

**BR-29 — Success Confirmation**

After a successful role update, the system shall return to the Roles List and display `Role updated successfully`.

## 7. Acceptance Criteria

### AC-01 — Access Edit Role

**Given** the user is logged in as a Platform Admin,  
**And** the selected role is available,  
**When** the user navigates to Roles & Permissions → Roles and selects Edit,  
**Then** the system displays the Edit Role form containing:

- Role Name (mandatory, pre-filled)
- Description (optional, pre-filled)
- Save
- Cancel
- Back

### AC-02 — Update Role Successfully

**Given** the Platform Admin is on the Edit Role form,  
**And** valid changes have been made to Role Name and/or Description,  
**And** the selected role remains available,  
**When** the Platform Admin clicks Save,  
**And** saving succeeds,  
**Then** the system shall:

1. Save the modified Role Name and/or Description.
2. Preserve the role's existing permissions.
3. Return to the Roles List.
4. Display the message `Role updated successfully`.

### AC-03 — Empty Role Name

**Given** the Platform Admin clears the Role Name field,  
**When** the Platform Admin clicks Save,  
**Then**:

1. The changes shall not be saved.
2. The Platform Admin shall remain on the Edit Role form.
3. The message `Please fill required field` shall appear under Role Name.

### AC-04 — Spaces-only Role Name

**Given** the Platform Admin enters only spaces in Role Name,  
**When** validation occurs,  
**Then**:

1. The value shall be treated as empty.
2. The message `Please fill required field` shall appear.
3. The changes shall not be saved.

### AC-05 — Invalid Characters During Direct Entry

**Given** the Platform Admin is editing a Role Name,  
**When** the Platform Admin attempts to type a disallowed character, such as `@`, `#`, or `!`,  
**Then** the system shall prevent that character from being entered.

### AC-06 — Invalid Characters Through Paste

**Given** the Platform Admin pastes a Role Name containing invalid characters,  
**When** the pasted value is evaluated,  
**Then**:

1. The pasted value shall be temporarily accepted.
2. The system shall display the invalid-character error.
3. The Platform Admin shall be able to correct the value.

**And when** the Platform Admin clicks Save while invalid characters remain,  
**Then**:

1. The changes shall not be saved.
2. The Platform Admin shall remain on the Edit Role form.
3. The entered data shall be retained.
4. The invalid-character error shall remain available for correction.

**Error Message:**

`Role name must contain only Arabic or English letters, numbers and spaces`

### AC-07 — Case-insensitive Duplicate Role Name

**Given** the Platform Admin is editing a role other than `Admin`,  
**And** a role named `Admin` already exists,  
**When** the Platform Admin enters `admin` and clicks Save,  
**Then**:

1. The changes shall not be saved.
2. The message `Role name already exists` shall appear.
3. The Platform Admin shall remain on the Edit Role form.

### AC-08 — Duplicate Name with Trailing Spaces

**Given** another role named `Admin` already exists,  
**When** the Platform Admin enters `Admin ` and clicks Save,  
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

**Given** the Platform Admin edits a Role Name,  
**When** the normalized value reaches 100 characters,  
**Then** the system shall prevent additional characters from being entered.

### AC-13 — Role Name Paste Exceeding Maximum Length

**Given** the Platform Admin pastes a Role Name that exceeds 100 characters after space normalization,  
**When** the pasted text is processed,  
**Then** the system shall accept only the first 100 characters of the normalized value.

### AC-14 — Description Is Optional

**Given** the Platform Admin is editing an existing role,  
**And** the Description field is empty,  
**And** a valid change exists in Role Name,  
**When** the Platform Admin clicks Save,  
**And** saving succeeds,  
**Then** the role shall be updated successfully without requiring Description.

### AC-15 — Description Maximum Length

**Given** the Platform Admin enters text in Description,  
**When** the field reaches 1000 characters,  
**Then** the system shall prevent additional characters from being entered.

### AC-16 — Description Paste Exceeding Maximum Length

**Given** the Platform Admin pastes text exceeding the available Description character limit,  
**When** the pasted text is processed,  
**Then** only the characters that fit within the 1000-character limit shall be accepted.

### AC-17 — Save Disabled Without Changes

**Given** the Platform Admin opens the Edit Role form,  
**And** Role Name and Description contain their original values,  
**When** no changes are made,  
**Then** Save shall remain disabled.

### AC-18 — Save Enabled After Role Name Change

**Given** the Platform Admin is on the Edit Role form,  
**When** the Platform Admin changes Role Name to a different value,  
**Then** Save shall become enabled.

### AC-19 — Save Enabled After Description-only Change

**Given** the Platform Admin is on the Edit Role form,  
**When** the Platform Admin changes Description without changing Role Name,  
**Then** Save shall become enabled.

### AC-20 — Save Disabled After Reverting Changes

**Given** the Platform Admin modifies Role Name and/or Description,  
**When** all modified values are restored to their original values,  
**Then** Save shall become disabled again.

### AC-21 — Space Normalization Does Not Count as a Change

**Given** the original Role Name is `Admin`,  
**And** Description remains unchanged,  
**When** the Platform Admin enters additional spaces that are removed by normalization,  
**And** the resulting Role Name is `Admin`,  
**Then** Save shall remain disabled.

### AC-22 — Save Remains Enabled With Invalid Modified Data

**Given** the Platform Admin makes an actual change to Role Name,  
**And** the modified value contains invalid pasted characters,  
**When** the validation error is displayed,  
**Then** Save shall remain enabled.

**When** the Platform Admin clicks Save,  
**Then**:

1. The system shall prevent saving.
2. The invalid-character error shall be displayed.
3. The Platform Admin shall remain on the Edit Role form with entered data retained.

### AC-23 — Existing Role Name Is Not a Duplicate of Itself

**Given** the Platform Admin opens an existing role named `Admin`,  
**And** changes only Description,  
**When** the Platform Admin clicks Save,  
**Then** the system shall not treat the unchanged Role Name as a duplicate.

**And** the update shall be allowed if all other validations pass and saving succeeds.

### AC-24 — Preserve Existing Permissions

**Given** the selected role has existing permissions,  
**When** the Platform Admin successfully updates Role Name and/or Description,  
**Then**:

1. The existing permissions shall remain unchanged.
2. No permissions shall be added or removed as a result of editing the role.
3. The role's updated name and/or description shall be saved.

### AC-25 — Cancel Role Editing

**Given** the Platform Admin is on the Edit Role form,  
**And** has made unsaved changes,  
**When** the Platform Admin clicks Cancel,  
**Then**:

1. The unsaved changes shall be discarded.
2. The original role data shall remain unchanged.
3. The system shall return to the Roles List.
4. No confirmation dialog shall appear.

### AC-26 — Back Without Unsaved Changes

**Given** the Platform Admin is on the Edit Role form,  
**And** no actual unsaved changes exist,  
**When** the Platform Admin clicks Back,  
**Then** the system shall return to the Roles List without displaying a confirmation dialog.

### AC-27 — Back With Unsaved Changes

**Given** the Platform Admin has made actual unsaved changes,  
**When** the Platform Admin clicks Back,  
**Then** the system shall display:

`You have unsaved changes. Are you sure you want to leave?`

The dialog shall provide:

- Leave
- Stay

### AC-28 — Confirm Leaving the Form

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Leave,  
**Then**:

1. The unsaved changes shall be discarded.
2. The original role data shall remain unchanged.
3. The system shall return to the Roles List.

### AC-29 — Stay on the Form

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Stay,  
**Then**:

1. The dialog shall close.
2. The Platform Admin shall remain on the Edit Role form.
3. The entered changes shall be retained.

### AC-30 — Back After Reverting Changes

**Given** the Platform Admin has modified Role Name and/or Description,  
**And** subsequently restored all values to their original state,  
**When** the Platform Admin clicks Back,  
**Then** the system shall return directly to the Roles List without displaying a confirmation dialog.

### AC-31 — General Saving Failure

**Given** the Platform Admin enters valid changes,  
**When** the Platform Admin clicks Save,  
**And** saving fails for a reason other than role unavailability,  
**Then**:

1. The changes shall not be saved.
2. The system shall display an error dialog containing:

   `Something went wrong. Please try again.`

3. The dialog shall provide an OK action.
4. After the Platform Admin clicks OK, the system shall remain on the Edit Role form.
5. The entered changes shall be retained for another Save attempt.

### AC-32 — Invalid Characters Take Priority Over Duplicate Validation

**Given** the Platform Admin has entered a Role Name containing invalid characters,  
**And** the name would otherwise conflict with another existing Role Name,  
**When** the Platform Admin clicks Save,  
**Then**:

1. The system shall display the invalid-character error first.
2. The changes shall not be saved.
3. The Platform Admin shall remain on the Edit Role form.

**When** the Platform Admin corrects the invalid characters and clicks Save again,  
**Then** the system shall check uniqueness and display `Role name already exists` if the corrected name matches another role.

### AC-33 — Role Unavailable During Save

**Given** the Platform Admin has opened the Edit Role form,  
**And** the selected role has been deleted or become unavailable,  
**When** the Platform Admin clicks Save,  
**Then**:

1. The changes shall not be saved.
2. The system shall display an error dialog containing:

   `Role is no longer available.`

3. The dialog shall provide an OK action.
4. After the Platform Admin clicks OK, the system shall return to the Roles List.

### AC-34 — Role Unavailable Before Opening Edit Form

**Given** the Platform Admin is on the Roles List,  
**And** the selected role has been deleted or become unavailable,  
**When** the Platform Admin clicks Edit,  
**Then**:

1. The Edit Role form shall not open.
2. The system shall display an error dialog containing:

   `Role is no longer available.`

3. The dialog shall provide an OK action.
4. After the Platform Admin clicks OK, the system shall remain on the Roles List.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Open Edit for an available role | Form opens with pre-filled Role Name and Description |
| EX-02 | Open Edit without changing any fields | Save remains disabled |
| EX-03 | Change `Admin` to `Operations Admin` | Save becomes enabled |
| EX-04 | Change Description only | Save becomes enabled |
| EX-05 | Change Role Name and restore the original value | Save becomes disabled if Description is unchanged |
| EX-06 | Enter additional spaces around `Admin` | Normalized value matches original; no actual change |
| EX-07 | Clear Role Name and click Save | Required-field error; changes not saved |
| EX-08 | Enter spaces only in Role Name | Required-field error |
| EX-09 | Directly type `@` into Role Name | Character entry prevented |
| EX-10 | Paste `Sales@Manager` | Temporarily accepted; invalid-character error displayed; saving blocked until corrected |
| EX-11 | Paste a normalized Role Name of 120 characters | First 100 characters accepted |
| EX-12 | Input of 105 characters normalized to 98 | Accepted with respect to length |
| EX-13 | Arabic Role Name `مدير النظام` | Accepted if unique among other roles and otherwise valid |
| EX-14 | Leave Description empty while making a valid Role Name change | Update allowed if other validations pass |
| EX-15 | Description of exactly 1000 characters | Accepted |
| EX-16 | Paste 1200 characters into an empty Description | First 1000 characters accepted |
| EX-17 | Edit Description while keeping the original Role Name | No self-duplicate error |
| EX-18 | Rename a role to `admin` when another role named `Admin` exists | Duplicate error |
| EX-19 | Rename a role to `Admin ` when another role named `Admin` exists | Duplicate error |
| EX-20 | Save valid changes | Return to Roles List with `Role updated successfully` |
| EX-21 | Edit a role with existing permissions | All existing permissions remain unchanged |
| EX-22 | Cancel after making changes | Changes discarded; return to Roles List without confirmation |
| EX-23 | Back with unsaved changes, then Stay | Dialog closes; changes retained |
| EX-24 | Back with unsaved changes, then Leave | Changes discarded; return to Roles List |
| EX-25 | Back after restoring all original values | Return to Roles List without confirmation |
| EX-26 | Save modified data containing invalid pasted characters | Save remains enabled before clicking; validation blocks saving |
| EX-27 | General saving failure | Error dialog shown; OK keeps user on Edit Form with changes retained |
| EX-28 | Role becomes unavailable before Save | Saving blocked; error dialog; OK returns to Roles List |
| EX-29 | Role unavailable when Edit is selected | Form does not open; error dialog; OK keeps user on Roles List |

## 9. Attachments and Related Requirements

### 9.1 Attachments

- No CP-69 wireframe was supplied for review.
- No Jira attachments were listed in the original CP-69 story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-67 | Create Role | Related story providing the confirmed shared field-validation rules |
| CP-69 | Edit Role | Current User Story |
| CP-75 | Assign Permission To Role | Related story responsible for permission assignment; outside CP-69 |

### 9.3 Dependencies

- Role editing is restricted to the Platform Admin.
- The selected role must exist and be available before the Edit Role form can open.
- The Edit Role form requires the existing Role Name and Description to be pre-filled.
- Role Name validation follows the confirmed CP-67 rules, with uniqueness checks excluding the role being edited.
- The Roles List is the destination after successful editing, Cancel, Back/Leave, and acknowledged role-unavailability errors.
- A general saving failure retains the entered changes and keeps the Platform Admin on the Edit Role form.
- Existing role permissions remain unchanged when Role Name or Description is updated.
- Permission assignment is handled separately through CP-75.

## 10. Out of Scope

The following activities are not included in CP-69:

1. Creating new roles — covered by CP-67.
2. Assigning permissions to a role — covered by CP-75.
3. Changing or removing existing role permissions.
4. Deleting roles.
5. Assigning roles to admin users.
6. Creating or editing permissions.
7. Managing user accounts.
8. Technical implementation details, APIs, database schemas, and system architecture.

---
