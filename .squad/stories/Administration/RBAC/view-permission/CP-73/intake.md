# Functional Specification — CP-73: View Permission

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-73 |
| Story Title | View Permission |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** view the list of permissions,  
**So that** I know what access controls exist in the system.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Permissions

The Platform Admin selects **Permissions** from the Roles & Permissions section to open the Permissions List.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to the Roles & Permissions section.
3. The Platform Admin has selected Permissions.
4. The system can retrieve permission records and their associated module information.
5. The system can retrieve the current number of distinct roles assigned to each permission.

An empty Permissions List is a valid state and shall not prevent access to the screen.

## 5. Fields and Validation

### 5.1 Permissions Table

The system shall display permissions in a table containing the five data columns specified in Jira, together with the row-level Actions column shown in the wireframe.

| Column | Data Type | Display Behavior |
|---|---|---|
| Permission Name | Text | Displays the permission's name |
| Permission Key | Text | Displays the permission's unique key |
| Module | Text | Displays the module associated with the permission |
| Description | Text | Displays the permission's description; empty cell if no description exists |
| Number of Roles Using It | Non-negative integer | Displays the current number of distinct roles assigned to the permission |
| Actions | Action controls | Displays Edit and Delete for each permission row |

**Display Behavior:**

1. Each table row shall represent one permission.
2. The table shall display the Permission Name, Permission Key, Module, Description, and Number of Roles Using It for each permission.
3. The Actions column shall provide Edit and Delete for each permission row.
4. If a permission has no Description, its Description cell shall remain empty.
5. Number of Roles Using It shall display the count of distinct roles currently assigned to the permission.
6. Roles with no assigned users shall still be included in the count if they are assigned to the permission.
7. The role count shall represent roles, not the number of users who obtain access through those roles.
8. A permission with no assigned roles shall display `0`.
9. The system shall display the current distinct-role count whenever the Permissions List is loaded or refreshed.
10. The table shall be ordered alphabetically by Permission Name, A–Z, using case-insensitive sorting.
11. The same sorting rule shall apply to displayed search and filter results.

### 5.2 Search by Name or Key

| Attribute | Specification |
|---|---|
| Field Name | Search by Name or Key |
| Field Type | Text input |
| Placeholder | `Enter permission name or key` |
| Mandatory | No |
| Searchable Fields | Permission Name and Permission Key |
| Matching Type | Partial match |
| Case Sensitivity | Case-insensitive |
| Execution | Real-time while typing |
| Leading / Trailing Spaces | Ignored for matching |
| Spaces-only Search | Treated as an empty search |
| Combination With Module Filter | AND logic |
| No Matching Results | `No results found` |

**Search Behavior:**

1. The system shall provide a search input for Permission Name or Permission Key.
2. Search shall be optional.
3. Search shall perform a case-insensitive partial match.
4. A permission shall match when either its Permission Name or Permission Key contains the search text.
5. Search results shall update in real time as the Platform Admin types.
6. The system shall ignore leading and trailing spaces in the search input when performing matching.
7. A search containing only spaces shall behave like an empty search.
8. An empty search shall not restrict the Permissions List by name or key.
9. Search shall operate together with the selected Module filter using AND logic.
10. Clearing the search input shall remove the search restriction while preserving the selected Module filter.
11. If no permissions match the active search and filter conditions, the system shall display `No results found`.
12. Matching results shall remain sorted alphabetically by Permission Name, A–Z, using case-insensitive ordering.

### 5.3 Module Filter

| Attribute | Specification |
|---|---|
| Field Name | Module |
| Field Type | Dropdown |
| Mandatory | No |
| Default Selection | `All Modules` |
| Available Options | All available modules in the Admin Panel, together with `All Modules` |
| Filtering Behavior | Display permissions associated with the selected module |
| Combination With Search | AND logic |
| No Matching Results | `No results found` |

**Filter Behavior:**

1. The system shall provide a Module dropdown.
2. The dropdown shall initially display `All Modules`.
3. The dropdown shall include all available Admin Panel modules.
4. Selecting `All Modules` shall remove the module-specific restriction.
5. Selecting a specific module shall display only permissions associated with that module.
6. The selected Module filter shall operate together with the current search text using AND logic.
7. Changing the Module selection shall update the displayed results.
8. Returning the Module filter to `All Modules` shall remove the module restriction while preserving the search text.
9. If no permissions match the selected Module and search conditions, the system shall display `No results found`.
10. Filtered results shall remain sorted alphabetically by Permission Name, A–Z, using case-insensitive ordering.

### 5.4 Search and Module Filter Combination

The Permissions List shall apply both conditions together.

**Matching Condition:**

A permission shall be displayed when:

- Its Permission Name or Permission Key matches the search text, or the search is empty; **and**
- Its Module matches the selected Module, or `All Modules` is selected.

| Search | Module Filter | Expected Behavior |
|---|---|---|
| Empty | All Modules | Display all permissions |
| `view` | All Modules | Display permissions whose name or key contains `view`, ignoring case |
| Empty | Customers | Display permissions associated with Customers |
| `view` | Customers | Display only Customers permissions whose name or key contains `view` |
| Spaces only | Customers | Display all Customers permissions |
| `  view  ` | Customers | Apply the same matching behavior as `view` with Customers |
| No matching text | All Modules | Display `No results found` |

**Combined Behavior:**

1. Search and Module filtering shall remain independently selectable.
2. Changing one condition shall not automatically clear the other.
3. Both active conditions shall be applied using AND logic.
4. Clearing either condition shall update the displayed results while preserving the other condition.
5. All displayed results shall follow the confirmed alphabetical sorting rule.

### 5.5 Number of Roles Using It

| Attribute | Specification |
|---|---|
| Column Name | Number of Roles Using It |
| Data Type | Non-negative integer |
| Counted Entity | Distinct roles currently assigned to the permission |
| Roles Without Users | Included |
| Unassigned Permission | Displays `0` |
| Update Timing | Whenever the Permissions List is loaded or refreshed |

**Count Behavior:**

1. The system shall display the number of distinct roles currently assigned to each permission.
2. Each assigned role shall be counted once.
3. Roles without assigned users shall be included in the count.
4. The count shall not represent the number of users associated with those roles.
5. A permission not assigned to any role shall display `0`.
6. The system shall retrieve and display the current count whenever the Permissions List is loaded or refreshed.
7. A change to role-permission assignments shall be reflected in the count on the next list load or refresh.

### 5.6 Page-Level and Row-Level Actions

| Action | Location | Required Behavior | Related Story |
|---|---|---|---|
| Create Permission | Page level | Opens Create Permission | CP-71 |
| Edit | Each permission row | Opens Edit Permission for the selected permission | CP-72 |
| Delete | Each permission row | Opens the Delete Permission flow for the selected permission | CP-74 |
| Back | Page level | Returns to Roles & Permissions without confirmation | CP-73 |

**Action Behavior:**

1. The Permissions List and its specified actions shall be available to the Platform Admin.
2. Create Permission shall appear at page level.
3. Edit and Delete shall appear for each permission row.
4. Selecting Create Permission shall open CP-71.
5. Selecting Edit shall open CP-72 for the selected permission.
6. Selecting Delete shall initiate CP-74 for the selected permission.
7. Selecting Back shall return directly to Roles & Permissions.
8. Back shall not require confirmation when Search or Module filtering is active.
9. The detailed creation, editing, and deletion rules shall remain within their respective user stories.

### 5.7 Empty States and Loading Errors

| Condition | Required Behavior |
|---|---|
| No permissions exist | Display `No results found` |
| Search returns no matching permissions | Display `No results found` |
| Module filter returns no matching permissions | Display `No results found` |
| Combined Search/Filter returns no matching permissions | Display `No results found` |
| Permission data fails to load | Display `Unable to load data. Please try again.` with Retry |
| Retry selected | Attempt to reload the list while preserving current Search and Module filter selections |

**Error Handling:**

1. If the system contains no permissions, the system shall display `No results found`.
2. If Search, Module filtering, or their combination returns no matching permissions, the system shall display `No results found`.
3. An empty result shall not be treated as a data-loading failure.
4. If permission data fails to load, the system shall display `Unable to load data. Please try again.`.
5. The loading-failure state shall provide a Retry button.
6. Selecting Retry shall attempt to load the Permissions List again.
7. Retry shall preserve the current Search text and Module filter selection.
8. If the retry succeeds, the system shall display the permissions matching the preserved conditions.
9. If the retry fails, the system shall continue to display the confirmed loading-failure message with Retry available.

### 5.8 List Refresh and State Preservation

| Scenario | List Behavior | Search / Module Filter |
|---|---|---|
| Initial list opening | Load permissions and current role counts | Empty search; `All Modules` |
| Search text changes | Update matching results in real time | Preserve selected Module |
| Module selection changes | Update matching results | Preserve search text |
| Successful Create Permission | Refresh list to show current permissions | Reset to defaults |
| Successful Edit Permission | Refresh list to show updated permissions | Reset to defaults |
| Successful Delete Permission | Refresh list to reflect deletion | Reset to defaults |
| Return from Create/Edit without saving | Return to list without applying changes | Preserve previous selections |
| Retry after loading failure | Attempt to reload permissions | Preserve current selections |
| Back from Permissions List | Navigate to Roles & Permissions | No confirmation |

**State Behavior:**

1. The initial Permissions List shall display an empty search input and `All Modules` as the selected Module filter.
2. Following successful Create, Edit, or Delete, the Permissions List shall refresh to reflect the current permission data.
3. Following those successful operations, the search input shall reset to empty and the Module filter shall reset to `All Modules`.
4. The refreshed list shall apply the confirmed alphabetical sorting rule.
5. Returning from Create or Edit without saving shall preserve the previous Search and Module filter selections.
6. Selecting Retry after a loading failure shall preserve the current Search and Module filter selections.
7. The system shall display the current distinct-role counts when the list is loaded or refreshed.

## 6. Business Rules

**BR-01 — Permissions List Authorization**

Only the Platform Admin shall have access to the Permissions List functionality and its specified actions.

**BR-02 — Permissions List Entry Point**

The system shall provide access to the Permissions List through Dashboard → Roles & Permissions → Permissions.

**BR-03 — Permissions Table**

The system shall display existing permissions in a table.

**BR-04 — Required Data Columns**

The table shall display Permission Name, Permission Key, Module, Description, and Number of Roles Using It.

**BR-05 — Actions Column**

The table shall include an Actions column containing Edit and Delete for each permission row.

**BR-06 — Permission Record Display**

Each table row shall represent one permission and display its corresponding data.

**BR-07 — Empty Description**

If a permission has no Description, the Description column shall display an empty cell without a placeholder.

**BR-08 — Role Count Definition**

Number of Roles Using It shall represent the count of distinct roles currently assigned to the permission.

**BR-09 — Roles Without Users**

Roles with no assigned users shall be included in the role count if they are assigned to the permission.

**BR-10 — Unassigned Permission Count**

If a permission is not assigned to any role, Number of Roles Using It shall display `0`.

**BR-11 — Role Count Update Timing**

The system shall display the current distinct-role count whenever the Permissions List is loaded or refreshed.

**BR-12 — Default Sorting**

The Permissions List shall be sorted alphabetically by Permission Name, A–Z, using case-insensitive ordering.

**BR-13 — Search Availability**

The system shall provide an optional search field for Permission Name or Permission Key.

**BR-14 — Search Matching**

Search shall use case-insensitive partial matching against Permission Name and Permission Key.

**BR-15 — Search Match Condition**

A permission shall match the search condition when either its Permission Name or Permission Key contains the search text.

**BR-16 — Real-time Search**

Search results shall update in real time as the Platform Admin types.

**BR-17 — Search Whitespace Handling**

Leading and trailing spaces in the search input shall be ignored for matching.

**BR-18 — Empty Search Behavior**

An empty or spaces-only search shall not restrict the Permissions List by Permission Name or Permission Key.

**BR-19 — Module Filter Availability**

The system shall provide a Module dropdown for filtering permissions.

**BR-20 — Module Filter Default**

The Module filter shall default to `All Modules`.

**BR-21 — Module Filter Options**

The Module filter shall provide all available Admin Panel modules, together with `All Modules`.

**BR-22 — Module Filter Matching**

Selecting a specific Module shall display only permissions associated with that Module.

**BR-23 — Combined Search and Filter**

Search and Module filtering shall operate together using AND logic.

**BR-24 — Independent Filter Changes**

Changing or clearing Search or Module filtering shall update results without automatically clearing the other condition.

**BR-25 — No Results Message**

If no permissions exist or no permissions match the active conditions, the system shall display `No results found`.

**BR-26 — Data Loading Failure**

If permission data fails to load, the system shall display `Unable to load data. Please try again.` with Retry.

**BR-27 — Retry Behavior**

Selecting Retry shall attempt to reload the Permissions List while preserving the current Search and Module filter selections.

**BR-28 — Create Permission Action**

The system shall provide Create Permission at page level and open CP-71 when selected.

**BR-29 — Edit Permission Action**

The system shall provide Edit for each permission row and open CP-72 for the selected permission.

**BR-30 — Delete Permission Action**

The system shall provide Delete for each permission row and initiate CP-74 for the selected permission.

**BR-31 — Back Navigation**

Selecting Back shall return directly to Roles & Permissions without confirmation, regardless of active Search or Module filtering.

**BR-32 — Refresh After Successful Creation**

After successful Create Permission, the Permissions List shall refresh to include the newly created permission.

**BR-33 — Refresh After Successful Editing**

After successful Edit Permission, the Permissions List shall refresh to display the updated permission data.

**BR-34 — Refresh After Successful Deletion**

After successful Delete Permission, the Permissions List shall refresh to reflect the deletion.

**BR-35 — Reset After Successful Operations**

After successful Create, Edit, or Delete, the Search field shall reset to empty and the Module filter shall reset to `All Modules`.

**BR-36 — Preserve State Without Saving**

Returning to the Permissions List from Create or Edit without saving shall preserve the previous Search and Module filter selections.

**BR-37 — Sorting After Search, Filter, or Refresh**

Displayed permissions shall remain sorted alphabetically by Permission Name, A–Z, using case-insensitive ordering.

**BR-38 — Current Role Count on Refresh**

Whenever the Permissions List is refreshed, the system shall display the current number of distinct roles assigned to each permission.

## 7. Acceptance Criteria

### AC-01 — Access Permissions List

**Given** the user is logged in as a Platform Admin,  
**And** the user has navigated to Roles & Permissions,  
**When** the user selects Permissions,  
**Then** the system shall open the Permissions List.

### AC-02 — Display Permissions Table

**Given** the Permissions List contains existing permissions,  
**When** the Platform Admin opens the list,  
**Then** the system shall display a table containing:

1. Permission Name.
2. Permission Key.
3. Module.
4. Description.
5. Number of Roles Using It.
6. Actions.

### AC-03 — Display Permission Records

**Given** one or more permissions exist,  
**When** the Permissions List loads successfully,  
**Then** each permission shall be displayed in a separate table row with its corresponding values.

### AC-04 — Empty Description Display

**Given** a permission has no Description,  
**When** the permission appears in the list,  
**Then** the Description cell shall be empty, without a placeholder.

### AC-05 — Role Count Display

**Given** a permission is assigned to three distinct roles,  
**When** the Permissions List loads,  
**Then** Number of Roles Using It shall display `3`.

### AC-06 — Include Roles Without Users

**Given** a permission is assigned to two distinct roles,  
**And** one of those roles has no assigned users,  
**When** the Permissions List loads,  
**Then** Number of Roles Using It shall display `2`.

### AC-07 — Unassigned Permission

**Given** a permission is not assigned to any role,  
**When** the Permissions List loads,  
**Then** Number of Roles Using It shall display `0`.

### AC-08 — Refresh Role Count

**Given** a permission's role assignments have changed,  
**When** the Permissions List is loaded or refreshed,  
**Then** the system shall display the current count of distinct assigned roles.

### AC-09 — Alphabetical Sorting

**Given** permissions named `View Customers`, `Edit Settings`, and `Update Settlements` exist,  
**When** the Permissions List loads,  
**Then** the rows shall be displayed in alphabetical order by Permission Name:

1. Edit Settings.
2. Update Settlements.
3. View Customers.

**And** the ordering shall be case-insensitive.

### AC-10 — Display Search Field

**Given** the Platform Admin opens the Permissions List,  
**When** the page is displayed,  
**Then** the system shall provide a Search by Name or Key input.

### AC-11 — Search by Permission Name

**Given** a permission named `View Customers` exists,  
**When** the Platform Admin enters `view` in the search field,  
**Then** the permission shall appear in the results.

### AC-12 — Search by Permission Key

**Given** a permission with key `customers.view` exists,  
**When** the Platform Admin enters `customers.view`,  
**Then** the permission shall appear in the results.

### AC-13 — Partial Search Matching

**Given** a permission named `View Subscriptions` exists,  
**When** the Platform Admin enters `Subscr`,  
**Then** the permission shall appear in the results.

### AC-14 — Case-insensitive Search

**Given** a permission named `View Customers` exists,  
**When** the Platform Admin enters `VIEW`,  
**Then** the permission shall appear in the results.

### AC-15 — Real-time Search

**Given** the Permissions List is open,  
**When** the Platform Admin types or changes the search text,  
**Then** the displayed results shall update in real time without requiring a separate Search action.

### AC-16 — Search Leading and Trailing Spaces

**Given** a permission named `View Customers` exists,  
**When** the Platform Admin enters `  view  `,  
**Then** the search shall match the permission as though the input were `view`.

### AC-17 — Spaces-only Search

**Given** the Platform Admin enters only spaces in the search field,  
**When** the search condition is applied,  
**Then** the system shall treat the search as empty and shall not restrict results by name or key.

### AC-18 — Clear Search

**Given** a search condition is active,  
**When** the Platform Admin clears the search field,  
**Then** the search restriction shall be removed,  
**And** the selected Module filter shall remain unchanged.

### AC-19 — Default Module Filter

**Given** the Platform Admin opens the Permissions List,  
**When** the Module dropdown is displayed,  
**Then** `All Modules` shall be selected by default.

### AC-20 — Module Filter Options

**Given** the Platform Admin opens the Module dropdown,  
**When** the available options are displayed,  
**Then** the dropdown shall include all available Admin Panel modules and `All Modules`.

### AC-21 — Filter by Module

**Given** permissions exist across Customers and Subscriptions modules,  
**When** the Platform Admin selects Customers,  
**Then** the system shall display only permissions associated with Customers.

### AC-22 — All Modules Selection

**Given** a specific Module filter is active,  
**When** the Platform Admin selects `All Modules`,  
**Then** the module-specific restriction shall be removed,  
**And** the current search text shall remain unchanged.

### AC-23 — Combined Search and Module Filter

**Given** the permissions `View Customers` and `View Subscriptions` exist,  
**When** the Platform Admin searches for `view` and selects Customers,  
**Then** the system shall display only permissions that match `view` in their name or key and belong to Customers.

### AC-24 — Independent Search and Filter Conditions

**Given** Search and Module filtering are both active,  
**When** the Platform Admin changes or clears one condition,  
**Then** the other condition shall remain unchanged,  
**And** the results shall update accordingly.

### AC-25 — Empty Permissions List

**Given** no permissions exist,  
**When** the Platform Admin opens the Permissions List and data loading succeeds,  
**Then** the system shall display `No results found`.

### AC-26 — Search With No Results

**Given** permissions exist,  
**When** the Platform Admin enters search text that matches no Permission Name or Permission Key,  
**Then** the system shall display `No results found`.

### AC-27 — Module Filter With No Results

**Given** no permissions belong to the selected Module,  
**When** the Platform Admin selects that Module,  
**Then** the system shall display `No results found`.

### AC-28 — Combined Search and Filter With No Results

**Given** Search and Module filtering are both active,  
**When** no permission satisfies both conditions,  
**Then** the system shall display `No results found`.

### AC-29 — Data Loading Failure

**Given** the Platform Admin opens the Permissions List,  
**When** permission data fails to load,  
**Then** the system shall display:

`Unable to load data. Please try again.`

**And** provide a Retry button.

### AC-30 — Retry After Loading Failure

**Given** the loading-failure message and Retry button are displayed,  
**When** the Platform Admin selects Retry,  
**Then** the system shall attempt to reload the Permissions List.

### AC-31 — Preserve Search and Filter During Retry

**Given** Search text and a Module filter are selected,  
**And** a list-loading attempt fails,  
**When** the Platform Admin selects Retry,  
**Then** the system shall preserve the selected Search text and Module filter.

### AC-32 — Successful Retry

**Given** the Permissions List failed to load,  
**When** the Platform Admin selects Retry and loading succeeds,  
**Then** the system shall display permissions matching the preserved Search and Module filter conditions.

### AC-33 — Repeated Loading Failure

**Given** the Permissions List failed to load,  
**When** the Platform Admin selects Retry and loading fails again,  
**Then** the system shall display `Unable to load data. Please try again.` with Retry available.

### AC-34 — Create Permission Action

**Given** the Platform Admin is viewing the Permissions List,  
**When** the Platform Admin selects Create Permission,  
**Then** the system shall open CP-71 – Create Permission.

### AC-35 — Edit Permission Action

**Given** the Platform Admin is viewing an existing permission row,  
**When** the Platform Admin selects Edit,  
**Then** the system shall open CP-72 – Edit Permission for the selected permission.

### AC-36 — Delete Permission Action

**Given** the Platform Admin is viewing an existing permission row,  
**When** the Platform Admin selects Delete,  
**Then** the system shall initiate CP-74 – Delete Permission for the selected permission.

### AC-37 — Action Visibility

**Given** the Platform Admin opens the Permissions List,  
**When** the page is displayed,  
**Then** Create Permission shall be available at page level,  
**And** Edit and Delete shall be available for each permission row.

### AC-38 — Back Navigation

**Given** the Platform Admin is viewing the Permissions List,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to Roles & Permissions without displaying a confirmation dialog.

### AC-39 — Back With Active Search or Filter

**Given** the Platform Admin has entered Search text or selected a Module filter,  
**When** the Platform Admin selects Back,  
**Then** the system shall return directly to Roles & Permissions without confirmation.

### AC-40 — Refresh After Successful Creation

**Given** the Platform Admin successfully creates a permission through CP-71,  
**When** the system returns to the Permissions List,  
**Then**:

1. The list shall refresh to include the newly created permission.
2. Search shall reset to empty.
3. Module shall reset to `All Modules`.
4. The list shall follow the confirmed alphabetical sorting rule.

### AC-41 — Refresh After Successful Editing

**Given** the Platform Admin successfully updates a permission through CP-72,  
**When** the system returns to the Permissions List,  
**Then**:

1. The list shall refresh to display the updated permission data.
2. Search shall reset to empty.
3. Module shall reset to `All Modules`.
4. The list shall follow the confirmed alphabetical sorting rule.

### AC-42 — Refresh After Successful Deletion

**Given** a permission is successfully deleted through CP-74,  
**When** the system returns to the Permissions List,  
**Then**:

1. The list shall refresh to reflect the deletion.
2. Search shall reset to empty.
3. Module shall reset to `All Modules`.
4. The list shall follow the confirmed alphabetical sorting rule.

### AC-43 — Preserve State After Cancel

**Given** the Platform Admin has selected Search text and a Module filter,  
**And** opens Create Permission or Edit Permission,  
**When** the Platform Admin selects Cancel without saving,  
**Then** the system shall return to the Permissions List with the previous Search and Module filter selections preserved.

### AC-44 — Preserve State After Back Without Saving

**Given** the Platform Admin has selected Search text and a Module filter,  
**And** opens Create Permission or Edit Permission,  
**When** the Platform Admin returns through Back without saving,  
**Then** the system shall preserve the previous Search and Module filter selections.

### AC-45 — Sorting After Search or Filter

**Given** multiple permissions match the active Search or Module filter,  
**When** the results are displayed,  
**Then** the permissions shall be sorted alphabetically by Permission Name, A–Z, using case-insensitive ordering.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin selects Permissions | Permissions List opens |
| EX-02 | Existing permissions are available | Permissions displayed in a table |
| EX-03 | Permission `View Customers` exists | Name, key, module, description, and role count displayed |
| EX-04 | Permission has no Description | Description cell remains empty |
| EX-05 | Permission is assigned to three distinct roles | Role count displays `3` |
| EX-06 | Permission is assigned to two roles, one with no users | Role count displays `2` |
| EX-07 | Permission has no assigned roles | Role count displays `0` |
| EX-08 | Role assignments change, then list refreshes | Current distinct-role count displayed |
| EX-09 | Permissions `View Customers`, `Edit Settings`, `Update Settlements` exist | Displayed A–Z by Permission Name |
| EX-10 | Search `view` | Matches Permission Name or Permission Key containing `view` |
| EX-11 | Search `VIEW` | Same results as `view` |
| EX-12 | Search `Subscr` | Matches `View Subscriptions` |
| EX-13 | Search `customers.view` | Matches corresponding Permission Key |
| EX-14 | Type into Search | Results update in real time |
| EX-15 | Search `  view  ` | Leading/trailing spaces ignored |
| EX-16 | Search contains spaces only | Treated as empty search |
| EX-17 | Clear Search while Customers is selected | Customers filter remains active |
| EX-18 | Open Permissions List | Module defaults to `All Modules` |
| EX-19 | Open Module dropdown | All available modules displayed |
| EX-20 | Select Customers | Only Customers permissions displayed |
| EX-21 | Select `All Modules` | Module restriction removed |
| EX-22 | Search `view` + Module Customers | Only matching Customers permissions displayed |
| EX-23 | Change Module while Search is active | Search remains active |
| EX-24 | No permissions exist | `No results found` |
| EX-25 | Search returns no matches | `No results found` |
| EX-26 | Module filter returns no matches | `No results found` |
| EX-27 | Combined Search/Filter returns no matches | `No results found` |
| EX-28 | Permission data fails to load | `Unable to load data. Please try again.` with Retry |
| EX-29 | Select Retry | System attempts to reload list |
| EX-30 | Retry with active Search/Filter | Previous selections preserved |
| EX-31 | Retry succeeds | Matching permissions displayed |
| EX-32 | Retry fails again | Loading-failure message and Retry remain available |
| EX-33 | Select Create Permission | Opens CP-71 |
| EX-34 | Select Edit for a permission | Opens CP-72 for selected permission |
| EX-35 | Select Delete for a permission | Initiates CP-74 for selected permission |
| EX-36 | View Permissions List | Create at page level; Edit/Delete per row |
| EX-37 | Select Back | Returns to Roles & Permissions |
| EX-38 | Select Back with active Search/Filter | Returns without confirmation |
| EX-39 | Return after successful Create | List refreshed; Search/Filter reset |
| EX-40 | Return after successful Edit | Updated data displayed; Search/Filter reset |
| EX-41 | Return after successful Delete | Deleted permission removed; Search/Filter reset |
| EX-42 | Return from Create/Edit using Cancel | Previous Search/Filter preserved |
| EX-43 | Return from Create/Edit using Back without saving | Previous Search/Filter preserved |
| EX-44 | Search/Filter returns multiple matching permissions | Results remain sorted A–Z |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| Permissions List wireframe | Supplied for review; illustrates the table, Search by Name or Key, Module filter, Create Permission, Edit, Delete, and Back actions |

No attachments were listed in the original CP-73 Jira story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-71 | Create Permission | Opens from the Permissions List; successful creation is reflected in the refreshed list |
| CP-72 | Edit Permission | Opens for a selected permission; successful updates are reflected in the refreshed list |
| CP-73 | View Permission | Current User Story |
| CP-74 | Delete Permission | Initiated from the selected permission row; successful deletion is reflected in the refreshed list |
| CP-75 | Assign Permission To Role | Related functionality affecting the Number of Roles Using It count |

### 9.3 Dependencies

1. **Platform Admin Access:** The Permissions List and its specified actions are available to the Platform Admin.
2. **Permission Records:** The list depends on the availability of existing permission data, including Permission Name, Permission Key, Module, and Description.
3. **Role-Permission Assignments:** Number of Roles Using It depends on the current distinct roles assigned to each permission.
4. **Admin Panel Modules:** The Module filter depends on the available modules in the Admin Panel.
5. **Create Permission (CP-71):** The Create Permission action opens CP-71. Successful creation is reflected when the list refreshes.
6. **Edit Permission (CP-72):** The Edit action opens CP-72 for the selected permission. Successful updates are reflected when the list refreshes.
7. **Delete Permission (CP-74):** The Delete action initiates CP-74. Successful deletion is reflected when the list refreshes.
8. **Roles & Permissions Navigation:** Back returns to the Roles & Permissions screen.
9. **Loading and Retry:** The list depends on successful retrieval of permission data and provides the specified Retry behavior if loading fails.

## 10. Out of Scope

The following activities are not included in CP-73:

1. Creating a permission — covered by CP-71.
2. Editing a permission's details — covered by CP-72.
3. Deleting a permission — covered by CP-74.
4. Assigning permissions to roles — covered by CP-75.
5. Creating, editing, or deleting roles.
6. Assigning roles to users or managing user accounts.
7. Creating or configuring Admin Panel modules.
8. Editing permission data directly within the Permissions List table.
9. Editing Permission Key from the Permissions List.
10. Defining deletion eligibility, confirmation, or deletion-specific error handling.
11. Pagination or page-size rules, which are not defined in the source requirements.
12. User-configurable sorting controls, which are not defined in the source requirements.
13. Live role-count updates while the list remains open without loading or refreshing.
14. Technical implementation details, APIs, database schemas, and system architecture.

---

