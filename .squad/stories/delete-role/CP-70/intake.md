# Functional Specification — CP-70: Delete Role

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-70 |
| Story Title | Delete Role |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** delete a role that is no longer needed,  
**So that** the role list stays clean and relevant.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Roles → Delete

The Platform Admin selects the **Delete** action for a specific role from the Roles List.

If the selected role is available, the system opens the Delete Role confirmation dialog.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to Roles & Permissions → Roles.
3. The Roles List is accessible to the Platform Admin.
4. The Platform Admin has selected a role for deletion.
5. Only the Platform Admin is authorized to perform the Delete Role functionality.

**Note:** Having no linked users is a deletion eligibility condition checked when Confirm Delete is selected, not a prerequisite for opening the confirmation dialog.

## 5. Fields and Validation

### 5.1 Delete Role Confirmation Dialog

The system shall display a confirmation dialog when the Platform Admin selects Delete for an available role.

| Attribute | Specification |
|---|---|
| Dialog Title | Delete Role |
| Role Name | Displays the name of the selected role |
| Confirmation Message | `Are you sure you want to delete this role? This action cannot be undone.` |
| Confirm Delete | Initiates the deletion process |
| Cancel | Closes the confirmation dialog without making changes |

**Dialog Behavior:**

1. The dialog shall display the name of the role selected for deletion.
2. The dialog shall display the confirmation message exactly as specified.
3. The Platform Admin shall be able to select Confirm Delete or Cancel.
4. Selecting Cancel shall close the dialog and leave the Platform Admin on the Roles List without modifying the role or its relationships.
5. Selecting Confirm Delete shall initiate the deletion process for the selected role.
6. The Confirm Delete action shall be disabled while the deletion request is being processed to prevent duplicate execution of the same operation.
7. If the role is already unavailable when Delete is selected from the Roles List, the confirmation dialog shall not open. The system shall display the role-unavailability error instead.

### 5.2 Deletion Eligibility Validation

| Validation | Required Behavior |
|---|---|
| Role Availability | The selected role must still be available when deletion is attempted |
| Linked Users | The role must have no linked users |
| Assigned Permissions | Assigned permissions do not prevent deletion |
| Validation Timing | Linked-user eligibility is verified when Confirm Delete is selected |
| Data Used | The latest available role-user relationship data |
| Multiple Error Conditions | Role unavailability takes priority over linked-user validation errors |

**Validation Behavior:**

1. When Confirm Delete is selected, the system shall verify that the selected role is available.
2. The system shall verify that no users are linked to the role, using the latest data at the time of the deletion attempt.
3. If one or more users are linked to the role, deletion shall be rejected.
4. If users become linked to the role after the confirmation dialog opens but before Confirm Delete is selected, deletion shall be rejected.
5. A role with assigned permissions and no linked users shall be eligible for deletion.
6. A role with neither assigned permissions nor linked users shall be eligible for deletion.
7. If the role is unavailable, deletion shall be prevented regardless of previously known linked-user information.
8. If deletion is rejected, the role and its existing relationships shall remain unchanged.

### 5.3 Deletion Results and Messages

| Scenario | Message | Presentation / Navigation |
|---|---|---|
| Successful deletion | `Role deleted successfully` | Success notification on the Roles List |
| Role linked to users | `This role can’t be deleted because it is linked to users. Please change their role first.` | Error dialog with OK; remain on Roles List |
| General deletion failure | `Something went wrong. Please try again.` | Error dialog with OK; remain on Roles List |
| Role unavailable | `Role is no longer available.` | Error dialog with OK; return to or remain on Roles List |

**Result Behavior:**

1. Successful deletion shall remove the selected role and its permission links.
2. The permissions themselves shall remain in the system.
3. Following successful deletion, the system shall display the Roles List without the deleted role.
4. The system shall display `Role deleted successfully` as a success notification.
5. If deletion is rejected because users are linked to the role, the system shall display the exact linked-users error message.
6. If deletion fails for another reason, the system shall display the exact general failure message.
7. If the selected role is unavailable, the system shall display the exact role-unavailability message.
8. Error dialogs shall provide an OK action.
9. Failed or rejected deletion attempts shall not partially delete the role or its permission links.

## 6. Business Rules

**BR-01 — Delete Role Authorization**

Only the Platform Admin shall be authorized to perform the Delete Role functionality.

**BR-02 — Delete Action Entry Point**

The system shall provide access to Delete Role through the Delete action for a selected role in the Roles List.

**BR-03 — Confirmation Dialog**

Selecting Delete for an available role shall open the Delete Role confirmation dialog.

**BR-04 — Selected Role Identification**

The confirmation dialog shall display the name of the role selected for deletion.

**BR-05 — Confirmation Message**

The confirmation dialog shall display the following exact message:

`Are you sure you want to delete this role? This action cannot be undone.`

**BR-06 — Confirmation Actions**

The confirmation dialog shall provide Confirm Delete and Cancel actions.

**BR-07 — Cancel Without Changes**

Selecting Cancel shall close the confirmation dialog and leave the Platform Admin on the Roles List without modifying the role or its relationships.

**BR-08 — Deletion Initiation**

Selecting Confirm Delete shall initiate the deletion process for the selected role.

**BR-09 — Role Availability Validation**

The system shall prevent deletion if the selected role is no longer available when deletion is attempted.

**BR-10 — Linked Users Validation**

When Confirm Delete is selected, the system shall verify that no users are linked to the selected role, using the latest available data.

**BR-11 — Linked Users Restriction**

The system shall reject deletion if one or more users are linked to the selected role.

**BR-12 — Concurrent User Assignment**

If a user becomes linked to the selected role after the confirmation dialog opens but before deletion is confirmed, the system shall reject deletion.

**BR-13 — Role With Assigned Permissions**

The system shall allow deletion of a role with assigned permissions if no users are linked to that role.

**BR-14 — Role Without Permissions or Users**

The system shall allow deletion of a role that has no assigned permissions and no linked users.

**BR-15 — Role and Permission Link Removal**

Upon successful deletion, the system shall delete the selected role and its associated permission links.

**BR-16 — Permission Preservation**

Deleting a role shall not delete the permissions themselves.

**BR-17 — Successful Deletion Navigation**

Upon successful deletion, the system shall return the Platform Admin to the Roles List.

**BR-18 — Successful Deletion Feedback**

Upon successful deletion, the system shall display the following success notification on the Roles List:

`Role deleted successfully`

**BR-19 — Roles List Update**

Following successful deletion, the deleted role shall no longer appear in the Roles List, which shall display the latest role data.

**BR-20 — Linked Users Error Message**

If deletion is rejected because users are linked to the role, the system shall display an error dialog containing:

`This role can’t be deleted because it is linked to users. Please change their role first.`

**BR-21 — Linked Users Error Navigation**

The linked-users error dialog shall provide OK. After selecting OK, the Platform Admin shall remain on the Roles List.

**BR-22 — Rejected Deletion Data Preservation**

If deletion is rejected because users are linked to the role, the role and all its existing relationships shall remain unchanged.

**BR-23 — General Deletion Failure Message**

If deletion fails for a reason other than linked users or role unavailability, the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**BR-24 — General Failure Navigation**

The general deletion failure dialog shall provide OK. After selecting OK, the Platform Admin shall remain on the Roles List.

**BR-25 — No Partial Deletion**

If the deletion operation fails, the role and its permission links shall remain unchanged. Partial deletion shall not be permitted.

**BR-26 — Duplicate Deletion Prevention**

The system shall prevent duplicate execution of the same deletion operation by disabling Confirm Delete while the deletion request is being processed.

**BR-27 — Role Unavailable Before Opening Confirmation**

If the selected role is unavailable when Delete is selected from the Roles List, the system shall not open the confirmation dialog.

Instead, the system shall display an error dialog containing:

`Role is no longer available.`

**BR-28 — Role Unavailable During Deletion**

If the role becomes unavailable after the confirmation dialog opens but before deletion is completed, the system shall prevent deletion and display:

`Role is no longer available.`

**BR-29 — Role Unavailability Navigation**

The role-unavailability error dialog shall provide OK. After selecting OK, the Platform Admin shall return to or remain on the Roles List.

**BR-30 — Error Priority**

If role unavailability and linked-user conditions both apply, the system shall prioritize the role-unavailability error.

**BR-31 — Roles List Ordering**

Following deletion or a rejected deletion attempt, the Roles List shall follow the alphabetical Role Name ordering (A–Z) defined in CP-68.

## 7. Acceptance Criteria

### AC-01 — Access Delete Role

**Given** the user is logged in as a Platform Admin,  
**And** the user has navigated to Dashboard → Roles & Permissions → Roles,  
**When** the user selects Delete for an available role,  
**Then** the system shall open the Delete Role confirmation dialog.

### AC-02 — Display Selected Role Name

**Given** the Platform Admin selects Delete for an available role,  
**When** the confirmation dialog opens,  
**Then** the dialog shall display the name of the selected role.

### AC-03 — Display Confirmation Message

**Given** the Delete Role confirmation dialog is open,  
**When** the Platform Admin views the dialog,  
**Then** the system shall display:

`Are you sure you want to delete this role? This action cannot be undone.`

### AC-04 — Display Confirmation Actions

**Given** the Delete Role confirmation dialog is open,  
**When** the Platform Admin views the available actions,  
**Then** the dialog shall provide:

1. Confirm Delete.
2. Cancel.

### AC-05 — Cancel Deletion

**Given** the Delete Role confirmation dialog is open,  
**When** the Platform Admin selects Cancel,  
**Then**:

1. The confirmation dialog shall close.
2. The Platform Admin shall remain on the Roles List.
3. The selected role shall remain unchanged.
4. Its permission links and user relationships shall remain unchanged.

### AC-06 — Validate Linked Users on Confirmation

**Given** the Delete Role confirmation dialog is open,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall verify that no users are linked to the selected role using the latest data.

### AC-07 — Delete Role Without Linked Users

**Given** the selected role is available,  
**And** no users are linked to the role,  
**When** the Platform Admin selects Confirm Delete,  
**Then**:

1. The system shall delete the selected role.
2. The system shall remove the role's permission links.
3. The permissions themselves shall remain available.
4. The system shall return to the Roles List.
5. The deleted role shall no longer appear in the Roles List.
6. The system shall display `Role deleted successfully`.

### AC-08 — Reject Deletion of Role Linked to Users

**Given** the selected role is available,  
**And** one or more users are linked to the role,  
**When** the Platform Admin selects Confirm Delete,  
**Then**:

1. The system shall reject deletion.
2. The system shall display an error dialog containing:

   `This role can’t be deleted because it is linked to users. Please change their role first.`

3. The dialog shall provide OK.
4. The role and its relationships shall remain unchanged.

### AC-09 — Linked Users Error Navigation

**Given** deletion has been rejected because users are linked to the role,  
**And** the linked-users error dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then** the system shall close the dialog and leave the Platform Admin on the Roles List.

### AC-10 — Users Linked After Confirmation Dialog Opens

**Given** the Delete Role confirmation dialog is open,  
**And** the selected role initially has no linked users,  
**And** a user becomes linked to the role before Confirm Delete is selected,  
**When** the Platform Admin selects Confirm Delete,  
**Then**:

1. The system shall reject deletion.
2. The system shall display the linked-users error message defined in AC-08.
3. The role and its relationships shall remain unchanged.

### AC-11 — Delete Role With Assigned Permissions

**Given** the selected role is available,  
**And** the role has one or more assigned permissions,  
**And** the role has no linked users,  
**When** the Platform Admin selects Confirm Delete,  
**Then**:

1. The system shall delete the role.
2. The system shall remove its permission links.
3. The assigned permissions themselves shall remain available.
4. The system shall display the successful deletion result.

### AC-12 — Delete Role With No Permissions or Users

**Given** the selected role is available,  
**And** the role has no assigned permissions,  
**And** the role has no linked users,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall delete the role and display the successful deletion result.

### AC-13 — Preserve Permissions After Deletion

**Given** the selected role has assigned permissions but no linked users,  
**When** the Platform Admin successfully deletes the role,  
**Then**:

1. The role's permission links shall be removed.
2. The permissions themselves shall not be deleted.

### AC-14 — Prevent Duplicate Confirmation

**Given** the Platform Admin has selected Confirm Delete,  
**And** the deletion request is being processed,  
**When** the Platform Admin attempts to select Confirm Delete again,  
**Then**:

1. Confirm Delete shall be disabled during processing.
2. The system shall not execute a duplicate deletion operation for the same request.

### AC-15 — Successful Deletion Notification

**Given** a role has been successfully deleted,  
**When** the system returns to the Roles List,  
**Then** the system shall display the success notification:

`Role deleted successfully`

### AC-16 — Remove Deleted Role From Roles List

**Given** a role has been successfully deleted,  
**When** the Roles List is displayed,  
**Then**:

1. The deleted role shall no longer appear in the list.
2. The list shall display the latest role data.
3. The remaining roles shall be ordered alphabetically by Role Name (A–Z).

### AC-17 — General Deletion Failure

**Given** the Platform Admin confirms deletion of a role,  
**When** the deletion operation fails for a reason other than linked users or role unavailability,  
**Then** the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**And** the dialog shall provide OK.

### AC-18 — General Failure Navigation

**Given** the general deletion failure dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then**:

1. The error dialog shall close.
2. The Platform Admin shall remain on the Roles List.

### AC-19 — Preserve Role and Permission Links on Failure

**Given** a deletion operation has started,  
**When** the deletion operation fails,  
**Then**:

1. The role shall remain unchanged.
2. Its permission links shall remain unchanged.
3. No partial deletion shall occur.

### AC-20 — Role Unavailable Before Selecting Delete

**Given** the Platform Admin is viewing the Roles List,  
**And** the selected role has already been deleted or become unavailable,  
**When** the Platform Admin selects Delete,  
**Then**:

1. The Delete Role confirmation dialog shall not open.
2. The system shall display an error dialog containing:

   `Role is no longer available.`

3. The dialog shall provide OK.

### AC-21 — Role Unavailable Before Selecting Delete: Navigation

**Given** the role-unavailability error dialog is displayed after selecting Delete,  
**When** the Platform Admin selects OK,  
**Then** the system shall close the dialog and leave the Platform Admin on the Roles List.

### AC-22 — Role Becomes Unavailable Before Confirmation

**Given** the Delete Role confirmation dialog is open,  
**And** the selected role becomes unavailable before Confirm Delete is selected,  
**When** the Platform Admin selects Confirm Delete,  
**Then**:

1. The system shall prevent deletion.
2. The system shall display an error dialog containing:

   `Role is no longer available.`

3. The dialog shall provide OK.

### AC-23 — Role Becomes Unavailable During Deletion

**Given** the Platform Admin has selected Confirm Delete,  
**And** the deletion request is being processed,  
**When** the selected role is found to be unavailable before deletion is completed,  
**Then**:

1. The system shall prevent deletion.
2. The system shall display `Role is no longer available.` in an error dialog.
3. The dialog shall provide OK.

### AC-24 — Role Unavailability Error Navigation

**Given** a role-unavailability error dialog is displayed during the deletion flow,  
**When** the Platform Admin selects OK,  
**Then** the system shall return to or remain on the Roles List.

### AC-25 — Prioritize Role Unavailability Error

**Given** the selected role is unavailable,  
**And** previously known information indicates that users were linked to the role,  
**When** the Platform Admin attempts to confirm deletion,  
**Then**:

1. The system shall display `Role is no longer available.`
2. The system shall not display the linked-users error in place of the role-unavailability error.

### AC-26 — Preserve Data After Linked Users Rejection

**Given** the selected role is linked to users,  
**When** deletion is rejected,  
**Then**:

1. The role shall remain available.
2. Its permission links shall remain unchanged.
3. Its user relationships shall remain unchanged.

### AC-27 — Return to Updated Roles List

**Given** the Platform Admin has completed a successful or unsuccessful deletion attempt,  
**When** the deletion flow returns to the Roles List,  
**Then** the Roles List shall display the latest role data in alphabetical order by Role Name (A–Z).

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin selects Delete for an available role | Delete Role confirmation dialog opens |
| EX-02 | Delete selected for `Content Admin` | Dialog displays `Content Admin` |
| EX-03 | Confirmation dialog opens | Exact irreversible-deletion warning is displayed |
| EX-04 | Confirmation dialog opens | Confirm Delete and Cancel are available |
| EX-05 | Platform Admin selects Cancel | Dialog closes; role and relationships remain unchanged |
| EX-06 | Confirm Delete selected | Latest linked-user information is checked |
| EX-07 | Available role has 0 linked users | Role is eligible for deletion |
| EX-08 | Available role has 2 linked users | Deletion rejected with the exact linked-users message |
| EX-09 | Platform Admin selects OK on linked-users error | Returns to Roles List; role remains |
| EX-10 | User becomes linked after dialog opens | Deletion rejected when Confirm Delete is selected |
| EX-11 | Role has 4 permissions and 0 linked users | Role and permission links deleted; permissions preserved |
| EX-12 | Role has 0 permissions and 0 linked users | Role deleted successfully |
| EX-13 | Role deletion succeeds | `Role deleted successfully` displayed |
| EX-14 | Role deletion succeeds | Deleted role no longer appears in Roles List |
| EX-15 | Role deletion succeeds | Remaining roles appear alphabetically A–Z |
| EX-16 | Confirm Delete selected while request is processing | Confirm Delete disabled; duplicate execution prevented |
| EX-17 | General deletion failure | `Something went wrong. Please try again.` displayed in error dialog |
| EX-18 | Platform Admin selects OK on general error | Dialog closes; remains on Roles List |
| EX-19 | Deletion operation fails | Role and permission links remain unchanged |
| EX-20 | Role unavailable before Delete is selected | Confirmation dialog does not open |
| EX-21 | Delete selected for unavailable role | `Role is no longer available.` displayed |
| EX-22 | Role becomes unavailable while confirmation dialog is open | Confirm Delete is rejected with role-unavailability error |
| EX-23 | Role becomes unavailable during processing | Deletion prevented; role-unavailability error displayed |
| EX-24 | Platform Admin selects OK on role-unavailability error | Returns to or remains on Roles List |
| EX-25 | Role unavailable and previously linked to users | Role-unavailability error takes priority |
| EX-26 | Deletion rejected because role has linked users | Role, permission links, and user relationships remain unchanged |
| EX-27 | Return to Roles List after deletion flow | Latest role data displayed in A–Z order |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| `attachments/IMG_4002.jpeg` | Delete Role wireframe showing the Roles List, selected role, confirmation dialog, irreversible-deletion warning, Confirm Delete, and Cancel |

The supplied wireframe confirms the main confirmation-dialog layout and actions.

Error dialogs and success notifications are defined by the original Jira requirements and the confirmed clarification decisions; their detailed visual layouts were not supplied.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-67 | Create Role | Related role-management story |
| CP-68 | View Role | Provides the Roles List and Delete action entry point |
| CP-69 | Edit Role | Related story with consistent role-unavailability handling |
| CP-70 | Delete Role | Current User Story |
| CP-75 | Assign Permission To Role | Related story responsible for assigning permissions to roles |

### 9.3 Dependencies

1. **Roles List — CP-68:** Delete Role is initiated from the Roles List. After deletion, the Roles List displays the latest data without the deleted role.
2. **Role Availability:** The selected role must be available for the deletion operation to proceed.
3. **Role-User Relationships:** The system must determine whether users are linked to the selected role at the time deletion is confirmed.
4. **Role-Permission Relationships:** Successful deletion removes the role's permission links while preserving the permissions themselves.
5. **Deletion Data Integrity:** Rejected or failed deletion attempts must not modify the role or its relationships through partial deletion.
6. **Confirmation and Feedback:** The deletion flow requires the specified confirmation dialog, error dialogs, and success notification.

## 10. Out of Scope

The following activities are not included in CP-70:

1. Creating roles — covered by CP-67.
2. Viewing or searching roles — covered by CP-68.
3. Editing role information — covered by CP-69.
4. Assigning or removing permissions from a role as a separate management action — covered by CP-75.
5. Creating, editing, or deleting permission definitions.
6. Assigning, changing, or removing roles from users.
7. Automatically reassigning users to another role to make deletion possible.
8. Deleting users linked to a role.
9. Deleting permission definitions when a role is deleted.
10. Restoring a deleted role.
11. Technical implementation details, APIs, database schemas, transaction mechanisms, and system architecture.

---

