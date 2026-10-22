# Functional Specification — CP-71: Create Permission

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-71 |
| Story Title | Create Permission |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** create a new permission,  
**So that** I can control access to a new action or module in the admin panel.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Permissions → Create Permission

The Platform Admin selects **Create Permission** from the Permissions List to open the permission creation form.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to Roles & Permissions → Permissions.
3. The Create Permission functionality is accessible to the Platform Admin.
4. The Admin Panel has available modules from which the Platform Admin can select.

## 5. Fields and Validation

### 5.1 Permission Name

| Attribute | Specification |
|---|---|
| Field Name | Permission Name |
| Field Type | Text input |
| Mandatory | Yes |
| Allowed Characters | Arabic letters, English letters (uppercase/lowercase), numbers, and spaces |
| Maximum Length | 100 characters after applicable normalization |
| Uniqueness | Required |
| Uniqueness Comparison | Case-insensitive; ignores leading and trailing spaces |
| Validation Timing | Required-field and format validation in real time and on Create; uniqueness validation on Create only |
| Empty / Spaces-only Error | `Please fill required field` |
| Duplicate Error | `Permission name already exists` |
| Invalid Format Error | `Permission name must contain only Arabic or English letters, numbers and spaces` |

**Validation Behavior:**

1. Permission Name shall be mandatory.
2. The field shall accept Arabic letters, English letters, numbers, and spaces.
3. The system shall remove leading spaces from the entered value.
4. The system shall collapse consecutive internal spaces into a single space.
5. The system shall retain at most one trailing space.
6. The normalized Permission Name shall not exceed 100 characters.
7. When pasted text exceeds the maximum length, the system shall retain the first 100 characters after applicable normalization.
8. Invalid characters entered through direct typing shall be blocked.
9. Invalid characters introduced through paste may remain temporarily in the field.
10. If pasted text contains invalid characters, the system shall display `Permission name must contain only Arabic or English letters, numbers and spaces` and prevent creation until the value is corrected.
11. A blank or spaces-only Permission Name shall be treated as missing and display `Please fill required field`.
12. Permission Name shall be unique using a case-insensitive comparison that ignores leading and trailing spaces.
13. The system shall check Permission Name uniqueness when Create is selected.
14. If the Permission Name already exists, the system shall display `Permission name already exists`.
15. If the field contains invalid characters and its value also matches an existing name, the invalid-format error shall take priority.
16. The system shall not create the permission until all applicable Permission Name validations pass.

### 5.2 Permission Key

| Attribute | Specification |
|---|---|
| Field Name | Permission Key |
| Field Type | Text input |
| Example | `customers.view` |
| Mandatory | Yes |
| Allowed Characters | Lowercase English letters (`a-z`), numbers (`0-9`), dots (`.`), and underscores (`_`) |
| Maximum Length | 100 characters |
| Uniqueness | Required |
| Uniqueness Comparison | Exact match of the complete key |
| Editability After Creation | Cannot be edited after creation |
| Validation Timing | Required-field and format validation in real time and on Create; uniqueness validation on Create only |
| Empty / Spaces-only Error | `Please fill required field` |
| Duplicate Error | `Permission key already exists` |
| Invalid Format Error | `Key must contain lowercase English letters, numbers, dots, and underscores only` |

**Validation Behavior:**

1. Permission Key shall be mandatory.
2. The field shall accept lowercase English letters, numbers, dots, and underscores only.
3. Uppercase English letters, spaces, and other characters outside the allowed set shall be invalid.
4. The system shall not automatically convert uppercase letters to lowercase or remove invalid characters as a substitute for validation.
5. Any combination of permitted characters shall be accepted, including leading, trailing, and repeated dots or underscores.
6. The Permission Key shall not exceed 100 characters.
7. When pasted text exceeds 100 characters, the system shall retain the first 100 characters.
8. Invalid characters entered through direct typing shall be blocked.
9. Invalid characters introduced through paste may remain temporarily in the field.
10. If pasted text contains invalid characters, the system shall display `Key must contain lowercase English letters, numbers, dots, and underscores only` and prevent creation until corrected.
11. A blank or spaces-only Permission Key shall display `Please fill required field`.
12. Permission Key uniqueness shall be evaluated using an exact match of the complete key.
13. The system shall check Permission Key uniqueness when Create is selected.
14. If the Permission Key already exists, the system shall display `Permission key already exists`.
15. If the Permission Key has an invalid format and also matches an existing key, the invalid-format error shall take priority.
16. After successful creation, the Permission Key shall not be editable.
17. The system shall not create the permission until all applicable Permission Key validations pass.

### 5.3 Module

| Attribute | Specification |
|---|---|
| Field Name | Module |
| Field Type | Dropdown |
| Mandatory | Yes |
| Default Selection | None |
| Initial Placeholder | `Select module` |
| Available Options | All available modules in the Admin Panel |
| Examples from Jira | Customers, Subscriptions, Settlements, Static Pages, Settings, RBAC |
| Validation Timing | Required-field validation in real time and on Create |
| Empty Selection Error | `Please fill required field` |

**Validation Behavior:**

1. Module shall be mandatory.
2. The dropdown shall initially display `Select module`.
3. No module shall be selected by default.
4. The dropdown shall display all available modules in the Admin Panel.
5. Customers, Subscriptions, Settlements, Static Pages, Settings, and RBAC are examples and shall not be treated as an exhaustive list.
6. The Platform Admin shall select a module before the permission can be created.
7. If no module is selected, the system shall display `Please fill required field`.
8. The system shall prevent creation until a valid module selection is provided.

### 5.4 Description

| Attribute | Specification |
|---|---|
| Field Name | Description |
| Field Type | Multiline text input |
| Mandatory | No |
| Maximum Length | 1,000 characters |
| Allowed Content | Text; no additional character restrictions confirmed |
| Validation | Maximum-length handling |
| Empty Value | Allowed |

**Validation Behavior:**

1. Description shall be optional.
2. The field shall support multiline text.
3. The field shall accept up to 1,000 characters.
4. When pasted text exceeds 1,000 characters, the system shall retain the first 1,000 characters.
5. The Platform Admin may leave Description empty without preventing permission creation.
6. No additional Description character restrictions shall be imposed by this specification.

### 5.5 Form-Level Validation and Actions

| Action / Condition | Required Behavior |
|---|---|
| Create | Validate the form and create the permission only when all validations pass |
| Invalid or Missing Data | Keep Create enabled, display applicable field errors, and prevent saving |
| Multiple Invalid Fields | Display errors under all invalid fields simultaneously |
| Duplicate Permission Name | Display `Permission name already exists` |
| Duplicate Permission Key | Display `Permission key already exists` |
| Successful Creation | Save the permission, return to Permissions List, and display `Permission created successfully` |
| Role Assignment | Do not assign the newly created permission to any role automatically |
| Cancel | Discard all entered data and return to Permissions List without confirmation |
| Back — No Changes | Return directly to Permissions List |
| Back — Unsaved Changes | Display the confirmed unsaved-changes dialog |
| Save Failure | Display the general error dialog; after OK, return to Permissions List and discard entered data |

**Validation Behavior:**

1. Required-field and format validation shall run in real time and when Create is selected.
2. Permission Name and Permission Key uniqueness validation shall run only when Create is selected.
3. The Create button shall remain enabled even when the form contains invalid or missing data.
4. Selecting Create shall validate the applicable fields before saving.
5. The system shall display errors under all invalid fields simultaneously.
6. Within the same field, invalid-format errors shall take priority over duplicate errors.
7. The system shall prevent saving while any mandatory field is missing or any applicable validation fails.
8. If all validations pass, the system shall create the permission and return to the Permissions List.
9. The newly created permission shall appear in the Permissions List and shall not be assigned to any role automatically.

## 6. Business Rules

**BR-01 — Create Permission Authorization**

Only the Platform Admin shall be authorized to access the Create Permission functionality.

**BR-02 — Create Permission Entry Point**

The system shall provide access to Create Permission through Roles & Permissions → Permissions → Create Permission.

**BR-03 — Required Fields**

Permission Name, Permission Key, and Module shall be mandatory.

**BR-04 — Optional Description**

Description shall be optional.

**BR-05 — Permission Name Allowed Characters**

Permission Name shall accept Arabic letters, English letters, numbers, and spaces only.

**BR-06 — Permission Name Maximum Length**

Permission Name shall have a maximum length of 100 characters after applicable normalization.

**BR-07 — Permission Name Normalization**

The system shall remove leading spaces, collapse consecutive internal spaces into one, and retain at most one trailing space in Permission Name.

**BR-08 — Permission Name Uniqueness**

Permission Name shall be unique using a case-insensitive comparison that ignores leading and trailing spaces.

**BR-09 — Permission Name Invalid Input Handling**

Invalid characters entered through direct typing shall be blocked. Invalid pasted characters may remain temporarily with a validation error and shall prevent creation until corrected.

**BR-10 — Permission Name Length Handling**

If pasted Permission Name text exceeds 100 characters after applicable normalization, the system shall retain the first 100 characters.

**BR-11 — Permission Key Allowed Characters**

Permission Key shall accept lowercase English letters, numbers, dots, and underscores only.

**BR-12 — Permission Key Character Arrangement**

The system shall accept any arrangement of permitted Permission Key characters, including leading, trailing, and repeated dots or underscores.

**BR-13 — Permission Key Maximum Length**

Permission Key shall have a maximum length of 100 characters.

**BR-14 — Permission Key Uniqueness**

Permission Key shall be unique based on an exact match of the complete key.

**BR-15 — Permission Key Invalid Input Handling**

Invalid characters entered through direct typing shall be blocked. Invalid pasted characters may remain temporarily with a validation error and shall prevent creation until corrected.

**BR-16 — Permission Key Length Handling**

If pasted Permission Key text exceeds 100 characters, the system shall retain the first 100 characters.

**BR-17 — Permission Key Immutability**

Permission Key shall not be editable after permission creation.

**BR-18 — Module Selection**

Module shall be selected from the available modules in the Admin Panel.

**BR-19 — Module Default State**

The Module dropdown shall have no default selection and shall initially display `Select module`.

**BR-20 — Module Options**

The Module dropdown shall display all available Admin Panel modules; the modules listed in Jira are examples rather than an exhaustive list.

**BR-21 — Description Maximum Length**

Description shall support multiline text with a maximum length of 1,000 characters.

**BR-22 — Description Length Handling**

If pasted Description text exceeds 1,000 characters, the system shall retain the first 1,000 characters.

**BR-23 — Required-Field Validation**

If a mandatory field is empty, the system shall prevent creation and display `Please fill required field` under that field.

**BR-24 — Spaces-only Mandatory Text Fields**

A blank or spaces-only Permission Name or Permission Key shall be treated as missing and display `Please fill required field`.

**BR-25 — Permission Name Duplicate Error**

If Permission Name already exists, the system shall prevent creation and display `Permission name already exists`.

**BR-26 — Permission Key Duplicate Error**

If Permission Key already exists, the system shall prevent creation and display `Permission key already exists`.

**BR-27 — Permission Name Format Error**

If Permission Name contains invalid characters, the system shall prevent creation and display `Permission name must contain only Arabic or English letters, numbers and spaces`.

**BR-28 — Permission Key Format Error**

If Permission Key contains invalid characters, the system shall prevent creation and display `Key must contain lowercase English letters, numbers, dots, and underscores only`.

**BR-29 — Validation Timing**

Required-field and format validation shall run in real time and on Create. Permission Name and Permission Key uniqueness checks shall run only when Create is selected.

**BR-30 — Multiple Field Errors**

The system shall display applicable errors under all invalid fields simultaneously.

**BR-31 — Error Priority**

Within the same field, invalid-format errors shall take priority over duplicate errors.

**BR-32 — Create Button Availability**

The Create button shall remain enabled when the form contains missing or invalid data. Selecting Create shall validate the form and prevent saving until all validations pass.

**BR-33 — Successful Permission Creation**

If all validations pass and saving succeeds, the system shall create the permission and return to the Permissions List.

**BR-34 — Successful Creation Feedback**

Upon successful creation, the system shall display `Permission created successfully` as a success notification on the Permissions List.

**BR-35 — Newly Created Permission Visibility**

The newly created permission shall appear in the Permissions List after successful creation.

**BR-36 — No Automatic Role Assignment**

A newly created permission shall not be assigned to any role automatically.

**BR-37 — Cancel Without Confirmation**

Selecting Cancel shall discard all entered data and return the Platform Admin to the Permissions List without displaying a confirmation dialog.

**BR-38 — Back Without Changes**

If no changes have been made, selecting Back shall return the Platform Admin directly to the Permissions List.

**BR-39 — Back With Unsaved Changes**

If unsaved changes exist, selecting Back shall display the following confirmation dialog:

`You have unsaved changes. Are you sure you want to leave?`

**BR-40 — Leave Action**

Selecting Leave from the unsaved-changes confirmation dialog shall discard entered changes and return to the Permissions List.

**BR-41 — Stay Action**

Selecting Stay shall close the confirmation dialog and keep the Platform Admin on the Create Permission form with the entered data.

**BR-42 — General Save Failure**

If saving fails after validation succeeds, the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**BR-43 — Save Failure Navigation**

The general save failure dialog shall provide OK. After selecting OK, the system shall return the Platform Admin to the Permissions List and discard the entered form data.

## 7. Acceptance Criteria

### AC-01 — Access Create Permission

**Given** the user is logged in as a Platform Admin,  
**And** the user has navigated to Roles & Permissions → Permissions,  
**When** the user selects Create Permission,  
**Then** the system shall open the Create Permission form.

### AC-02 — Display Creation Fields

**Given** the Create Permission form is open,  
**When** the Platform Admin views the form,  
**Then** the system shall display:

1. Permission Name.
2. Permission Key.
3. Module.
4. Description.
5. Create.
6. Cancel.
7. Back.

### AC-03 — Mandatory Fields

**Given** the Create Permission form is open,  
**When** the Platform Admin views the required fields,  
**Then** Permission Name, Permission Key, and Module shall be mandatory, while Description shall be optional.

### AC-04 — Valid Permission Name Characters

**Given** the Platform Admin is entering Permission Name,  
**When** the entered value contains Arabic letters, English letters, numbers, and spaces,  
**Then** the system shall accept those characters.

### AC-05 — Permission Name Normalization

**Given** the Platform Admin enters `  Admin   Access  ` as Permission Name,  
**When** the system normalizes the value,  
**Then** the resulting field value shall be `Admin Access `, with no leading spaces, single internal spaces, and at most one trailing space.

### AC-06 — Permission Name Maximum Length

**Given** the Platform Admin pastes a Permission Name exceeding 100 characters after applicable normalization,  
**When** the pasted value is processed,  
**Then** the system shall retain only the first 100 characters of the normalized value.

### AC-07 — Invalid Permission Name During Typing

**Given** the Platform Admin is entering Permission Name,  
**When** the Platform Admin attempts to type an invalid character,  
**Then** the system shall block the invalid character.

### AC-08 — Invalid Permission Name Through Paste

**Given** the Platform Admin pastes text containing invalid characters into Permission Name,  
**When** the system validates the pasted value,  
**Then**:

1. The invalid characters may remain temporarily in the field.
2. The system shall display `Permission name must contain only Arabic or English letters, numbers and spaces`.
3. Permission creation shall be prevented until the value is corrected.

### AC-09 — Empty Permission Name

**Given** the Platform Admin leaves Permission Name empty,  
**When** the field is validated,  
**Then** the system shall display `Please fill required field`.

### AC-10 — Spaces-only Permission Name

**Given** the Platform Admin enters only spaces in Permission Name,  
**When** the field is validated,  
**Then** the system shall treat the value as missing and display `Please fill required field`.

### AC-11 — Duplicate Permission Name

**Given** a permission named `Manage Users` already exists,  
**When** the Platform Admin enters `manage users` and selects Create,  
**Then**:

1. The system shall detect the duplicate using case-insensitive comparison.
2. The system shall display `Permission name already exists`.
3. The permission shall not be created.

### AC-12 — Duplicate Permission Name With Spaces

**Given** a permission named `Manage Users` already exists,  
**When** the Platform Admin enters `  Manage   Users  ` and selects Create,  
**Then** the system shall identify the normalized name as a duplicate and display `Permission name already exists`.

### AC-13 — Valid Permission Key Characters

**Given** the Platform Admin is entering Permission Key,  
**When** the Platform Admin enters `customers.view_1`,  
**Then** the system shall accept the value as valid in character format.

### AC-14 — Permission Key Character Arrangement

**Given** the Platform Admin enters a Permission Key containing only permitted characters,  
**When** the value contains leading, trailing, or repeated dots or underscores,  
**Then** the system shall accept the character arrangement without imposing additional naming restrictions.

### AC-15 — Permission Key Maximum Length

**Given** the Platform Admin pastes a Permission Key exceeding 100 characters,  
**When** the pasted value is processed,  
**Then** the system shall retain only the first 100 characters.

### AC-16 — Invalid Permission Key During Typing

**Given** the Platform Admin is entering Permission Key,  
**When** the Platform Admin attempts to type uppercase letters, spaces, or other invalid characters,  
**Then** the system shall block those characters.

### AC-17 — Invalid Permission Key Through Paste

**Given** the Platform Admin pastes a Permission Key containing invalid characters,  
**When** the system validates the pasted value,  
**Then**:

1. The invalid characters may remain temporarily.
2. The system shall display `Key must contain lowercase English letters, numbers, dots, and underscores only`.
3. Permission creation shall be prevented until corrected.

### AC-18 — Empty Permission Key

**Given** the Platform Admin leaves Permission Key empty,  
**When** the field is validated,  
**Then** the system shall display `Please fill required field`.

### AC-19 — Spaces-only Permission Key

**Given** the Platform Admin pastes only spaces into Permission Key,  
**When** the field is validated,  
**Then** the system shall display `Please fill required field` rather than the invalid-format error.

### AC-20 — Duplicate Permission Key

**Given** a permission with key `customers.view` already exists,  
**When** the Platform Admin enters `customers.view` and selects Create,  
**Then**:

1. The system shall identify an exact duplicate.
2. The system shall display `Permission key already exists`.
3. The permission shall not be created.

### AC-21 — Distinct Permission Keys

**Given** a permission with key `customers.view` already exists,  
**When** the Platform Admin enters `customers_view`,  
**Then** the new key shall not be considered a duplicate of `customers.view`.

### AC-22 — Permission Key Immutability

**Given** a permission has been successfully created,  
**When** the permission is subsequently accessed through a permission-editing function,  
**Then** its Permission Key shall not be editable.

### AC-23 — Module Initial State

**Given** the Platform Admin opens the Create Permission form,  
**When** the Module dropdown is displayed,  
**Then**:

1. No module shall be selected by default.
2. The dropdown shall display `Select module`.

### AC-24 — Module Options

**Given** the Platform Admin opens the Module dropdown,  
**When** the available options are displayed,  
**Then** the dropdown shall include all available modules in the Admin Panel, including the Jira-listed modules when available.

### AC-25 — Missing Module Selection

**Given** the Platform Admin has not selected a Module,  
**When** the Module field is validated,  
**Then** the system shall display `Please fill required field` and prevent creation.

### AC-26 — Optional Description

**Given** Permission Name, Permission Key, and Module are valid,  
**And** Description is empty,  
**When** the Platform Admin selects Create,  
**Then** the empty Description shall not prevent permission creation.

### AC-27 — Multiline Description

**Given** the Platform Admin is entering Description,  
**When** the Platform Admin enters text across multiple lines,  
**Then** the Description field shall accept the multiline text within the maximum length.

### AC-28 — Description Maximum Length

**Given** the Platform Admin pastes Description text exceeding 1,000 characters,  
**When** the pasted value is processed,  
**Then** the system shall retain only the first 1,000 characters.

### AC-29 — Real-time Required and Format Validation

**Given** the Platform Admin is entering or modifying form data,  
**When** a mandatory field is missing or a field contains an invalid format,  
**Then** the system shall evaluate and display the applicable required-field or format validation in real time.

### AC-30 — Uniqueness Validation on Create

**Given** the Platform Admin enters Permission Name and Permission Key,  
**When** the Platform Admin selects Create,  
**Then** the system shall evaluate uniqueness for both fields.

**And** uniqueness validation shall not be triggered solely by typing in the fields.

### AC-31 — Multiple Validation Errors

**Given** Permission Name and Permission Key contain invalid values,  
**And** Module has not been selected,  
**When** the Platform Admin selects Create,  
**Then** the system shall display applicable validation errors under all invalid fields simultaneously.

### AC-32 — Invalid Format Priority

**Given** a field contains an invalid format,  
**And** a duplicate condition also applies to that field,  
**When** the Platform Admin selects Create,  
**Then** the invalid-format error shall take priority over the duplicate error for that field.

### AC-33 — Create Button Remains Enabled

**Given** the Create Permission form contains missing or invalid data,  
**When** the Platform Admin views the Create action,  
**Then** the Create button shall remain enabled.

### AC-34 — Prevent Creation With Invalid Data

**Given** one or more applicable field validations fail,  
**When** the Platform Admin selects Create,  
**Then**:

1. The system shall prevent permission creation.
2. The Platform Admin shall remain on the Create Permission form.
3. The system shall display applicable validation errors.

### AC-35 — Successful Permission Creation

**Given** Permission Name, Permission Key, and Module are valid,  
**And** all uniqueness validations pass,  
**When** the Platform Admin selects Create and saving succeeds,  
**Then**:

1. The system shall create the permission.
2. The system shall return to the Permissions List.
3. The newly created permission shall appear in the Permissions List.
4. The system shall display `Permission created successfully`.

### AC-36 — No Automatic Role Assignment

**Given** a new permission has been successfully created,  
**When** the permission is saved,  
**Then** the system shall not automatically assign the permission to any role.

### AC-37 — Cancel With Entered Data

**Given** the Platform Admin has entered data in the Create Permission form,  
**When** the Platform Admin selects Cancel,  
**Then**:

1. The system shall discard all entered data.
2. The system shall return to the Permissions List.
3. No unsaved-changes confirmation dialog shall appear.

### AC-38 — Back Without Changes

**Given** the Platform Admin has opened the Create Permission form,  
**And** no changes have been made,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to the Permissions List.

### AC-39 — Back With Unsaved Changes

**Given** the Platform Admin has unsaved changes in the Create Permission form,  
**When** the Platform Admin selects Back,  
**Then** the system shall display the confirmation dialog:

`You have unsaved changes. Are you sure you want to leave?`

**And** provide Leave and Stay actions.

### AC-40 — Leave With Unsaved Changes

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Leave,  
**Then**:

1. The system shall discard the entered changes.
2. The system shall return to the Permissions List.

### AC-41 — Stay With Unsaved Changes

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Stay,  
**Then**:

1. The dialog shall close.
2. The Platform Admin shall remain on the Create Permission form.
3. The entered data shall be preserved.

### AC-42 — General Save Failure

**Given** all form validations have passed,  
**And** the Platform Admin selects Create,  
**When** saving the permission fails,  
**Then** the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**And** the dialog shall provide OK.

### AC-43 — Save Failure Navigation

**Given** the general save failure dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then**:

1. The error dialog shall close.
2. The system shall return to the Permissions List.
3. All entered form data shall be discarded.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin selects Create Permission | Create Permission form opens |
| EX-02 | Open Create Permission form | Four fields and Create, Cancel, Back actions displayed |
| EX-03 | Permission Name `Manage Users` | Accepted |
| EX-04 | Permission Name `إدارة المستخدمين 2` | Accepted |
| EX-05 | Permission Name `  Admin   Access  ` | Normalized to `Admin Access ` |
| EX-06 | Paste Permission Name exceeding 100 normalized characters | First 100 characters retained |
| EX-07 | Type `@` in Permission Name | Character blocked |
| EX-08 | Paste `Manage@Users` in Permission Name | Invalid-format error; creation blocked |
| EX-09 | Leave Permission Name empty | `Please fill required field` |
| EX-10 | Enter only spaces in Permission Name | `Please fill required field` |
| EX-11 | Existing `Manage Users`; enter `manage users` | `Permission name already exists` |
| EX-12 | Existing `Manage Users`; enter `  Manage   Users  ` | Duplicate detected |
| EX-13 | Permission Key `customers.view_1` | Valid format |
| EX-14 | Permission Key `customers..view` | Valid format |
| EX-15 | Permission Key `_customers.view_` | Valid format |
| EX-16 | Paste Permission Key exceeding 100 characters | First 100 characters retained |
| EX-17 | Type uppercase `A` in Permission Key | Character blocked |
| EX-18 | Paste `Customers.View` in Permission Key | Invalid-format error |
| EX-19 | Leave Permission Key empty | `Please fill required field` |
| EX-20 | Paste spaces only into Permission Key | `Please fill required field` |
| EX-21 | Existing key `customers.view`; enter same key | `Permission key already exists` |
| EX-22 | Existing key `customers.view`; enter `customers_view` | Not considered duplicate |
| EX-23 | Access Permission Key after creation | Key is not editable |
| EX-24 | Open Create Permission form | Module displays `Select module` |
| EX-25 | Open Module dropdown | All available Admin Panel modules displayed |
| EX-26 | Leave Module unselected | `Please fill required field` |
| EX-27 | Leave Description empty | Allowed |
| EX-28 | Enter multiline Description | Accepted within 1,000 characters |
| EX-29 | Paste Description exceeding 1,000 characters | First 1,000 characters retained |
| EX-30 | Enter invalid format | Validation error displayed in real time |
| EX-31 | Enter duplicate Permission Name without selecting Create | No uniqueness validation triggered solely by typing |
| EX-32 | Multiple fields invalid | Errors displayed under all invalid fields |
| EX-33 | Invalid format and duplicate condition in same field | Invalid-format error takes priority |
| EX-34 | Form contains invalid data | Create button remains enabled |
| EX-35 | Select Create with invalid fields | Creation prevented; form remains open |
| EX-36 | Select Create with valid, unique data | Permission created and displayed in Permissions List |
| EX-37 | Permission created successfully | `Permission created successfully` notification displayed |
| EX-38 | New permission created | Not assigned to any role automatically |
| EX-39 | Select Cancel after entering data | Data discarded; returns to Permissions List without confirmation |
| EX-40 | Select Back without changes | Returns directly to Permissions List |
| EX-41 | Select Back with unsaved changes | Unsaved-changes confirmation dialog displayed |
| EX-42 | Select Leave | Changes discarded; returns to Permissions List |
| EX-43 | Select Stay | Remains on form with entered data |
| EX-44 | Saving fails after valid submission | `Something went wrong. Please try again.` displayed |
| EX-45 | Select OK on save failure dialog | Returns to Permissions List; entered data discarded |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| Create Permission wireframe | Supplied for review; illustrates the creation form and its fields and actions |

No attachments were listed in the original CP-71 Jira story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-67 | Create Role | Related story with aligned name and description validation behavior |
| CP-68 | View Role | Related RBAC role-list and navigation functionality |
| CP-69 | Edit Role | Related story with comparable unsaved-changes navigation |
| CP-71 | Create Permission | Current User Story |
| CP-75 | Assign Permission To Role | Related story responsible for permission-to-role assignment |

### 9.3 Dependencies

1. **Platform Admin Access:** Create Permission is available to the Platform Admin through the Roles & Permissions section.
2. **Permissions List:** The creation flow begins from the Permissions List and returns to it after successful creation, cancellation, Back navigation, or acknowledged save failure.
3. **Existing Permission Data:** Permission Name and Permission Key uniqueness validation depends on the existing permission records.
4. **Admin Panel Modules:** The Module dropdown depends on the set of available modules in the Admin Panel.
5. **Permission Creation:** Successful saving creates a permission with the validated Permission Name, Permission Key, selected Module, and optional Description.
6. **Role-Permission Assignment:** Newly created permissions remain unassigned to roles until a separate assignment action is performed.
7. **Validation and Feedback:** The creation flow depends on the specified field-level validation messages, success notification, unsaved-changes confirmation, and general save failure dialog.

## 10. Out of Scope

The following activities are not included in CP-71:

1. Creating roles — covered by CP-67.
2. Viewing or searching roles — covered by CP-68.
3. Editing roles — covered by CP-69.
4. Deleting roles — covered by CP-70.
5. Assigning permissions to roles — covered by CP-75.
6. Automatically assigning a newly created permission to a role.
7. Editing an existing permission.
8. Changing a Permission Key after creation.
9. Deleting an existing permission.
10. Creating or configuring Admin Panel modules.
11. Assigning roles to users or managing user accounts.
12. Technical implementation details, APIs, database schemas, and system architecture.

---
