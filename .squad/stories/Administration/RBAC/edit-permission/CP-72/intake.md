# Functional Specification — CP-72: Edit Permission

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-72 |
| Story Title | Edit Permission |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** edit a permission’s details,  
**So that** its name and description stay clear and accurate.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Permissions → Edit

The Platform Admin selects **Edit** for an existing permission from the Permissions List to open the Edit Permission form.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to Roles & Permissions → Permissions.
3. The permission to be edited already exists.
4. The Platform Admin has selected the Edit action for that permission.
5. The existing permission data is available for display in the edit form.
6. The Admin Panel has available modules from which the Platform Admin can select.

## 5. Fields and Validation

### 5.1 Permission Name

| Attribute | Specification |
|---|---|
| Field Name | Permission Name |
| Field Type | Editable text input |
| Initial Value | Pre-filled with the permission's current name |
| Mandatory | Yes |
| Allowed Characters | Arabic letters, English letters (uppercase/lowercase), numbers, and spaces |
| Maximum Length | 100 characters after applicable normalization |
| Uniqueness | Required |
| Uniqueness Comparison | Case-insensitive; ignores leading and trailing spaces |
| Duplicate Check Scope | Other existing permissions; excludes the permission currently being edited |
| Validation Timing | Required-field and format validation in real time and on Save; uniqueness validation on Save only |
| Empty / Spaces-only Error | `Please fill required field` |
| Duplicate Error | `Permission name already exists` |
| Invalid Format Error | `Permission name must contain only Arabic or English letters, numbers and spaces` |

**Validation Behavior:**

1. Permission Name shall be pre-filled with the existing permission's current name.
2. The field shall be editable and mandatory.
3. The field shall accept Arabic letters, English letters, numbers, and spaces.
4. The system shall remove leading spaces from the entered value.
5. The system shall collapse consecutive internal spaces into a single space.
6. The system shall retain at most one trailing space.
7. The normalized Permission Name shall not exceed 100 characters.
8. When pasted text exceeds the maximum length, the system shall retain the first 100 characters after applicable normalization.
9. Invalid characters entered through direct typing shall be blocked.
10. Invalid characters introduced through paste may remain temporarily in the field.
11. If pasted text contains invalid characters, the system shall display `Permission name must contain only Arabic or English letters, numbers and spaces` and prevent saving until corrected.
12. An empty or spaces-only Permission Name shall be treated as missing and display `Please fill required field`.
13. Permission Name shall be unique using a case-insensitive comparison that ignores leading and trailing spaces.
14. The uniqueness check shall exclude the permission currently being edited.
15. The system shall check Permission Name uniqueness when Save is selected.
16. If another permission already has the same name under the confirmed comparison rules, the system shall display `Permission name already exists`.
17. Required-field validation shall take priority for empty or spaces-only values.
18. Otherwise, invalid-format errors shall take priority over duplicate errors.
19. The system shall prevent saving until all applicable Permission Name validations pass.

### 5.2 Permission Key

| Attribute | Specification |
|---|---|
| Field Name | Permission Key |
| Field Type | Read-only / display-only |
| Initial Value | Pre-filled with the permission's existing key |
| Editable | No |
| Example | `customers.view` |
| Behavior When Module Changes | Remains unchanged |
| Behavior After Save | Remains unchanged |

**Field Behavior:**

1. Permission Key shall display the existing permission's current key.
2. The Platform Admin shall not be able to edit the Permission Key.
3. The Permission Key shall remain unchanged throughout the editing process.
4. Changing Permission Name, Module, or Description shall not modify the Permission Key.
5. Saving the permission shall preserve the existing Permission Key.

Permission Key creation validation is not part of this edit operation because the field is immutable.

### 5.3 Module

| Attribute | Specification |
|---|---|
| Field Name | Module |
| Field Type | Editable dropdown |
| Initial Value | Current module pre-selected |
| Mandatory | Yes |
| Available Options | All available modules in the Admin Panel |
| Examples from CP-71 | Customers, Subscriptions, Settlements, Static Pages, Settings, RBAC |
| Validation Timing | Required-field validation in real time and on Save |
| Empty Selection Error | `Please fill required field` |

**Validation Behavior:**

1. Module shall be pre-filled with the permission's current module.
2. The field shall be editable and mandatory.
3. The dropdown shall display all available modules in the Admin Panel.
4. The modules listed in CP-71 are examples and shall not be treated as an exhaustive list.
5. The Platform Admin shall be able to select a different available module.
6. If the Module selection is cleared or left empty, the system shall display `Please fill required field`.
7. The system shall prevent saving until a valid Module selection is provided.
8. If a different Module is selected and saving succeeds, the system shall save the selected Module.
9. Changing Module shall not change Permission Key.

### 5.4 Description

| Attribute | Specification |
|---|---|
| Field Name | Description |
| Field Type | Editable multiline text input |
| Initial Value | Pre-filled with the existing description, if any |
| Mandatory | No |
| Maximum Length | 1,000 characters |
| Allowed Content | Text; no additional character restrictions confirmed |
| Empty Value | Allowed |
| Overlength Paste Handling | Retain the first 1,000 characters |

**Validation Behavior:**

1. Description shall display the existing permission's description, if one exists.
2. The field shall be editable and optional.
3. The field shall support multiline text.
4. The field shall accept up to 1,000 characters.
5. When pasted text exceeds 1,000 characters, the system shall retain the first 1,000 characters.
6. The Platform Admin may update, clear, or leave Description unchanged.
7. An empty Description shall not prevent saving.
8. No additional Description character restrictions shall be imposed by this specification.

### 5.5 Form-Level Validation and Actions

| Action / Condition | Required Behavior |
|---|---|
| Form Opening | Display the existing permission's current values |
| Save | Validate editable fields and save only when all applicable validations pass |
| Save Without Changes | Allowed; return to Permissions List and display the confirmed success notification |
| Invalid or Missing Data | Keep Save enabled, display applicable field errors, and prevent saving |
| Multiple Invalid Fields | Display errors under all invalid fields simultaneously |
| Duplicate Permission Name | Display `Permission name already exists` |
| Successful Save | Save changes, return to Permissions List, and display `Permission updated successfully` |
| Permission Key | Remain read-only and unchanged |
| Cancel | Discard all unsaved changes and return to Permissions List without confirmation |
| Back — No Changes | Return directly to Permissions List |
| Back — Unsaved Changes | Display the confirmed unsaved-changes dialog |
| Save Failure | Display the general error dialog; after OK, remain on the Edit Permission form and preserve entered changes |

**Validation and Form Behavior:**

1. Required-field and format validation shall run in real time and when Save is selected.
2. Permission Name uniqueness validation shall run only when Save is selected.
3. The uniqueness check shall exclude the permission currently being edited.
4. The Save button shall remain enabled when the form contains invalid or missing data.
5. Selecting Save shall validate the applicable editable fields before saving.
6. The system shall display errors under all invalid fields simultaneously.
7. For Permission Name, validation priority shall be Required → Invalid Format → Duplicate.
8. The system shall prevent saving while any mandatory field is missing or any applicable validation fails.
9. Saving without making changes shall be allowed.
10. On successful Save, the system shall return to the Permissions List and display `Permission updated successfully`.
11. Permission Key shall remain unchanged after saving.

### 5.6 Unsaved Changes Detection

1. The system shall compare the current values of editable fields against the original values loaded into the Edit Permission form.
2. Editable fields considered in the comparison shall be Permission Name, Module, and Description.
3. If any current editable field value differs from its original loaded value, the form shall be treated as having unsaved changes.
4. If all modified fields are restored to their original loaded values, the form shall be treated as unchanged.
5. Permission Key shall not contribute to unsaved-changes detection because it is read-only and cannot be modified.
6. Unsaved-changes detection shall determine whether selecting Back requires a confirmation dialog.

## 6. Business Rules

**BR-01 — Edit Permission Authorization**

Only the Platform Admin shall be authorized to access the Edit Permission functionality.

**BR-02 — Edit Permission Entry Point**

The system shall provide access to Edit Permission through Roles & Permissions → Permissions → Edit.

**BR-03 — Existing Permission Data**

The Edit Permission form shall display the selected permission's existing data.

**BR-04 — Editable Fields**

Permission Name, Module, and Description shall be editable.

**BR-05 — Permission Key Immutability**

Permission Key shall be displayed as read-only and shall remain unchanged throughout editing and saving.

**BR-06 — Required Fields**

Permission Name and Module shall be mandatory.

**BR-07 — Optional Description**

Description shall be optional.

**BR-08 — Permission Name Allowed Characters**

Permission Name shall accept Arabic letters, English letters, numbers, and spaces only.

**BR-09 — Permission Name Maximum Length**

Permission Name shall have a maximum length of 100 characters after applicable normalization.

**BR-10 — Permission Name Normalization**

The system shall remove leading spaces, collapse consecutive internal spaces into one, and retain at most one trailing space in Permission Name.

**BR-11 — Permission Name Uniqueness**

Permission Name shall be unique using a case-insensitive comparison that ignores leading and trailing spaces.

**BR-12 — Current Permission Exclusion**

The Permission Name uniqueness check shall exclude the permission currently being edited.

**BR-13 — Permission Name Invalid Input Handling**

Invalid characters entered through direct typing shall be blocked. Invalid pasted characters may remain temporarily with a validation error and shall prevent saving until corrected.

**BR-14 — Permission Name Length Handling**

If pasted Permission Name text exceeds 100 characters after applicable normalization, the system shall retain the first 100 characters.

**BR-15 — Permission Key Read-only Display**

The system shall display the existing Permission Key without allowing modification.

**BR-16 — Permission Key Independence From Module**

Changing the selected Module shall not modify the Permission Key.

**BR-17 — Module Selection**

Module shall be selected from the available modules in the Admin Panel.

**BR-18 — Module Initial State**

The Module dropdown shall initially display the permission's current Module as the selected value.

**BR-19 — Module Options**

The Module dropdown shall display all available Admin Panel modules, rather than being restricted to the examples listed in CP-71.

**BR-20 — Module Update**

When a different Module is selected and saving succeeds, the system shall save the selected Module while preserving the Permission Key.

**BR-21 — Description Maximum Length**

Description shall support multiline text with a maximum length of 1,000 characters.

**BR-22 — Description Length Handling**

If pasted Description text exceeds 1,000 characters, the system shall retain the first 1,000 characters.

**BR-23 — Required-field Validation**

If Permission Name or Module is empty, the system shall prevent saving and display `Please fill required field` under the affected field.

**BR-24 — Spaces-only Permission Name**

A spaces-only Permission Name shall be treated as missing and display `Please fill required field`.

**BR-25 — Duplicate Permission Name**

If another permission has the same name under the confirmed uniqueness rules, the system shall prevent saving and display `Permission name already exists`.

**BR-26 — Invalid Permission Name**

If Permission Name contains invalid characters, the system shall prevent saving and display `Permission name must contain only Arabic or English letters, numbers and spaces`.

**BR-27 — Validation Timing**

Required-field and format validation shall run in real time and on Save. Permission Name uniqueness validation shall run only when Save is selected.

**BR-28 — Validation Priority**

For Permission Name, required-field errors shall take priority for empty or spaces-only values. Otherwise, invalid-format errors shall take priority over duplicate errors.

**BR-29 — Multiple Field Errors**

The system shall display applicable validation errors under all invalid fields simultaneously.

**BR-30 — Save Button Availability**

The Save button shall remain enabled when the form contains missing or invalid data.

**BR-31 — Prevent Invalid Updates**

Selecting Save shall validate the editable fields and prevent saving until all applicable validations pass.

**BR-32 — Save Without Changes**

The Platform Admin shall be permitted to select Save without changing any field values.

**BR-33 — Successful Permission Update**

If all validations pass and saving succeeds, the system shall save the permission's editable details and return to the Permissions List.

**BR-34 — Successful Update Feedback**

Upon successful Save, the system shall display `Permission updated successfully` as a success notification on the Permissions List.

**BR-35 — Cancel Without Confirmation**

Selecting Cancel shall discard all unsaved changes and return to the Permissions List without displaying a confirmation dialog.

**BR-36 — Back Without Changes**

If no unsaved changes exist, selecting Back shall return directly to the Permissions List.

**BR-37 — Unsaved Changes Comparison**

The system shall determine whether unsaved changes exist by comparing current editable field values with their original loaded values.

**BR-38 — Reverted Changes**

If all edited values are restored to their original loaded values, the form shall be treated as unchanged.

**BR-39 — Back With Unsaved Changes**

If unsaved changes exist, selecting Back shall display the confirmation dialog:

`You have unsaved changes. Are you sure you want to leave?`

**BR-40 — Leave Action**

Selecting Leave shall discard unsaved changes and return to the Permissions List.

**BR-41 — Stay Action**

Selecting Stay shall close the confirmation dialog and keep the Platform Admin on the Edit Permission form with the entered values preserved.

**BR-42 — General Save Failure**

If saving fails after validation succeeds, the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**BR-43 — Save Failure Navigation**

The error dialog shall provide OK. After selecting OK, the system shall remain on the Edit Permission form and preserve the entered changes.

## 7. Acceptance Criteria

### AC-01 — Access Edit Permission

**Given** the user is logged in as a Platform Admin,  
**And** the user has navigated to Roles & Permissions → Permissions,  
**When** the user selects Edit for an existing permission,  
**Then** the system shall open the Edit Permission form for the selected permission.

### AC-02 — Display Pre-filled Fields

**Given** the Platform Admin opens Edit Permission for an existing permission,  
**When** the form is displayed,  
**Then**:

1. Permission Name shall display the existing name.
2. Permission Key shall display the existing key.
3. Module shall display the current selected module.
4. Description shall display the existing description, if any.
5. Save, Cancel, and Back actions shall be available.

### AC-03 — Editable and Read-only Fields

**Given** the Edit Permission form is open,  
**When** the Platform Admin interacts with the fields,  
**Then** Permission Name, Module, and Description shall be editable,  
**And** Permission Key shall remain read-only.

### AC-04 — Mandatory Fields

**Given** the Edit Permission form is open,  
**When** the Platform Admin views the fields,  
**Then** Permission Name and Module shall be mandatory,  
**And** Description shall be optional.

### AC-05 — Valid Permission Name Characters

**Given** the Platform Admin is editing Permission Name,  
**When** the value contains Arabic letters, English letters, numbers, and spaces,  
**Then** the system shall accept those characters.

### AC-06 — Permission Name Normalization

**Given** the Platform Admin enters `  View   Customers  ` as Permission Name,  
**When** the system normalizes the value,  
**Then** the resulting field value shall be `View Customers `, with no leading spaces, single internal spaces, and at most one trailing space.

### AC-07 — Permission Name Maximum Length

**Given** the Platform Admin pastes a Permission Name exceeding 100 characters after applicable normalization,  
**When** the pasted value is processed,  
**Then** the system shall retain only the first 100 characters of the normalized value.

### AC-08 — Invalid Permission Name During Typing

**Given** the Platform Admin is editing Permission Name,  
**When** the Platform Admin attempts to type an invalid character,  
**Then** the system shall block the invalid character.

### AC-09 — Invalid Permission Name Through Paste

**Given** the Platform Admin pastes text containing invalid characters into Permission Name,  
**When** the system validates the pasted value,  
**Then**:

1. The invalid characters may remain temporarily in the field.
2. The system shall display `Permission name must contain only Arabic or English letters, numbers and spaces`.
3. Saving shall be prevented until the value is corrected.

### AC-10 — Empty Permission Name

**Given** the Platform Admin clears Permission Name,  
**When** the field is validated,  
**Then** the system shall display `Please fill required field`.

### AC-11 — Spaces-only Permission Name

**Given** the Platform Admin enters only spaces in Permission Name,  
**When** the field is validated,  
**Then** the system shall treat the value as missing and display `Please fill required field`.

### AC-12 — Duplicate Permission Name

**Given** another permission named `Manage Customers` already exists,  
**When** the Platform Admin enters `manage customers` and selects Save,  
**Then**:

1. The system shall detect the duplicate using the confirmed comparison rules.
2. The system shall display `Permission name already exists`.
3. The system shall not save the changes.

### AC-13 — Duplicate Permission Name With Spaces

**Given** another permission named `Manage Customers` already exists,  
**When** the Platform Admin enters `  Manage   Customers  ` and selects Save,  
**Then** the system shall identify the normalized name as a duplicate and display `Permission name already exists`.

### AC-14 — Current Permission Excluded From Duplicate Check

**Given** the permission being edited is named `View Customers`,  
**And** no other permission has the same name under the confirmed comparison rules,  
**When** the Platform Admin keeps `View Customers` unchanged and selects Save,  
**Then** the system shall not treat the current permission as a duplicate.

### AC-15 — Permission Key Display

**Given** the permission being edited has key `customers.view`,  
**When** the Edit Permission form is displayed,  
**Then** the system shall show `customers.view` as read-only.

### AC-16 — Prevent Permission Key Editing

**Given** the Edit Permission form is open,  
**When** the Platform Admin attempts to modify Permission Key,  
**Then** the system shall not allow the key to be changed.

### AC-17 — Permission Key Unchanged When Module Changes

**Given** the permission has key `customers.view`,  
**When** the Platform Admin changes Module and saves successfully,  
**Then** the Permission Key shall remain `customers.view`.

### AC-18 — Module Pre-selection

**Given** the permission's current Module is Customers,  
**When** the Platform Admin opens Edit Permission,  
**Then** Customers shall be pre-selected in the Module dropdown.

### AC-19 — Module Options

**Given** the Platform Admin opens the Module dropdown,  
**When** the options are displayed,  
**Then** the dropdown shall include all available Admin Panel modules.

### AC-20 — Change Module

**Given** the permission currently belongs to Customers,  
**When** the Platform Admin selects another available Module and saves successfully,  
**Then** the system shall save the newly selected Module,  
**And** the Permission Key shall remain unchanged.

### AC-21 — Missing Module Selection

**Given** the Platform Admin clears the Module selection,  
**When** the field is validated,  
**Then** the system shall display `Please fill required field` and prevent saving.

### AC-22 — Optional Description

**Given** Permission Name and Module are valid,  
**When** the Platform Admin clears Description and selects Save,  
**Then** the empty Description shall not prevent saving.

### AC-23 — Multiline Description

**Given** the Platform Admin is editing Description,  
**When** the Platform Admin enters text across multiple lines,  
**Then** the Description field shall accept the multiline text within the maximum length.

### AC-24 — Description Maximum Length

**Given** the Platform Admin pastes Description text exceeding 1,000 characters,  
**When** the pasted value is processed,  
**Then** the system shall retain only the first 1,000 characters.

### AC-25 — Real-time Required and Format Validation

**Given** the Platform Admin is modifying editable fields,  
**When** Permission Name or Module is missing, or Permission Name contains an invalid format,  
**Then** the system shall evaluate and display the applicable validation errors in real time.

### AC-26 — Uniqueness Validation on Save

**Given** the Platform Admin changes Permission Name,  
**When** the Platform Admin selects Save,  
**Then** the system shall check the name against other existing permissions, excluding the current permission.

**And** uniqueness validation shall not be triggered solely by typing in the field.

### AC-27 — Required-field Error Priority

**Given** Permission Name is empty or contains only spaces,  
**When** the field is validated,  
**Then** the system shall display `Please fill required field` as the priority error.

### AC-28 — Invalid Format Priority

**Given** Permission Name contains an invalid format,  
**And** a duplicate condition also applies to the field,  
**When** the Platform Admin selects Save,  
**Then** the invalid-format error shall take priority over the duplicate error.

### AC-29 — Multiple Validation Errors

**Given** Permission Name is invalid,  
**And** Module is unselected,  
**When** the Platform Admin selects Save,  
**Then** the system shall display applicable validation errors under both invalid fields simultaneously.

### AC-30 — Save Button Remains Enabled

**Given** the Edit Permission form contains missing or invalid data,  
**When** the Platform Admin views the Save action,  
**Then** the Save button shall remain enabled.

### AC-31 — Prevent Saving Invalid Data

**Given** one or more applicable field validations fail,  
**When** the Platform Admin selects Save,  
**Then**:

1. The system shall not save the changes.
2. The Platform Admin shall remain on the Edit Permission form.
3. The system shall display applicable validation errors.

### AC-32 — Save Without Changes

**Given** the Platform Admin opens Edit Permission and makes no changes,  
**When** the Platform Admin selects Save,  
**Then**:

1. Saving shall be allowed.
2. The system shall return to the Permissions List after successful saving.
3. The system shall display `Permission updated successfully`.

### AC-33 — Successful Permission Update

**Given** the Platform Admin has entered valid editable field values,  
**And** the Permission Name uniqueness check passes,  
**When** the Platform Admin selects Save and saving succeeds,  
**Then**:

1. The system shall save the updated Permission Name, Module, and Description values, as applicable.
2. The Permission Key shall remain unchanged.
3. The system shall return to the Permissions List.
4. The system shall display `Permission updated successfully`.

### AC-34 — Cancel With Unsaved Changes

**Given** the Platform Admin has modified editable fields,  
**When** the Platform Admin selects Cancel,  
**Then**:

1. The system shall discard all unsaved changes.
2. The system shall return to the Permissions List.
3. No unsaved-changes confirmation dialog shall appear.

### AC-35 — Back Without Changes

**Given** the Platform Admin has opened Edit Permission,  
**And** the current editable field values match the original loaded values,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to the Permissions List.

### AC-36 — Back With Unsaved Changes

**Given** at least one editable field value differs from its original loaded value,  
**When** the Platform Admin selects Back,  
**Then** the system shall display:

`You have unsaved changes. Are you sure you want to leave?`

**And** provide Leave and Stay actions.

### AC-37 — Leave With Unsaved Changes

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Leave,  
**Then**:

1. The system shall discard the unsaved changes.
2. The system shall return to the Permissions List.

### AC-38 — Stay With Unsaved Changes

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Stay,  
**Then**:

1. The dialog shall close.
2. The Platform Admin shall remain on the Edit Permission form.
3. The entered changes shall be preserved.

### AC-39 — Reverted Changes

**Given** the Platform Admin changes Permission Name, Module, or Description,  
**And** subsequently restores all editable fields to their original loaded values,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to the Permissions List without displaying an unsaved-changes confirmation dialog.

### AC-40 — General Save Failure

**Given** all applicable validations have passed,  
**And** the Platform Admin selects Save,  
**When** saving the changes fails,  
**Then** the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**And** the dialog shall provide OK.

### AC-41 — Save Failure Navigation

**Given** the general save failure dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then**:

1. The dialog shall close.
2. The Platform Admin shall remain on the Edit Permission form.
3. All entered changes shall be preserved.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin selects Edit for an existing permission | Edit Permission form opens |
| EX-02 | Open Edit Permission for `View Customers` | Current permission values are pre-filled |
| EX-03 | Permission Name `Manage Customers` | Accepted when unique |
| EX-04 | Permission Name `إدارة المستخدمين 2` | Accepted when unique |
| EX-05 | Permission Name `  View   Customers  ` | Normalized to `View Customers ` |
| EX-06 | Paste Permission Name exceeding 100 normalized characters | First 100 characters retained |
| EX-07 | Type `@` in Permission Name | Character blocked |
| EX-08 | Paste `View@Customers` | Invalid-format error; saving blocked |
| EX-09 | Clear Permission Name | `Please fill required field` |
| EX-10 | Enter only spaces in Permission Name | `Please fill required field` |
| EX-11 | Another permission named `Manage Customers` exists; enter `manage customers` | `Permission name already exists` |
| EX-12 | Another permission named `Manage Customers` exists; enter `  Manage   Customers  ` | Duplicate detected |
| EX-13 | Keep the current permission's existing name | No duplicate error caused by the current record |
| EX-14 | Open permission with key `customers.view` | Key displayed read-only |
| EX-15 | Attempt to edit Permission Key | Modification prevented |
| EX-16 | Change Module from Customers to RBAC | Module update allowed; key unchanged |
| EX-17 | Open permission whose Module is Customers | Customers pre-selected |
| EX-18 | Open Module dropdown | All available Admin Panel modules displayed |
| EX-19 | Clear Module selection | `Please fill required field` |
| EX-20 | Clear Description | Allowed |
| EX-21 | Enter multiline Description | Accepted within 1,000 characters |
| EX-22 | Paste Description exceeding 1,000 characters | First 1,000 characters retained |
| EX-23 | Enter invalid Permission Name format | Validation error displayed in real time |
| EX-24 | Enter duplicate Permission Name without selecting Save | No uniqueness check triggered solely by typing |
| EX-25 | Empty Permission Name and unselected Module | Required-field errors displayed under both fields |
| EX-26 | Invalid format and duplicate condition apply | Invalid-format error takes priority |
| EX-27 | Form contains invalid data | Save button remains enabled |
| EX-28 | Select Save with invalid fields | Saving prevented; form remains open |
| EX-29 | Select Save without changing values | Successful save returns to Permissions List |
| EX-30 | Save valid updated details | Changes saved; Permissions List displayed |
| EX-31 | Successful Save | `Permission updated successfully` notification displayed |
| EX-32 | Select Cancel after modifying data | Changes discarded; returns to Permissions List without confirmation |
| EX-33 | Select Back without changes | Returns directly to Permissions List |
| EX-34 | Select Back with unsaved changes | Confirmation dialog displayed |
| EX-35 | Select Leave | Changes discarded; returns to Permissions List |
| EX-36 | Select Stay | Remains on form with entered values preserved |
| EX-37 | Modify a field, then restore its original value | No unsaved-changes confirmation if all editable values match originals |
| EX-38 | Saving fails after valid submission | `Something went wrong. Please try again.` displayed |
| EX-39 | Select OK on save failure dialog | Remains on Edit Permission form with entered changes preserved |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| Edit Permission wireframe | Supplied for review; illustrates pre-filled fields, read-only Permission Key, Module dropdown, Description, and Save/Cancel/Back actions |

No attachments were listed in the original CP-72 Jira story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-67 | Create Role | Related RBAC story with comparable field validation conventions |
| CP-69 | Edit Role | Related edit workflow with comparable save-failure handling |
| CP-71 | Create Permission | Source of explicitly confirmed shared Permission Name, Module, Description, and Permission Key immutability rules |
| CP-72 | Edit Permission | Current User Story |
| CP-75 | Assign Permission To Role | Related functionality for assigning permissions to roles |

### 9.3 Dependencies

1. **Platform Admin Access:** Edit Permission is available to the Platform Admin through the Roles & Permissions section.
2. **Existing Permission:** The selected permission must exist and have current data available for display.
3. **Permissions List:** The edit flow begins from the Permissions List and returns to it after successful saving, cancellation, or confirmed Back navigation.
4. **Existing Permission Data:** Permission Name uniqueness validation depends on other existing permission records and excludes the permission currently being edited.
5. **Admin Panel Modules:** The Module dropdown depends on the available modules in the Admin Panel.
6. **Permission Key Immutability:** The existing Permission Key remains unchanged regardless of updates to other editable fields.
7. **Validation and Feedback:** The edit flow uses the specified field-level validation messages, success notification, unsaved-changes confirmation, and general save failure dialog.

## 10. Out of Scope

The following activities are not included in CP-72:

1. Creating a new permission — covered by CP-71.
2. Creating roles — covered by CP-67.
3. Editing roles — covered by CP-69.
4. Deleting roles — covered by CP-70.
5. Assigning permissions to roles — covered by CP-75.
6. Editing or regenerating Permission Key.
7. Automatically changing Permission Key when Module changes.
8. Deleting an existing permission.
9. Creating or configuring Admin Panel modules.
10. Assigning roles to users or managing user accounts.
11. Changing role-permission assignments as part of editing permission details.
12. Technical implementation details, APIs, database schemas, and system architecture.

---

