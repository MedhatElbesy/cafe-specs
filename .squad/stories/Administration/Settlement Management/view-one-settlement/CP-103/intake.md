# Functional Specification — CP-103: View One Settlement

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-103 |
| Story Title | View One Settlement |
| Epic | Administration (CP-49) |
| Feature | Admin Platform (CP-50) → Settlement Management (CP-101) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Medhat Elbesy |
| Labels | MVP, Sprint1 |
| Primary Actor | Platform Admin |
| Related Story | CP-102 — View Settlement |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** view a Café's current outstanding amount and its breakdown,  
**So that** I can review the full amount owed before recording its settlement.

## 3. Trigger

Platform Dashboard → Settlement Management → View Details

The Platform Admin selects **View Details** for a Café from the Settlement Management list.

The system opens View One Settlement for the selected Café and retrieves its latest saved outstanding Coffee and Delivery Receivables.

## 4. Preconditions

1. The Platform Admin is authenticated.
2. The Platform Admin has permission to view settlements.
3. A Café has been selected from the Settlement Management list.
4. The system can identify the selected Café independently of other Cafés.
5. The system has access to saved Coffee and Delivery Receivables.
6. The system has access to saved settlement records required to exclude previously settled receivables.
7. The system has access to the rates recorded for the relevant receivable transactions.

**Permission Consideration:**

View Settlements and Update Settlement are separate permissions.

The Platform Admin must have View Settlements permission to access this page.

Update Settlement permission is required only to enable the Update Settlement action, subject to the outstanding balance.

**Financial Consideration:**

Only successfully created and unsettled receivables contribute to the outstanding amounts.

Previously settled receivables are excluded.

## 5. Fields and Validation

### 5.1 View One Settlement Screen

| Element | Type | Required Behavior |
|---|---|---|
| Café Name | Read-only text | Displays the selected Café's name |
| Coffee Receivables | Financial section | Displays outstanding Coffee Receivables and rate breakdown |
| Delivery Receivables | Financial section | Displays outstanding Delivery Receivables and rate breakdown |
| Total Outstanding Amount | Read-only monetary value | Displays Coffee Receivables + Delivery Receivables |
| Refresh | Action | Reloads the latest saved financial data |
| Update Settlement | Action | Opens the Settlement Recording Form when permitted |
| Back | Navigation action | Returns to the Settlement Management list |
| Retry | Error-state action | Retries loading settlement details after failure |

**Screen Behavior:**

1. The system shall display settlement details for the selected Café only.
2. The system shall display the Café Name.
3. The system shall display the outstanding Coffee Receivables.
4. The system shall display the outstanding Delivery Receivables.
5. The system shall display the Total Outstanding Amount.
6. Coffee and Delivery Receivables shall include breakdowns grouped by their recorded Unit Rates.
7. The system shall exclude previously settled receivables.
8. All monetary values shall be displayed in SAR with two decimal places.
9. The system shall provide Refresh.
10. The system shall provide Update Settlement and Back actions.
11. The system shall enforce the confirmed Update Settlement availability conditions.
12. Suspended Cafés shall remain accessible for settlement review.
13. The system shall display the confirmed zero, not-found, or loading-error states when applicable.

### 5.2 Café Name

| Attribute | Specification |
|---|---|
| Field | Café Name |
| Type | Read-only text |
| Source | Selected Café record |
| Required | Yes |
| Editable | No |
| Entity Scope | Individual Café |
| Suspended Café | Remains accessible |

**Café Name Behavior:**

1. The system shall display the name of the Café selected through View Details.
2. The displayed Café shall correspond to the selected Café record.
3. Each Café shall be treated as an independent entity.
4. Cafés sharing the same owner or brand shall not have their settlement details combined.
5. The system shall not display outstanding amounts belonging to another Café.
6. A suspended Café shall remain accessible because suspension does not remove its financial entitlements.

### 5.3 Coffee Receivables Section

| Attribute | Specification |
|---|---|
| Section | Coffee Receivables |
| Type | Read-only financial breakdown |
| Data Scope | Selected Café |
| Quantity | Total Unpaid Cups |
| Unit Rate | Rate recorded for each receivable transaction |
| Grouping | Transactions grouped by identical Unit Rate |
| Group Amount | Group Quantity × Recorded Unit Rate |
| Total | Sum of outstanding Coffee Receivable groups |
| Settled Receivables | Excluded |
| Currency | SAR |
| Display Precision | Two decimal places |

**Coffee Receivables Behavior:**

1. The system shall retrieve successfully created and unsettled Coffee Receivables for the selected Café.
2. The system shall use the rate recorded for each transaction.
3. The system shall not recalculate historical receivables using the Café's current configured rate.
4. Transactions with the same recorded Coffee Unit Rate shall be grouped into one row.
5. Each rate group shall display the total unpaid quantity, Unit Rate, and corresponding amount.
6. The amount for each rate group shall represent the outstanding amount attributable to the transactions in that group.
7. The section shall display the total number of unpaid cups.
8. The total Coffee Receivables amount shall equal the sum of the Coffee Rate Group amounts.
9. Previously settled Coffee Receivables shall be excluded.
10. Successful In-Café redemption shall contribute Coffee Receivables only.
11. Delivery orders finalized as Delivered or Not Responding shall contribute Coffee Receivables when those receivables are successfully created and remain unsettled.
12. All displayed monetary amounts shall use SAR with two decimal places.
13. The exact calculation rounding rule remains pending Finance confirmation.

### 5.4 Coffee Rate Breakdown

| Column | Type | Required Behavior |
|---|---|---|
| Unpaid Cups | Read-only number | Displays total unpaid cups within the rate group |
| Unit Rate | Read-only monetary value | Displays the recorded Coffee Rate shared by the group |
| Amount | Read-only monetary value | Displays the outstanding amount for the group |

**Rate Grouping Behavior:**

1. The system shall group unpaid Coffee Receivables by identical recorded Unit Rates.
2. Each Unit Rate shall appear in one group row.
3. The quantity displayed for a group shall equal the total unpaid cups associated with that rate.
4. The group amount shall be calculated from the group's quantity and recorded rate, subject to the Finance-approved rounding rule.
5. The total Coffee Receivables amount shall reconcile with the sum of the displayed group amounts.
6. Where multiple recorded rates exist, the system shall display multiple rate groups.
7. The system shall not present a single Unit Rate as though it applied to all transactions when different rates exist.

### 5.5 Delivery Receivables Section

| Attribute | Specification |
|---|---|
| Section | Delivery Receivables |
| Type | Read-only financial breakdown |
| Data Scope | Selected Café |
| Quantity | Total Unpaid Deliveries |
| Unit Rate | Rate recorded for each receivable transaction |
| Grouping | Transactions grouped by identical Unit Rate |
| Group Amount | Group Quantity × Recorded Unit Rate |
| Total | Sum of outstanding Delivery Receivable groups |
| Settled Receivables | Excluded |
| Currency | SAR |
| Display Precision | Two decimal places |

**Delivery Receivables Behavior:**

1. The system shall retrieve successfully created and unsettled Delivery Receivables for the selected Café.
2. The system shall use the Delivery Rate recorded for each transaction.
3. The system shall not recalculate historical Delivery Receivables using the Café's current configured rate.
4. Transactions with the same recorded Delivery Unit Rate shall be grouped into one row.
5. Each rate group shall display the total unpaid delivery quantity, Unit Rate, and corresponding amount.
6. The section shall display the total number of unpaid deliveries.
7. The total Delivery Receivables amount shall equal the sum of the Delivery Rate Group amounts.
8. Previously settled Delivery Receivables shall be excluded.
9. Delivery orders finalized as Delivered or Not Responding shall contribute Delivery Receivables when successfully created and unsettled.
10. Creating a delivery order or changing its status to Preparing or Out for Delivery shall not, by itself, create Delivery Receivables.
11. All monetary amounts shall be displayed in SAR with two decimal places.
12. The exact calculation rounding rule remains pending Finance confirmation.

### 5.6 Delivery Rate Breakdown

| Column | Type | Required Behavior |
|---|---|---|
| Unpaid Deliveries | Read-only number | Displays total unpaid deliveries within the rate group |
| Unit Rate | Read-only monetary value | Displays the recorded Delivery Rate shared by the group |
| Amount | Read-only monetary value | Displays the outstanding amount for the group |

**Rate Grouping Behavior:**

1. The system shall group unpaid Delivery Receivables by identical recorded Unit Rates.
2. Each Unit Rate shall appear in one group row.
3. The quantity displayed for a group shall equal the total unpaid deliveries associated with that rate.
4. The group amount shall be calculated from the group's quantity and recorded rate, subject to the Finance-approved rounding rule.
5. The total Delivery Receivables amount shall reconcile with the sum of the displayed group amounts.
6. Where multiple recorded rates exist, the system shall display multiple rate groups.
7. The system shall not present a single Unit Rate as though it applied to all transactions when different rates exist.

### 5.7 Total Outstanding Amount

| Attribute | Specification |
|---|---|
| Field | Total Outstanding Amount |
| Type | Read-only monetary value |
| Calculation | Coffee Receivables + Delivery Receivables |
| Scope | Selected Café |
| Previously Settled Amounts | Excluded |
| Currency | SAR |
| Display Precision | Two decimal places |
| Editable | No |

**Total Outstanding Behavior:**

1. The system shall calculate Total Outstanding Amount for the selected Café only.
2. Total Outstanding Amount shall equal Coffee Receivables plus Delivery Receivables.
3. The Coffee and Delivery components shall be based on successfully created and unsettled receivables.
4. Previously settled receivables shall be excluded.
5. The displayed Total Outstanding Amount shall reconcile with the Coffee and Delivery breakdown totals.
6. The system shall display the amount in SAR with two decimal places.
7. The amount shall determine whether the Update Settlement action is eligible to be enabled, together with the Platform Admin's Update Settlement permission.
8. A zero Total Outstanding Amount shall be displayed as SAR 0.00.

### 5.8 Update Settlement Action

| Attribute | Specification |
|---|---|
| Action | Update Settlement |
| Type | Navigation action |
| Destination | Settlement Recording Form |
| Selected Context | Selected Café |
| Required View Permission | View Settlements |
| Required Action Permission | Update Settlement |
| Outstanding Condition | Total Outstanding Amount > 0 |
| Disabled When | Outstanding Amount = 0 |
| Disabled When | Admin lacks Update Settlement permission |
| Suspended Café | Action may remain enabled when all conditions are satisfied |
| Settlement Execution | Outside CP-103 scope |

**Update Settlement Behavior:**

1. The system shall display Update Settlement on the View One Settlement page.
2. The action shall be enabled only when the selected Café has a Total Outstanding Amount greater than zero and the Platform Admin has Update Settlement permission.
3. The action shall be disabled when Total Outstanding Amount equals zero.
4. The action shall be disabled when the Platform Admin lacks Update Settlement permission.
5. Café suspension alone shall not disable the action.
6. Selecting an enabled Update Settlement action shall open the Settlement Recording Form for the selected Café.
7. The selected Café's identity shall be preserved during navigation.
8. The system shall revalidate Update Settlement permission when opening the Settlement Recording Form.
9. The Settlement Recording Form shall revalidate the latest outstanding amount before recording settlement.
10. The actual settlement recording, financial revalidation workflow, and transaction processing belong to the separate Settlement Execution functionality.

### 5.9 Refresh Action

| Attribute | Specification |
|---|---|
| Action | Refresh |
| Type | Page action |
| Purpose | Reload latest saved settlement details |
| Affected Data | Coffee Receivables, Delivery Receivables, Total Outstanding Amount |
| Automatic Live Updates | Not required by CP-103 |
| Failure Behavior | Loading error and Retry |

**Refresh Behavior:**

1. The system shall provide a Refresh action.
2. Selecting Refresh shall retrieve the latest saved receivables and settlement records for the selected Café.
3. Following successful Refresh, the system shall update the Coffee Receivables breakdown.
4. Following successful Refresh, the system shall update the Delivery Receivables breakdown.
5. Following successful Refresh, the system shall update the Total Outstanding Amount.
6. The system shall exclude receivables successfully settled before the refreshed data is retrieved.
7. The Update Settlement action shall reflect the refreshed outstanding amount and applicable permission conditions.
8. If Refresh fails, the system shall follow the confirmed loading failure behavior.

### 5.10 Back to List Action

| Attribute | Specification |
|---|---|
| Action | Back |
| Type | Navigation action |
| Destination | CP-102 — Settlement Management List |
| Previous Search | Preserved |
| Previous Filters | Preserved |
| Previous Page | Preserved |

**Back Navigation Behavior:**

1. The system shall provide a Back action.
2. Selecting Back shall return the Platform Admin to the Settlement Management list.
3. The system shall restore the previously used Search state, where applicable.
4. The system shall restore the previously applied Filters, where applicable.
5. The system shall restore the previously selected Page.
6. The behavior shall remain consistent with navigation from CP-102 to CP-103.

### 5.11 Zero Outstanding State

| Attribute | Specification |
|---|---|
| Condition | Selected Café exists with no outstanding receivables |
| Café Name | Displayed |
| Unpaid Cups | 0 |
| Unpaid Deliveries | 0 |
| Coffee Receivables | SAR 0.00 |
| Delivery Receivables | SAR 0.00 |
| Total Outstanding | SAR 0.00 |
| Update Settlement | Disabled |
| Back | Available |

**Zero Outstanding Behavior:**

1. The system shall continue displaying the selected Café when its outstanding amount is zero.
2. The system shall display the Café Name.
3. The system shall display Unpaid Cups as 0.
4. The system shall display Unpaid Deliveries as 0.
5. Coffee Receivables shall display SAR 0.00.
6. Delivery Receivables shall display SAR 0.00.
7. Total Outstanding Amount shall display SAR 0.00.
8. Update Settlement shall be disabled.
9. Back shall remain available.

### 5.12 No Cafés Found

| Attribute | Specification |
|---|---|
| Condition | No Cafés exist |
| Exact Message | `No cafés found` |
| Distinct From | Selected Café Not Found |

**No Cafés Found Behavior:**

1. When no Café records exist, the system shall display:

   `No cafés found`

2. The system shall not use this message merely because the selected Café has no outstanding receivables.
3. The system shall distinguish this condition from a selected Café record that cannot be found.

### 5.13 Selected Café Not Found

| Attribute | Specification |
|---|---|
| Condition | Selected Café does not exist |
| Exact Message | `Café not found.` |
| Action | Back to List |
| Financial Details | Not displayed |

**Selected Café Not Found Behavior:**

1. If the selected Café cannot be found because its record does not exist, the system shall display:

   `Café not found.`

2. The system shall provide Back to List.
3. Selecting Back to List shall return the Platform Admin to Settlement Management.
4. The system shall not display settlement amounts for another Café in place of the missing selected Café.

### 5.14 Loading Failure and Retry

| Attribute | Specification |
|---|---|
| Condition | Settlement details cannot be loaded |
| Exact Message | `Unable to load settlement details. Please try again.` |
| Action | Retry |
| Unverified Financial Data | Not displayed |
| Retry Result | New loading attempt |

**Loading Failure Behavior:**

1. If settlement details cannot be loaded, the system shall display:

   `Unable to load settlement details. Please try again.`

2. The system shall provide Retry.
3. The system shall not display unverified financial amounts.
4. Selecting Retry shall initiate another attempt to retrieve the selected Café's settlement details.
5. If Retry succeeds, the system shall display the retrieved financial details.
6. If Retry fails, the system shall continue displaying the confirmed loading error state.
7. A loading failure shall not be treated as a zero-outstanding condition.

### 5.15 Permission Enforcement

| Attribute | Specification |
|---|---|
| Protected Page | View One Settlement |
| Required Permission | View Settlements |
| Protected Action | Update Settlement |
| Required Action Permission | Update Settlement |
| UI Access | Permission enforced |
| Direct URL Access | Permission enforced |
| Direct Request / API Access | Permission enforced |
| Unauthorized Data Disclosure | Not allowed |
| Exact Permission Keys | Not specified |

**Permission Behavior:**

1. The system shall require View Settlements permission to access CP-103.
2. Users without View Settlements permission shall not access settlement details through the user interface.
3. Direct URLs shall not bypass View Settlements permission.
4. Direct requests shall not bypass View Settlements permission.
5. Unauthorized requests shall not disclose financial details.
6. Update Settlement shall require a separate Update Settlement permission.
7. A Platform Admin with View Settlements permission but without Update Settlement permission may view the page, but Update Settlement shall be disabled.
8. The system shall revalidate Update Settlement permission when opening the Settlement Recording Form.
9. Exact permission keys and unauthorized-access messages are not specified by the confirmed requirements.

## 6. Business Rules

**BR-01 — Selected Café Scope**

The system shall display settlement details for the selected Café only.

**BR-02 — Independent Café Entity**

Each Café shall be treated as an independent entity, even when Cafés share an owner or brand.

**BR-03 — Suspended Café Entitlements**

Suspended Cafés shall remain accessible because suspension does not remove financial entitlements.

**BR-04 — Eligible Receivables**

Only successfully created and unsettled receivables shall contribute to the outstanding amounts.

**BR-05 — Previously Settled Receivables**

Previously settled receivables shall be excluded from all outstanding amounts.

**BR-06 — Transaction-Specific Rates**

Receivable calculations shall use the Unit Rate recorded for each transaction.

**BR-07 — Historical Rate Preservation**

A change to a Café's configured rate shall not cause existing unsettled receivables to be recalculated using the new rate.

**BR-08 — Coffee Rate Grouping**

Unsettled Coffee Receivables with identical recorded Unit Rates shall be grouped together.

**BR-09 — Delivery Rate Grouping**

Unsettled Delivery Receivables with identical recorded Unit Rates shall be grouped together.

**BR-10 — Coffee Group Amount**

Each Coffee Rate Group shall display its unpaid quantity, recorded Unit Rate, and corresponding amount.

**BR-11 — Delivery Group Amount**

Each Delivery Rate Group shall display its unpaid quantity, recorded Unit Rate, and corresponding amount.

**BR-12 — Coffee Receivables Reconciliation**

Total Coffee Receivables shall reconcile with the sum of Coffee Rate Group amounts.

**BR-13 — Delivery Receivables Reconciliation**

Total Delivery Receivables shall reconcile with the sum of Delivery Rate Group amounts.

**BR-14 — Total Outstanding Calculation**

Total Outstanding Amount shall equal Coffee Receivables plus Delivery Receivables.

**BR-15 — In-Café Redemption Receivables**

Successful In-Café redemption shall create Coffee Receivables only.

**BR-16 — Delivered Receivables**

A delivery order finalized as Delivered shall create Coffee and Delivery Receivables.

**BR-17 — Not Responding Receivables**

A delivery order finalized as Not Responding shall create Coffee and Delivery Receivables.

**BR-18 — Non-Receivable Delivery States**

Creating a delivery order or changing its status to Preparing or Out for Delivery shall not, by itself, create receivables.

**BR-19 — Full Settlement**

Each settlement shall cover the entire current outstanding balance. Partial settlements shall not be supported.

**BR-20 — Currency**

All monetary amounts shall be displayed in SAR.

**BR-21 — Display Precision**

All monetary amounts shall be displayed with two decimal places.

**BR-22 — Finance Rounding Dependency**

The exact calculation rounding rule shall be confirmed by Finance before financial calculation implementation is finalized.

**BR-23 — Latest Saved Data**

The system shall retrieve the latest saved receivables and settlements when CP-103 is opened or manually refreshed.

**BR-24 — Manual Refresh**

The system shall provide a Refresh action to reload the selected Café's financial details.

**BR-25 — View Settlements Permission**

The Platform Admin shall have View Settlements permission to access CP-103.

**BR-26 — Update Settlement Permission**

Update Settlement shall require a separate Update Settlement permission.

**BR-27 — Update Settlement Availability**

Update Settlement shall be enabled only when Total Outstanding Amount is greater than zero and the Platform Admin has Update Settlement permission.

**BR-28 — Zero Outstanding Restriction**

Update Settlement shall be disabled when Total Outstanding Amount equals zero.

**BR-29 — Missing Update Permission**

Update Settlement shall be disabled when the Platform Admin lacks Update Settlement permission.

**BR-30 — Suspended Café Update Eligibility**

Suspension alone shall not prevent Update Settlement when the outstanding amount and permission conditions are satisfied.

**BR-31 — Update Settlement Navigation**

Selecting an enabled Update Settlement action shall open the Settlement Recording Form for the selected Café.

**BR-32 — Update Permission Revalidation**

Update Settlement permission shall be revalidated when the Settlement Recording Form is opened.

**BR-33 — Financial Revalidation Dependency**

The Settlement Recording Form shall revalidate the latest outstanding amount before recording settlement. The execution behavior belongs to the separate Settlement Execution functionality.

**BR-34 — Back Navigation**

Back shall return the Platform Admin to the Settlement Management list.

**BR-35 — List State Preservation**

Back navigation shall preserve the previous Search, Filters, and Page state.

**BR-36 — Zero Outstanding Display**

A Café with no outstanding receivables shall display zero quantities and SAR 0.00 amounts.

**BR-37 — No Cafés Found**

When no Cafés exist, the system shall display `No cafés found`.

**BR-38 — Selected Café Not Found**

When the selected Café does not exist, the system shall display `Café not found.` and provide Back to List.

**BR-39 — Loading Failure**

When settlement details cannot be loaded, the system shall display `Unable to load settlement details. Please try again.`

**BR-40 — Retry**

The loading failure state shall provide Retry.

**BR-41 — Unverified Financial Data**

The system shall not display unverified financial amounts following a loading failure.

**BR-42 — Unauthorized Access**

Users without View Settlements permission shall not access settlement details through the UI, direct URL, or direct requests.

**BR-43 — Current Outstanding Only**

CP-103 shall display current unpaid receivables only.

**BR-44 — Settlement History Exclusion**

CP-103 shall not display Settlement History or previously settled transactions.

## 7. Acceptance Criteria

### AC-01 — Open View One Settlement

**Given** the Platform Admin is authenticated,  
**And** has View Settlements permission,  
**When** the Platform Admin selects View Details for a Café in CP-102,  
**Then** the system shall open View One Settlement for the selected Café.

### AC-02 — Display Selected Café Name

**Given** the selected Café exists,  
**When** CP-103 loads successfully,  
**Then** the system shall display the selected Café's name.

### AC-03 — Maintain Independent Café Scope

**Given** two Cafés share the same owner or brand,  
**When** the Platform Admin opens View One Settlement for one Café,  
**Then** the system shall display financial details for the selected Café only.

### AC-04 — Display Coffee Receivables

**Given** the selected Café has unsettled Coffee Receivables,  
**When** CP-103 loads successfully,  
**Then** the system shall display the outstanding Coffee Receivables amount and its rate breakdown.

### AC-05 — Display Delivery Receivables

**Given** the selected Café has unsettled Delivery Receivables,  
**When** CP-103 loads successfully,  
**Then** the system shall display the outstanding Delivery Receivables amount and its rate breakdown.

### AC-06 — Use Recorded Transaction Rates

**Given** unsettled receivables were created using recorded transaction rates,  
**When** CP-103 calculates outstanding amounts,  
**Then** the system shall use those recorded rates rather than the Café's current configured rates.

### AC-07 — Group Coffee Receivables by Rate

**Given** multiple unsettled Coffee Receivables share the same Unit Rate,  
**When** the Coffee Receivables breakdown is displayed,  
**Then** those receivables shall appear in one rate group,  
**And** the group shall display total quantity, Unit Rate, and Amount.

### AC-08 — Display Multiple Coffee Rate Groups

**Given** unsettled Coffee Receivables have different recorded Unit Rates,  
**When** the Coffee Receivables breakdown is displayed,  
**Then** the system shall display separate groups for the different rates.

### AC-09 — Group Delivery Receivables by Rate

**Given** multiple unsettled Delivery Receivables share the same Unit Rate,  
**When** the Delivery Receivables breakdown is displayed,  
**Then** those receivables shall appear in one rate group,  
**And** the group shall display total quantity, Unit Rate, and Amount.

### AC-10 — Display Multiple Delivery Rate Groups

**Given** unsettled Delivery Receivables have different recorded Unit Rates,  
**When** the Delivery Receivables breakdown is displayed,  
**Then** the system shall display separate groups for the different rates.

### AC-11 — Reconcile Coffee Receivables

**Given** the Coffee Receivables breakdown contains one or more rate groups,  
**When** the system calculates total Coffee Receivables,  
**Then** the total shall reconcile with the sum of the Coffee Rate Group amounts.

### AC-12 — Reconcile Delivery Receivables

**Given** the Delivery Receivables breakdown contains one or more rate groups,  
**When** the system calculates total Delivery Receivables,  
**Then** the total shall reconcile with the sum of the Delivery Rate Group amounts.

### AC-13 — Calculate Total Outstanding

**Given** the selected Café has outstanding Coffee and Delivery Receivables,  
**When** the system calculates Total Outstanding Amount,  
**Then** Total Outstanding Amount shall equal Coffee Receivables plus Delivery Receivables.

### AC-14 — Exclude Previously Settled Receivables

**Given** the selected Café has previously settled receivables,  
**When** CP-103 retrieves its current outstanding amounts,  
**Then** the settled receivables shall be excluded.

### AC-15 — Include In-Café Coffee Receivables

**Given** a successful In-Café redemption created an unsettled Coffee Receivable,  
**When** CP-103 calculates Coffee Receivables,  
**Then** that receivable shall be included,  
**And** no Delivery Receivable shall be created by that redemption.

### AC-16 — Include Delivered Receivables

**Given** a Delivery order was finalized as Delivered,  
**And** its Coffee and Delivery Receivables were successfully created and remain unsettled,  
**When** CP-103 calculates outstanding amounts,  
**Then** both receivables shall be included.

### AC-17 — Include Not Responding Receivables

**Given** a Delivery order was finalized as Not Responding,  
**And** its Coffee and Delivery Receivables were successfully created and remain unsettled,  
**When** CP-103 calculates outstanding amounts,  
**Then** both receivables shall be included.

### AC-18 — Exclude Non-Receivable Delivery States

**Given** a Delivery order has only been created or has reached Preparing or Out for Delivery,  
**When** CP-103 calculates outstanding amounts,  
**Then** no receivables shall be included solely because of those states.

### AC-19 — Display SAR Currency

**Given** monetary amounts are displayed,  
**When** the Platform Admin views CP-103,  
**Then** all amounts shall be displayed in SAR.

### AC-20 — Display Two Decimal Places

**Given** monetary amounts are available,  
**When** the system displays those amounts,  
**Then** each amount shall use two decimal places.

### AC-21 — Display Suspended Café Details

**Given** the selected Café is suspended,  
**When** the Platform Admin opens CP-103,  
**Then** the system shall display the Café's current outstanding receivables.

### AC-22 — Enable Update Settlement

**Given** the selected Café has Total Outstanding Amount greater than zero,  
**And** the Platform Admin has Update Settlement permission,  
**When** CP-103 loads successfully,  
**Then** Update Settlement shall be enabled.

### AC-23 — Disable Update Settlement for Zero Balance

**Given** the selected Café has Total Outstanding Amount equal to zero,  
**When** CP-103 loads successfully,  
**Then** Update Settlement shall be disabled.

### AC-24 — Disable Update Settlement Without Permission

**Given** the Platform Admin has View Settlements permission,  
**But** does not have Update Settlement permission,  
**When** CP-103 loads successfully,  
**Then** the Platform Admin shall be able to view the details,  
**And** Update Settlement shall be disabled.

### AC-25 — Allow Update Settlement for Suspended Café

**Given** the selected Café is suspended,  
**And** its Total Outstanding Amount is greater than zero,  
**And** the Platform Admin has Update Settlement permission,  
**When** CP-103 loads successfully,  
**Then** Update Settlement shall be enabled.

### AC-26 — Navigate to Settlement Recording Form

**Given** Update Settlement is enabled,  
**When** the Platform Admin selects Update Settlement,  
**Then** the system shall open the Settlement Recording Form for the selected Café.

### AC-27 — Revalidate Update Permission

**Given** the Platform Admin selects Update Settlement,  
**When** the Settlement Recording Form is opened,  
**Then** the system shall revalidate Update Settlement permission before allowing the update workflow to proceed.

### AC-28 — Load Latest Saved Details

**Given** saved receivables and settlement records exist,  
**When** the Platform Admin opens CP-103,  
**Then** the system shall retrieve the latest saved outstanding amounts for the selected Café.

### AC-29 — Refresh Settlement Details

**Given** the Platform Admin is viewing CP-103,  
**When** the Platform Admin selects Refresh,  
**Then** the system shall reload the latest saved settlement details,  
**And** update the Coffee Receivables, Delivery Receivables, and Total Outstanding Amount after successful loading.

### AC-30 — Update Action After Refresh

**Given** the selected Café's outstanding amount has changed,  
**When** Refresh succeeds,  
**Then** Update Settlement shall reflect the refreshed amount and applicable permission conditions.

### AC-31 — Back to Settlement Management

**Given** the Platform Admin is viewing CP-103,  
**When** the Platform Admin selects Back,  
**Then** the system shall return to the Settlement Management list.

### AC-32 — Preserve List State

**Given** the Platform Admin opened CP-103 from a Settlement Management list with a Search, Filter, or Page selection,  
**When** the Platform Admin selects Back,  
**Then** the system shall restore the previous list state.

### AC-33 — Display Zero Outstanding Details

**Given** the selected Café exists but has no outstanding receivables,  
**When** CP-103 loads successfully,  
**Then** the system shall display the Café Name,  
**And** Unpaid Cups as 0,  
**And** Unpaid Deliveries as 0,  
**And** Coffee Receivables as SAR 0.00,  
**And** Delivery Receivables as SAR 0.00,  
**And** Total Outstanding Amount as SAR 0.00.

### AC-34 — Display No Cafés Found

**Given** no Cafés exist,  
**When** the system retrieves the Café records,  
**Then** the system shall display:

`No cafés found`

### AC-35 — Handle Selected Café Not Found

**Given** the selected Café record does not exist,  
**When** CP-103 attempts to load its details,  
**Then** the system shall display:

`Café not found.`

**And** provide Back to List.

### AC-36 — Navigate Back From Café Not Found

**Given** the selected Café not-found state is displayed,  
**When** the Platform Admin selects Back to List,  
**Then** the system shall return to Settlement Management.

### AC-37 — Handle Settlement Details Loading Failure

**Given** the Platform Admin opens CP-103,  
**When** settlement details cannot be loaded,  
**Then** the system shall display:

`Unable to load settlement details. Please try again.`

**And** provide Retry.

### AC-38 — Do Not Display Unverified Financial Data

**Given** settlement details cannot be loaded,  
**When** the loading error state is displayed,  
**Then** the system shall not display unverified financial amounts.

### AC-39 — Retry Successful Loading

**Given** the loading error state is displayed,  
**When** the Platform Admin selects Retry,  
**And** the loading attempt succeeds,  
**Then** the system shall display the retrieved settlement details.

### AC-40 — Retry Failed Loading

**Given** the loading error state is displayed,  
**When** the Platform Admin selects Retry,  
**And** the loading attempt fails,  
**Then** the system shall continue displaying the confirmed loading error state.

### AC-41 — Deny Unauthorized UI Access

**Given** a user does not have View Settlements permission,  
**When** the user attempts to open CP-103 through the user interface,  
**Then** the system shall deny access.

### AC-42 — Deny Unauthorized Direct URL Access

**Given** a user does not have View Settlements permission,  
**When** the user attempts to open CP-103 through a direct URL,  
**Then** the system shall deny access,  
**And** shall not disclose settlement information.

### AC-43 — Deny Unauthorized Direct Request Access

**Given** a user does not have View Settlements permission,  
**When** the user attempts to retrieve CP-103 details through a direct request,  
**Then** the system shall deny access,  
**And** shall not disclose settlement information.

### AC-44 — Exclude Settlement History

**Given** the selected Café has previously recorded settlements,  
**When** CP-103 loads,  
**Then** the system shall display current unpaid receivables only,  
**And** shall not display Settlement History or previously settled transactions.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Authorized Platform Admin selects View Details for Café A | CP-103 opens for Café A |
| EX-02 | Café A and Café B share the same owner | Only Café A details displayed when Café A is selected |
| EX-03 | Café A has 10 unpaid cups at SAR 17.50 | Coffee group shows 10 × SAR 17.50 = SAR 175.00 |
| EX-04 | Café A has 5 additional unpaid cups at SAR 20.00 | Separate group shows 5 × SAR 20.00 = SAR 100.00 |
| EX-05 | Café A has both Coffee Rate Groups | Total Unpaid Cups = 15; Coffee Receivables = SAR 275.00 |
| EX-06 | Café A has 4 unpaid deliveries at SAR 5.00 | Delivery group shows 4 × SAR 5.00 = SAR 20.00 |
| EX-07 | Café A has 2 additional unpaid deliveries at SAR 7.00 | Separate group shows 2 × SAR 7.00 = SAR 14.00 |
| EX-08 | Café A has both Delivery Rate Groups | Total Unpaid Deliveries = 6; Delivery Receivables = SAR 34.00 |
| EX-09 | Coffee Receivables SAR 275.00; Delivery Receivables SAR 34.00 | Total Outstanding Amount = SAR 309.00 |
| EX-10 | Previously settled Coffee Receivable exists | Settled receivable excluded |
| EX-11 | Previously settled Delivery Receivable exists | Settled receivable excluded |
| EX-12 | Café configured rate changes after transaction | Existing unsettled transaction retains its recorded rate |
| EX-13 | Successful In-Café redemption | Coffee Receivable included; no Delivery Receivable from that redemption |
| EX-14 | Delivered order creates unsettled Coffee and Delivery Receivables | Both included |
| EX-15 | Not Responding order creates unsettled Coffee and Delivery Receivables | Both included |
| EX-16 | Delivery order is Preparing | No receivables created solely by that status |
| EX-17 | Delivery order is Out for Delivery | No receivables created solely by that status |
| EX-18 | Monetary amount is zero | Display SAR 0.00 |
| EX-19 | Monetary amount is 500 | Display SAR 500.00 |
| EX-20 | Suspended Café has outstanding receivables | Details remain accessible |
| EX-21 | Total Outstanding > 0 and Admin has Update Permission | Update Settlement enabled |
| EX-22 | Total Outstanding = 0 | Update Settlement disabled |
| EX-23 | Admin has View Permission but lacks Update Permission | Details visible; Update Settlement disabled |
| EX-24 | Suspended Café has outstanding > 0 and Admin has Update Permission | Update Settlement enabled |
| EX-25 | Enabled Update Settlement selected | Settlement Recording Form opens for selected Café |
| EX-26 | Update Permission revoked before Form opens | Permission revalidated at Form entry |
| EX-27 | New receivable saved after CP-103 opened | Refresh retrieves latest saved amount |
| EX-28 | Receivable settled after CP-103 opened | Refresh excludes settled receivable |
| EX-29 | Refresh changes Total Outstanding to zero | Update Settlement becomes disabled |
| EX-30 | Admin returns from CP-103 to CP-102 | Previous Search / Filters / Page restored |
| EX-31 | Café exists with zero unpaid cups and deliveries | All quantities zero; all amounts SAR 0.00 |
| EX-32 | No Cafés exist | `No cafés found` |
| EX-33 | Selected Café record does not exist | `Café not found.` |
| EX-34 | Café not-found state; Back to List selected | Returns to Settlement Management |
| EX-35 | Settlement details loading fails | Exact loading error displayed |
| EX-36 | Settlement details loading fails | Retry available |
| EX-37 | Settlement details loading fails | Unverified financial data not displayed |
| EX-38 | Retry succeeds | Selected Café's details displayed |
| EX-39 | Retry fails | Loading error remains displayed |
| EX-40 | User lacks View Settlements permission and attempts UI access | Access denied |
| EX-41 | User lacks View Settlements permission and attempts direct URL access | Access denied |
| EX-42 | User lacks View Settlements permission and attempts direct API request | Access denied; no financial data disclosed |
| EX-43 | Café has previously settled transactions | Settlement History and settled transactions not displayed |
| EX-44 | Café's entire outstanding balance successfully settled, with no new receivables | CP-103 displays SAR 0.00 after successful refresh |

*Example amounts are illustrative and assume transaction amounts reconcile under the Finance-approved rounding rule. They do not define the rounding method.*

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| `attachments/image-20261010-151040.png` | CP-103 Jira attachment — View One Settlement wireframe |
| CP-103 Jira Story | Original View One Settlement requirements |
| Confirmed Decisions D-01 to D-20 | Business clarification decisions used for this specification |

**Wireframe Alignment:**

1. Café Name shall remain displayed.
2. Coffee Receivables shall support multiple recorded Unit Rate groups.
3. Delivery Receivables shall support multiple recorded Unit Rate groups.
4. Total Outstanding Amount shall reconcile with the Coffee and Delivery breakdown totals.
5. Refresh shall be added.
6. Update Settlement shall have enabled and disabled states.
7. Back shall return to the previous Settlement Management list state.
8. Zero Outstanding, Selected Café Not Found, and Loading Failure states shall be represented in the wireframe or accompanying interaction notes.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-50 | Admin Platform | Parent Feature |
| CP-101 | Settlement Management | Settlement Management Feature |
| CP-102 | View Settlement | Source list and View Details navigation |
| CP-103 | View One Settlement | Current User Story |
| CP-52 | RBAC | Related View and Update permission management |
| ID not provided | Settlement Recording / Execution | Destination of Update Settlement |
| ID not provided | Coffee Receivable Creation | Source of outstanding Coffee Receivables |
| ID not provided | Delivery Receivable Creation | Source of outstanding Delivery Receivables |

### 9.3 Dependencies

1. **Selected Café Identity:** CP-102 must pass the identity of the selected Café to CP-103.
2. **Independent Café Records:** Each Café must be identifiable independently, even when Cafés share a brand or owner.
3. **Coffee Receivables:** The system must retrieve successfully created, unsettled Coffee Receivables for the selected Café.
4. **Delivery Receivables:** The system must retrieve successfully created, unsettled Delivery Receivables for the selected Café.
5. **Recorded Transaction Rates:** Each receivable must retain the rate applicable when the transaction was recorded.
6. **Settlement Records:** The system must identify previously settled receivables to exclude them.
7. **Rate Grouping:** The system must group unsettled receivables by identical recorded Unit Rates.
8. **Financial Reconciliation:** Group totals must reconcile with Coffee Receivables, Delivery Receivables, and Total Outstanding Amount.
9. **Finance Rounding Rule:** Finance must confirm the calculation rounding rule before finalizing implementation.
10. **View Settlements Permission:** The system must enforce permission to access CP-103 through the UI, direct URLs, and direct requests.
11. **Update Settlement Permission:** The system must independently enforce permission for the Update Settlement action.
12. **Settlement Recording Form:** The destination must receive the selected Café's identity.
13. **Settlement Execution:** The settlement workflow must revalidate the latest outstanding amount before recording a full settlement.
14. **CP-102 List State:** The system must preserve the previous Settlement Management list state for Back navigation.
15. **Latest Saved Financial Data:** The system must retrieve current saved receivable and settlement information on page load and manual Refresh.

### 9.4 Pending Confirmations

| ID | Area | Pending Item |
|---|---|---|
| P-01 | Finance Rounding | Exact calculation rounding rule |
| P-02 | Permission Configuration | Exact View Settlements and Update Settlement permission keys |
| P-03 | Unauthorized Feedback | Exact unauthorized-access message |
| P-04 | Settlement Execution | Detailed handling of changes to outstanding amounts during settlement processing — covered by separate story |

## 10. Out of Scope

The following functionality is excluded from CP-103:

1. Recording or executing a Café settlement.
2. Processing payments to Cafés.
3. Supporting partial settlement transactions.
4. Defining settlement approval workflows.
5. Displaying Settlement History.
6. Displaying previously settled transactions.
7. Editing Coffee Receivables.
8. Editing Delivery Receivables.
9. Editing Café rates or financial configurations.
10. Creating or modifying Café records.
11. Creating In-Café redemptions.
12. Creating or managing Delivery orders.
13. Defining the exact Finance rounding method before confirmation.
14. Defining the detailed financial revalidation and concurrency workflow of Settlement Execution.
15. Defining payment gateway or settlement payment integrations.
16. Defining database schemas, API contracts, or technical architecture.
17. Automatically updating settlement amounts while the page remains open.
18. Combining settlement details across Cafés sharing the same owner or brand.

---

