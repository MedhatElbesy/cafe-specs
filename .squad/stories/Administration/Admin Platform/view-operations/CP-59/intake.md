# Functional Specification — CP-59: View Operations

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-59 |
| Story Title | View Operations |
| Epic | Not confirmed in the available story information |
| Feature | Not confirmed in the available story information |
| Work Item Type | User Story |
| Specification Status | Draft — Pending Business Approval |
| Scope | Operations List |
| Primary Actor | Platform Admin |
| Business Decisions | D-01 through D-30 |

## 2. User Story

**As a** Platform Admin,  
**I want to** view and search recorded operations,  
**So that** I can monitor in-café redemptions and delivery operations.

## 3. Trigger

Platform Dashboard → Operations

The Platform Admin navigates to the Operations section.

The system retrieves recorded operations and displays the Operations List, including the available search, filters, and pagination controls.

## 4. Preconditions

1. The user is authenticated as a Platform Admin.
2. The system has access to recorded operations.
3. Recorded operations are associated with their respective subscribers and Cafés.
4. The system can identify an operation's type and applicable date/time.
5. Delivery operations have an Order Status maintained by the relevant delivery workflow.
6. The system can retrieve the information required for the Operations List.

**Access Consideration:**

All Platform Admin users may access the Operations List. A separate View Operations permission is not required.

**Scope Consideration:**

CP-59 displays recorded operations, including delivery orders that have not reached a final status.

The screen does not provide View Details or display an Order Status column.

## 5. Fields and Validation

### 5.1 View Operations Screen

| Element | Type | Required Behavior |
|---|---|---|
| Page | Operations List | Displays recorded operations |
| Search | Single text input | Searches by Operation ID or Mobile Number |
| Café Filter | Dropdown | Filters operations by Café |
| Operation Type Filter | Dropdown | Filters In-Café or Delivery operations |
| Date Range Filter | Date inputs | Filters operations within a selected date range |
| Operations Table | Read-only table | Displays recorded operations |
| Pagination | Navigation control | Displays 10 operations per page |
| Retry | Error-state action | Retries loading after failure |

**Screen Behavior:**

1. The system shall allow a Platform Admin to open the Operations List.
2. The system shall retrieve recorded operations.
3. The list shall include both In-Café and Delivery operations.
4. The list shall include recorded delivery orders with non-final statuses.
5. The system shall display the operations in descending order of the applicable displayed Date & Time.
6. The system shall display 10 operations per page.
7. The system shall provide Search, Café Filter, Operation Type Filter, and Date Range Filter.
8. The system shall not provide a View Details action.
9. The system shall not display an Order Status column.
10. The system shall display the confirmed empty or error state when applicable.

### 5.2 Operations Table

| Attribute | Specification |
|---|---|
| Element | Operations Table |
| Type | Read-only table |
| Data Scope | Recorded Operations |
| Default Sorting | Newest First |
| Pagination | 10 records per page |
| View Details | Not available |
| Status Column | Not available |

**Table Columns:**

| Column | Type | Required Behavior |
|---|---|---|
| Operation ID | Read-only identifier | Displays the operation's identifier |
| Date & Time | Read-only datetime | Displays the applicable operation timestamp |
| Subscriber Name | Read-only text | Displays the associated subscriber's name |
| Café Name | Read-only text | Displays the associated Café's name |
| Operation Type | Read-only text | Displays In-Café or Delivery |

**Table Behavior:**

1. The system shall display one row for each recorded operation returned by the active search or filter criteria.
2. Each row shall display Operation ID, Date & Time, Subscriber Name, Café Name, and Operation Type.
3. Operation Type shall display either In-Café or Delivery.
4. The system shall not display a Status column.
5. The system shall not display View Details.
6. The table shall display a maximum of 10 operations per page.
7. Records shall be sorted from newest to oldest using the displayed Date & Time.
8. Rejected QR scans that do not create recorded operations shall not appear in the list.

### 5.3 Operation ID

| Attribute | Specification |
|---|---|
| Field | Operation ID |
| Type | Read-only identifier |
| Source | Recorded operation |
| Searchable | Yes |
| Matching Rule | Exact Match |
| Editable | No |

**Field Behavior:**

1. The system shall display the Operation ID associated with each recorded operation.
2. The Platform Admin shall be able to search for an operation using its Operation ID.
3. Search matching shall use Exact Match.
4. The system shall not provide editing functionality for Operation ID.

### 5.4 Date & Time

| Attribute | Specification |
|---|---|
| Field | Date & Time |
| Type | Read-only datetime |
| Timezone | Saudi Arabia Time (UTC+3) |
| In-Progress Delivery | Creation Date & Time |
| Finalized Delivery | Completion Date & Time |
| Completed In-Café Redemption | Completion Date & Time |
| Sorting | Based on displayed Date & Time |
| Date Filtering | Based on displayed Date & Time |

**Date & Time Behavior:**

1. The system shall display Date & Time using Saudi Arabia Time.
2. For a delivery order that has not reached a final status, the system shall display its creation timestamp.
3. For a delivery order finalized as Delivered, the system shall display its completion timestamp.
4. For a delivery order finalized as Not Responding, the system shall display its completion timestamp.
5. For a completed In-Café redemption, the system shall display its completion timestamp.
6. The system shall use the displayed Date & Time for descending sorting.
7. The system shall use the displayed Date & Time when applying the Date Range Filter.
8. The exact mapping of final delivery timestamps to stored system fields remains pending confirmation.

### 5.5 Subscriber Name

| Attribute | Specification |
|---|---|
| Field | Subscriber Name |
| Type | Read-only text |
| Source | Subscriber associated with the recorded operation |
| Editable | No |

**Field Behavior:**

1. The system shall display the Subscriber Name associated with each recorded operation.
2. The displayed name shall correspond to the operation's subscriber.
3. The field shall be read-only.

### 5.6 Café Name

| Attribute | Specification |
|---|---|
| Field | Café Name |
| Type | Read-only text |
| Source | Café associated with the recorded operation |
| Editable | No |
| Filterable | Yes |

**Field Behavior:**

1. The system shall display the Café Name associated with each recorded operation.
2. Each Café shall be treated as an independent entity.
3. Cafés sharing the same brand or owner shall not be treated as one Café.
4. The Café Filter shall allow the Platform Admin to filter recorded operations by Café.

### 5.7 Operation Type

| Attribute | Specification |
|---|---|
| Field | Operation Type |
| Type | Read-only text |
| Allowed Values | In-Café / Delivery |
| Filterable | Yes |
| Editable | No |

**Operation Type Behavior:**

1. The system shall classify each displayed operation as In-Café or Delivery.
2. Delivered and Not Responding shall not be treated as Operation Type values.
3. The Operation Type Filter shall use In-Café and Delivery as its selectable types.
4. The field shall be read-only.

### 5.8 Delivery Order Status

| Attribute | Specification |
|---|---|
| Concept | Delivery Order Status |
| Applies To | Delivery Orders only |
| Allowed Values | New / Preparing / Out for Delivery / Delivered / Not Responding |
| Final Statuses | Delivered / Not Responding |
| In-Café Status | Not applicable |
| Displayed as Table Column | No |
| Available as Filter | No |

**Order Status Behavior:**

1. Delivery orders may have the status New, Preparing, Out for Delivery, Delivered, or Not Responding.
2. Delivered and Not Responding shall be treated as final Order Statuses.
3. In-Café operations shall not require an Order Status.
4. The Operations List shall include recorded delivery orders with non-final statuses.
5. The system shall use Order Status to determine whether a delivery operation displays Creation Date & Time or Completion Date & Time.
6. The system shall not display Order Status as a table column.
7. The system shall not provide an Order Status Filter.

### 5.9 Search

| Attribute | Specification |
|---|---|
| Element | Search |
| Type | Single text input |
| Searchable Fields | Operation ID / Mobile Number |
| Matching | Exact Match |
| Trigger | Automatic while typing |
| Field Identification | Based on input format |
| Search and Filters | Mutually exclusive |
| No-Match Message | `No matching operations found` |

**Search Behavior:**

1. The system shall provide one Search input.
2. The Search input shall accept an Operation ID or Mobile Number.
3. The system shall identify the intended search field based on the entered input format.
4. The system shall use Exact Match.
5. Search shall execute automatically as the input changes.
6. The system shall not require a Search button to initiate the search.
7. When Search is activated, the system shall clear active filters.
8. When the search value changes, pagination shall return to Page 1.
9. When no operations match the search value, the system shall display:

   `No matching operations found`

10. The exact Operation ID and Mobile Number formats used for automatic identification remain pending confirmation.

### 5.10 Café Filter

| Attribute | Specification |
|---|---|
| Element | Café Filter |
| Type | Dropdown |
| Default Value | All Cafés |
| Available Values | Existing Cafés |
| Selection Scope | Individual Café |
| Interaction With Search | Applying filter clears Search |
| Pagination | Reset to Page 1 when changed |

**Café Filter Behavior:**

1. The system shall provide a Café Filter.
2. The default selection shall be All Cafés.
3. The Platform Admin shall be able to select a Café.
4. The system shall display operations associated with the selected Café.
5. Each Café shall be treated as an independent entity.
6. Applying the Café Filter shall clear the Search value.
7. Changing the Café Filter shall reset pagination to Page 1.
8. If the resulting list is empty, the system shall display `No matching operations found`.

### 5.11 Operation Type Filter

| Attribute | Specification |
|---|---|
| Element | Operation Type Filter |
| Type | Dropdown |
| Default Value | All |
| Available Values | In-Café / Delivery |
| Interaction With Search | Applying filter clears Search |
| Pagination | Reset to Page 1 when changed |

**Operation Type Filter Behavior:**

1. The system shall provide an Operation Type Filter.
2. The default selection shall be All.
3. The Platform Admin shall be able to select In-Café or Delivery.
4. The system shall display operations matching the selected type.
5. Delivered and Not Responding shall not be available as Operation Type Filter values.
6. Applying the Operation Type Filter shall clear the Search value.
7. Changing the filter shall reset pagination to Page 1.
8. If no operations match, the system shall display `No matching operations found`.

### 5.12 Date Range Filter

| Attribute | Specification |
|---|---|
| Element | Date Range Filter |
| Type | From Date / To Date |
| From Date | Required when applying Date Range Filter |
| To Date | Required when applying Date Range Filter |
| Validation | From Date ≤ To Date |
| Timezone | Saudi Arabia Time (UTC+3) |
| Boundaries | Inclusive |
| Date Reference | Displayed Date & Time |
| Interaction With Search | Applying filter clears Search |
| Pagination | Reset to Page 1 when changed |

**Date Range Behavior:**

1. The system shall provide From Date and To Date inputs.
2. Both dates shall be required when applying the Date Range Filter.
3. The system shall not apply a date range containing only one of the two dates.
4. From Date shall be earlier than or equal to To Date.
5. The selected date range shall include both the From Date and To Date in full.
6. Date comparisons shall use Saudi Arabia Time.
7. The system shall filter operations using their displayed Date & Time.
8. Applying the Date Range Filter shall clear Search.
9. Changing the applied date range shall reset pagination to Page 1.
10. If no operations match, the system shall display `No matching operations found`.
11. The exact validation message for incomplete or invalid dates has not been confirmed.

### 5.13 Search and Filters Interaction

| Attribute | Specification |
|---|---|
| Search + Filters | Cannot be active simultaneously |
| Activating Search | Clears active filters |
| Applying a Filter | Clears Search |
| Filter-to-Filter Combination | Pending confirmation |
| Pagination Reset | Page 1 |

**Interaction Behavior:**

1. Search and Filters shall operate separately.
2. Activating Search shall clear active filters.
3. Applying a Filter shall clear the Search value.
4. The system shall not combine Search results with active filters.
5. Changing Search or an applied Filter shall reset pagination to Page 1.
6. Whether Café, Operation Type, and Date Range Filters may be combined with each other remains pending confirmation.

### 5.14 Sorting

| Attribute | Specification |
|---|---|
| Default Sorting | Newest First |
| Sort Field | Displayed Date & Time |
| Sort Direction | Descending |
| Applies To | Operations List |
| Timezone | Saudi Arabia Time |

**Sorting Behavior:**

1. The system shall display operations from newest to oldest.
2. Sorting shall use the displayed Date & Time.
3. In-progress delivery orders shall be sorted using their creation timestamp.
4. Completed In-Café operations and finalized delivery orders shall be sorted using their completion timestamp.
5. Sorting shall apply to the result set before pagination.
6. The order of operations with identical displayed timestamps has not been defined.

### 5.15 Pagination

| Attribute | Specification |
|---|---|
| Element | Pagination |
| Page Size | 10 Operations |
| Navigation | Page Numbers + Previous / Next |
| Search Change | Reset to Page 1 |
| Filter Change | Reset to Page 1 |
| Sorting | Newest First |

**Pagination Behavior:**

1. The system shall display a maximum of 10 operations per page.
2. The system shall provide page-number navigation.
3. The system shall provide Previous and Next navigation.
4. Selecting a page number shall display the corresponding page of results.
5. Selecting Previous or Next shall navigate between available pages.
6. Changing Search shall reset the list to Page 1.
7. Changing an applied Filter shall reset the list to Page 1.
8. Pagination shall operate on the active result set.

### 5.16 Empty States

| Condition | Expected Message |
|---|---|
| No recorded operations exist | `No operations found` |
| Search returns no matching operations | `No matching operations found` |
| Filter returns no matching operations | `No matching operations found` |

**Empty-State Behavior:**

1. If no recorded operations exist, the system shall display:

   `No operations found`

2. If recorded operations exist but Search returns no matches, the system shall display:

   `No matching operations found`

3. If recorded operations exist but the applied Filters return no matches, the system shall display:

   `No matching operations found`

4. The system shall distinguish between no recorded data and no matching results.

### 5.17 Loading Failure and Retry

| Attribute | Specification |
|---|---|
| Condition | Operations cannot be loaded |
| Error Message | `Unable to load operations. Please try again.` |
| Action | Retry |
| Unverified Data | Must not be displayed |
| Retry | Attempts to reload operations |

**Loading Failure Behavior:**

1. If the Operations List cannot be loaded, the system shall display:

   `Unable to load operations. Please try again.`

2. The system shall provide Retry.
3. The system shall not display unverified operation data.
4. Selecting Retry shall initiate another attempt to load operations.
5. If Retry succeeds, the system shall display the retrieved Operations List.
6. If Retry fails, the system shall continue to display the confirmed loading error state.

### 5.18 Platform Admin Access

| Attribute | Specification |
|---|---|
| Functionality | View Operations |
| Authorized Role | Platform Admin |
| Separate Permission | Not required |
| Other User Roles | Not authorized by this story |
| Screen Access | Platform Admin |

**Access Behavior:**

1. The system shall allow authenticated Platform Admin users to access the Operations List.
2. A separate View Operations permission shall not be required.
3. The screen shall not provide functionality outside the confirmed View Operations scope.

## 6. Business Rules

**BR-01 — Platform Admin Access**

The system shall allow authenticated Platform Admin users to view the Operations List without requiring a separate View Operations permission.

**BR-02 — Operations List Scope**

CP-59 shall provide a list of recorded operations, not an Operation Details screen.

**BR-03 — Recorded Operations**

The system shall display recorded In-Café and Delivery operations.

**BR-04 — In-Progress Delivery Operations**

The system shall include recorded delivery orders with New, Preparing, and Out for Delivery statuses.

**BR-05 — Final Delivery Operations**

The system shall include recorded delivery orders finalized as Delivered or Not Responding.

**BR-06 — Rejected QR Scans**

Rejected QR scans that do not create recorded operations shall not appear in the Operations List.

**BR-07 — Operation Type Values**

Operation Type shall have the values In-Café and Delivery.

**BR-08 — Delivery Order Status Values**

Delivery Order Status shall have the values New, Preparing, Out for Delivery, Delivered, and Not Responding.

**BR-09 — Final Order Statuses**

Delivered and Not Responding shall be treated as final Delivery Order Statuses.

**BR-10 — In-Café Status**

In-Café operations shall not require an Order Status.

**BR-11 — No Status Column**

The Operations List shall not display an Order Status column.

**BR-12 — No View Details**

The Operations List shall not provide a View Details action.

**BR-13 — Required Table Columns**

The Operations List shall display Operation ID, Date & Time, Subscriber Name, Café Name, and Operation Type.

**BR-14 — Independent Cafés**

Each Café shall be treated as an independent entity.

**BR-15 — In-Progress Date & Time**

An in-progress Delivery operation shall display its creation timestamp.

**BR-16 — Completed Date & Time**

A completed In-Café operation or finalized Delivery operation shall display its completion timestamp.

**BR-17 — Saudi Arabia Timezone**

Date and time display and date-range filtering shall use Saudi Arabia Time (UTC+3).

**BR-18 — Newest First Sorting**

The Operations List shall be sorted by displayed Date & Time in descending order.

**BR-19 — Search Fields**

The Search input shall support Operation ID and Mobile Number.

**BR-20 — Single Search Input**

The system shall provide one Search input for both supported search fields.

**BR-21 — Search Field Detection**

The system shall identify the intended search field using the input format.

**BR-22 — Exact Search Matching**

Search shall use Exact Match.

**BR-23 — Automatic Search**

Search shall execute automatically as the input changes.

**BR-24 — Search and Filter Exclusivity**

Search and Filters shall not be active simultaneously.

**BR-25 — Search Clears Filters**

Activating Search shall clear active filters.

**BR-26 — Filter Clears Search**

Applying a Filter shall clear the Search value.

**BR-27 — Available Filters**

The Operations List shall provide Café, Operation Type, and Date Range Filters only.

**BR-28 — Café Filter**

The Café Filter shall allow filtering by an individual Café and shall default to All Cafés.

**BR-29 — Operation Type Filter**

The Operation Type Filter shall allow In-Café and Delivery selections and shall default to All.

**BR-30 — Date Range Completeness**

From Date and To Date shall both be required when applying the Date Range Filter.

**BR-31 — Date Range Validation**

From Date shall be earlier than or equal to To Date.

**BR-32 — Inclusive Date Boundaries**

The Date Range Filter shall include the complete From Date and To Date.

**BR-33 — Date Filter Reference**

Date filtering shall use the displayed Date & Time.

**BR-34 — Pagination Page Size**

The Operations List shall display 10 operations per page.

**BR-35 — Pagination Navigation**

Pagination shall provide Page Numbers, Previous, and Next.

**BR-36 — Pagination Reset**

Changing Search or an applied Filter shall reset pagination to Page 1.

**BR-37 — No Operations Found**

When no recorded operations exist, the system shall display `No operations found`.

**BR-38 — No Matching Operations Found**

When Search or Filters return no matches, the system shall display `No matching operations found`.

**BR-39 — Loading Failure**

When operations cannot be loaded, the system shall display `Unable to load operations. Please try again.`

**BR-40 — Retry**

The loading failure state shall provide Retry.

**BR-41 — Unverified Data Protection**

The system shall not display unverified operation data following a loading failure.

**BR-42 — Daily Entitlement and Not Responding**

A delivery order finalized as Not Responding shall consume the subscriber's daily coffee entitlement and prevent another redemption on the same day, subject to the established Saudi Arabia daily-entitlement rule.

**BR-43 — One Daily Coffee Entitlement**

A subscriber shall not consume more than one daily coffee entitlement across Cafés and redemption methods within the same Saudi Arabia calendar day.

## 7. Acceptance Criteria

### AC-01 — Open Operations List

**Given** the user is authenticated as a Platform Admin,  
**When** the user navigates to Operations,  
**Then** the system shall display the Operations List.

### AC-02 — Display Recorded Operations

**Given** recorded operations exist,  
**When** the Operations List loads successfully,  
**Then** the system shall display the recorded operations.

### AC-03 — Display Required Columns

**Given** the Operations List loads successfully,  
**When** the Platform Admin views the table,  
**Then** the system shall display Operation ID, Date & Time, Subscriber Name, Café Name, and Operation Type.

### AC-04 — Exclude View Details

**Given** the Operations List is displayed,  
**When** the Platform Admin views an operation row,  
**Then** the system shall not display a View Details action.

### AC-05 — Exclude Status Column

**Given** the Operations List is displayed,  
**When** the Platform Admin views the table columns,  
**Then** the system shall not display an Order Status column.

### AC-06 — Display In-Café Operations

**Given** recorded In-Café operations exist,  
**When** the Operations List loads,  
**Then** the system shall display those operations with Operation Type In-Café.

### AC-07 — Display In-Progress Delivery Orders

**Given** recorded Delivery operations have New, Preparing, or Out for Delivery statuses,  
**When** the Operations List loads,  
**Then** the system shall include those operations.

### AC-08 — Display Finalized Delivery Orders

**Given** recorded Delivery operations have Delivered or Not Responding statuses,  
**When** the Operations List loads,  
**Then** the system shall include those operations.

### AC-09 — Exclude Rejected QR Scans

**Given** a QR scan is rejected and does not create a recorded operation,  
**When** the Operations List loads,  
**Then** the rejected scan shall not appear as an operation.

### AC-10 — Display Creation Time for In-Progress Delivery

**Given** a recorded Delivery order is New, Preparing, or Out for Delivery,  
**When** the system displays its Date & Time,  
**Then** the system shall use the order creation timestamp.

### AC-11 — Display Completion Time for Finalized Delivery

**Given** a recorded Delivery order is Delivered or Not Responding,  
**When** the system displays its Date & Time,  
**Then** the system shall use the order completion timestamp.

### AC-12 — Display Completion Time for In-Café Redemption

**Given** an In-Café redemption is completed and recorded,  
**When** the system displays its Date & Time,  
**Then** the system shall use the redemption completion timestamp.

### AC-13 — Display Saudi Arabia Time

**Given** an operation has a recorded timestamp,  
**When** the system displays Date & Time,  
**Then** the timestamp shall be represented in Saudi Arabia Time.

### AC-14 — Sort Newest First

**Given** multiple recorded operations have different displayed timestamps,  
**When** the Operations List loads,  
**Then** the system shall sort them by displayed Date & Time in descending order.

### AC-15 — Search by Operation ID

**Given** an operation exists with a specific Operation ID,  
**When** the Platform Admin enters that ID in Search,  
**Then** the system shall return operations matching the ID exactly.

### AC-16 — Search by Mobile Number

**Given** recorded operations are associated with a subscriber's Mobile Number,  
**When** the Platform Admin enters that Mobile Number in Search,  
**Then** the system shall return operations matching the number exactly.

### AC-17 — Automatic Search

**Given** the Platform Admin is viewing the Operations List,  
**When** the Search value changes,  
**Then** the system shall automatically execute the search without requiring a Search button.

### AC-18 — Clear Filters When Searching

**Given** one or more filters are active,  
**When** the Platform Admin activates Search,  
**Then** the system shall clear the active filters.

### AC-19 — Clear Search When Filtering

**Given** a Search value is active,  
**When** the Platform Admin applies a Filter,  
**Then** the system shall clear the Search value.

### AC-20 — Filter by Café

**Given** recorded operations exist for multiple Cafés,  
**When** the Platform Admin selects a Café,  
**Then** the system shall display operations associated with the selected Café.

### AC-21 — Filter by Operation Type

**Given** recorded In-Café and Delivery operations exist,  
**When** the Platform Admin selects an Operation Type,  
**Then** the system shall display operations matching the selected type.

### AC-22 — Require Both Dates

**Given** the Platform Admin is applying a Date Range Filter,  
**When** only From Date or only To Date is provided,  
**Then** the system shall not apply the incomplete date range.

### AC-23 — Validate Date Order

**Given** the Platform Admin enters a From Date later than the To Date,  
**When** the date range is applied,  
**Then** the system shall reject the invalid range.

### AC-24 — Include Date Range Boundaries

**Given** the Platform Admin selects a valid From Date and To Date,  
**When** the Date Range Filter is applied,  
**Then** operations occurring on either boundary date shall be included if they otherwise match the filter.

### AC-25 — Filter by Displayed Date & Time

**Given** recorded operations have applicable displayed timestamps,  
**When** the Platform Admin applies a Date Range Filter,  
**Then** the system shall evaluate operations using their displayed Date & Time in Saudi Arabia Time.

### AC-26 — Display 10 Operations per Page

**Given** more than 10 operations match the active criteria,  
**When** the Operations List is displayed,  
**Then** the system shall display a maximum of 10 operations on the current page.

### AC-27 — Navigate Using Page Numbers

**Given** multiple result pages exist,  
**When** the Platform Admin selects a page number,  
**Then** the system shall display the corresponding page.

### AC-28 — Navigate Using Previous and Next

**Given** multiple result pages exist,  
**When** the Platform Admin selects Previous or Next for an available page,  
**Then** the system shall navigate to that page.

### AC-29 — Reset Pagination After Search Change

**Given** the Platform Admin is on a page other than Page 1,  
**When** the Search value changes,  
**Then** the system shall return to Page 1.

### AC-30 — Reset Pagination After Filter Change

**Given** the Platform Admin is on a page other than Page 1,  
**When** an applied Filter changes,  
**Then** the system shall return to Page 1.

### AC-31 — Display No Operations Found

**Given** no recorded operations exist,  
**When** the Operations List loads successfully,  
**Then** the system shall display:

`No operations found`

### AC-32 — Display No Matching Operations Found for Search

**Given** recorded operations exist,  
**When** Search returns no matching operations,  
**Then** the system shall display:

`No matching operations found`

### AC-33 — Display No Matching Operations Found for Filters

**Given** recorded operations exist,  
**When** the applied Filter returns no matching operations,  
**Then** the system shall display:

`No matching operations found`

### AC-34 — Handle Loading Failure

**Given** the Platform Admin opens the Operations List,  
**When** operations cannot be loaded,  
**Then** the system shall display:

`Unable to load operations. Please try again.`

**And** provide Retry.

### AC-35 — Do Not Display Unverified Operations

**Given** the Operations List cannot be loaded,  
**When** the loading error is displayed,  
**Then** the system shall not display unverified operation data.

### AC-36 — Retry Successful Loading

**Given** the loading error is displayed,  
**When** the Platform Admin selects Retry,  
**And** loading succeeds,  
**Then** the system shall display the retrieved Operations List.

### AC-37 — Retry Failed Loading

**Given** the loading error is displayed,  
**When** the Platform Admin selects Retry,  
**And** loading fails again,  
**Then** the system shall continue displaying the confirmed error message and Retry action.

### AC-38 — Platform Admin Access Without Separate Permission

**Given** the user is an authenticated Platform Admin,  
**When** the user opens the Operations List,  
**Then** the system shall allow access without requiring a separate View Operations permission.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Platform Admin opens Operations | Operations List displayed |
| EX-02 | Recorded operations exist | Operations displayed |
| EX-03 | Operations table loads | Five confirmed columns displayed |
| EX-04 | Platform Admin views a row | No View Details action |
| EX-05 | Platform Admin views table columns | No Status column |
| EX-06 | Completed In-Café redemption exists | Operation Type = In-Café |
| EX-07 | Delivery order status = New | Recorded operation included |
| EX-08 | Delivery order status = Preparing | Recorded operation included |
| EX-09 | Delivery order status = Out for Delivery | Recorded operation included |
| EX-10 | Delivery order status = Delivered | Recorded operation included |
| EX-11 | Delivery order status = Not Responding | Recorded operation included |
| EX-12 | Rejected QR scan creates no recorded operation | Scan not displayed |
| EX-13 | In-progress Delivery order created at 10:00 KSA | Displayed Date & Time = creation time |
| EX-14 | Delivered order finalized at 13:00 KSA | Displayed Date & Time = completion time |
| EX-15 | Not Responding order finalized at 14:00 KSA | Displayed Date & Time = completion time |
| EX-16 | In-Café redemption completed at 09:00 KSA | Displayed Date & Time = completion time |
| EX-17 | Operation A displayed at 14:00 and B at 12:00 | A appears before B |
| EX-18 | Exact existing Operation ID entered | Matching operation returned |
| EX-19 | Nonmatching Operation ID entered | `No matching operations found` |
| EX-20 | Exact existing Mobile Number entered | Associated operations returned |
| EX-21 | Search input changes | Search executes automatically |
| EX-22 | Café Filter active, then Search activated | Active filters cleared |
| EX-23 | Search active, then Café Filter applied | Search cleared |
| EX-24 | Café A selected | Café A operations displayed |
| EX-25 | Operation Type = In-Café | In-Café operations displayed |
| EX-26 | Operation Type = Delivery | Delivery operations displayed |
| EX-27 | From Date provided without To Date | Incomplete date range not applied |
| EX-28 | To Date provided without From Date | Incomplete date range not applied |
| EX-29 | From Date later than To Date | Invalid date range rejected |
| EX-30 | From Date equals To Date | Operations for the full selected KSA date included |
| EX-31 | Operation occurs on From Date | Included if otherwise matching |
| EX-32 | Operation occurs on To Date | Included if otherwise matching |
| EX-33 | 25 matching operations | 10 displayed on Page 1 |
| EX-34 | Platform Admin selects Page 2 | Second page displayed |
| EX-35 | Platform Admin selects Next | Next available page displayed |
| EX-36 | Platform Admin selects Previous | Previous available page displayed |
| EX-37 | Platform Admin on Page 3 changes Search | Returns to Page 1 |
| EX-38 | Platform Admin on Page 3 changes Filter | Returns to Page 1 |
| EX-39 | No recorded operations exist | `No operations found` |
| EX-40 | Search returns zero matches | `No matching operations found` |
| EX-41 | Filter returns zero matches | `No matching operations found` |
| EX-42 | Operations loading fails | Confirmed loading error displayed |
| EX-43 | Operations loading fails | Retry available |
| EX-44 | Operations loading fails | Unverified operation data not displayed |
| EX-45 | Retry succeeds | Operations List displayed |
| EX-46 | Retry fails | Error state remains displayed |
| EX-47 | Authenticated Platform Admin opens Operations | Access allowed without separate View Operations permission |
| EX-48 | Not Responding finalizes and consumes daily entitlement | Another redemption is prevented for that subscriber on the same KSA calendar day |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| CP-59 Jira Story | Original View Operation Details story, subject to updates for View Operations |
| Operations List Wireframe | Screen reference requiring alignment with confirmed decisions |

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-59 | View Operations | Current User Story |
| ID not confirmed | In-Café Redemption | Source of recorded In-Café operations |
| ID not confirmed | Delivery Order Management | Source of Delivery orders and Order Status |
| ID not confirmed | Daily Entitlement Control | Defines the one-coffee-per-day restriction |
| ID not confirmed | Café Management | Source of Café records and identifiers |
| ID not confirmed | Subscriber Management | Source of subscriber identity and Mobile Number |

### 9.3 Dependencies

1. **Recorded Operations:** The system must store and retrieve recorded In-Café and Delivery operations.
2. **Subscriber Information:** Each recorded operation must be associated with its subscriber.
3. **Café Information:** Each recorded operation must be associated with its Café.
4. **Delivery Order Status:** Delivery operations must have an identifiable Order Status.
5. **Creation and Completion Timestamps:** The system must provide the appropriate timestamp for each operation's state.
6. **Search Identification:** Operation ID and Mobile Number formats must be confirmed to support automatic field detection.
7. **Date and Time Conversion:** Date and time presentation and filtering must use Saudi Arabia Time.
8. **Pagination:** The result set must support page-based retrieval or equivalent pagination behavior.
9. **Daily Entitlement Control:** The existing daily-entitlement rules must prevent duplicate consumption, including after Not Responding.
10. **Filter Interaction:** The business must confirm whether Café, Operation Type, and Date Range Filters may be combined with each other.

### 9.4 Pending Business Clarifications

| ID | Area | Pending Confirmation |
|---|---|---|
| P-01 | Search Field Detection | Exact Operation ID and Mobile Number formats, including handling of ambiguous formats |
| P-02 | Timestamp Mapping | Confirmation that final-status timestamps correspond to the stored Completion Date & Time used by the list |
| P-03 | Filter Combination | Whether Café, Operation Type, and Date Range Filters can be applied together |
| P-04 | Date Validation Feedback | Exact message displayed for incomplete or invalid date ranges |
| P-05 | Sorting Ties | Ordering of operations with identical displayed Date & Time values |

P-01 through P-03 are the previously identified outstanding business clarifications. P-04 and P-05 are implementation-level details not defined by the confirmed decisions.

## 10. Out of Scope

1. Viewing individual Operation Details.
2. Providing a View Details action.
3. Editing or deleting recorded operations.
4. Editing Delivery Order Status from the Operations List.
5. Displaying an Order Status column.
6. Filtering by Order Status.
7. Creating In-Café redemptions.
8. Creating Delivery orders.
9. Executing Delivery status transitions.
10. Processing QR scans or QR validation.
11. Modifying the daily-entitlement business rule.
12. Modifying Coffee or Delivery Receivables.
13. Settlement processing.
14. Exporting operations.
15. Implementing a separate View Operations permission.
16. Defining the exact Operation ID or Mobile Number format before confirmation.
17. Defining combined-filter behavior before confirmation.
18. Defining database schemas, API contracts, or technical architecture.

---

