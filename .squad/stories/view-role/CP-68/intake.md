# Functional Specification — CP-68: View Role

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-68 |
| Story Title | View Role |
| Epic | Administration (CP-49) |
| Feature | RBAC (CP-52) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Sara Salah El-Din |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** view the list of roles and their details,  
**So that** I know what access each role gives.

## 3. Trigger

Login → Dashboard → Roles & Permissions → Roles

The Platform Admin navigates to the **Roles** section to view the Roles List.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has navigated to Roles & Permissions.
3. The Roles section is accessible to the Platform Admin.
4. Only the Platform Admin is authorized to access the View Role functionality.

## 5. Fields and Validation

### 5.1 Roles List

The Roles List shall display existing roles in a table containing the following columns:

| Column | Specification |
|---|---|
| Role Name | Displays the name of each role |
| Description | Displays the description associated with each role |
| Number of Permissions | Displays the actual number of permissions assigned to each role |
| Number of Assigned Users | Displays the actual number of users linked to each role |

**Display Behavior:**

1. Each table row represents an existing role.
2. The Roles List shall be sorted alphabetically by Role Name (A–Z).
3. The Number of Permissions shall reflect the actual number of permissions assigned to the corresponding role.
4. The Number of Assigned Users shall reflect the actual number of users linked to the corresponding role.
5. When the Platform Admin returns to the Roles List from another role-management story, the list shall display the latest role data.
6. If no roles exist, the system shall display `No results found`.

### 5.2 Search Field

| Attribute | Specification |
|---|---|
| Field Type | Search input |
| Search Criteria | Role Name |
| Matching Type | Partial match |
| Case Sensitivity | Case-insensitive |
| Search Trigger | Real-time while typing |
| Leading Spaces | Ignored |
| Trailing Spaces | Ignored |
| Results Ordering | Alphabetical by Role Name (A–Z) |
| Empty Search | Displays all roles |
| No Matching Results | Displays `No results found` |

**Search Behavior:**

1. The Platform Admin can search for roles using Role Name.
2. Search results shall update in real time while the Platform Admin types.
3. Search shall support partial matching.
4. Search shall be case-insensitive.
5. Leading and trailing spaces in the search text shall be ignored.
6. Matching roles shall be displayed alphabetically by Role Name (A–Z).
7. Clearing the search field shall restore the complete Roles List in alphabetical order.
8. If no roles match the search text, the system shall display `No results found`.

### 5.3 Role Details

Selecting **View Details** for an available role shall open a separate, read-only Role Details screen.

| Field / Section | Specification |
|---|---|
| Role Name | Displays the selected role's name |
| Description | Displays the selected role's description |
| Assigned Permissions | Displays the names of all permissions assigned to the role in a separate list |
| Linked Users | Displays the names of all users linked to the role in a separate list |

**Display Behavior:**

1. All information on the Role Details screen shall be read-only.
2. Assigned Permissions and Linked Users shall be displayed in two separate lists.
3. Each assigned permission shall be displayed by name.
4. Each linked user shall be displayed by name.
5. If the role has no assigned permissions, the system shall display `No permissions assigned` in the corresponding section.
6. If the role has no linked users, the system shall display `No users assigned` in the corresponding section.
7. Selecting Back from Role Details shall return the Platform Admin to the Roles List.

## 6. Business Rules

**BR-01 — Role Viewing Authorization**

Only the Platform Admin is authorized to access the Roles List and Role Details functionality.

**BR-02 — Roles List Display**

The system shall display existing roles in a table containing Role Name, Description, Number of Permissions, and Number of Assigned Users.

**BR-03 — Roles List Ordering**

The Roles List shall be displayed alphabetically by Role Name (A–Z).

**BR-04 — Permission Count Accuracy**

The Number of Permissions column shall display the actual number of permissions assigned to each role.

**BR-05 — Assigned User Count Accuracy**

The Number of Assigned Users column shall display the actual number of users linked to each role.

**BR-06 — Search by Role Name**

The system shall allow the Platform Admin to search for roles by Role Name.

**BR-07 — Partial and Case-insensitive Search**

Search shall support partial Role Name matching and shall be case-insensitive.

**BR-08 — Real-time Search**

The system shall update search results in real time while the Platform Admin types.

**BR-09 — Search Space Handling**

The system shall ignore leading and trailing spaces in the search text.

**BR-10 — Search Results Ordering**

Matching roles shall be displayed alphabetically by Role Name (A–Z).

**BR-11 — Clearing Search**

Clearing the search field shall restore all roles in alphabetical order.

**BR-12 — Empty Roles List and No Search Results**

If no roles exist or the search returns no matching roles, the system shall display `No results found`.

**BR-13 — View Role Details**

Selecting View Details for an available role shall open a separate Role Details screen.

**BR-14 — Read-only Role Details**

The Role Details screen shall display Role Name, Description, Assigned Permissions, and Linked Users as read-only information.

**BR-15 — Assigned Permissions Display**

The Role Details screen shall display all assigned permissions by name in a separate list.

**BR-16 — Linked Users Display**

The Role Details screen shall display all linked users by name in a separate list.

**BR-17 — Empty Assigned Permissions**

If the selected role has no assigned permissions, the system shall display `No permissions assigned`.

**BR-18 — Empty Linked Users**

If the selected role has no linked users, the system shall display `No users assigned`.

**BR-19 — Create Role Navigation**

Selecting Create Role shall open CP-67 — Create Role.

**BR-20 — Edit Role Navigation**

Selecting Edit shall open CP-69 — Edit Role, subject to the availability behavior defined in that story.

**BR-21 — Delete Role Navigation**

Selecting Delete shall open CP-70 — Delete Role.

**BR-22 — Manage Permissions Navigation**

Selecting Manage Permissions shall open CP-75 — Assign Permission To Role.

**BR-23 — Back Navigation from Roles List**

Selecting Back from the Roles List shall return the Platform Admin to Roles & Permissions.

**BR-24 — Back Navigation from Role Details**

Selecting Back from Role Details shall return the Platform Admin to the Roles List.

**BR-25 — Latest Role Data**

When the Platform Admin returns to the Roles List from another role-management story, the system shall display the latest role data.

**BR-26 — Roles List Loading Failure**

If the Roles List data fails to load, the system shall display `Unable to load data. Please try again.` with a Retry button.

**BR-27 — Roles List Retry**

Selecting Retry after a Roles List loading failure shall attempt to reload the data while remaining on the Roles List screen.

**BR-28 — Role Details Loading Failure**

If Role Details data fails to load, the system shall display `Unable to load data. Please try again.` with a Retry button on the Role Details screen.

**BR-29 — Role Details Retry**

Selecting Retry after a Role Details loading failure shall attempt to reload the data while remaining on the Role Details screen.

**BR-30 — Role Unavailable Before Opening Details**

If the selected role has been deleted or becomes unavailable before Role Details opens, the system shall prevent opening the details screen and display an error dialog containing `Role is no longer available.`

After selecting OK, the Platform Admin shall remain on the Roles List.

**BR-31 — Role Unavailable While Viewing Details**

If the selected role becomes unavailable while the Platform Admin is viewing its details, the system shall display an error dialog containing `Role is no longer available.` when attempting to reload the role's data.

After selecting OK, the system shall return to the Roles List.

## 7. Acceptance Criteria

### AC-01 — Access Roles List

**Given** the user is logged in as a Platform Admin,  
**When** the user navigates to Dashboard → Roles & Permissions → Roles,  
**Then** the system shall display the Roles List containing:

1. Role Name.
2. Description.
3. Number of Permissions.
4. Number of Assigned Users.
5. Search field.
6. View Details action.
7. Create Role action.
8. Edit action.
9. Delete action.
10. Manage Permissions action.
11. Back action.

### AC-02 — Display Existing Roles

**Given** existing roles are available,  
**When** the Platform Admin opens the Roles List,  
**Then**:

1. The system shall display the existing roles in a table.
2. Each role shall display its Role Name and Description.
3. Each role shall display its actual Number of Permissions.
4. Each role shall display its actual Number of Assigned Users.

### AC-03 — Alphabetical Roles List Ordering

**Given** multiple roles exist,  
**When** the Roles List is displayed,  
**Then** the roles shall be ordered alphabetically by Role Name (A–Z).

### AC-04 — Permission Count Accuracy

**Given** a role has assigned permissions,  
**When** the role is displayed in the Roles List,  
**Then** the Number of Permissions column shall display the actual count of permissions assigned to that role.

### AC-05 — Assigned User Count Accuracy

**Given** a role has linked users,  
**When** the role is displayed in the Roles List,  
**Then** the Number of Assigned Users column shall display the actual count of users linked to that role.

### AC-06 — Search by Partial Role Name

**Given** the Roles List contains a role named `Operations Admin`,  
**When** the Platform Admin enters `Admin` in the search field,  
**Then** the system shall display `Operations Admin` among the matching results.

### AC-07 — Case-insensitive Search

**Given** the Roles List contains a role named `Finance Admin`,  
**When** the Platform Admin enters `finance admin`,  
**Then** the system shall include `Finance Admin` in the search results.

### AC-08 — Real-time Search

**Given** the Platform Admin is viewing the Roles List,  
**When** the Platform Admin types in the search field,  
**Then** the system shall update the matching roles in real time without requiring a separate search action.

### AC-09 — Ignore Leading Search Spaces

**Given** the Roles List contains a role named `Manager`,  
**When** the Platform Admin enters `  Manager`,  
**Then** the system shall ignore the leading spaces and include `Manager` in the search results.

### AC-10 — Ignore Trailing Search Spaces

**Given** the Roles List contains a role named `Manager`,  
**When** the Platform Admin enters `Manager  `,  
**Then** the system shall ignore the trailing spaces and include `Manager` in the search results.

### AC-11 — Alphabetical Search Results

**Given** multiple roles match the entered search text,  
**When** the search results are displayed,  
**Then** the matching roles shall be ordered alphabetically by Role Name (A–Z).

### AC-12 — Clear Search

**Given** the Platform Admin has entered search text,  
**And** the Roles List displays matching results,  
**When** the Platform Admin clears the search field,  
**Then** the system shall display all roles in alphabetical order (A–Z).

### AC-13 — Empty Roles List

**Given** no roles exist,  
**When** the Platform Admin opens the Roles List,  
**Then** the system shall display:

`No results found`

### AC-14 — Search Returns No Results

**Given** the Platform Admin enters a Role Name search term,  
**And** no roles match the entered text,  
**When** the search results are evaluated,  
**Then** the system shall display:

`No results found`

### AC-15 — Open Role Details

**Given** the Platform Admin is viewing the Roles List,  
**And** the selected role is available,  
**When** the Platform Admin selects View Details,  
**Then** the system shall open a separate Role Details screen.

### AC-16 — Display Role Details

**Given** the Platform Admin has opened Role Details for an available role,  
**When** the Role Details screen is displayed,  
**Then** the system shall display:

1. Role Name.
2. Description.
3. Assigned Permissions.
4. Linked Users.

### AC-17 — Read-only Role Details

**Given** the Platform Admin is viewing Role Details,  
**When** the role information is displayed,  
**Then** Role Name, Description, Assigned Permissions, and Linked Users shall be read-only.

### AC-18 — Display Assigned Permissions

**Given** the selected role has assigned permissions,  
**When** the Platform Admin opens Role Details,  
**Then**:

1. The system shall display the Assigned Permissions section.
2. Each assigned permission shall be displayed by name.
3. The permissions shall be displayed in a separate read-only list.

### AC-19 — Display Linked Users

**Given** the selected role has linked users,  
**When** the Platform Admin opens Role Details,  
**Then**:

1. The system shall display the Linked Users section.
2. Each linked user shall be displayed by name.
3. The users shall be displayed in a separate read-only list.

### AC-20 — No Assigned Permissions

**Given** the selected role has no assigned permissions,  
**When** the Platform Admin opens Role Details,  
**Then** the Assigned Permissions section shall display:

`No permissions assigned`

### AC-21 — No Linked Users

**Given** the selected role has no linked users,  
**When** the Platform Admin opens Role Details,  
**Then** the Linked Users section shall display:

`No users assigned`

### AC-22 — Open Create Role

**Given** the Platform Admin is viewing the Roles List,  
**When** the Platform Admin selects Create Role,  
**Then** the system shall open CP-67 — Create Role.

### AC-23 — Open Edit Role

**Given** the Platform Admin is viewing the Roles List,  
**And** the selected role is available,  
**When** the Platform Admin selects Edit,  
**Then** the system shall open CP-69 — Edit Role.

### AC-24 — Open Delete Role

**Given** the Platform Admin is viewing the Roles List,  
**When** the Platform Admin selects Delete for a role,  
**Then** the system shall open CP-70 — Delete Role.

### AC-25 — Open Manage Permissions

**Given** the Platform Admin is viewing the Roles List,  
**When** the Platform Admin selects Manage Permissions for a role,  
**Then** the system shall open CP-75 — Assign Permission To Role.

### AC-26 — Back from Roles List

**Given** the Platform Admin is viewing the Roles List,  
**When** the Platform Admin selects Back,  
**Then** the system shall return to Roles & Permissions.

### AC-27 — Back from Role Details

**Given** the Platform Admin is viewing the Role Details screen,  
**When** the Platform Admin selects Back,  
**Then** the system shall return to the Roles List.

### AC-28 — Display Latest Role Data After Returning

**Given** the Platform Admin has completed an action in another role-management story,  
**When** the Platform Admin returns to the Roles List,  
**Then** the system shall display the latest role data.

### AC-29 — Roles List Loading Failure

**Given** the Platform Admin attempts to open the Roles List,  
**When** the Roles List data fails to load,  
**Then** the system shall display:

`Unable to load data. Please try again.`

**And** provide a Retry button.

### AC-30 — Retry Roles List Loading

**Given** the Roles List displays a loading failure message,  
**When** the Platform Admin selects Retry,  
**Then**:

1. The system shall attempt to reload the Roles List data.
2. The Platform Admin shall remain on the Roles List screen.

### AC-31 — Role Details Loading Failure

**Given** the Platform Admin attempts to load Role Details,  
**When** the Role Details data fails to load,  
**Then** the system shall display:

`Unable to load data. Please try again.`

**And** provide a Retry button on the Role Details screen.

### AC-32 — Retry Role Details Loading

**Given** the Role Details screen displays a loading failure message,  
**When** the Platform Admin selects Retry,  
**Then**:

1. The system shall attempt to reload the Role Details data.
2. The Platform Admin shall remain on the Role Details screen.

### AC-33 — Role Unavailable Before Opening Details

**Given** the Platform Admin is viewing the Roles List,  
**And** the selected role has been deleted or become unavailable,  
**When** the Platform Admin selects View Details,  
**Then**:

1. The Role Details screen shall not open.
2. The system shall display an error dialog containing:

   `Role is no longer available.`

3. The dialog shall provide an OK action.
4. After the Platform Admin selects OK, the system shall remain on the Roles List.

### AC-34 — Role Unavailable While Viewing Details

**Given** the Platform Admin is viewing Role Details,  
**And** the selected role has been deleted or become unavailable,  
**When** the system attempts to reload the role's data,  
**Then**:

1. The system shall display an error dialog containing:

   `Role is no longer available.`

2. The dialog shall provide an OK action.
3. After the Platform Admin selects OK, the system shall return to the Roles List.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Open Roles List with existing roles | Roles displayed with the four required columns |
| EX-02 | Roles named `Admin`, `Manager`, `Finance` | Displayed alphabetically: Admin, Finance, Manager |
| EX-03 | Role has 5 assigned permissions | Number of Permissions displays 5 |
| EX-04 | Role has 3 linked users | Number of Assigned Users displays 3 |
| EX-05 | Search `Admin` when `Operations Admin` exists | Matching role displayed |
| EX-06 | Search `admin` when `Finance Admin` exists | Matching role displayed |
| EX-07 | Type in Search field | Results update in real time |
| EX-08 | Search `  Manager` | Leading spaces ignored |
| EX-09 | Search `Manager  ` | Trailing spaces ignored |
| EX-10 | Multiple matching roles | Results displayed alphabetically A–Z |
| EX-11 | Clear Search field | All roles restored alphabetically |
| EX-12 | No roles exist | `No results found` |
| EX-13 | Search with no matches | `No results found` |
| EX-14 | Select View Details for available role | Separate Role Details screen opens |
| EX-15 | Open Role Details | Role Name, Description, Assigned Permissions, and Linked Users displayed |
| EX-16 | Role has assigned permissions | Permission names displayed in a read-only list |
| EX-17 | Role has linked users | User names displayed in a separate read-only list |
| EX-18 | Role has no permissions | `No permissions assigned` |
| EX-19 | Role has no linked users | `No users assigned` |
| EX-20 | Select Create Role | Opens CP-67 |
| EX-21 | Select Edit for available role | Opens CP-69 |
| EX-22 | Select Delete | Opens CP-70 |
| EX-23 | Select Manage Permissions | Opens CP-75 |
| EX-24 | Back from Roles List | Returns to Roles & Permissions |
| EX-25 | Back from Role Details | Returns to Roles List |
| EX-26 | Return after updating a role in CP-69 | Roles List displays latest role data |
| EX-27 | Roles List loading failure | Error message with Retry |
| EX-28 | Retry Roles List loading | Reload attempted on same screen |
| EX-29 | Role Details loading failure | Error message with Retry |
| EX-30 | Retry Role Details loading | Reload attempted on same screen |
| EX-31 | Select View Details for unavailable role | Error dialog; OK remains on Roles List |
| EX-32 | Role becomes unavailable while viewing details and data is reloaded | Error dialog; OK returns to Roles List |

## 9. Attachments and Related Requirements

### 9.1 Attachments

- Roles List wireframe — supplied for review.
- No separate Role Details wireframe was supplied.
- No Jira attachments were listed in the original CP-68 story.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-52 | RBAC | Parent Feature |
| CP-67 | Create Role | Related story opened through Create Role |
| CP-68 | View Role | Current User Story |
| CP-69 | Edit Role | Related story opened through Edit |
| CP-70 | Delete Role | Related story opened through Delete |
| CP-75 | Assign Permission To Role | Related story opened through Manage Permissions |

### 9.3 Dependencies

- Role viewing is restricted to the Platform Admin.
- The Roles List requires access to existing role data, including Role Name, Description, assigned permission counts, and linked user counts.
- Search operates on Role Name and displays matching roles in alphabetical order.
- Role Details requires the selected role's data, assigned permission names, and linked user names.
- The Roles List must display the latest role data when the Platform Admin returns from another role-management story.
- Create Role, Edit, Delete, and Manage Permissions actions navigate to their respective user stories.
- Role unavailability must be handled when opening Role Details or attempting to reload its data.
- Loading failures on Roles List and Role Details support Retry without navigating away from the respective screen.

## 10. Out of Scope

The following activities are not included in CP-68:

1. Creating roles — covered by CP-67.
2. Editing roles — covered by CP-69.
3. Deleting roles — covered by CP-70.
4. Assigning or removing role permissions — covered by CP-75.
5. Creating or editing permissions.
6. Assigning or removing roles from users.
7. Modifying role information directly from the read-only Role Details screen.
8. Managing user accounts.
9. Technical implementation details, APIs, database schemas, and system architecture.

---
