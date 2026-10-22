# CP-126 — View Subscription Details

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-50
- **Feature Slug:** view-subscription-details

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-126
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed
- **Label:** MVP

## User Story

**As a** Platform Admin,  
**I want** to view the details of a customer’s subscription,  
**so that** I can review its validity, entitlement usage, payment, invoice, and consumption operations.

## Trigger

Dashboard → Subscriptions → View Details

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view subscription details.
- A subscription has been selected.

## Business Rules

### BR-01 — Selected Subscription

- Display information belonging to the selected subscription.
- Do not substitute another subscription belonging to the same customer.
- Consumption totals and operations must relate only to the selected subscription.

### BR-02 — Read-only Access

All information on this page is read-only.

The page does not change subscriptions, entitlements, payments, invoices, or reporting statuses.

### BR-03 — Subscription Status and Option

Subscription statuses:

- Active.
- Expired.

Subscription options:

- With Delivery.
- Without Delivery.

Determine subscription status using the existing subscription validity rules.

### BR-04 — Purchase Type

- The customer’s first subscription purchase is New Purchase.
- Every subsequent subscription purchase is Renewal, including purchases with a different subscription option.
- Purchase Type does not change when the subscription expires.

### BR-05 — Currency and Timezone

- Display payment amounts in SAR using two decimal places.
- Display dates and times using Asia/Riyadh, Saudi Arabia time (UTC+3).
- Today starts at 12:00 AM in Asia/Riyadh.

### BR-06 — Total Cups Consumed

Total Cups Consumed includes:

- Successful In-café redemptions.
- Delivered orders.
- Not Responding delivery operations.

Each operation counts once.

Not Responding consumes the daily entitlement and contributes to this total even though the customer did not receive the cup.

Orders still in progress and failed redemption attempts that did not consume an entitlement must not increase the total.

### BR-07 — Days Remaining

- Calculate remaining time using the selected subscription’s expiry date and time.
- Round any positive fraction of a 24-hour day up to one day.
- If two hours remain, display 1.
- At or after expiry, display 0.
- Never display a negative number.

### BR-08 — Today’s Entitlement

For an Active subscription:

- Display Used after successful daily redemption or a Not Responding operation consuming today’s entitlement.
- Otherwise display Unused.
- An order in progress does not itself indicate successful redemption.
- Unused does not override the delivery lock preventing another order or in-café redemption while a delivery order is in progress.

For an Expired subscription:

- Display 0, regardless of any earlier consumption on its expiry date.

Unused entitlements from previous days are not carried over.

### BR-09 — Consumption History

- Include successful In-café redemptions, Delivered orders, and Not Responding operations belonging to this subscription.
- Display the most recent operations first.
- Use the recorded consumption date and time for successful redemption.
- For Not Responding, use the time café staff changed the order to that status.
- Display the total operation count across all pages.
- Under these inclusion rules, the total operation count equals Total Cups Consumed.

### BR-10 — Invoice Reporting Status

Display the reporting status recorded by the invoice integration:

- Pending Reporting.
- Reported.
- Reported with Warnings.
- Rejected.
- Submission Failed.

Payment success and invoice reporting status are separate. A reporting failure must not be presented as a payment failure or automatically change subscription status.

Reporting, resubmission, and correction are outside this page’s scope.

### BR-11 — Pagination

- Display up to 10 consumption operations per page.
- Pagination must not change the subscription-wide totals.
- Maintain newest-operation-first ordering across pages.

### BR-12 — Permissions

- Viewing subscription details requires the corresponding permission.
- View Customer Profile requires permission to view user details.
- Invoice access must enforce the applicable invoice-view permission.
- Direct URLs and requests must enforce the same restrictions.

## Acceptance Criteria

### AC-01 — View Subscription Information

Display:

- Subscription ID: the unique reference of the selected subscription.
- Subscription Option: With Delivery or Without Delivery.
- Subscription Status: Active or Expired.
- Purchase Type: New Purchase or Renewal.

### AC-02 — View Customer Information

Display:

- Customer name.
- Customer mobile number.
- View Customer Profile action, subject to permission.

Selecting View Customer Profile opens the associated customer’s profile, covered by CP-124.

Do not allow the action to bypass user-profile permissions.

### AC-03 — View Validity Dates

Display:

- Activation date and time.
- Expiry date.

Use Asia/Riyadh for displayed dates and times.

Days Remaining and subscription status must use the actual expiry date and time, even if only the expiry date is displayed.

### AC-04 — View Entitlement Usage

Display:

- Total Cups Consumed.
- Days Remaining.
- Today’s Entitlement.
- The note: “Unused entitlements from previous days are not carried over.”

Apply the counting and expiry rules in BR-06, BR-07, and BR-08.

For an Expired subscription, display:

- Days Remaining: 0.
- Today’s Entitlement: 0.

Historical consumption totals remain visible after expiry.

### AC-05 — View Payment Details

Display the payment associated with the selected subscription:

- Payment amount.
- Currency: SAR.
- Recorded payment status.

Subscriptions opened from CP-56 have successfully paid and verified payments.

If payment details cannot be loaded, show a section error and Retry rather than inventing a payment status or displaying a zero amount.

### AC-06 — View Invoice Details

Where an invoice exists, display:

- ZATCA invoice number.
- Electronic Invoice Reporting Status.
- View Invoice action, subject to permission.

Reporting status must display the recorded value from:

- Pending Reporting.
- Reported.
- Reported with Warnings.
- Rejected.
- Submission Failed.

A reporting status must not prevent access to an existing invoice that the admin is authorized to view.

If no invoice exists:

- Display “No invoice available”.
- Prevent View Invoice.
- Do not invent an invoice number or reporting status.

### AC-07 — Open Invoice

- View Invoice opens the invoice associated with the selected subscription.
- Enforce access permission before exposing invoice content.
- If the invoice cannot be opened, display “Unable to load invoice. Please try again.”
- Provide Retry.
- Keep other loaded subscription information visible.

### AC-08 — View Consumption Operations

Display the selected subscription’s consumption history, newest first.

Each operation must display:

- Date.
- Time.
- Café name.
- Consumption type: Delivery or In-café.
- Related delivery status, where applicable: Delivered or Not Responding.

Requirements:

- Include Not Responding in both history and Total Cups Consumed.
- Distinguish consumption type from delivery status.
- Display the total number of operations across all pages.
- Do not include another subscription’s operations.

If no operations exist:

- Display a total count of 0.
- Display “No consumption operations found for this subscription”.

### AC-09 — Paginate Consumption Operations

- Display up to 10 operations per page.
- Display the current page number and total number of pages when records exist.
- Provide Previous and Next.
- Disable Previous on the first page.
- Disable Next on the last page.
- Disable both controls when only one page exists.
- Do not display active pagination controls when no operations exist.
- Keep total operations and Total Cups Consumed unchanged during page navigation.

### AC-10 — Return to Subscriptions

Provide Back to Subscriptions.

Selecting it returns to the list with the default state defined in CP-56:

- Empty mobile search.
- Empty activation-period dates.
- Subscription Status: All.
- Subscription Option: All.
- First page.
- Newest activation first.

### AC-11 — Handle Page Loading Failure

If subscription details cannot be loaded:

- Display “Unable to load subscription details. Please try again.”
- Provide Retry.
- Retry attempts to load the selected subscription again.

If the subscription no longer exists:

- Display “Subscription not found”.
- Provide a way to return to the Subscriptions list.

### AC-12 — Handle Individual Section Failure

If one section fails to load:

- Keep other successfully loaded sections visible.
- Display an error and Retry within the failed section.
- Retry only the affected section.
- Preserve the consumption page number when retrying another section.
- Do not present failed data as zero, empty history, or an unavailable invoice.

Section error messages:

- “Unable to load entitlement usage. Please try again.”
- “Unable to load payment details. Please try again.”
- “Unable to load invoice details. Please try again.”
- “Unable to load consumption operations. Please try again.”

### AC-13 — Prevent Unauthorized Access

- Prevent access without permission to view subscription details.
- Apply restrictions to navigation, direct URLs, and requests.
- Enforce user-profile and invoice permissions separately for their respective actions.
- Do not expose restricted customer, subscription, or invoice information.

## Acceptance Examples

| Scenario                                                        | Expected Result                                                     |
| --------------------------------------------------------------- | ------------------------------------------------------------------- |
| Admin selects an older subscription                             | Display that subscription, not the customer’s latest subscription   |
| Customer purchases their first subscription                     | Display New Purchase                                                |
| Customer purchases another subscription with a different option | Display Renewal                                                     |
| Two hours remain before expiry                                  | Display Days Remaining = 1                                          |
| Subscription reaches expiry                                     | Display Expired, Days Remaining = 0, and Today’s Entitlement = 0    |
| Active subscription has no consumption today                    | Display Unused                                                      |
| Active subscription has a delivery order in progress            | Display Unused; the delivery lock still applies                     |
| Customer redeems successfully today                             | Display Used and increase Total Cups Consumed once                  |
| Delivery ends as Not Responding today                           | Display Used and include the operation in Total Cups Consumed       |
| Subscription has 4 successful redemptions and 1 Not Responding  | Display Total Cups Consumed = 5 and total operations = 5            |
| A redemption attempt fails without consuming entitlement        | Do not increase Total Cups Consumed                                 |
| There are 11 consumption operations                             | Display 10 on page 1 and 1 on page 2; keep total = 11               |
| Invoice reporting status is Rejected                            | Display Rejected without changing payment or subscription status    |
| No invoice exists                                               | Display “No invoice available” and prevent View Invoice             |
| Invoice details fail to load                                    | Display section error and Retry, rather than “No invoice available” |
| One section fails                                               | Keep other loaded sections visible                                  |
| Admin lacks user-profile permission                             | Prevent access through View Customer Profile                        |
| Admin returns to Subscriptions                                  | Reset list criteria and pagination according to CP-56               |

## Attachments

| File                                  | Description                    |
| ------------------------------------- | ------------------------------ |
| attachments/image-20261014-182729.png | Subscription details wireframe |

Review the wireframe against these requirements before approval, especially:

- Not Responding inclusion in Total Cups Consumed.
- Today’s Entitlement = 0 for expired subscriptions.
- Invoice reporting statuses.
- Consumption pagination.
- Independent section error and Retry states.

## Related Requirements

- CP-56 — View Subscriptions.
- CP-124 — View User Profile Details.
- Subscription purchase, renewal, activation, and expiry.
- Daily entitlement and delivery lock.
- QR redemption and delivery completion.
- Not Responding handling.
- Payment verification.
- Invoice generation and electronic reporting.
- Admin permissions.

## Out of Scope

- Purchasing, renewing, cancelling, or editing subscriptions.
- Changing daily entitlements.
- Updating delivery order statuses.
- Processing payments or refunds.
- Generating, correcting, reporting, or resubmitting invoices from this page.
- Editing customer information.
- Filtering consumption history.
- Technical implementation planning and code.
