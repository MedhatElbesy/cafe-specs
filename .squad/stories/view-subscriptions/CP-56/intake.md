# CP-56 — View Subscriptions

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-50
- **Feature Slug:** view-subscriptions

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-56
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed

## User Story

**As a** Platform Admin,  
**I want** to view subscriptions with successfully paid and verified payments, search by customer mobile number, and filter by activation period, subscription status, and subscription option,  
**so that** I can monitor subscription records, track their validity, and quickly find specific customer subscriptions.

## Trigger

Dashboard → Subscriptions

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view subscriptions.

## Business Rules

### BR-01 — Access Permissions

Only Platform Admins with permission to view subscriptions can access the list, search, and filters.

Apply this restriction to navigation, direct URLs, and requests.

### BR-02 — Eligible Subscription Records

- Display subscriptions with successfully paid and verified payments only.
- Exclude subscriptions whose payments failed, are pending, or have not been verified.
- Payment verification follows the payment requirements; it is not performed by this screen.
- Display each subscription once as a separate record.
- Include both Active and Expired subscriptions.
- Multiple subscriptions belonging to the same customer appear as separate records, including subscriptions purchased through renewal.

### BR-03 — Ordering

Display subscriptions by activation date and time, newest first.

Maintain this order across paginated results.

### BR-04 — Subscription Status and Option

Subscription statuses are:

- Active.
- Expired.

Determine status using the existing subscription validity rules.

Subscription options are:

- With Delivery.
- Without Delivery.

### BR-05 — Currency and Timezone

- Display Amount Paid in SAR using two decimal places.
- Display dates and interpret activation-date filters using Asia/Riyadh, Saudi Arabia time (UTC+3).
- Date boundaries follow calendar days in Asia/Riyadh.

### BR-06 — Mobile Number Search

- Search by part or all of the customer’s mobile number.
- Update results as the admin types.
- Display all eligible subscriptions belonging to matching customers, subject to the selected filters.
- An empty search imposes no mobile-number restriction.

### BR-07 — Activation Period

Provide optional From and To dates.

- Both dates empty: no activation-date restriction.
- From only: include subscriptions activated on or after that date.
- To only: include subscriptions activated on or before that date.
- Both dates: include subscriptions activated within the selected period.
- Include the full start and end dates.
- From must not be later than To.
- Filter by activation date, not payment date or expiry date.

### BR-08 — Combined Criteria

Apply mobile search, activation period, subscription status, and subscription option together.

A subscription must match every applied criterion to appear.

Changing or clearing one criterion must preserve the others.

### BR-09 — Automatic Updates and Pagination

- Apply search and valid filter changes automatically without an Apply action.
- Display up to 25 subscriptions per page.
- Return to the first page whenever valid search or filter criteria change.
- Preserve all applied criteria during page navigation.
- Calculate total pages from matching eligible subscriptions only.

### BR-10 — Default List State

When the admin opens the list, including when returning from subscription details:

- Mobile search is empty.
- From and To dates are empty.
- Subscription Status is All.
- Subscription Option is All.
- The first page is displayed.
- Subscriptions are ordered by newest activation first.

Do not restore the previous search, filters, or page number.

### BR-11 — Missing Data

- Display “—” for missing display values.
- Do not treat missing values as zero or invent subscription details.
- Missing data must not break the list.
- Missing or unverified payment evidence does not qualify a subscription for inclusion.

### BR-12 — Read-only List

The list provides viewing, search, filtering, and navigation only.

It does not modify subscriptions, payments, or subscription statuses.

## Acceptance Criteria

### AC-01 — Display Subscriptions

Display only subscriptions with successfully paid and verified payments.

Each record must display:

- Customer Name.
- Mobile Number.
- Subscription Option.
- Activation Date.
- Expiry Date.
- Amount Paid and currency.
- Subscription Status: Active or Expired.
- View Details action.

Additional requirements:

- Display each subscription once.
- Display separate subscriptions for the same customer as separate records.
- Order by activation date and time, newest first.
- Display “—” for missing display values without breaking the list.

### AC-02 — Search by Mobile Number

- Provide a mobile-number search field.
- Support partial and complete number matching.
- Update results during typing without requiring a Search action.
- Apply the search together with all selected filters.
- Return to the first page when search text changes.
- Clearing the search removes only the mobile-number restriction.

### AC-03 — Filter by Activation Period

Provide:

- Activation Period From.
- Activation Period To.

Requirements:

- Leave both fields empty by default.
- Support either date independently.
- Include subscriptions activated on the selected boundary dates.
- Apply valid date changes automatically.
- Preserve mobile search, status, and option filters.
- Return to the first page after a valid date-filter change.

### AC-04 — Validate Activation Period

If From is later than To:

- Display an inline validation message:
  “From Date must be on or before To Date.”
- Do not apply the invalid range.
- Retain the previously applied results.
- Clearly distinguish the invalid entered range from the criteria associated with the displayed results.

A single valid date must not trigger a missing-date error because both fields are optional.

### AC-05 — Filter by Subscription Status

Provide:

- All — default.
- Active.
- Expired.

Requirements:

- All imposes no status restriction.
- Active displays currently active subscriptions.
- Expired displays expired subscriptions.
- Preserve other search and filter criteria.
- Apply changes automatically and return to the first page.

### AC-06 — Filter by Subscription Option

Provide:

- All — default.
- With Delivery.
- Without Delivery.

Requirements:

- All imposes no subscription-option restriction.
- A specific option displays only matching subscriptions.
- Preserve other search and filter criteria.
- Apply changes automatically and return to the first page.

### AC-07 — Combine Search and Filters

- Display only subscriptions matching all applied criteria.
- Allow each criterion to be changed or cleared independently.
- Clearing one criterion must not clear the others.
- Retain criteria when no results match.

### AC-08 — Paginate Results

- Display up to 25 matching subscriptions per page.
- Display the current page number and total number of pages when results exist.
- Provide Previous and Next.
- Previous opens the previous page.
- Next opens the next page.
- Disable Previous on the first page.
- Disable Next on the last page.
- Disable both controls when only one page exists.
- Preserve search, filters, and newest-activation-first sorting during page navigation.
- When no results exist, do not display active pagination controls or an invalid page count.

### AC-09 — View Subscription Details

- Provide View Details for each subscription.
- Selecting it opens the details of the selected subscription.
- The destination must correspond to that subscription, not merely the customer’s latest subscription.
- Subscription detail content is covered by its separate requirement.

### AC-10 — Return from Subscription Details

When the admin returns to the list:

- Clear mobile search.
- Clear From and To dates.
- Reset Subscription Status and Subscription Option to All.
- Display the first page.
- Apply newest-activation-first sorting.

### AC-11 — Handle Empty Results

If no eligible subscriptions exist or no records match the applied criteria:

- Display “No subscriptions found.”
- Retain the current search and filter selections.
- Allow the admin to change or clear criteria.

### AC-12 — Handle Loading, Search, or Filter Failure

If the list or updated results cannot be loaded:

- Display “Subscriptions could not be loaded. Please try again.”
- Provide Retry.
- Retain the search and valid filter criteria associated with the failed request.
- Retry using those criteria.
- Do not present a loading failure as an empty result.
- Do not present earlier results as if they matched a failed new search or filter request.

### AC-13 — Prevent Unauthorized Access

- Deny access to admins without permission to view subscriptions.
- Enforce the restriction for navigation, direct URLs, and requests.
- Do not expose subscription or customer information to unauthorized users.

## Acceptance Examples

| Scenario                                                             | Expected Result                                                                        |
| -------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Admin opens Subscriptions                                            | Empty search and dates, both dropdowns set to All, first page, newest activation first |
| Payment succeeded and was verified                                   | Include the subscription                                                               |
| Payment failed, is pending, or is unverified                         | Exclude the subscription                                                               |
| Customer has an expired subscription and a later active subscription | Display both as separate records when no status filter applies                         |
| Admin enters part of a mobile number                                 | Update matching subscription records while typing                                      |
| From is provided alone                                               | Include activations on or after that date                                              |
| To is provided alone                                                 | Include activations on or before that date                                             |
| From and To are the same date                                        | Include activations throughout that calendar day in Asia/Riyadh                        |
| From is later than To                                                | Show validation and do not apply the invalid range                                     |
| Active and With Delivery are selected                                | Show only active delivery-inclusive subscriptions matching the remaining criteria      |
| Admin changes a criterion on page 3                                  | Return to page 1 of the updated results                                                |
| There are 26 matching subscriptions                                  | Show 25 on page 1 and 1 on page 2                                                      |
| There are 25 matching subscriptions                                  | Show one page and disable Previous and Next                                            |
| No records match                                                     | Show “No subscriptions found.” and retain criteria                                     |
| A display value is missing                                           | Show “—” without breaking the list                                                     |
| Admin opens an older subscription                                    | Open that specific subscription’s details                                              |
| Admin returns from details                                           | Reset the list to its default state                                                    |
| Search or filter loading fails                                       | Show the loading error and Retry; retain the failed request’s valid criteria           |
| Admin lacks permission                                               | Deny access without exposing records                                                   |

## Attachments

| File                                                       | Description                                      |
| ---------------------------------------------------------- | ------------------------------------------------ |
| attachments/Subscriptions — list, search and filter@2x.png | Subscriptions list, search, and filter wireframe |

Review the wireframe against the updated requirements before approval, particularly:

- Automatic search and filtering.
- Optional activation-date boundaries.
- Pagination with 25 records per page.
- Validation, empty, and loading-error states.

## Related Requirements

- Subscription purchase and renewal.
- Payment success and verification.
- Subscription activation, expiry, and validity rules.
- Subscription details.
- Subscriber information.
- Admin permissions.

## Out of Scope

- Creating, renewing, cancelling, or editing subscriptions.
- Manually changing subscription status.
- Verifying payments from this page.
- Processing payments or refunds.
- Searching by customer name or other additional fields.
- Filtering by payment date or expiry date.
- Preserving list state when returning from subscription details.
- Subscription detail content.
- Technical implementation planning and code.
