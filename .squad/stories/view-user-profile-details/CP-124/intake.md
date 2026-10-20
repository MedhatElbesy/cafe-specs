# CP-124 — View User Profile Details

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-54
- **Feature Slug:** view-user-profile-details

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-124
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed
- **Label:** MVP

## User Story

**As a** Platform Admin,  
**I want** to view a user’s profile details, current subscription, subscription history, consumption history, and payment history,  
**so that** I can review the user’s account and activity on the platform.

## Trigger

Dashboard → Users → View Details

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view user details.
- A user has been selected from the Users list.

## Business Rules

### BR-01 — Read-only Access

All information on this page is read-only. Viewing this page does not change subscriptions, entitlements, payments, or delivery order statuses.

### BR-02 — Access Permissions

Only Platform Admins with permission to view user details can access the page, including through direct URLs or requests.

### BR-03 — Subscription Statuses

Subscription statuses displayed on this page are:

- Active.
- Expired.

### BR-04 — Subscription Options

Subscription options are:

- With Delivery.
- Without Delivery.

### BR-05 — Subscription History Coverage

Subscription History includes the current subscription, where one exists, and previous subscriptions.

### BR-06 — Consumption Types

- **Delivery:** The user orders their entitlement for delivery.
- **In-café:** The user visits a café to redeem their entitlement.

Consumption type is separate from delivery order status.

### BR-07 — Daily Entitlement

- Today’s entitlement is Used when the user successfully receives their daily entitlement.
- A delivery order ending as Not Responding also consumes the daily entitlement.
- A delivery order in progress must not be presented as successful receipt of the entitlement.
- Displaying entitlement or order information does not grant permission to redeem again.

### BR-08 — Delivery Order Status

Display the related order’s current status as recorded:

- New.
- Preparing.
- Out for Delivery.
- Delivered.
- Not Responding.

Status changes follow the delivery workflow and staff permissions. The Platform Admin cannot update statuses from this page.

### BR-09 — History Ordering

Display each history with the most recent records first:

- Subscription History: by activation date.
- Consumption History: by operation date and time.
- Payment History: by payment date and time.

### BR-10 — Timezone and Today Label

- Display all dates and times using Asia/Riyadh, Saudi Arabia time (UTC+3).
- A calendar day starts at 12:00 AM in Asia/Riyadh.
- An operation receives the Today label when its operation date matches the current date in Asia/Riyadh.

### BR-11 — Not Responding Timestamp

For a Not Responding operation, the date and time displayed in Consumption History are the date and time when café staff changed the order status to Not Responding.

### BR-12 — Pagination

Subscription, consumption, and payment histories each use independent pagination with 10 records per page.

### BR-13 — Filtering

History filtering is outside this story’s scope.

## Acceptance Criteria

### AC-01 — View Profile Information

The page must display:

- User name.
- Mobile number.
- Registration date.
- The user’s saved location on a map.
- Latitude and longitude of the saved location.

If no saved location exists, display:

“No saved location”.

### AC-02 — View Current Subscription

Where a current subscription exists, display:

- Subscription status.
- Subscription option: With Delivery or Without Delivery.
- Activation date.
- Expiry date.
- Today’s entitlement status: Used or Unused.

If today’s entitlement has been used through successful redemption, also display:

- Consumption time.
- Café name.
- Consumption type: Delivery or In-café.

Where a related delivery order exists:

- Display its current order status separately from entitlement status.
- Display the related café name and consumption type as Delivery.
- Show New, Preparing, Out for Delivery, Delivered, or Not Responding as recorded.
- If the order is Not Responding, show today’s entitlement as Used.
- Do not describe a Not Responding operation as successful delivery.

If no current subscription exists, display:

“No current subscription”.

Existing histories must remain available.

### AC-03 — View Subscription History

Display all the user’s subscriptions, including the current subscription where one exists, with the most recent first.

Each subscription must display:

- Subscription option.
- Activation date.
- Expiry date.
- Amount paid.
- Currency.
- Subscription status: Active or Expired.

If no subscription history exists, display:

“No subscription history found”.

### AC-04 — View Consumption History

Display the user’s recorded consumption operations, with the most recent first.

Each operation must display:

- Date.
- Time.
- Café name.
- Consumption type: Delivery or In-café.
- Related delivery order status, where applicable.

Additional requirements:

- Consumption type and delivery order status must be distinguishable.
- Successful redemption uses its recorded consumption date and time.
- A Not Responding operation uses the time café staff changed the order to that status.
- Operations recorded today must display a Today label according to Asia/Riyadh.

If no consumption operations exist, display:

“No consumption operations found”.

### AC-05 — View Payment History

Display the user’s payment transactions, with the most recent first.

Each transaction must display:

- Amount.
- Currency.
- Payment date.
- Transaction status: Successful or Failed.

If no payment transactions exist, display:

“No payment history found”.

### AC-06 — Navigate History Pages

Provide separate pagination for Subscription History, Consumption History, and Payment History.

For each history:

- Display up to 10 records per page.
- Display the current page number.
- Provide Previous and Next actions.
- Disable Previous on the first page.
- Disable Next on the last page.
- Changing one history’s page must not change the pages of the other histories.
- When only one page exists, disable both Previous and Next.
- When no records exist, display the relevant empty-state message without active pagination controls.

### AC-07 — Return to Users

Provide a Back to Users action.

Selecting it returns the admin to the Users list.

### AC-08 — Handle Page Loading Failure

If the selected user’s details cannot be loaded:

- Display “Unable to load user details. Please try again.”
- Provide a Retry action.
- Retry attempts to load the selected user’s details again.

Do not display a loading failure as missing user data or empty history.

### AC-09 — Handle Individual History Loading Failure

If one history section fails to load:

- Keep successfully loaded profile information and other sections visible.
- Display an error within the failed section.
- Provide a Retry action for that section.
- Retry reloads the failed section without resetting the other histories’ pages.
- Do not display the failed section as an empty history.

Section error messages:

- “Unable to load subscription history. Please try again.”
- “Unable to load consumption history. Please try again.”
- “Unable to load payment history. Please try again.”

### AC-10 — Handle Missing User

If the selected user no longer exists:

- Display “User not found”.
- Provide a way to return to the Users list.

### AC-11 — Prevent Unauthorized Access

- Prevent access by admins without permission to view user details.
- Apply this restriction to navigation, direct URLs, and requests.
- Do not expose the selected user’s information to unauthorized users.

## Acceptance Examples

| Scenario                                       | Expected Result                                                                            |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------ |
| User has an active subscription                | Display its details and include it in Subscription History                                 |
| User has no current subscription               | Display “No current subscription”; existing histories remain available                     |
| User redeems successfully in a café            | Show Used, consumption time, café name, and In-café                                        |
| Delivery order is Preparing                    | Display Preparing as the order status                                                      |
| Delivery order is Out for Delivery             | Display Out for Delivery separately from entitlement status                                |
| Delivery ends as Not Responding                | Show Used and Not Responding; use the staff’s status-update time for the history operation |
| Operation falls on today’s date in Asia/Riyadh | Display the Today label                                                                    |
| User has no saved location                     | Display “No saved location”                                                                |
| A history contains 11 records                  | Show 10 on page 1 and 1 on page 2                                                          |
| A history contains 10 records                  | Show one page; disable Previous and Next                                                   |
| Admin changes the Consumption History page     | Keep the other histories on their existing pages                                           |
| Payment History fails to load                  | Keep other loaded sections visible and show Retry in Payment History                       |
| Consumption History is empty                   | Display “No consumption operations found”                                                  |
| User details fail to load                      | Display the page loading error and Retry                                                   |
| Selected user no longer exists                 | Display “User not found”                                                                   |
| Admin lacks permission                         | Deny access without exposing user information                                              |

## Attachments

| File                                  | Description                    |
| ------------------------------------- | ------------------------------ |
| attachments/image-20261014-182608.png | User profile details wireframe |

The wireframe must be reviewed against these updated requirements before approval.

## Related Requirements

- Users list.
- Subscriber profile and saved location.
- Subscription management.
- Daily entitlement and QR redemption.
- Delivery workflow and staff permissions.
- Payment transactions.
- Admin permissions.

## Out of Scope

- Editing user information or saved location.
- Changing subscriptions or daily entitlements.
- Updating delivery order statuses.
- Performing payments or refunds.
- Filtering history records.
- Technical implementation planning and code.
