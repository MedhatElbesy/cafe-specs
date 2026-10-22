# Functional Specification — CP-75: Assign Permission To Role

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-75 |
| Story Title | Assign Permission To Role |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** assign permissions to a role,  
**So that** admin users with that role get exactly the access they need.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Roles → View Details → Manage Permissions

The Platform Admin opens the Role Details screen and selects **Manage Permissions** for the selected role.

The system displays the Manage Permissions screen, showing the available permissions grouped by Module and indicating which permissions are currently assigned to the role.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to Roles & Permissions → Roles.
3. The Platform Admin has opened View Details for a selected role.
4. The selected role is available for viewing.
5. The system has access to the available permissions and the selected role's current permission assignments.

**Protected Role Consideration:**

A protected role may still be opened in Manage Permissions, but the screen shall be read-only. Permission selections and Save shall be disabled, and the confirmed protected-role message shall be displayed.

**Self-Access Consideration:**

When the Platform Admin manages their own role, the system shall prevent saving a selection that removes the `roles.manage` permission.

## 5. Fields and Validation

### 5.1 Manage Permissions Screen

| Element | Type | Required Behavior |
|---|---|---|
| Selected Role | Display context | Identifies the role whose permissions are being managed |
| Module Groups | Grouped permission sections | Displays all available permissions grouped by Module |
| Permission Name | Read-only text | Displays the permission's name |
| Permission Key | Read-only text | Displays the permission's key |
| Permission Checkbox | Checkbox | Indicates and allows modification of the permission's assignment |
| Select All | Module-level checkbox | Selects or deselects all permissions within its module |
| Save | Button | Saves the selected permission assignments when permitted |
| Cancel | Button | Discards changes and returns to View Details |
| Back | Navigation action | Returns to View Details, with confirmation when unsaved changes exist |

**Screen Behavior:**

1. The system shall display all available permissions grouped by Module.
2. Each permission shall have an individual checkbox.
3. Permissions currently assigned to the selected role shall appear checked when the screen loads.
4. Permissions not currently assigned to the role shall appear unchecked.
5. Each module shall provide its own Select All control.
6. The Platform Admin shall be able to add and remove permission selections for a non-protected role, subject to the confirmed self-access restriction.
7. The system shall provide Save, Cancel, and Back.
8. A protected role shall use the read-only behavior defined in Section 5.5.

### 5.2 Permission Name and Permission Key

| Attribute | Permission Name | Permission Key |
|---|---|---|
| Field Type | Read-only text | Read-only text |
| Display Location | Permission row | Permission row |
| Value | Existing permission name | Existing permission key |
| Editable | No | No |
| Associated Control | Permission checkbox | Permission checkbox |
| Purpose | Identifies the permission | Identifies the permission by key |

**Field Behavior:**

1. The system shall display the name and key of each available permission.
2. The Platform Admin shall not edit Permission Name or Permission Key through Manage Permissions.
3. The individual checkbox shall represent whether that permission is selected for assignment to the role.
4. The permission identified by `roles.manage` shall be subject to the self-access protection defined in Section 5.6.

### 5.3 Individual Permission Checkbox

| Attribute | Specification |
|---|---|
| Control | Checkbox |
| Scope | One permission within a module |
| Initial State | Checked if currently assigned; unchecked otherwise |
| Checked Meaning | Permission selected for assignment to the role |
| Unchecked Meaning | Permission not selected for assignment to the role |
| Editable for Non-Protected Roles | Yes |
| Editable for Protected Roles | No |
| Persistence | Selection changes are saved only through a successful Save |

**Checkbox Behavior:**

1. A checked permission shall represent a permission selected for assignment to the role.
2. An unchecked permission shall represent a permission not selected for assignment to the role.
3. The initial state shall reflect the selected role's current assignments when Manage Permissions loads.
4. For a non-protected role, selecting an unchecked permission shall include it in the pending selection.
5. For a non-protected role, deselecting a checked permission shall remove it from the pending selection.
6. Changes shall remain unsaved until Save succeeds.
7. If the role is protected, individual permission checkboxes shall be disabled.
8. For the Platform Admin's own role, removing `roles.manage` from the pending selection shall be validated on Save.

### 5.4 Module-Level Select All

| Attribute | Specification |
|---|---|
| Control | Select All checkbox |
| Scope | Permissions within its corresponding Module only |
| Checked State | All permissions in the module are selected |
| Unchecked State | No permissions in the module are selected |
| Indeterminate State | Some, but not all, permissions in the module are selected |
| Selecting Select All | Selects all permissions in the module |
| Unchecking Select All | Deselects all permissions in the module |
| Protected Role | Disabled |

**Select All Behavior:**

1. Each Module shall have a separate Select All control.
2. Selecting Select All shall select every permission within that module.
3. Unchecking Select All shall deselect every permission within that module.
4. Select All shall display Checked when every permission in the module is selected.
5. Select All shall display Unchecked when no permission in the module is selected.
6. Select All shall display Indeterminate when some, but not all, permissions in the module are selected.
7. Changes to individual permission checkboxes shall be reflected in the corresponding module's Select All state.
8. Select All shall affect only permissions within its corresponding module.
9. For protected roles, Select All controls shall be disabled.
10. If Select All causes `roles.manage` to be deselected from the Platform Admin's own role, the self-access restriction shall be evaluated when Save is selected.

### 5.5 Protected Role — Read-Only Behavior

| Attribute | Specification |
|---|---|
| Condition | Selected role is protected |
| Screen Access | Manage Permissions opens |
| Display | All available permissions grouped by Module |
| Current Assignments | Displayed as checked |
| Individual Checkboxes | Disabled |
| Select All | Disabled |
| Save | Disabled |
| Message | `This role’s permissions cannot be modified` |

**Protected Role Behavior:**

1. The system shall allow the Platform Admin to open Manage Permissions for a protected role.
2. The screen shall display all available permissions grouped by Module.
3. Permissions currently assigned to the protected role shall appear checked.
4. Permissions not assigned to the protected role shall appear unchecked.
5. All individual permission checkboxes shall be disabled.
6. All module-level Select All controls shall be disabled.
7. Save shall be disabled.
8. The system shall display the exact message:

   `This role’s permissions cannot be modified`

9. The Platform Admin shall not be able to change the protected role's permission assignments through this screen.
10. The screen shall retain the confirmed Cancel and Back navigation behavior.

### 5.6 Self-Access Protection — `roles.manage`

| Attribute | Specification |
|---|---|
| Protected Permission Key | `roles.manage` |
| Applies When | Platform Admin is managing their own role |
| Restricted Action | Saving a selection that removes `roles.manage` |
| Applies To | Individual checkbox changes and module-level Select All |
| Validation Timing | Save |
| Result | Save rejected if final selection excludes `roles.manage` |
| Feedback Type | Error dialog |
| Action | OK |
| Navigation After OK | Remain on Manage Permissions |
| Unsaved Selections | Preserved |

**Exact Error Message:**

`You can’t remove the role management permission from your current role`

**Validation Behavior:**

1. The system shall identify whether the selected role is the Platform Admin's own role.
2. Self-access protection shall apply specifically to the permission key `roles.manage`.
3. The Platform Admin may change the `roles.manage` checkbox while editing their own role.
4. The system shall evaluate the final permission selection when Save is selected.
5. If the final selection removes `roles.manage` from the Platform Admin's own role, Save shall be rejected.
6. This restriction shall apply whether `roles.manage` was deselected individually or through Select All.
7. The system shall display an error dialog containing the exact confirmed self-access error message.
8. The error dialog shall provide OK.
9. After OK, the Platform Admin shall remain on Manage Permissions.
10. The unsaved permission selections shall be preserved.
11. If `roles.manage` is restored before Save, the removal restriction shall not reject Save on that basis.
12. The restriction shall not automatically apply to every permission within the RBAC module.

### 5.7 Save — Permission Assignment Changes

| Attribute | Specification |
|---|---|
| Action | Save |
| Applies To | Non-protected roles |
| Saved Data | Final selected permission assignments |
| Additions | Newly selected permissions are assigned to the role |
| Removals | Deselected permissions are removed from the role |
| Self-Access Validation | Required when managing own role |
| Concurrent-Change Validation | Required |
| Success Navigation | View Details for the selected role |
| Success Feedback | `Role permissions updated successfully` |
| Save Without Changes | Allowed |

**Save Behavior:**

1. Save shall be available for a non-protected role.
2. Selecting Save shall initiate validation of the pending permission assignments.
3. If the Platform Admin is managing their own role, the system shall reject Save when the final selection removes `roles.manage`.
4. If the role's permission assignments have changed since Manage Permissions was loaded, the system shall reject Save according to Section 5.10.
5. If the applicable validations pass and Save succeeds, the system shall save the final selected permissions.
6. The saved selection shall include additions and removals.
7. After successful Save, the system shall return to View Details for the selected role.
8. The system shall display `Role permissions updated successfully` as a success notification on View Details.
9. Selecting Save without changing any permissions shall be allowed.
10. A successful Save without changes shall produce the same navigation and success notification.
11. The saved changes shall affect users linked to the role according to Section 5.11.

### 5.8 Cancel Behavior

| Attribute | Specification |
|---|---|
| Action | Cancel |
| Result | Discard unsaved changes |
| Confirmation | Not required |
| Navigation | View Details for the selected role |
| Saved Assignments | Unchanged |

**Cancel Behavior:**

1. Selecting Cancel shall discard all unsaved permission selection changes.
2. The system shall return to View Details for the selected role.
3. No confirmation dialog shall be displayed.
4. The role's saved permission assignments shall remain unchanged.
5. Cancel shall be available as a navigation action when Manage Permissions is displayed, including in protected-role read-only mode.

### 5.9 Back Behavior and Unsaved Changes

| Condition | Expected Behavior |
|---|---|
| No unsaved changes | Return directly to View Details |
| Unsaved changes exist | Display confirmation dialog |
| Select Leave | Discard changes and return to View Details |
| Select Stay | Close dialog and remain on Manage Permissions |
| All changes reverted | Treat as no unsaved changes |

**Exact Confirmation Message:**

`You have unsaved changes. Are you sure you want to leave?`

**Back Behavior:**

1. The system shall compare the current permission selections with the originally loaded assignments.
2. If the current selection matches the original assignments, the system shall treat the form as having no unsaved changes.
3. If the current selection differs from the original assignments, the system shall treat the form as having unsaved changes.
4. Selecting Back without unsaved changes shall return directly to View Details.
5. Selecting Back with unsaved changes shall display the exact confirmation message.
6. The confirmation dialog shall provide Leave and Stay.
7. Selecting Leave shall discard unsaved changes and return to View Details.
8. Selecting Stay shall close the confirmation dialog and keep the Platform Admin on Manage Permissions with the current selections.
9. If the Platform Admin changes a checkbox and later restores the original selection, that interaction alone shall not count as an unsaved change.

### 5.10 Save Failure and Concurrent Assignment Changes

#### 5.10.1 General Save Failure

| Attribute | Specification |
|---|---|
| Condition | Save operation fails |
| Feedback Type | Error dialog |
| Message | `Something went wrong. Please try again.` |
| Action | OK |
| Navigation After OK | Remain on Manage Permissions |
| Unsaved Selections | Preserved |
| Success Notification | Not displayed |

**Behavior:**

1. If Save fails, the system shall display an error dialog containing `Something went wrong. Please try again.`.
2. The error dialog shall provide OK.
3. Selecting OK shall close the dialog.
4. The Platform Admin shall remain on Manage Permissions.
5. The current unsaved selections shall be preserved.
6. The system shall not display `Role permissions updated successfully`.
7. The failed operation shall not be treated as a successful permission assignment update.

#### 5.10.2 Concurrent Assignment Changes

| Attribute | Specification |
|---|---|
| Condition | Role assignments have changed since Manage Permissions was loaded |
| Validation Timing | Save |
| Result | Save rejected |
| Message | `Something went wrong. Please try again.` |
| Feedback Type | Error dialog |
| Action | OK |
| Navigation After OK | Remain on Manage Permissions |
| Unsaved Selections | Preserved |

**Behavior:**

1. When Save is selected, the system shall determine whether the selected role's permission assignments have changed since the Manage Permissions screen was loaded.
2. If the assignments have changed, the system shall reject Save.
3. The system shall display the general error dialog containing `Something went wrong. Please try again.`.
4. The error dialog shall provide OK.
5. After OK, the Platform Admin shall remain on Manage Permissions.
6. The unsaved selections shall be preserved.
7. The system shall not overwrite the newer saved assignments through the rejected Save.
8. The system shall not display the success notification.
9. No technical concurrency mechanism is prescribed by this specification.

### 5.11 Effective Timing of Saved Permission Changes

| Attribute | Specification |
|---|---|
| Affected Users | All users linked to the modified role |
| Effective Timing | Next session or permission refresh |
| Immediate Active-Session Update | Not required |
| Applies To | Successfully saved additions and removals |

**Behavior:**

1. Successfully saved permission assignment changes shall apply to all users linked to the modified role.
2. The changes shall take effect for those users on their next session or permission refresh.
3. An immediate update to every existing active session shall not be required.
4. The timing rule shall apply to both added and removed permissions.
5. Changes that are cancelled, rejected, or not successfully saved shall not be treated as successfully applied updates.

## 6. Business Rules

**BR-01 — Platform Admin Access**

The Manage Permissions functionality shall be available to the Platform Admin.

**BR-02 — Manage Permissions Entry Point**

The system shall provide access through Roles & Permissions → Roles → View Details → Manage Permissions.

**BR-03 — Permission Grouping**

The system shall display all available permissions grouped by Module.

**BR-04 — Permission Identification**

Each permission shall display its Permission Name and Permission Key.

**BR-05 — Initial Assignment State**

Permissions currently assigned to the selected role shall appear checked when Manage Permissions loads.

**BR-06 — Unassigned Permission State**

Permissions not currently assigned to the selected role shall appear unchecked.

**BR-07 — Individual Permission Selection**

For a non-protected role, the Platform Admin shall be able to select and deselect individual permissions.

**BR-08 — Module-Level Select All**

Each Module shall provide a Select All control affecting only the permissions within that Module.

**BR-09 — Select All Checked State**

Select All shall appear checked when all permissions in the corresponding Module are selected.

**BR-10 — Select All Unchecked State**

Select All shall appear unchecked when no permissions in the corresponding Module are selected.

**BR-11 — Select All Indeterminate State**

Select All shall appear indeterminate when some, but not all, permissions in the corresponding Module are selected.

**BR-12 — Select All Selection Behavior**

Selecting Select All shall select all permissions within the corresponding Module.

**BR-13 — Select All Deselection Behavior**

Unchecking Select All shall deselect all permissions within the corresponding Module.

**BR-14 — Protected Role Display**

Manage Permissions shall open for a protected role in read-only mode.

**BR-15 — Protected Role Restrictions**

For a protected role, individual permission checkboxes, module-level Select All controls, and Save shall be disabled.

**BR-16 — Protected Role Message**

The system shall display `This role’s permissions cannot be modified` for a protected role.

**BR-17 — Protected Role Assignment Preservation**

The Platform Admin shall not modify a protected role's permission assignments through Manage Permissions.

**BR-18 — Self-Access Protected Permission**

The permission key protected against removal from the Platform Admin's own role shall be `roles.manage`.

**BR-19 — Self-Access Validation Timing**

The system shall evaluate the final selected permissions for self-access protection when Save is selected.

**BR-20 — Self-Access Removal Restriction**

The system shall reject Save if the final selection removes `roles.manage` from the Platform Admin's own role.

**BR-21 — Self-Access Restriction Scope**

The self-access restriction shall apply whether `roles.manage` was deselected individually or through Select All.

**BR-22 — Self-Access Error Message**

The system shall display `You can’t remove the role management permission from your current role` when the self-access removal restriction is violated.

**BR-23 — Self-Access Error Navigation**

After acknowledging the self-access error dialog with OK, the Platform Admin shall remain on Manage Permissions with the unsaved selections preserved.

**BR-24 — Self-Access Restoration**

If `roles.manage` is restored before Save, the system shall not reject Save solely because it was previously deselected during editing.

**BR-25 — Save Assignment Changes**

A successful Save shall persist the final selected permissions, including additions and removals.

**BR-26 — Save Without Changes**

Save shall be allowed when the selected permissions have not changed.

**BR-27 — Successful Save Navigation**

After successful Save, the system shall return to View Details for the selected role.

**BR-28 — Successful Save Notification**

After successful Save, the system shall display `Role permissions updated successfully` as a success notification on View Details.

**BR-29 — Cancel Behavior**

Cancel shall discard all unsaved permission changes and return to View Details without confirmation.

**BR-30 — Back Without Changes**

Back shall return directly to View Details when no unsaved changes exist.

**BR-31 — Back With Unsaved Changes**

Back shall display `You have unsaved changes. Are you sure you want to leave?` when the current selections differ from the originally loaded assignments.

**BR-32 — Back Confirmation Actions**

The Back confirmation dialog shall provide Leave and Stay.

**BR-33 — Leave Behavior**

Leave shall discard unsaved changes and return to View Details.

**BR-34 — Stay Behavior**

Stay shall close the confirmation dialog and preserve the current selections on Manage Permissions.

**BR-35 — Unsaved Changes Detection**

Unsaved changes shall be determined by comparing the current selected permissions with the originally loaded assignments.

**BR-36 — Reverted Changes**

If all permission selections are restored to their original state, the system shall treat the form as having no unsaved changes.

**BR-37 — General Save Failure**

If Save fails, the system shall display `Something went wrong. Please try again.` in an error dialog with OK.

**BR-38 — General Save Failure Navigation**

After OK following a general Save failure, the Platform Admin shall remain on Manage Permissions with the unsaved selections preserved.

**BR-39 — Concurrent Assignment Validation**

When Save is selected, the system shall determine whether the role's permission assignments have changed since Manage Permissions was loaded.

**BR-40 — Concurrent Assignment Change Restriction**

If the role's permission assignments have changed since the screen was loaded, the system shall reject Save.

**BR-41 — Concurrent Change Error Message**

Concurrent assignment changes shall produce the general error dialog containing `Something went wrong. Please try again.`.

**BR-42 — Concurrent Change Navigation**

After OK following a concurrent-change rejection, the Platform Admin shall remain on Manage Permissions with the unsaved selections preserved.

**BR-43 — No Overwrite on Concurrent-Change Rejection**

A rejected Save shall not overwrite the newer saved role-permission assignments.

**BR-44 — Effective Permission Changes**

Successfully saved permission changes shall apply to all users linked to the role on their next session or permission refresh.

**BR-45 — Active Session Timing**

Immediate permission updates to all existing active sessions shall not be required.

**BR-46 — No Success Feedback for Unsuccessful Saves**

The system shall not display `Role permissions updated successfully` when Save is rejected or fails.

**BR-47 — Permission Assignment Relationship**

Saved permission additions and removals shall determine the role-permission relationships reflected by related permission-management functionality.

## 7. Acceptance Criteria

### AC-01 — Access Manage Permissions

**Given** the user is logged in as a Platform Admin,  
**And** the user has opened View Details for a role,  
**When** the user selects Manage Permissions,  
**Then** the system shall open the Manage Permissions screen for the selected role.

### AC-02 — Display Permissions Grouped by Module

**Given** the Platform Admin opens Manage Permissions,  
**When** the screen loads,  
**Then** the system shall display all available permissions grouped by Module.

### AC-03 — Display Permission Details

**Given** the Manage Permissions screen is displayed,  
**When** the Platform Admin views a permission,  
**Then** the system shall display its Permission Name and Permission Key,  
**And** provide an individual checkbox for its assignment selection.

### AC-04 — Display Existing Assignments

**Given** the selected role has existing permission assignments,  
**When** Manage Permissions loads,  
**Then** each currently assigned permission shall appear checked,  
**And** each unassigned permission shall appear unchecked.

### AC-05 — Select an Individual Permission

**Given** the selected role is not protected,  
**And** a permission is unchecked,  
**When** the Platform Admin checks that permission,  
**Then** the permission shall be included in the pending selection.

### AC-06 — Deselect an Individual Permission

**Given** the selected role is not protected,  
**And** a permission is checked,  
**When** the Platform Admin unchecks that permission,  
**Then** the permission shall be removed from the pending selection.

### AC-07 — Select All Permissions Within a Module

**Given** the selected role is not protected,  
**And** a Module contains unselected permissions,  
**When** the Platform Admin selects that Module's Select All control,  
**Then** all permissions in that Module shall become selected.

### AC-08 — Deselect All Permissions Within a Module

**Given** the selected role is not protected,  
**And** all permissions in a Module are selected,  
**When** the Platform Admin unchecks that Module's Select All control,  
**Then** all permissions in that Module shall become unselected.

### AC-09 — Select All Checked State

**Given** every permission within a Module is selected,  
**When** the system displays the Module's Select All control,  
**Then** Select All shall appear checked.

### AC-10 — Select All Unchecked State

**Given** no permissions within a Module are selected,  
**When** the system displays the Module's Select All control,  
**Then** Select All shall appear unchecked.

### AC-11 — Select All Indeterminate State

**Given** some, but not all, permissions within a Module are selected,  
**When** the system displays the Module's Select All control,  
**Then** Select All shall appear indeterminate.

### AC-12 — Select All Module Isolation

**Given** the Manage Permissions screen contains multiple Modules,  
**When** the Platform Admin changes Select All for one Module,  
**Then** the selection change shall affect only permissions within that Module.

### AC-13 — Open Protected Role in Read-Only Mode

**Given** the selected role is protected,  
**When** the Platform Admin opens Manage Permissions,  
**Then** the system shall display all permissions grouped by Module,  
**And** show current assignments as checked,  
**And** display the screen in read-only mode.

### AC-14 — Disable Protected Role Controls

**Given** Manage Permissions is open for a protected role,  
**When** the Platform Admin views the available controls,  
**Then** all individual permission checkboxes shall be disabled,  
**And** all Select All controls shall be disabled,  
**And** Save shall be disabled.

### AC-15 — Protected Role Message

**Given** Manage Permissions is open for a protected role,  
**When** the screen is displayed,  
**Then** the system shall display:

`This role’s permissions cannot be modified`

### AC-16 — Save Permission Additions

**Given** the selected role is not protected,  
**And** the Platform Admin selects one or more previously unassigned permissions,  
**And** no applicable Save restriction is violated,  
**When** the Platform Admin selects Save and the operation succeeds,  
**Then** the selected permissions shall be assigned to the role.

### AC-17 — Save Permission Removals

**Given** the selected role is not protected,  
**And** the Platform Admin deselects one or more previously assigned permissions,  
**And** no applicable Save restriction is violated,  
**When** the Platform Admin selects Save and the operation succeeds,  
**Then** the deselected permissions shall be removed from the role.

### AC-18 — Save Additions and Removals Together

**Given** the Platform Admin has selected new permissions and deselected existing permissions,  
**And** the selected role is not protected,  
**And** no applicable Save restriction is violated,  
**When** Save succeeds,  
**Then** the system shall save both additions and removals.

### AC-19 — Save Without Changes

**Given** the selected role is not protected,  
**And** the current selections match the originally loaded assignments,  
**When** the Platform Admin selects Save and the operation succeeds,  
**Then** the system shall return to View Details,  
**And** display `Role permissions updated successfully`.

### AC-20 — Successful Save Navigation and Notification

**Given** the Platform Admin successfully saves permission assignments,  
**When** the save completes,  
**Then** the system shall return to View Details for the selected role,  
**And** display `Role permissions updated successfully` as a success notification.

### AC-21 — Prevent Individual Removal of `roles.manage` From Own Role

**Given** the Platform Admin is managing their own role,  
**And** `roles.manage` is currently selected,  
**When** the Platform Admin deselects `roles.manage` individually and selects Save,  
**Then** the system shall reject Save,  
**And** display an error dialog containing:

`You can’t remove the role management permission from your current role`

### AC-22 — Prevent Removal of `roles.manage` Through Select All

**Given** the Platform Admin is managing their own role,  
**And** `roles.manage` is selected,  
**When** the Platform Admin unchecks Select All for the RBAC Module,  
**And** selects Save while `roles.manage` remains unselected,  
**Then** the system shall reject Save,  
**And** display the confirmed self-access error message.

### AC-23 — Preserve Selections After Self-Access Error

**Given** the system rejects Save because the final selection removes `roles.manage` from the Platform Admin's own role,  
**When** the error dialog is displayed,  
**Then** the dialog shall provide OK,  
**And** when OK is selected, the Platform Admin shall remain on Manage Permissions,  
**And** the unsaved selections shall be preserved.

### AC-24 — Allow Save After Restoring `roles.manage`

**Given** the Platform Admin is managing their own role,  
**And** `roles.manage` was deselected during editing,  
**When** the Platform Admin restores `roles.manage` before selecting Save,  
**Then** the system shall not reject Save solely because `roles.manage` was previously deselected.

### AC-25 — Restrict Only the Confirmed Self-Access Permission

**Given** the Platform Admin is managing their own role,  
**When** the Platform Admin deselects an RBAC permission other than `roles.manage`,  
**Then** the system shall not reject Save solely under the `roles.manage` self-access restriction.

### AC-26 — Cancel With Unsaved Changes

**Given** the Platform Admin has changed one or more permission selections,  
**When** the Platform Admin selects Cancel,  
**Then** the system shall discard all unsaved changes,  
**And** return to View Details without displaying a confirmation dialog.

### AC-27 — Back Without Unsaved Changes

**Given** the current selections match the originally loaded assignments,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to View Details.

### AC-28 — Back With Unsaved Changes

**Given** the current selections differ from the originally loaded assignments,  
**When** the Platform Admin selects Back,  
**Then** the system shall display:

`You have unsaved changes. Are you sure you want to leave?`

**And** provide Leave and Stay.

### AC-29 — Back Confirmation: Leave

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Leave,  
**Then** the system shall discard the unsaved changes,  
**And** return to View Details.

### AC-30 — Back Confirmation: Stay

**Given** the unsaved-changes confirmation dialog is displayed,  
**When** the Platform Admin selects Stay,  
**Then** the system shall close the dialog,  
**And** remain on Manage Permissions,  
**And** preserve the current selections.

### AC-31 — Reverted Changes Are Not Unsaved Changes

**Given** the Platform Admin changes one or more permission selections,  
**And** later restores all selections to their originally loaded states,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to View Details without an unsaved-changes confirmation.

### AC-32 — General Save Failure

**Given** the Platform Admin selects Save for a non-protected role,  
**When** the Save operation fails,  
**Then** the system shall display an error dialog containing:

`Something went wrong. Please try again.`

**And** provide OK.

### AC-33 — Preserve Selections After General Save Failure

**Given** the general Save failure dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then** the system shall remain on Manage Permissions,  
**And** preserve the current unsaved selections.

### AC-34 — Reject Save After Concurrent Assignment Changes

**Given** the Platform Admin opened Manage Permissions for a role,  
**And** the role's saved permission assignments have changed since the screen was loaded,  
**When** the Platform Admin selects Save,  
**Then** the system shall reject Save,  
**And** display an error dialog containing:

`Something went wrong. Please try again.`

### AC-35 — Preserve Selections After Concurrent-Change Rejection

**Given** Save was rejected because the role's permission assignments changed after the screen was loaded,  
**When** the Platform Admin selects OK in the error dialog,  
**Then** the system shall remain on Manage Permissions,  
**And** preserve the current unsaved selections.

### AC-36 — Do Not Overwrite Newer Assignments

**Given** another admin changed the selected role's permission assignments after Manage Permissions was loaded,  
**When** the Platform Admin attempts to Save the previously loaded selections,  
**Then** the system shall reject Save,  
**And** shall not overwrite the newer saved assignments.

### AC-37 — Apply Changes to Linked Users

**Given** the Platform Admin successfully saves permission assignment changes for a role,  
**And** users are linked to that role,  
**When** a linked user starts their next session or their permissions are refreshed,  
**Then** the successfully saved additions and removals shall apply to that user.

### AC-38 — No Immediate Active-Session Update Required

**Given** the Platform Admin successfully updates a role's permission assignments,  
**When** linked users have existing active sessions,  
**Then** an immediate update to every active session shall not be required,  
**And** the changes shall apply on the next session or permission refresh.

### AC-39 — No Success Notification for Self-Access Rejection

**Given** Save is rejected because the Platform Admin attempted to remove `roles.manage` from their own role,  
**When** the error is displayed,  
**Then** the system shall not display `Role permissions updated successfully`.

### AC-40 — No Success Notification for General Failure

**Given** the Save operation fails,  
**When** the error is displayed,  
**Then** the system shall not display `Role permissions updated successfully`.

### AC-41 — No Success Notification for Concurrent-Change Rejection

**Given** Save is rejected because the role's assignments changed after the screen was loaded,  
**When** the error is displayed,  
**Then** the system shall not display `Role permissions updated successfully`.

### AC-42 — No Permission Changes for Protected Role

**Given** the selected role is protected,  
**When** the Platform Admin opens Manage Permissions,  
**Then** the permission selections shall be read-only,  
**And** Save shall be disabled,  
**And** no permission assignment changes shall be made through the screen.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin selects Manage Permissions from Role Details | Manage Permissions opens for selected role |
| EX-02 | Manage Permissions loads | All available permissions displayed by Module |
| EX-03 | Permission `roles.manage` exists | Permission Name and Key displayed |
| EX-04 | Permission already assigned to role | Checkbox checked |
| EX-05 | Permission not assigned to role | Checkbox unchecked |
| EX-06 | Select an unchecked permission | Permission added to pending selection |
| EX-07 | Deselect a checked permission | Permission removed from pending selection |
| EX-08 | Select All in Customers Module | All Customers permissions selected |
| EX-09 | Uncheck Select All in Customers Module | All Customers permissions deselected |
| EX-10 | All permissions selected within a Module | Select All checked |
| EX-11 | No permissions selected within a Module | Select All unchecked |
| EX-12 | Some permissions selected within a Module | Select All indeterminate |
| EX-13 | Change Select All in one Module | Other Modules' selections unaffected |
| EX-14 | Open Manage Permissions for protected role | Read-only permission display |
| EX-15 | Protected role has existing assignments | Existing assignments shown checked |
| EX-16 | Protected role screen | Individual checkboxes disabled |
| EX-17 | Protected role screen | Select All controls disabled |
| EX-18 | Protected role screen | Save disabled |
| EX-19 | Protected role screen | `This role’s permissions cannot be modified` displayed |
| EX-20 | Save one new permission assignment | Permission assigned after successful Save |
| EX-21 | Save one permission removal | Permission removed after successful Save |
| EX-22 | Save additions and removals together | Final selection saved |
| EX-23 | Save without changes | Returns to View Details with success notification |
| EX-24 | Successful Save | `Role permissions updated successfully` displayed on View Details |
| EX-25 | Admin unchecks `roles.manage` in own role and saves | Save rejected |
| EX-26 | Admin removes `roles.manage` through RBAC Select All and saves | Save rejected |
| EX-27 | Self-access rejection | Exact Jira error dialog displayed |
| EX-28 | Select OK after self-access error | Remain on Manage Permissions; selections preserved |
| EX-29 | Admin restores `roles.manage` before Save | No self-access rejection solely from previous deselection |
| EX-30 | Admin removes a different RBAC permission from own role | Not rejected solely by `roles.manage` restriction |
| EX-31 | Cancel after changing selections | Changes discarded; return to View Details |
| EX-32 | Cancel with unsaved changes | No confirmation displayed |
| EX-33 | Back without changes | Return directly to View Details |
| EX-34 | Back with unsaved changes | Exact confirmation dialog displayed |
| EX-35 | Select Leave | Discard changes; return to View Details |
| EX-36 | Select Stay | Remain on Manage Permissions; selections preserved |
| EX-37 | Change checkbox then restore original state | Back returns without confirmation |
| EX-38 | General Save failure | `Something went wrong. Please try again.` displayed |
| EX-39 | Select OK after general Save failure | Remain on Manage Permissions; selections preserved |
| EX-40 | Another admin changes role assignments before Save | Save rejected |
| EX-41 | Concurrent assignment change detected | General error dialog displayed |
| EX-42 | Select OK after concurrent-change rejection | Remain on Manage Permissions; selections preserved |
| EX-43 | Concurrent assignment change detected | Newer saved assignments not overwritten |
| EX-44 | Successfully save additions | Added permissions apply on linked users' next session or permission refresh |
| EX-45 | Successfully save removals | Removed permissions apply on linked users' next session or permission refresh |
| EX-46 | Linked user has an active session | Immediate session update not required |
| EX-47 | Self-access Save rejected | No success notification |
| EX-48 | General Save fails | No success notification |
| EX-49 | Concurrent-change Save rejected | No success notification |
| EX-50 | Protected role opened | No assignment changes allowed |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| Manage Permissions wireframe | Supplied for review; illustrates permission grouping by Module, individual checkboxes, permission names and keys, module-level Select All controls, partial selection states, and Save/Cancel/Back actions |

No attachments were listed in the original CP-75 Jira story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-68 | View Role | Provides the selected role's View Details screen and entry point to Manage Permissions |
| CP-69 | Edit Role | Related role-management functionality |
| CP-71 | Create Permission | Creates permissions available for assignment to roles |
| CP-72 | Edit Permission | Maintains permission details displayed in Manage Permissions |
| CP-73 | View Permission | Displays permissions and their current distinct-role assignment counts |
| CP-74 | Delete Permission | Restricts deletion of permissions currently assigned to roles |
| CP-75 | Assign Permission To Role | Current User Story |

### 9.3 Dependencies

1. **Platform Admin Access:** The Manage Permissions functionality is available to the Platform Admin.
2. **Role Details (CP-68):** The workflow starts from View Details for the selected role and returns to that screen after successful Save, Cancel, or completed Back navigation.
3. **Available Permissions:** The system requires the set of available permissions, including their names, keys, and Module associations.
4. **Existing Role-Permission Assignments:** The initial checkbox states depend on the selected role's current permission assignments.
5. **Module Grouping:** Permissions must be associated with Modules to support grouped display and module-level Select All.
6. **Protected Role Status:** The system requires the selected role's protected/non-protected classification to determine whether the screen is editable.
7. **Platform Admin's Own Role:** The system requires the relationship between the current Platform Admin and their role to enforce the `roles.manage` self-access restriction.
8. **Protected Permission Key:** The self-access restriction depends on the confirmed permission key `roles.manage`.
9. **Current Role Assignments at Save:** The system must determine whether the role's permission assignments have changed since Manage Permissions was loaded.
10. **Linked Users:** Successfully saved permission changes apply to users linked to the modified role on their next session or permission refresh.
11. **Permissions List (CP-73):** Updated role-permission relationships affect the distinct-role counts shown when the Permissions List is subsequently loaded or refreshed.
12. **Delete Permission (CP-74):** Successfully removed assignments may affect deletion eligibility, which CP-74 verifies using current role assignments at Confirm Delete.

## 10. Out of Scope

The following activities are not included in CP-75:

1. Creating, editing, or deleting roles.
2. Creating, editing, or deleting permissions — covered by CP-71, CP-72, and CP-74.
3. Editing Permission Name, Permission Key, Module, or Description through Manage Permissions.
4. Creating new Modules or changing permission-to-module associations.
5. Assigning roles to users or changing which users are linked to a role.
6. Defining how roles are classified as protected.
7. Modifying permission assignments for protected roles.
8. Automatically removing `roles.manage` from the Platform Admin's own role.
9. Applying self-access protection automatically to every permission in the RBAC Module.
10. Automatically refreshing all currently active user sessions immediately after Save.
11. Defining the technical mechanism used for permission refresh.
12. Defining the technical mechanism used to detect concurrent assignment changes.
13. Automatically merging conflicting permission assignment changes.
14. Defining permission or Module display ordering beyond the confirmed grouping requirement.
15. Defining loading indicators or progress animations during Save.
16. Defining audit logging for role-permission assignment changes.
17. Defining APIs, database schemas, authorization implementation details, or system architecture.

---

