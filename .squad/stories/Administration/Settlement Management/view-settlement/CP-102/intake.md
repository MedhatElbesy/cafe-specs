# Functional Specification — CP-102: View Settlement

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-102 |
| Story Title | View Settlement |
| Epic | Administration (CP-49) |
| Feature | Admin Platform (CP-50) → Settlement Management (CP-101) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Medhat Elbesy |
| Labels | MVP, Sprint1 |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** view the current outstanding amounts for partner Cafés,  
**So that** I can identify the amounts that need to be settled with each Café.

## 3. Trigger

Platform Dashboard → Settlement Management

The Platform Admin selects **Settlements** from the Platform Dashboard navigation.

The system opens the Settlements page, retrieves the latest saved receivable and settlement data, and displays the settlement summary and Café Receivables table.

## 4. Preconditions

1. The user is logged in as a Platform Admin.
2. The Platform Admin has permission to view settlements.
3. The system has access to the existing Café records.
4. The system has access to saved Coffee and Delivery Receivables.
5. The system has access to saved settlement records needed to exclude settled receivables.

**Access Control Consideration:**

Users without permission to view settlements shall not be allowed to access the page or its data through the user interface, direct URL, or direct requests.

The exact permission key and unauthorized-access message have not been confirmed.

**Financial Data Consideration:**

Only successfully created and unsettled receivables shall contribute to outstanding amounts.

Previously settled receivables shall be excluded.

## 5. Fields and Validation

### 5.1 View Settlements Screen

| Element | Type | Required Behavior |
|---|---|---|
| Page Title | Read-only text | Displays Settlements |
| Outstanding Coffee | Summary card | Displays total outstanding Coffee Receivables across all Cafés |
| Outstanding Delivery | Summary card | Displays total outstanding Delivery Receivables across all Cafés |
| Total Outstanding | Summary card | Displays combined outstanding Coffee and Delivery Receivables |
| Café Receivables | Table | Displays the outstanding amounts for each Café |
| Refresh | Action | Reloads the latest saved settlement data |
| View Details | Row action | Opens View One Settlement for the selected Café |
| Retry | Error-state action | Reloads settlement data following a loading failure |

**Screen Behavior:**

1. The system shall display the Settlements page to an authorized Platform Admin.
2. The page shall contain three summary cards: Outstanding Coffee, Outstanding Delivery, and Total Outstanding.
3. The system shall display the Café Receivables table.
4. Each existing Café shall appear as an independent record.
5. The page shall include suspended Cafés and Cafés with zero outstanding balances.
6. All monetary values shall be displayed in SAR with two decimal places.
7. The table shall be sorted by Total Outstanding in descending order.
8. The system shall provide Refresh to reload the latest saved data.
9. Each Café row shall provide View Details.
10. Loading failures and empty states shall follow the confirmed behaviors defined in Sections 5.9 and 5.10.

### 5.2 Outstanding Coffee Summary Card

| Attribute | Specification |
|---|---|
| Element | Outstanding Coffee |
| Type | Read-only summary card |
| Value | Sum of outstanding Coffee Receivables across all existing Cafés |
| Included Cafés | All existing Cafés, including suspended and zero-balance Cafés |
| Excluded Amounts | Successfully settled Coffee Receivables |
| Currency | SAR |
| Display Precision | Two decimal places |
| Editable | No |
| Refresh | Updated on page load or successful Refresh |

**Outstanding Coffee Behavior:**

1. The system shall calculate Outstanding Coffee using successfully created, unsettled Coffee Receivables.
2. The calculation shall include all existing Cafés.
3. Successfully settled Coffee Receivables shall be excluded.
4. The displayed amount shall reconcile with the sum of Coffee Amount values in the Café Receivables table.
5. The system shall display the amount in SAR with two decimal places.
6. The value shall be updated when the page loads successfully or Refresh completes successfully.

### 5.3 Outstanding Delivery Summary Card

| Attribute | Specification |
|---|---|
| Element | Outstanding Delivery |
| Type | Read-only summary card |
| Value | Sum of outstanding Delivery Receivables across all existing Cafés |
| Included Cafés | All existing Cafés, including suspended and zero-balance Cafés |
| Excluded Amounts | Successfully settled Delivery Receivables |
| Currency | SAR |
| Display Precision | Two decimal places |
| Editable | No |
| Refresh | Updated on page load or successful Refresh |

**Outstanding Delivery Behavior:**

1. The system shall calculate Outstanding Delivery using successfully created, unsettled Delivery Receivables.
2. The calculation shall include all existing Cafés.
3. Successfully settled Delivery Receivables shall be excluded.
4. The displayed amount shall reconcile with the sum of Delivery Amount values in the Café Receivables table.
5. The system shall display the amount in SAR with two decimal places.
6. The value shall be updated when the page loads successfully or Refresh completes successfully.

### 5.4 Total Outstanding Summary Card

| Attribute | Specification |
|---|---|
| Element | Total Outstanding |
| Type | Read-only summary card |
| Calculation | Outstanding Coffee + Outstanding Delivery |
| Scope | All existing Cafés |
| Currency | SAR |
| Display Precision | Two decimal places |
| Editable | No |
| Reconciliation | Must equal the sum of Café Total Outstanding amounts |

**Total Outstanding Behavior:**

1. The system shall calculate Total Outstanding as the sum of Outstanding Coffee and Outstanding Delivery.
2. The calculation shall include all existing Cafés, including suspended Cafés.
3. Previously settled receivables shall not contribute to Total Outstanding.
4. The summary amount shall reconcile with the sum of Total Outstanding values in the Café Receivables table.
5. The system shall display the amount in SAR with two decimal places.
6. The calculation shall use the Finance-approved rounding rule once confirmed.

### 5.5 Café Receivables Table

| Attribute | Specification |
|---|---|
| Table Name | Café Receivables |
| Data Scope | All existing Cafés |
| Record Scope | One record per Café |
| Sorting | Total Outstanding — Descending |
| Zero-Balance Records | Included |
| Suspended Cafés | Included |
| Monetary Currency | SAR |
| Monetary Display | Two decimal places |
| Editable | No |

**Table Columns:**

| Column | Type | Required Behavior |
|---|---|---|
| Café Name | Read-only text | Displays the selected Café's name |
| Coffee Amount | Read-only monetary value | Displays outstanding Coffee Receivables |
| Delivery Amount | Read-only monetary value | Displays outstanding Delivery Receivables |
| Total Outstanding | Read-only monetary value | Displays Coffee Amount + Delivery Amount |
| Action | View Details | Opens View One Settlement for the selected Café |

**Table Behavior:**

1. The system shall display each existing Café as a separate record.
2. Cafés sharing the same owner or brand shall not have their balances combined.
3. The table shall include suspended Cafés.
4. The table shall include Cafés with zero outstanding balances.
5. Coffee Amount shall represent successfully created, unsettled Coffee Receivables.
6. Delivery Amount shall represent successfully created, unsettled Delivery Receivables.
7. Total Outstanding shall equal Coffee Amount plus Delivery Amount for the Café.
8. Successfully settled receivables shall be excluded.
9. The system shall sort Café records by Total Outstanding from highest to lowest.
10. Every Café row shall provide View Details.
11. The system shall display all monetary values in SAR with two decimal places.

### 5.6 Café Name and Independent Café Identification

| Attribute | Specification |
|---|---|
| Field | Café Name |
| Type | Read-only text |
| Source | Existing Café record |
| Entity Scope | Individual Café |
| Shared Brand or Owner | Does not combine Café records |
| Editable | No |
| Associated Action | View Details |

**Café Identification Behavior:**

1. The system shall display the name of each existing Café.
2. Each Café shall be treated as an independent entity for settlement purposes.
3. Cafés sharing the same brand or owner shall appear as separate records.
4. Outstanding amounts shall be calculated independently for each Café.
5. View Details shall preserve the identity of the selected Café.

### 5.7 Coffee Amount, Delivery Amount, and Total Outstanding

| Attribute | Coffee Amount | Delivery Amount | Total Outstanding |
|---|---|---|---|
| Type | Read-only monetary value | Read-only monetary value | Read-only monetary value |
| Scope | Selected Café | Selected Café | Selected Café |
| Source | Unsettled Coffee Receivables | Unsettled Delivery Receivables | Coffee + Delivery |
| Settled Amounts | Excluded | Excluded | Excluded |
| Currency | SAR | SAR | SAR |
| Display Precision | Two decimal places | Two decimal places | Two decimal places |
| Zero Value | SAR 0.00 | SAR 0.00 | SAR 0.00 |
| Editable | No | No | No |

**Amount Calculation Behavior:**

1. Coffee Amount shall include eligible unsettled Coffee Receivables for the Café.
2. Delivery Amount shall include eligible unsettled Delivery Receivables for the Café.
3. Total Outstanding shall equal Coffee Amount plus Delivery Amount.
4. Successfully settled receivables shall be excluded from all three amounts.
5. A successful in-café redemption shall contribute Coffee Receivables only.
6. A Delivered delivery outcome shall contribute both Coffee and Delivery Receivables.
7. A Not Responding delivery outcome shall contribute both Coffee and Delivery Receivables.
8. Creating a delivery order or moving it to Preparing or Out for Delivery shall not create receivables.
9. All amounts shall be displayed in SAR with two decimal places.
10. The calculation rounding rule shall remain pending Finance confirmation.

### 5.8 Refresh Action

| Attribute | Specification |
|---|---|
| Action | Refresh |
| Type | Page action |
| Purpose | Reload the latest saved settlement data |
| Affected Data | Summary cards and Café Receivables table |
| Automatic Refresh | Not required |
| Successful Result | Updated amounts displayed |
| Failure Result | Loading failure behavior in Section 5.9 |

**Refresh Behavior:**

1. The system shall provide a Refresh action on the Settlements page.
2. Selecting Refresh shall initiate retrieval of the latest saved Café, receivable, and settlement data.
3. Following a successful refresh, the system shall update the three summary cards.
4. Following a successful refresh, the system shall update the Café Receivables table.
5. Successfully settled receivables shall be excluded from the refreshed outstanding amounts.
6. New eligible receivables saved before the refreshed data is retrieved shall be reflected in the updated amounts.
7. Automatic live updates while the page remains open shall not be required.
8. If the refresh fails, the system shall follow the loading failure behavior defined in Section 5.9.

### 5.9 Settlement Loading Failure and Retry

| Attribute | Specification |
|---|---|
| Condition | Settlement data cannot be loaded |
| Feedback | Error message |
| Exact Message | `Unable to load settlements. Please try again.` |
| Action | Retry |
| Unverified Summary Values | Not displayed |
| Unverified Table Values | Not displayed |
| Retry Result | New attempt to retrieve settlement data |

**Loading Failure Behavior:**

1. If settlement data cannot be loaded, the system shall display the exact message:

   `Unable to load settlements. Please try again.`

2. The system shall provide Retry.
3. The system shall not display unverified financial amounts.
4. Selecting Retry shall initiate another attempt to load the settlement data.
5. If Retry succeeds, the system shall display the retrieved summary cards and Café Receivables table.
6. If Retry fails, the system shall continue to display the confirmed loading error state.
7. A failed loading attempt shall not be treated as a successful data refresh.

### 5.10 No Cafés Found and Zero Outstanding States

| Condition | Expected Behavior |
|---|---|
| No Cafés exist | Display `No cafés found` |
| Cafés exist with zero outstanding amounts | Display the Cafés in the table |
| Coffee Amount is zero | Display SAR 0.00 |
| Delivery Amount is zero | Display SAR 0.00 |
| Total Outstanding is zero | Display SAR 0.00 |
| All existing Cafés have zero balances | Continue displaying the Café records and zero-valued summary cards |

**Empty and Zero-Balance Behavior:**

1. The system shall display `No cafés found` only when no Cafés exist.
2. The system shall not display `No cafés found` merely because outstanding amounts are zero.
3. Existing Cafés with zero outstanding balances shall remain visible.
4. Suspended Cafés with zero outstanding balances shall also remain visible.
5. Zero monetary amounts shall be displayed as SAR 0.00.
6. When all existing Cafés have zero outstanding balances, the three summary cards shall display SAR 0.00.

### 5.11 View Details Navigation

| Attribute | Specification |
|---|---|
| Action | View Details |
| Location | Each Café Receivables table row |
| Source Screen | View Settlements |
| Destination | View One Settlement |
| Selected Context | Individual Café identity |
| Scope of CP-102 | Navigation only |

**View Details Behavior:**

1. Every Café row shall provide View Details.
2. Selecting View Details shall navigate to View One Settlement.
3. The navigation shall preserve the identity of the selected Café.
4. The destination shall correspond to the Café whose View Details action was selected.
5. The fields and behavior of View One Settlement shall be defined in its separate User Story.
6. CP-102 shall not define settlement detail fields or settlement execution actions.

### 5.12 Settlement Access Restriction

| Attribute | Specification |
|---|---|
| Protected Functionality | View Settlements |
| Required Access | Permission to view settlements |
| UI Access | Restricted |
| Direct URL Access | Restricted |
| Direct Request Access | Restricted |
| Unauthorized Result | Access denied |
| Permission Key | Not specified |
| Exact Error Message | Not specified |

**Access Restriction Behavior:**

1. The system shall allow authorized Platform Admin users to access View Settlements.
2. Users without the required permission shall not access settlement information through the user interface.
3. Direct URL access shall not bypass the permission restriction.
4. Direct requests shall not bypass the permission restriction.
5. Unauthorized requests shall not disclose settlement information.
6. No specific permission key or unauthorized-access message is prescribed by this specification.

## 6. Business Rules

**BR-01 — Authorized Settlement Access**

Only users with permission to view settlements shall be allowed to access the View Settlements functionality.

**BR-02 — Settlement Entry Point**

The Platform Admin shall access View Settlements through Platform Dashboard → Settlement Management.

**BR-03 — Summary Cards**

The system shall display Outstanding Coffee, Outstanding Delivery, and Total Outstanding summary cards.

**BR-04 — Summary Scope**

The summary cards shall aggregate outstanding receivables across all existing Cafés, including suspended Cafés.

**BR-05 — Café Receivables Table**

The system shall display Café Name, Coffee Amount, Delivery Amount, Total Outstanding, and View Details for each Café.

**BR-06 — Independent Café Records**

Each Café shall be treated as an independent entity, even when multiple Cafés share the same brand or owner.

**BR-07 — All Existing Cafés**

The settlement list shall include all existing Cafés.

**BR-08 — Suspended Café Inclusion**

Suspended Cafés shall remain visible, and their financial entitlements shall continue to contribute to outstanding amounts where applicable.

**BR-09 — Zero-Balance Café Inclusion**

Cafés with zero outstanding balances shall remain visible.

**BR-10 — Eligible Receivables**

Only successfully created, unsettled receivables shall contribute to outstanding amounts.

**BR-11 — In-Café Redemption Receivables**

Successful in-café redemption shall create a Coffee Receivable only.

**BR-12 — Delivered Receivables**

A delivery order finalized as Delivered shall create both Coffee and Delivery Receivables.

**BR-13 — Not Responding Receivables**

A delivery order finalized as Not Responding shall create both Coffee and Delivery Receivables.

**BR-14 — Non-Receivable Delivery States**

Creating a delivery order or changing its status to Preparing or Out for Delivery shall not create receivables.

**BR-15 — Settled Receivable Exclusion**

Successfully settled receivables shall be excluded from outstanding amounts.

**BR-16 — Full Settlement Requirement**

Each settlement shall cover the Café's entire current outstanding balance. Partial settlements shall not be supported.

**BR-17 — Café Coffee Amount**

Coffee Amount shall equal the Café's outstanding Coffee Receivables.

**BR-18 — Café Delivery Amount**

Delivery Amount shall equal the Café's outstanding Delivery Receivables.

**BR-19 — Café Total Outstanding**

Each Café's Total Outstanding shall equal Coffee Amount plus Delivery Amount.

**BR-20 — Outstanding Coffee Summary**

Outstanding Coffee shall equal the sum of outstanding Coffee Amounts across all Cafés.

**BR-21 — Outstanding Delivery Summary**

Outstanding Delivery shall equal the sum of outstanding Delivery Amounts across all Cafés.

**BR-22 — Total Outstanding Summary**

Total Outstanding shall equal Outstanding Coffee plus Outstanding Delivery.

**BR-23 — Summary Reconciliation**

The summary card totals shall reconcile with the Café Receivables table.

**BR-24 — Currency**

All monetary amounts shall be displayed in Saudi Riyals (SAR).

**BR-25 — Display Precision**

All monetary amounts shall be displayed with two decimal places.

**BR-26 — Calculation Rounding Rule**

Receivable calculations shall follow a consistent Finance-approved rounding rule. The exact rule remains pending Finance confirmation.

**BR-27 — Café Sorting**

The Café Receivables table shall be sorted by Total Outstanding in descending order.

**BR-28 — Latest Saved Data**

The system shall display the latest saved receivable and settlement data when the page is opened or manually refreshed.

**BR-29 — Manual Refresh**

The system shall provide a Refresh action to reload the latest saved settlement data.

**BR-30 — No Automatic Live Updates**

The system shall not be required to update settlement amounts automatically while the page remains open.

**BR-31 — Zero Outstanding Display**

A Café with no outstanding receivables shall display SAR 0.00 for Coffee Amount, Delivery Amount, and Total Outstanding.

**BR-32 — No Cafés Found Message**

When no Cafés exist, the system shall display `No cafés found`.

**BR-33 — Loading Failure Message**

When settlement data cannot be loaded, the system shall display `Unable to load settlements. Please try again.`

**BR-34 — Retry Action**

The loading failure state shall provide Retry to attempt loading the settlement data again.

**BR-35 — Unverified Financial Values**

The system shall not display unverified settlement summary or table values following a loading failure.

**BR-36 — View Details**

Each Café record shall provide View Details.

**BR-37 — Selected Café Context**

View Details shall navigate to View One Settlement while preserving the selected Café's identity.

**BR-38 — Direct URL Access Restriction**

Users without permission to view settlements shall not bypass access restrictions through a direct URL.

**BR-39 — Direct Request Access Restriction**

Users without permission to view settlements shall not bypass access restrictions through direct requests.

**BR-40 — Unauthorized Data Protection**

Unauthorized users shall not be provided with settlement information.

## 7. Acceptance Criteria

### AC-01 — Access View Settlements

**Given** the Platform Admin is authenticated,  
**And** has permission to view settlements,  
**When** the Platform Admin navigates to Settlement Management,  
**Then** the system shall open the Settlements page.

### AC-02 — Display Settlement Summary Cards

**Given** the Settlements page loads successfully,  
**When** the Platform Admin views the page,  
**Then** the system shall display Outstanding Coffee, Outstanding Delivery, and Total Outstanding summary cards.

### AC-03 — Display Café Receivables Table

**Given** one or more Cafés exist,  
**When** the Settlements page loads successfully,  
**Then** the system shall display the Café Receivables table,  
**And** include Café Name, Coffee Amount, Delivery Amount, Total Outstanding, and View Details.

### AC-04 — Display Independent Café Records

**Given** multiple Cafés share the same owner or brand,  
**When** the settlement list is displayed,  
**Then** each Café shall appear as a separate record,  
**And** its outstanding amounts shall be calculated independently.

### AC-05 — Include Suspended Cafés

**Given** a Café is suspended,  
**When** the settlement list loads,  
**Then** the Café shall remain visible,  
**And** its eligible unsettled receivables shall contribute to outstanding totals.

### AC-06 — Include Zero-Balance Cafés

**Given** a Café exists with no outstanding receivables,  
**When** the settlement list loads,  
**Then** the Café shall remain visible,  
**And** its Coffee Amount, Delivery Amount, and Total Outstanding shall display SAR 0.00.

### AC-07 — Include Only Eligible Receivables

**Given** the system contains financial operations,  
**When** outstanding amounts are calculated,  
**Then** only successfully created and unsettled receivables shall be included.

### AC-08 — Include In-Café Coffee Receivable

**Given** an in-café redemption completes successfully,  
**When** its receivables are included in settlement calculations,  
**Then** the Coffee Receivable shall contribute to outstanding Coffee Amount,  
**And** no Delivery Receivable shall be created by that redemption.

### AC-09 — Include Delivered Coffee and Delivery Receivables

**Given** a delivery order is finalized as Delivered,  
**And** its Coffee and Delivery Receivables are successfully created,  
**When** outstanding amounts are calculated,  
**Then** both receivables shall contribute while unsettled.

### AC-10 — Include Not Responding Coffee and Delivery Receivables

**Given** a delivery order is finalized as Not Responding,  
**And** its Coffee and Delivery Receivables are successfully created,  
**When** outstanding amounts are calculated,  
**Then** both receivables shall contribute while unsettled.

### AC-11 — Exclude Preparing and Out for Delivery

**Given** a delivery order has only been created or has reached Preparing or Out for Delivery,  
**When** outstanding amounts are calculated,  
**Then** no receivables shall be included solely because of those states.

### AC-12 — Exclude Settled Receivables

**Given** receivables have been successfully settled,  
**When** the Settlements page retrieves the latest saved data,  
**Then** those receivables shall not contribute to outstanding amounts.

### AC-13 — Calculate Café Total Outstanding

**Given** a Café has outstanding Coffee and Delivery Receivables,  
**When** the system calculates Total Outstanding,  
**Then** Total Outstanding shall equal Coffee Amount plus Delivery Amount.

### AC-14 — Calculate Outstanding Coffee Summary

**Given** the Café Receivables table contains outstanding Coffee Amounts,  
**When** the system calculates Outstanding Coffee,  
**Then** the summary shall equal the sum of outstanding Coffee Amounts across all Cafés.

### AC-15 — Calculate Outstanding Delivery Summary

**Given** the Café Receivables table contains outstanding Delivery Amounts,  
**When** the system calculates Outstanding Delivery,  
**Then** the summary shall equal the sum of outstanding Delivery Amounts across all Cafés.

### AC-16 — Calculate Total Outstanding Summary

**Given** Outstanding Coffee and Outstanding Delivery have been calculated,  
**When** the system calculates Total Outstanding,  
**Then** Total Outstanding shall equal Outstanding Coffee plus Outstanding Delivery.

### AC-17 — Reconcile Summary Cards With Table

**Given** the Settlements page has loaded successfully,  
**When** the Platform Admin views the summary cards and Café Receivables table,  
**Then** the summary amounts shall reconcile with the corresponding table amounts.

### AC-18 — Display SAR Currency

**Given** monetary amounts are displayed,  
**When** the Platform Admin views the summary cards or table,  
**Then** all amounts shall be displayed in SAR.

### AC-19 — Display Two Decimal Places

**Given** a monetary amount is available,  
**When** the system displays the amount,  
**Then** the amount shall use two decimal places.

### AC-20 — Sort Cafés by Highest Outstanding

**Given** multiple Cafés have different Total Outstanding amounts,  
**When** the Café Receivables table is displayed,  
**Then** the Cafés shall be sorted by Total Outstanding in descending order.

### AC-21 — Display Latest Saved Data on Page Load

**Given** eligible receivables and settlement records have been saved,  
**When** the Platform Admin opens the Settlements page,  
**Then** the system shall retrieve and display the latest saved outstanding amounts.

### AC-22 — Refresh Settlement Data

**Given** the Platform Admin is viewing the Settlements page,  
**When** the Platform Admin selects Refresh,  
**Then** the system shall reload the latest saved settlement data,  
**And** update the summary cards and Café Receivables table after successful loading.

### AC-23 — No Automatic Live Update Required

**Given** the Platform Admin is viewing the Settlements page,  
**And** new financial data is saved after the page was loaded,  
**When** the Platform Admin has not refreshed the page,  
**Then** the system shall not be required to update the displayed amounts automatically.

### AC-24 — Display No Cafés Found

**Given** no Cafés exist,  
**When** the system successfully retrieves the Café records,  
**Then** the system shall display:

`No cafés found`

### AC-25 — Display Cafés When All Balances Are Zero

**Given** Cafés exist,  
**And** all Cafés have zero outstanding balances,  
**When** the settlement list loads successfully,  
**Then** the system shall display the Café records,  
**And** display SAR 0.00 for their outstanding amounts,  
**And** shall not display `No cafés found`.

### AC-26 — Handle Initial Loading Failure

**Given** the Platform Admin opens the Settlements page,  
**When** settlement data cannot be loaded,  
**Then** the system shall display:

`Unable to load settlements. Please try again.`

**And** provide Retry.

### AC-27 — Do Not Display Unverified Amounts

**Given** settlement data cannot be loaded,  
**When** the loading failure state is displayed,  
**Then** the system shall not display unverified summary card or table values.

### AC-28 — Retry Successful Loading

**Given** the settlement loading error is displayed,  
**When** the Platform Admin selects Retry,  
**And** the loading attempt succeeds,  
**Then** the system shall display the latest retrieved settlement data.

### AC-29 — Retry Loading Failure

**Given** the settlement loading error is displayed,  
**When** the Platform Admin selects Retry,  
**And** the loading attempt fails,  
**Then** the system shall continue displaying the confirmed loading error state,  
**And** provide Retry.

### AC-30 — Navigate to View One Settlement

**Given** the Café Receivables table is displayed,  
**When** the Platform Admin selects View Details for a Café,  
**Then** the system shall navigate to View One Settlement for the selected Café.

### AC-31 — Preserve Selected Café Identity

**Given** the Platform Admin selects View Details for Café A,  
**When** the system navigates to View One Settlement,  
**Then** the selected Café context shall identify Café A.

### AC-32 — Deny Unauthorized UI Access

**Given** a user does not have permission to view settlements,  
**When** the user attempts to access the Settlements page through the user interface,  
**Then** the system shall deny access.

### AC-33 — Deny Unauthorized Direct URL Access

**Given** a user does not have permission to view settlements,  
**When** the user attempts to open the Settlements page through a direct URL,  
**Then** the system shall deny access,  
**And** shall not disclose settlement information.

### AC-34 — Deny Unauthorized Direct Request Access

**Given** a user does not have permission to view settlements,  
**When** the user attempts to retrieve settlement information through a direct request,  
**Then** the system shall deny access,  
**And** shall not disclose settlement information.

### AC-35 — Exclude Receivables After Full Settlement

**Given** a Café has outstanding receivables,  
**And** a successful settlement covering the Café's entire current outstanding balance has been recorded,  
**When** the Settlements page retrieves the latest saved data,  
**Then** the settled receivables shall be excluded from outstanding amounts.

### AC-36 — Display Zero Balance After Full Settlement

**Given** a Café's entire outstanding balance has been successfully settled,  
**And** no new receivables have been created,  
**When** the Settlements page is refreshed successfully,  
**Then** the Café shall remain visible,  
**And** its Total Outstanding shall display SAR 0.00.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Authorized Platform Admin opens Settlement Management | Settlements page displayed |
| EX-02 | Settlements page loads successfully | Three summary cards displayed |
| EX-03 | Existing Cafés are available | Café Receivables table displayed |
| EX-04 | Café has Coffee and Delivery Receivables | Corresponding amounts displayed |
| EX-05 | Two Cafés share the same owner | Two separate Café records displayed |
| EX-06 | Café is suspended | Café remains visible |
| EX-07 | Café has zero outstanding balance | Café remains visible with SAR 0.00 |
| EX-08 | Successful in-café redemption creates SAR 20 Coffee Receivable | Coffee Amount increases by SAR 20 while unsettled |
| EX-09 | Successful in-café redemption | No Delivery Receivable created |
| EX-10 | Delivered outcome creates SAR 20 Coffee and SAR 5 Delivery Receivables | Both amounts included while unsettled |
| EX-11 | Not Responding creates SAR 20 Coffee and SAR 5 Delivery Receivables | Both amounts included while unsettled |
| EX-12 | Delivery order created | No receivables created solely by order creation |
| EX-13 | Delivery order status becomes Preparing | No receivables created solely by this status |
| EX-14 | Delivery order status becomes Out for Delivery | No receivables created solely by this status |
| EX-15 | Receivable successfully settled | Receivable excluded from outstanding amounts |
| EX-16 | Café Coffee Amount SAR 350.00 and Delivery Amount SAR 150.00 | Total Outstanding SAR 500.00 |
| EX-17 | Café B Total SAR 1,000.00, Café A SAR 500.00, Café C SAR 0.00 | Table order: Café B, Café A, Café C |
| EX-18 | Coffee totals SAR 1,050.00 and Delivery totals SAR 450.00 | Total Outstanding SAR 1,500.00 |
| EX-19 | Café table contains Coffee Amounts of SAR 350.00 and SAR 700.00 | Outstanding Coffee summary SAR 1,050.00 |
| EX-20 | Café table contains Delivery Amounts of SAR 150.00 and SAR 300.00 | Outstanding Delivery summary SAR 450.00 |
| EX-21 | Monetary value is zero | Display SAR 0.00 |
| EX-22 | Monetary value is SAR 500 | Display SAR 500.00 |
| EX-23 | Platform Admin opens Settlements after new receivable saved | Latest saved amount displayed |
| EX-24 | New receivable saved while page remains open | Automatic live update not required |
| EX-25 | Platform Admin selects Refresh after new receivable saved | Updated outstanding amount displayed |
| EX-26 | No Cafés exist | `No cafés found` displayed |
| EX-27 | Cafés exist and all outstanding amounts are zero | Café records displayed with SAR 0.00 |
| EX-28 | Initial settlement data loading fails | Exact loading error displayed |
| EX-29 | Settlement data loading fails | Retry action displayed |
| EX-30 | Settlement data loading fails | Unverified financial amounts not displayed |
| EX-31 | Platform Admin selects Retry and loading succeeds | Settlement data displayed |
| EX-32 | Platform Admin selects Retry and loading fails again | Error state remains displayed |
| EX-33 | Platform Admin selects View Details for Café A | View One Settlement opens for Café A |
| EX-34 | Platform Admin selects View Details for Café B | View One Settlement opens for Café B |
| EX-35 | User without permission attempts UI access | Access denied |
| EX-36 | User without permission attempts direct URL access | Access denied |
| EX-37 | User without permission attempts direct request | Access denied; no settlement data disclosed |
| EX-38 | Café outstanding balance SAR 500.00 successfully settled in full | Previously outstanding receivables excluded |
| EX-39 | Café fully settled with no new receivables | Total Outstanding SAR 0.00 after refresh |
| EX-40 | Café is suspended with SAR 300.00 outstanding | Café displayed; SAR 300.00 included in totals |

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| `attachments/image-20261010-150929.png` | Attachment referenced in the CP-102 Jira story |
| View Settlements Wireframe | Illustrates the three summary cards, Café Receivables table, Café Name, Coffee Amount, Delivery Amount, Total Outstanding, and View Details |

**Wireframe Alignment:**

1. The three summary cards are included in the confirmed specification.
2. The Café Receivables table shall include all confirmed columns.
3. A Refresh action shall be added to the wireframe.
4. The example Café rows shall be sorted by Total Outstanding descending.
5. Loading failure, Retry, and No Cafés Found states shall be reflected in the wireframe or accompanying interaction notes.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-50 | Admin Platform | Parent Feature |
| CP-101 | Settlement Management | Settlement Management Feature |
| CP-102 | View Settlement | Current User Story |
| ID not provided | View One Settlement | Destination of View Details |
| ID not provided | Receivable Creation Requirements | Define eligible Coffee and Delivery Receivables |
| ID not provided | Settlement Execution | Defines successful full settlement and settlement recording |
| CP-52 | RBAC | Related permission-management functionality |

### 9.3 Dependencies

1. **Platform Admin Access:** The system must identify users authorized to view settlements.
2. **Café Records:** The system requires existing Café records, including suspended and zero-balance Cafés.
3. **Independent Café Identity:** Each Café must be identifiable independently, even when Cafés share the same brand or owner.
4. **Coffee Receivables:** The system requires successfully created Coffee Receivables associated with each Café.
5. **Delivery Receivables:** The system requires successfully created Delivery Receivables associated with each Café.
6. **Redemption and Delivery Outcomes:** Receivable creation depends on the confirmed successful in-café, Delivered, and Not Responding outcomes.
7. **Settlement Records:** The system must identify successfully settled receivables to exclude them from outstanding amounts.
8. **Full Settlement Rule:** Settlement execution must follow the confirmed requirement that each settlement covers the Café's entire current outstanding balance.
9. **Finance Rounding Rule:** Finance must confirm the calculation rounding rule before financial calculation implementation is finalized.
10. **View One Settlement:** The destination functionality must receive the identity of the Café selected through View Details.
11. **Permission Enforcement:** The system must enforce settlement-view access restrictions through the UI, direct URLs, and direct requests.
12. **Latest Saved Data:** The system requires access to the latest saved receivable and settlement records when the page is loaded or refreshed.

## 10. Out of Scope

The following activities are not included in CP-102:

1. Creating, editing, or deleting Café records.
2. Creating or editing Coffee Receivables through the View Settlements screen.
3. Creating or editing Delivery Receivables through the View Settlements screen.
4. Executing Café settlements.
5. Initiating or processing payments to Cafés.
6. Supporting partial settlement transactions.
7. Defining settlement approval workflows.
8. Displaying the detailed settlement records for an individual Café — covered by View One Settlement.
9. Editing financial rates or Café receivable configurations.
10. Defining the exact calculation rounding method before Finance confirmation.
11. Automatically updating settlement values while the page remains open.
12. Defining a tie-break sorting rule for Cafés with equal Total Outstanding values.
13. Defining the exact permission key required to view settlements.
14. Defining the exact unauthorized-access error message.
15. Defining settlement execution behavior when outstanding balances change during processing.
16. Defining APIs, database schemas, authorization implementation details, or system architecture.

---
