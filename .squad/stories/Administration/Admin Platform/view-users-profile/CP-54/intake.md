# CP-54 — View Users Profile

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-50
- **Feature Slug:** view-users-profile

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-54
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed

## User Story

**As a** Platform Admin,  
**I want** to view and search all registered subscribers, including their current subscription and today’s entitlement status,  
**so that** I can quickly find a subscriber and open their profile to respond to service requests.

## Trigger

Dashboard → Users

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view users.

## Business Rules

### BR-01 — Access Permissions

Only Platform Admins with permission to view users can access the list, search, and filters.

This restriction applies to navigation, direct URLs, and requests.

### BR-02 — List Coverage and Ordering

- Include registered subscribers with and without an active subscription.
- Display each subscriber once as a separate record.
- Display the newest registered subscribers first.

### BR-03 — Current Subscription

Display one of the following:

- With Delivery.
- Without Delivery.
- No Active Subscription.

Users who have never subscribed or whose subscriptions have expired are classified as No Active Subscription.

For users without an active subscription, display “—” for both Expiry Date and Today’s Entitlement Status.

### BR-04 — Today’s Entitlement

For users with an active subscription:

- Display Available if today’s entitlement has not been consumed.
- Display Used after successful daily redemption.
- Display Used if the related delivery order ends as Not Responding.
- While a delivery order is in progress, continue to display Available until the entitlement is consumed.

Available describes the consumption state only. It does not override the rule preventing another delivery order or in-café redemption while a delivery order is in progress.

### BR-05 — Search

- Support partial matching by subscriber name or mobile number.
- Update results as the admin types.
- An empty search displays all records matching the selected subscription filter.
- Apply search and subscription filter together.

### BR-06 — Pagination

- Display up to 25 users per page.
- Apply pagination to the results after search and filtering.
- Changing the search text or subscription filter returns to the first page.

### BR-07 — Default List State

When the admin opens the Users list, including when returning from a subscriber profile:

- Search text is empty.
- All Users is selected.
- The first page is displayed.

Do not restore the previous search, filter, or page number.

### BR-08 — Timezone

- Display dates and determine today’s entitlement using Asia/Riyadh, Saudi Arabia time (UTC+3).
- The daily boundary is 12:00 AM in Asia/Riyadh.

### BR-09 — Read-only List

The list displays subscriber information and provides navigation to profiles. It does not allow editing subscriber data, subscriptions, or entitlements.

## Acceptance Criteria

### AC-01 — View Subscribers List

The system must display all registered subscribers, including users without an active subscription.

Each record must display:

- Name.
- Mobile Number.
- Registration Date.
- Current Subscription:
  - With Delivery.
  - Without Delivery.
  - No Active Subscription.
- Expiry Date.
- Today’s Entitlement Status:
  - Available.
  - Used.
- View Profile action.

Additional requirements:

- Each subscriber appears once.
- The newest registered subscriber appears first.
- Subscription and entitlement information reflects the current business state.
- If no active subscription exists, display “—” for Expiry Date and Today’s Entitlement Status.

### AC-02 — Search Subscribers

- Allow the admin to enter search text.
- Match the text against subscriber names or mobile numbers.
- Support partial matches.
- Update results as the admin types without requiring a Search action.
- Return to the first page whenever the search text changes.
- Retain the selected subscription filter while searching.
- Clearing the search displays all users matching the selected filter.

### AC-03 — Filter by Subscription Status

Provide these filter options:

- All Users — default.
- Active Subscription.
- No Active Subscription.

Filter behaviour:

- All Users includes subscribers with and without active subscriptions.
- Active Subscription includes subscribers who currently have an active subscription.
- No Active Subscription includes subscribers without a subscription and those with expired subscriptions.
- Apply the selected filter together with the current search text.
- Return to the first page whenever the selected filter changes.

### AC-04 — Paginate Subscribers List

- Display up to 25 matching users per page.
- Display the current page number and total number of pages when results exist.
- Calculate the total pages from the results matching the current search and filter.
- Next opens the next page.
- Previous opens the previous page.
- Disable Previous on the first page.
- Disable Next on the last page.
- If only one page exists, disable both controls.
- Preserve search text and filter selection when navigating between pages.
- When no results exist, do not show active pagination controls or an invalid page count.

### AC-05 — Open Subscriber Profile

- Provide View Profile for each subscriber.
- Selecting View Profile opens the details of that specific subscriber.
- The destination is covered by CP-124 — View User Profile Details.

### AC-06 — Return from Subscriber Profile

When the admin returns to the Users list:

- Clear the previous search text.
- Select All Users.
- Display the first page.
- Display the newest registered subscribers first.

### AC-07 — Handle Empty Results

If there are no registered subscribers or no subscribers match the current search and filter:

- Display “No users found.”
- Retain the current search text and selected filter.
- Allow the admin to change or clear the search and change the filter.

### AC-08 — Handle Loading Errors

If the subscribers list cannot be loaded:

- Display “Users could not be loaded. Please try again.”
- Provide a Retry action.
- Retain the current search text and selected filter.
- Retry attempts to load the list using the retained search and filter.
- Do not present a loading failure as an empty result.

### AC-09 — Prevent Unauthorized Access

- Prevent admins without permission to view users from accessing the list.
- Apply the restriction to direct URLs and requests.
- Do not expose subscriber information to unauthorized users.

## Acceptance Examples

| Scenario                                          | Expected Result                                                          |
| ------------------------------------------------- | ------------------------------------------------------------------------ |
| Admin opens Users                                 | Empty search, All Users selected, first page, newest registrations first |
| User has an active delivery subscription          | Display With Delivery, expiry date, and today’s entitlement status       |
| User has an expired subscription                  | Display No Active Subscription and “—” for expiry and entitlement        |
| User has never subscribed                         | Include the user with No Active Subscription                             |
| Admin enters part of a name                       | Update the list with matching subscribers while typing                   |
| Admin enters part of a mobile number              | Update the list with matching subscribers while typing                   |
| Search and Active Subscription filter are applied | Display only active subscribers matching the search                      |
| Admin changes search while on page 3              | Display page 1 of the updated results                                    |
| Admin changes the filter                          | Display page 1 and retain the search text                                |
| There are 26 matching users                       | Display 25 on page 1 and 1 on page 2                                     |
| There are 25 matching users                       | Display one page and disable Previous and Next                           |
| No users match                                    | Display “No users found.” and retain search and filter                   |
| User has a delivery order in progress             | Display Available; the delivery lock still applies                       |
| Delivery ends as Not Responding                   | Display Used                                                             |
| Admin selects View Profile                        | Open the selected user’s CP-124 profile                                  |
| Admin returns from the profile                    | Reset search, filter, and page to their defaults                         |
| Loading fails                                     | Display the loading error and Retry; retain search and filter            |
| Admin lacks permission                            | Deny access without exposing subscriber information                      |

## Attachments

| File                                      | Description          |
| ----------------------------------------- | -------------------- |
| attachments/Users — all users list@2x.png | Users list wireframe |

The wireframe must be reviewed against these updated requirements before approval.

## Related Requirements

- CP-124 — View User Profile Details.
- Subscriber registration.
- Subscription status and expiry.
- Daily entitlement and QR redemption.
- Delivery lock and Not Responding rules.
- Admin permissions.

## Out of Scope

- Editing or deleting subscriber accounts.
- Changing subscriptions or daily entitlements.
- Updating delivery order statuses.
- Additional filters beyond the specified subscription filter.
- Preserving list state when returning from a profile.
- User profile details, covered by CP-124.
- Technical implementation planning and code.
