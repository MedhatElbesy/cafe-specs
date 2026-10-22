# Functional Specification — CP-74: Delete Permission

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-74 |
| Story Title | Delete Permission |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** delete a permission that is no longer used,  
**So that** the permission list stays clean.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Permissions → Delete

The Platform Admin selects **Delete** from the Actions column of an existing permission in the Permissions List.

The system opens the Delete Permission confirmation dialog for the selected permission.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to Roles & Permissions → Permissions.
3. The Permissions List has been displayed.
4. The Platform Admin has selected Delete for a permission row.
5. The selected permission's name and key are available for display in the confirmation dialog.

The selected permission's current existence and role assignments shall be verified when Confirm Delete is selected. Having no assigned roles is a condition for successful deletion, not a prerequisite for opening the confirmation dialog.

## 5. Fields and Validation

### 5.1 Delete Action

| Attribute | Specification |
|---|---|
| Action Name | Delete |
| Location | Actions column of each permission row |
| Available To | Platform Admin |
| Availability | Enabled for every permission row |
| Behavior | Opens the Delete Permission confirmation dialog |
| Eligibility Check | Performed when Confirm Delete is selected |
| Related Story | CP-73 – View Permission |

**Action Behavior:**

1. The system shall display a Delete action for every permission row.
2. Delete shall remain enabled regardless of the displayed Number of Roles Using It.
3. Selecting Delete shall open the confirmation dialog for the selected permission.
4. Selecting Delete shall not delete the permission immediately.
5. The system shall not determine final deletion eligibility solely from the role count displayed in the Permissions List.
6. The system shall verify current role assignments when Confirm Delete is selected.

### 5.2 Delete Confirmation Dialog

The confirmation dialog shall display the selected permission's identity, the deletion warning, and the available actions.

| Element | Type | Required Behavior |
|---|---|---|
| Dialog Title | Read-only text | `Delete Permission` |
| Permission Name | Read-only value | Displays the selected permission's name |
| Permission Key | Read-only value | Displays the selected permission's key |
| Confirmation Warning | Read-only text | Displays the exact Jira-defined confirmation message |
| Cancel | Button | Closes the dialog without deleting the permission |
| Confirm Delete | Button | Initiates deletion eligibility verification and the deletion attempt if eligible |

**Exact Confirmation Warning:**

`Are you sure you want to delete this permission? This action cannot be undone.`

**Dialog Behavior:**

1. The dialog shall open when the Platform Admin selects Delete for a permission.
2. The dialog shall display the selected Permission Name and Permission Key.
3. Both permission values shall be read-only.
4. The dialog shall display the exact confirmation warning.
5. The dialog shall provide Cancel and Confirm Delete.
6. Opening the dialog shall not delete or modify the permission.
7. Cancel shall close the dialog without changes.
8. Confirm Delete shall initiate verification of the selected permission's current role assignments and its eligibility for deletion.

### 5.3 Permission Name

| Attribute | Specification |
|---|---|
| Field Name | Permission Name |
| Display Location | Delete Confirmation Dialog |
| Field Type | Read-only text |
| Value | Name of the selected permission |
| Editable | No |
| Purpose | Identifies the permission selected for deletion |

**Field Behavior:**

1. The system shall display the name of the selected permission.
2. The Platform Admin shall not be able to modify Permission Name within the dialog.
3. The displayed name shall identify the permission targeted by Confirm Delete.

### 5.4 Permission Key

| Attribute | Specification |
|---|---|
| Field Name | Permission Key |
| Display Location | Delete Confirmation Dialog |
| Field Type | Read-only text |
| Value | Key of the selected permission |
| Editable | No |
| Example from Wireframe | `settings.edit` |
| Purpose | Identifies the selected permission alongside Permission Name |

**Field Behavior:**

1. The system shall display the selected permission's key.
2. The Platform Admin shall not be able to modify Permission Key within the dialog.
3. Permission Key shall remain unchanged while the confirmation dialog is open.

### 5.5 Deletion Eligibility Validation

| Validation Condition | Expected Behavior |
|---|---|
| Permission exists and has zero assigned roles | Deletion is permitted |
| Permission is assigned to one role | Deletion is rejected |
| Permission is assigned to multiple roles | Deletion is rejected |
| Permission is assigned to a role with no users | Deletion is rejected |
| Permission no longer exists | Deletion is not performed; general error dialog displayed |
| Eligible permission but deletion operation fails | General error dialog displayed |

**Validation Behavior:**

1. The system shall verify the selected permission's current role assignments when Confirm Delete is selected.
2. Deletion shall be permitted only if the permission exists and is not assigned to any role.
3. Any current role assignment shall prevent deletion.
4. A role shall block deletion even if no users are assigned to that role.
5. The displayed Number of Roles Using It shall not replace the eligibility verification performed at Confirm Delete.
6. If the permission is assigned to one or more roles, the system shall reject deletion and display the confirmed assigned-role error dialog.
7. If the permission no longer exists, the system shall display the confirmed general error dialog.
8. If the permission is eligible but the deletion operation fails, the system shall display the confirmed general error dialog.
9. The system shall display the success notification only when deletion succeeds.

### 5.6 Assigned-Role Rejection

| Attribute | Specification |
|---|---|
| Condition | Permission is currently assigned to one or more roles |
| Result | Deletion rejected |
| Feedback Type | Error dialog |
| Action | OK |
| Navigation After OK | Permissions List |
| List Refresh | Yes |
| Search / Filter | Preserve previous selections |

**Exact Error Message:**

`This permission can’t be deleted because it is assigned to roles. Please remove it from them first.`

**Error Behavior:**

1. If the permission is assigned to any role, the system shall reject deletion.
2. The system shall display the exact assigned-role error message in an error dialog.
3. The error dialog shall provide OK.
4. Selecting OK shall return the Platform Admin to the Permissions List.
5. The permission shall remain undeleted.
6. The Permissions List shall refresh permission data and current role counts.
7. The previous Search text and Module filter selection shall be preserved.
8. No success notification shall be displayed for the rejected deletion.

### 5.7 Successful Deletion

| Attribute | Specification |
|---|---|
| Condition | Permission exists, has no assigned roles, and deletion succeeds |
| Result | Permission deleted |
| Navigation | Return to Permissions List |
| Feedback Type | Success notification |
| Success Message | `Permission deleted successfully` |
| List Refresh | Yes |
| Search | Reset to empty |
| Module Filter | Reset to `All Modules` |
| Sorting | Permission Name A–Z, case-insensitive, as defined in CP-73 |

**Success Behavior:**

1. The system shall delete the selected permission only after confirming that it is not assigned to any role.
2. If deletion succeeds, the system shall return to the Permissions List.
3. The system shall display `Permission deleted successfully` as a success notification on the Permissions List.
4. The Permissions List shall refresh to reflect the deletion.
5. The deleted permission shall no longer appear in the refreshed list.
6. Search shall reset to empty.
7. Module shall reset to `All Modules`.
8. The refreshed list shall follow the confirmed CP-73 alphabetical sorting rule.
9. The system shall display current role counts for the permissions remaining in the refreshed list.

### 5.8 Cancel Behavior

| Attribute | Specification |
|---|---|
| Action | Cancel |
| Location | Delete Confirmation Dialog |
| Result | Dialog closes |
| Permission Data | Unchanged |
| Navigation | Remain on Permissions List |
| Search / Filter | Preserve current selections |
| Confirmation Required | No additional confirmation |

**Cancel Behavior:**

1. Selecting Cancel shall close the Delete Permission confirmation dialog.
2. The selected permission shall remain unchanged.
3. No deletion shall occur.
4. The Platform Admin shall remain on the Permissions List.
5. The current Search text and Module filter selection shall be preserved.
6. No deletion success or failure message shall be displayed as a result of cancellation.

### 5.9 General Deletion Failure

| Attribute | Specification |
|---|---|
| Condition | Deletion attempt fails |
| Result | No confirmed successful deletion |
| Feedback Type | Error dialog |
| Message | `Something went wrong. Please try again.` |
| Action | OK |
| Navigation After OK | Permissions List |
| List Refresh | Yes |
| Search / Filter | Preserve previous selections |

**Failure Behavior:**

1. If the deletion operation fails, the system shall display an error dialog.
2. The dialog shall contain `Something went wrong. Please try again.`.
3. The dialog shall provide OK.
4. Selecting OK shall close the error dialog and return to the Permissions List.
5. The system shall not display `Permission deleted successfully`.
6. The Permissions List shall refresh to reflect the current permission data and role counts.
7. The previous Search text and Module filter selection shall be preserved.
8. The deletion attempt shall not be treated as successful.

### 5.10 Permission No Longer Exists

| Attribute | Specification |
|---|---|
| Condition | Selected permission no longer exists when deletion is attempted |
| Result | No new deletion performed |
| Feedback Type | General error dialog |
| Message | `Something went wrong. Please try again.` |
| Action | OK |
| Navigation After OK | Permissions List |
| List Refresh | Yes |
| Search / Filter | Preserve previous selections |

**Behavior:**

1. If the selected permission no longer exists when Confirm Delete is selected, the system shall not treat the attempt as a successful deletion.
2. The system shall display the same general error dialog used for deletion failures.
3. The dialog shall contain `Something went wrong. Please try again.`.
4. The dialog shall provide OK.
5. Selecting OK shall return the Platform Admin to the Permissions List.
6. The list shall refresh to display current permission data and role counts.
7. The previous Search text and Module filter selection shall be preserved.
8. No dedicated permission-not-found message shall be introduced.
9. No success notification shall be displayed.

### 5.11 Permissions List State After Deletion Actions

| Scenario | Deletion Outcome | List Refresh | Search / Module Filter |
|---|---|---|---|
| Successful deletion | Permission deleted | Yes | Reset to defaults |
| Assigned-role rejection | Permission not deleted | Yes | Preserve |
| General deletion failure | No confirmed successful deletion | Yes | Preserve |
| Permission no longer exists | No new deletion | Yes | Preserve |
| Cancel | No deletion | Not required | Preserve |

**List State Behavior:**

1. Successful deletion shall follow CP-73's successful-operation refresh and reset behavior.
2. Assigned-role rejection shall refresh permission data and role counts while preserving Search and Module selections.
3. General deletion failure shall refresh permission data and role counts while preserving Search and Module selections.
4. A permission-no-longer-exists error shall refresh permission data and role counts while preserving Search and Module selections.
5. Cancel shall close the dialog without deleting the permission or resetting the current list selections.

## 6. Business Rules

**BR-01 — Delete Permission Authorization**

Only the Platform Admin shall have access to the Delete Permission action.

**BR-02 — Delete Permission Entry Point**

The system shall provide Delete through Roles & Permissions → Permissions → Delete.

**BR-03 — Delete Action Availability**

Delete shall remain enabled for every permission row, regardless of the displayed Number of Roles Using It.

**BR-04 — Confirmation Before Deletion**

Selecting Delete shall open a confirmation dialog. The permission shall not be deleted merely by opening the dialog.

**BR-05 — Selected Permission Identification**

The confirmation dialog shall display the selected Permission Name and Permission Key as read-only values.

**BR-06 — Confirmation Warning**

The confirmation dialog shall display the exact message `Are you sure you want to delete this permission? This action cannot be undone.`

**BR-07 — Confirmation Actions**

The dialog shall provide Cancel and Confirm Delete.

**BR-08 — Eligibility Check Timing**

The system shall verify deletion eligibility when Confirm Delete is selected.

**BR-09 — Current Role Assignments**

The system shall verify the selected permission's current role assignments rather than relying solely on the displayed role count.

**BR-10 — Deletion Eligibility**

A permission shall be eligible for deletion only when it exists and is not assigned to any role.

**BR-11 — Assigned-Role Restriction**

A permission assigned to one or more roles shall not be deleted.

**BR-12 — Roles Without Users**

An assigned role shall block deletion even if the role has no assigned users.

**BR-13 — Assigned-Role Error**

If deletion is rejected because the permission is assigned to roles, the system shall display `This permission can’t be deleted because it is assigned to roles. Please remove it from them first.`

**BR-14 — Assigned-Role Error Dialog**

The assigned-role error shall appear in a dialog with OK.

**BR-15 — Assigned-Role Error Navigation**

After OK, the system shall return to the Permissions List without deleting the permission.

**BR-16 — Assigned-Role Rejection List State**

After assigned-role rejection, the Permissions List shall refresh permission data and role counts while preserving Search and Module filter selections.

**BR-17 — Successful Deletion**

If the permission exists, has no assigned roles, and deletion succeeds, the system shall delete the permission and return to the Permissions List.

**BR-18 — Successful Deletion Feedback**

After successful deletion, the system shall display `Permission deleted successfully` as a success notification on the Permissions List.

**BR-19 — Successful Deletion List Refresh**

After successful deletion, the Permissions List shall refresh to reflect the deletion.

**BR-20 — Successful Deletion Filter Reset**

After successful deletion, Search shall reset to empty and Module shall reset to `All Modules`.

**BR-21 — Cancel Behavior**

Selecting Cancel shall close the confirmation dialog without deleting or modifying the selected permission.

**BR-22 — Cancel List State**

Cancel shall preserve the current Search and Module filter selections.

**BR-23 — General Deletion Failure**

If deletion fails, the system shall display `Something went wrong. Please try again.` in an error dialog.

**BR-24 — General Error Dialog Action**

The general error dialog shall provide OK.

**BR-25 — General Failure Navigation**

After selecting OK following a general deletion failure, the system shall return to the Permissions List without treating the deletion as successful.

**BR-26 — General Failure List State**

After a general deletion failure, the Permissions List shall refresh permission data and role counts while preserving Search and Module filter selections.

**BR-27 — Permission No Longer Exists**

If the selected permission no longer exists when deletion is attempted, the system shall not perform a new deletion or display a success notification.

**BR-28 — Missing Permission Error**

A permission that no longer exists shall produce the same general error dialog containing `Something went wrong. Please try again.` with OK.

**BR-29 — Missing Permission Navigation**

After OK for a permission that no longer exists, the system shall return to the refreshed Permissions List and preserve Search and Module filter selections.

**BR-30 — No Success Feedback for Unsuccessful Outcomes**

The system shall not display `Permission deleted successfully` when deletion is rejected, fails, is cancelled, or the selected permission no longer exists.

**BR-31 — Permissions List Role Counts**

When the Permissions List refreshes following a deletion outcome, it shall display the current distinct-role counts defined in CP-73.

**BR-32 — Deletion and Role Assignments**

CP-74 shall not automatically remove a permission from assigned roles in order to make deletion possible.

## 7. Acceptance Criteria

### AC-01 — Access Delete Permission

**Given** the user is logged in as a Platform Admin,  
**And** the user has navigated to Roles & Permissions → Permissions,  
**When** the user views the Permissions List,  
**Then** the system shall provide a Delete action for each permission row.

### AC-02 — Delete Action Remains Enabled

**Given** the Permissions List contains permissions with different role counts,  
**When** the Platform Admin views the Actions column,  
**Then** Delete shall remain enabled for every permission row,  
**Including** permissions whose Number of Roles Using It is greater than zero.

### AC-03 — Open Delete Confirmation Dialog

**Given** the Platform Admin is viewing the Permissions List,  
**When** the Platform Admin selects Delete for a permission,  
**Then** the system shall open the Delete Permission confirmation dialog,  
**And** shall not delete the permission immediately.

### AC-04 — Display Selected Permission Details

**Given** the Platform Admin selects Delete for the permission named `Edit Settings` with key `settings.edit`,  
**When** the confirmation dialog opens,  
**Then** the dialog shall display:

- Permission Name: `Edit Settings`
- Permission Key: `settings.edit`

**And** both values shall be read-only.

### AC-05 — Display Confirmation Warning

**Given** the Delete Permission confirmation dialog is open,  
**When** the Platform Admin views the dialog,  
**Then** the system shall display:

`Are you sure you want to delete this permission? This action cannot be undone.`

### AC-06 — Display Confirmation Actions

**Given** the Delete Permission confirmation dialog is open,  
**When** the Platform Admin views the available actions,  
**Then** the dialog shall provide Cancel and Confirm Delete.

### AC-07 — Verify Current Role Assignments on Confirmation

**Given** the Delete Permission confirmation dialog is open,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall verify the selected permission's current role assignments before allowing deletion.

### AC-08 — Do Not Rely Solely on Displayed Role Count

**Given** the Permissions List previously displayed `0` for Number of Roles Using It,  
**And** the permission has since been assigned to a role,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall use the current role assignments,  
**And** shall reject deletion.

### AC-09 — Delete an Unassigned Permission

**Given** the selected permission exists,  
**And** it is not assigned to any role,  
**When** the Platform Admin selects Confirm Delete and deletion succeeds,  
**Then** the system shall delete the permission,  
**And** return to the Permissions List.

### AC-10 — Successful Deletion Notification

**Given** the permission has been deleted successfully,  
**When** the Platform Admin returns to the Permissions List,  
**Then** the system shall display `Permission deleted successfully` as a success notification.

### AC-11 — Refresh After Successful Deletion

**Given** a permission has been deleted successfully,  
**When** the Permissions List is displayed,  
**Then** the system shall refresh the list,  
**And** the deleted permission shall no longer appear,  
**And** the remaining permissions shall follow the confirmed CP-73 sorting rule.

### AC-12 — Reset Search and Filter After Successful Deletion

**Given** Search text and a Module filter were active before deletion,  
**When** the permission is successfully deleted,  
**Then** the system shall return to the Permissions List,  
**And** reset Search to empty,  
**And** reset Module to `All Modules`.

### AC-13 — Reject Permission Assigned to One Role

**Given** the selected permission is currently assigned to one role,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall reject deletion,  
**And** display:

`This permission can’t be deleted because it is assigned to roles. Please remove it from them first.`

### AC-14 — Reject Permission Assigned to Multiple Roles

**Given** the selected permission is currently assigned to multiple roles,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall reject deletion,  
**And** display the confirmed assigned-role error message.

### AC-15 — Reject Permission Assigned to a Role Without Users

**Given** the selected permission is assigned to a role,  
**And** that role has no assigned users,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall reject deletion,  
**And** display the confirmed assigned-role error message.

### AC-16 — Assigned-Role Error Dialog

**Given** deletion is rejected because the permission is assigned to roles,  
**When** the error is displayed,  
**Then** the system shall show an error dialog containing:

`This permission can’t be deleted because it is assigned to roles. Please remove it from them first.`

**And** the dialog shall provide OK.

### AC-17 — Assigned-Role Error Navigation

**Given** the assigned-role error dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then** the system shall return to the Permissions List,  
**And** the permission shall remain undeleted.

### AC-18 — Preserve Search and Filter After Assigned-Role Rejection

**Given** Search text and a Module filter were active,  
**And** deletion is rejected because the permission is assigned to roles,  
**When** the Platform Admin selects OK,  
**Then** the Permissions List shall refresh permission data and role counts,  
**And** preserve the previous Search and Module filter selections.

### AC-19 — Cancel Deletion

**Given** the Delete Permission confirmation dialog is open,  
**When** the Platform Admin selects Cancel,  
**Then** the dialog shall close,  
**And** the permission shall remain unchanged,  
**And** no deletion shall occur.

### AC-20 — Preserve List State After Cancel

**Given** Search text and a Module filter were active before opening the confirmation dialog,  
**When** the Platform Admin selects Cancel,  
**Then** the system shall remain on the Permissions List,  
**And** preserve the previous Search and Module filter selections.

### AC-21 — General Deletion Failure

**Given** the selected permission exists and is not assigned to any role,  
**And** the Platform Admin selects Confirm Delete,  
**When** the deletion operation fails,  
**Then** the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**And** provide OK.

### AC-22 — General Failure Navigation

**Given** the general deletion failure dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then** the system shall return to the Permissions List,  
**And** shall not treat the deletion as successful.

### AC-23 — Preserve List State After General Failure

**Given** Search text and a Module filter were active before deletion,  
**And** the deletion operation fails,  
**When** the Platform Admin selects OK,  
**Then** the Permissions List shall refresh permission data and role counts,  
**And** preserve the previous Search and Module filter selections.

### AC-24 — Permission No Longer Exists

**Given** the Platform Admin opened Delete Permission for an existing permission,  
**And** that permission no longer exists when deletion is attempted,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall not perform a new deletion,  
**And** display an error dialog containing:

`Something went wrong. Please try again.`

**And** provide OK.

### AC-25 — Missing Permission Navigation

**Given** the permission-no-longer-exists error dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then** the system shall return to the Permissions List,  
**And** refresh permission data and role counts,  
**And** preserve the previous Search and Module filter selections.

### AC-26 — No Dedicated Missing-Permission Message

**Given** the selected permission no longer exists,  
**When** the system displays the deletion error,  
**Then** the system shall use `Something went wrong. Please try again.`,  
**And** shall not introduce a separate permission-not-found message.

### AC-27 — Refresh Current Role Counts After Rejection or Failure

**Given** the current role assignments differ from the role counts previously displayed,  
**And** deletion is rejected or fails,  
**When** the Platform Admin returns to the Permissions List after OK,  
**Then** the system shall display the current distinct-role counts.

### AC-28 — No Success Notification for Rejected Deletion

**Given** deletion is rejected because the permission is assigned to roles,  
**When** the Platform Admin returns to the Permissions List,  
**Then** the system shall not display `Permission deleted successfully`.

### AC-29 — No Success Notification for General Failure

**Given** the deletion operation fails,  
**When** the Platform Admin returns to the Permissions List,  
**Then** the system shall not display `Permission deleted successfully`.

### AC-30 — No Success Notification for Cancel

**Given** the Platform Admin opens the Delete Permission confirmation dialog,  
**When** the Platform Admin selects Cancel,  
**Then** the system shall not display `Permission deleted successfully`.

### AC-31 — No Success Notification for Missing Permission

**Given** the selected permission no longer exists when deletion is attempted,  
**When** the Platform Admin returns to the Permissions List,  
**Then** the system shall not display `Permission deleted successfully`.

### AC-32 — No Automatic Removal From Roles

**Given** the selected permission is assigned to one or more roles,  
**When** the Platform Admin selects Confirm Delete,  
**Then** the system shall reject deletion,  
**And** shall not automatically remove the permission from those roles.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin views Permissions List | Delete available for every permission row |
| EX-02 | Permission has Number of Roles Using It = `3` | Delete remains enabled |
| EX-03 | Select Delete for `Edit Settings` | Delete Confirmation Dialog opens |
| EX-04 | Selected permission is `Edit Settings`, key `settings.edit` | Name and Key displayed read-only |
| EX-05 | Open Delete Confirmation Dialog | Exact confirmation warning displayed |
| EX-06 | Open Delete Confirmation Dialog | Cancel and Confirm Delete available |
| EX-07 | Select Delete but not Confirm Delete | Permission remains unchanged |
| EX-08 | Select Confirm Delete | Current role assignments verified |
| EX-09 | Displayed role count is `0`, but current assignments include one role | Deletion rejected |
| EX-10 | Permission exists and has zero assigned roles | Deletion allowed |
| EX-11 | Eligible permission deleted successfully | Returns to Permissions List |
| EX-12 | Successful deletion | `Permission deleted successfully` displayed |
| EX-13 | Successful deletion | Deleted permission removed from refreshed list |
| EX-14 | Successful deletion with active Search/Filter | Search cleared; Module reset to `All Modules` |
| EX-15 | Permission assigned to one role | Deletion rejected |
| EX-16 | Permission assigned to three roles | Deletion rejected |
| EX-17 | Permission assigned to a role with no users | Deletion rejected |
| EX-18 | Assigned-role rejection | Exact assigned-role error message displayed |
| EX-19 | Assigned-role error dialog | OK available |
| EX-20 | Select OK after assigned-role rejection | Returns to Permissions List; permission not deleted |
| EX-21 | Assigned-role rejection with active Search/Filter | List refreshed; selections preserved |
| EX-22 | Select Cancel | Confirmation dialog closes; no deletion |
| EX-23 | Cancel with active Search/Filter | Search and Module selections preserved |
| EX-24 | Eligible permission deletion fails | General error dialog displayed |
| EX-25 | General deletion failure | `Something went wrong. Please try again.` displayed |
| EX-26 | Select OK after general failure | Returns to Permissions List without confirmed deletion |
| EX-27 | General failure with active Search/Filter | List refreshed; selections preserved |
| EX-28 | Permission deleted by another admin before confirmation | General error dialog displayed |
| EX-29 | Permission no longer exists | Same general error message used |
| EX-30 | Select OK after missing-permission error | Refreshed list displayed; selections preserved |
| EX-31 | Rejected or failed deletion after role assignments changed | Refreshed list displays current role counts |
| EX-32 | Assigned-role rejection | No success notification |
| EX-33 | General deletion failure | No success notification |
| EX-34 | Cancel deletion | No success notification |
| EX-35 | Permission no longer exists | No success notification |
| EX-36 | Permission assigned to roles | No automatic removal from assigned roles |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| Delete Permission wireframe | Supplied for review; illustrates the Delete Permission confirmation dialog, read-only Permission Name and Key, confirmation warning, Cancel, and Confirm Delete |

No attachments were listed in the original CP-74 Jira story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-71 | Create Permission | Related permission creation functionality |
| CP-72 | Edit Permission | Related permission editing functionality |
| CP-73 | View Permission | Provides the Permissions List, Delete entry point, role count display, list refresh, and Search/Filter behavior |
| CP-74 | Delete Permission | Current User Story |
| CP-75 | Assign Permission to Role | Related functionality affecting permission-role assignments and deletion eligibility |

### 9.3 Dependencies

1. **Platform Admin Access:** The Delete Permission action is available to the Platform Admin.
2. **Permissions List (CP-73):** The deletion flow starts from a permission row in the Permissions List.
3. **Selected Permission Details:** The confirmation dialog requires the selected Permission Name and Permission Key for display.
4. **Current Permission Existence:** Successful deletion requires the selected permission to exist when deletion is attempted.
5. **Current Role Assignments:** Deletion eligibility depends on the permission's current role assignments at Confirm Delete.
6. **Role-Permission Relationships:** Permissions assigned to any role cannot be deleted. This includes roles without users.
7. **Permissions List Refresh:** Following successful, rejected, or failed deletion, the list displays the appropriate current permission data and role counts.
8. **Search and Module Filter State:** The deletion outcome determines whether list selections are reset or preserved, as specified in Section 5.11.
9. **Confirmation and Feedback:** The flow depends on the specified confirmation dialog, assigned-role rejection message, success notification, and general failure dialog.

## 10. Out of Scope

The following activities are not included in CP-74:

1. Creating a new permission — covered by CP-71.
2. Editing Permission Name, Permission Key, Module, or Description — covered by CP-72 where applicable.
3. Searching, filtering, sorting, or displaying permissions beyond the related CP-73 list behavior.
4. Assigning permissions to roles — related to CP-75.
5. Automatically removing a permission from assigned roles before deletion.
6. Creating, editing, or deleting roles.
7. Assigning roles to users or managing user accounts.
8. Deleting multiple permissions in a single operation.
9. Restoring or undoing a successfully deleted permission.
10. Introducing a separate error message for a permission that no longer exists.
11. Automatically retrying a failed deletion.
12. Defining deletion loading indicators or progress animations.
13. Defining audit logging for deletion events.
14. Technical implementation details, APIs, database schemas, concurrency mechanisms, and system architecture.

---

