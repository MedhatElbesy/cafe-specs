# CP-125 — View Café Profile Details

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-50
- **Feature Slug:** view-cafe-profile-details

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-125
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed
- **Label:** MVP

## User Story

**As a** Platform Admin,  
**I want** to view a café’s profile details, statuses, agreed rates, financial summary, settlements, and consumption operations,  
**so that** I can review the café’s participation, activity, and financial position on the platform.

## Trigger

Dashboard → Cafés → View Profile

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view café details.
- A café has been selected.

## Business Rules

### BR-01 — Permissions and Read-only Information

- Only authorized Platform Admins can view café details.
- Profile information, operating status, agreed rates, financial figures, settlements, and consumption history are read-only.
- Access to Suspend Café requires the corresponding suspension permission.
- Suspension is performed through its separate flow.

### BR-02 — Missing Information

- Display “—” for missing basic information.
- If no café location exists, display “No location available”.
- An incomplete café profile must remain accessible to authorized admins.

### BR-03 — Café Statuses

Participation statuses:

- Active.
- Suspended.

Operating statuses:

- Open.
- Closed.

Suspending a café automatically changes its operating status to Closed. This page displays the resulting statuses.

### BR-04 — Currency and Timezone

- Display agreed rates and financial amounts in SAR, using two decimal places.
- Display dates and times using Asia/Riyadh, Saudi Arabia time (UTC+3).
- Interpret selected calendar dates using Asia/Riyadh.

### BR-05 — Period Selection

- Provide From Date and To Date.
- The default period is the first day of the current month through today.
- Both dates are required.
- From Date must not be later than To Date.
- Include the full selected dates, from the beginning of From Date through the end of To Date.
- Apply the period to Financial Summary and Consumption Operations only.
- Do not filter Settlements by this period.

### BR-06 — Consumption Types and Filters

Consumption types are:

- In-café.
- Delivery.

Provide one consumption filter selection:

- All Operations — default.
- In-café.
- Delivery.
- Not Responding.

Delivery includes Not Responding operations. Not Responding displays only delivery operations ending in that status.

Filter counts overlap; they must not be added together to calculate the total.

### BR-07 — Recorded Consumption Operations

- Include successful in-café redemptions, delivered orders, and Not Responding operations.
- Do not count an order still in progress as completed consumption.
- Successful consumption uses its recorded consumption time.
- A Not Responding operation uses the time café staff changed the order to Not Responding.
- Each operation is counted once.

### BR-08 — Financial Calculations

For operations belonging to the selected period:

- Each successful In-café operation earns one coffee receivable.
- Each Delivered operation earns one coffee receivable and one delivery receivable.
- Each Not Responding operation earns one coffee receivable and one delivery receivable.
- Calculate receivables using the café’s agreed rates applicable to the operations.

Financial figures:

- **Coffee Receivables:** Total coffee amounts earned by eligible operations in the period.
- **Delivery Receivables:** Total delivery amounts earned by eligible operations in the period.
- **Total Receivables:** Coffee Receivables + Delivery Receivables.
- **Amount Paid:** Recorded settlement amounts allocated to receivables from operations in the selected period.
- **Outstanding Amount:** Total Receivables − Amount Paid.

Settlement allocation must follow the settlement rules and must not count the same paid amount more than once.

A settlement may cover operations in the selected period even if its transfer date is outside that period. Do not subtract unrelated settlements merely because their transfer dates fall within the selected period.

### BR-09 — Summary and Operation Filters

- The selected period affects the financial summary and consumption operations.
- The consumption-type/status filter affects the operations list and its matching total only.
- It does not change the financial summary.
- Each filter’s count reflects all matching operations within the selected period, across all pages.

### BR-10 — Pagination and Ordering

- Consumption Operations and Settlements each have independent pagination.
- Display up to 10 records per page.
- Order consumption operations by operation date and time, newest first.
- Order settlements by transfer date, newest first.
- Changing the period or consumption filter returns Consumption Operations to page 1.
- Changes to consumption filters or pagination do not reset settlement pagination.

### BR-11 — Independent Section Failures

If one section fails to load:

- Keep other successfully loaded sections visible.
- Display an error and Retry within the failed section.
- Do not present a loading error as zero financial activity or empty history.

## Acceptance Criteria

### AC-01 — View Basic Information

Display:

- Café name.
- Owner name.
- Owner email.
- Café address.
- Account creation date.
- Café location on a map.

Display “—” for missing basic information.

If no location exists, display “No location available”.

### AC-02 — View Café Statuses

- Display Participation Status: Active or Suspended.
- Display Operating Status: Open or Closed.
- Operating Status is read-only.
- A suspended café must display Closed.

For an active café, provide access to Suspend Café only when the admin has suspension permission.

### AC-03 — View Agreed Rates

Display:

- Agreed price per cup.
- Agreed delivery cost.
- Currency: SAR.

Rates are read-only.

### AC-04 — Select and Validate Period

- Display From Date and To Date.
- Initially select the first day of the current month through today.
- Display the selected period.
- Apply a valid date pair to Financial Summary and Consumption Operations.
- Reset Consumption Operations to page 1.
- Keep the selected consumption filter.
- Leave Settlements unaffected.

For missing or invalid dates:

- Display inline validation identifying the issue.
- Do not apply the invalid period.
- Retain the previously applied results and clearly identify their applied period.

Validation messages:

- Missing date: “Select both From Date and To Date.”
- Invalid order: “From Date must be on or before To Date.”

### AC-05 — View Financial Summary

For the selected period, display:

- Total consumption count.
- Coffee Receivables.
- Delivery Receivables.
- Total Receivables.
- Amount Paid through recorded settlements against those receivables.
- Outstanding Amount.

Apply the calculations in BR-08.

The total consumption count includes each eligible operation once, including Not Responding.

If the selected period has no eligible activity, display zero for the corresponding counts and financial figures.

### AC-06 — View Settlements

Display all recorded settlements for the café, newest first, independently of the selected period.

Each settlement must display:

- Settlement amount.
- Currency.
- Transfer date.
- Transfer reference image, where available.
- View Image action, where an image exists.

If no settlements exist, display:

“No settlements found”.

### AC-07 — View Transfer Image

- View Image opens the image belonging to the selected settlement.
- If no image exists, display “No image available” and prevent View Image.
- If image loading fails, display “Unable to load transfer image. Please try again.”
- Provide Retry for the failed image.
- Keep the rest of the profile available.

### AC-08 — View Consumption Operations

Display operations belonging to the selected café and selected period, newest first.

Each operation must display:

- Date.
- Time.
- User name.
- Consumption type: In-café or Delivery.
- Related delivery status, where applicable, including Delivered or Not Responding.

Display the total number of operations matching the selected period and consumption filter, across all pages.

If no operations match, display:

“No consumption operations found”.

### AC-09 — Filter Consumption Operations

Provide:

- All Operations.
- In-café.
- Delivery.
- Not Responding.

Requirements:

- Default to All Operations.
- Display the matching count for every filter within the selected period.
- Delivery includes Not Responding.
- All Operations counts each operation once.
- Changing the filter updates the operations list and matching total.
- Return to page 1 after a filter change.
- Keep the period unchanged.
- Do not change the financial summary or settlements.

### AC-10 — Navigate History Pages

Provide separate pagination for Consumption Operations and Settlements.

For each section:

- Display up to 10 records per page.
- Display the current page number and total number of pages when records exist.
- Provide Previous and Next.
- Disable Previous on the first page.
- Disable Next on the last page.
- Disable both controls when only one page exists.
- Do not display active pagination controls when no records exist.

Consumption pagination must reflect the selected period and filter.

### AC-11 — Navigate Sections

Provide navigation to:

- Overview.
- Financial Summary.
- Settlements.
- Consumption Operations.

Navigating between sections must retain the selected period, consumption filter, and each history’s page number during the current profile visit.

### AC-12 — Return to Cafés

Provide Back to Cafés.

Selecting it returns to the list with its default state, as defined in CP-55:

- Empty search.
- Both filters set to All.
- First page.

### AC-13 — Open Suspension Flow

- For an active café, an admin with suspension permission can select Suspend Café.
- The action opens the separate suspension flow for the selected café.
- Viewing the profile alone must not suspend the café.
- Prevent unauthorized access to the suspension flow, including direct URLs or requests.

### AC-14 — Handle Page Loading Failure

If the selected café’s details cannot be loaded:

- Display “Unable to load café details. Please try again.”
- Provide Retry.
- Retry attempts to load the same café.

If the café no longer exists:

- Display “Café not found”.
- Provide a way to return to the Cafés list.

### AC-15 — Handle Individual Section Failure

If one section fails:

- Keep other loaded sections visible.
- Display a section-specific error and Retry.
- Retry the failed section using the currently applied period and filter where relevant.
- Preserve the other sections’ selections and page numbers.

Error messages:

- “Unable to load financial summary. Please try again.”
- “Unable to load settlements. Please try again.”
- “Unable to load consumption operations. Please try again.”

Do not display failed financial data as zero or failed histories as empty.

### AC-16 — Prevent Unauthorized Access

- Only admins with permission to view café details can access this profile.
- Enforce access restrictions for navigation, direct URLs, requests, and transfer images.
- Do not expose café information or images to unauthorized users.
- Viewing permission alone does not grant suspension permission.

## Acceptance Examples

| Scenario                                                          | Expected Result                                                                 |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Admin opens the profile                                           | Show the café’s information and default the period to month-to-date             |
| Café information is incomplete                                    | Display “—” for missing fields                                                  |
| Café has no location                                              | Display “No location available”                                                 |
| Café is suspended                                                 | Display Suspended and Closed                                                    |
| Admin changes the period                                          | Update Financial Summary and Consumption Operations; retain all settlements     |
| From Date is later than To Date                                   | Show validation and retain the previously applied results                       |
| Not Responding operation falls within the period                  | Count it once and include both coffee and delivery receivables                  |
| Delivery filter is selected                                       | Include Delivered and Not Responding delivery operations                        |
| Not Responding filter is selected                                 | Show only Not Responding operations                                             |
| Consumption filter changes                                        | Reset consumption pagination; leave financial summary and settlements unchanged |
| Period receivables are SAR 100 and SAR 40 is settled against them | Display Total Receivables 100, Amount Paid 40, and Outstanding Amount 60        |
| Settlement covers operations outside the selected period          | Do not subtract it from the selected period’s receivables                       |
| A settlement outside the period pays for operations inside it     | Include the relevant allocated amount in Amount Paid                            |
| A section contains 11 records                                     | Display 10 on page 1 and 1 on page 2                                            |
| No eligible financial activity exists                             | Display zero counts and financial figures                                       |
| Financial Summary fails to load                                   | Show its error and Retry; do not display false zeros                            |
| Transfer image is unavailable                                     | Display “No image available” and prevent View Image                             |
| Admin has view permission but no suspension permission            | Allow viewing; do not allow suspension                                          |

## Attachments

| File                                  | Description                    |
| ------------------------------------- | ------------------------------ |
| attachments/image-20261014-184146.png | Café profile details wireframe |

Review the wireframe against the updated period controls, financial labels, pagination, filters, and error states before approval.

## Related Requirements

- CP-55 — Cafés list.
- CP-113 — Create Café.
- Café profile completion.
- Café suspension flow and permissions.
- Consumption and delivery rules.
- Settlement recording and allocation to receivables.
- Admin permissions.

The settlement requirements must support identifying which receivables have been paid so the selected period’s outstanding amount can be calculated correctly.

## Out of Scope

- Editing café or owner information.
- Editing agreed rates.
- Manually changing operating status.
- Implementing the suspension flow within this story.
- Recording settlements or executing transfers.
- Updating consumption or delivery statuses.
- Filtering the settlement history by the selected period.
- Technical implementation planning and code.
