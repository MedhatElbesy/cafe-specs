# CP-77 — View Staff

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Feature:** Café Platform — CP-51
- **Sub-feature:** Admin — CP-596
- **Feature Slug:** view-staff

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-77
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Medhat Elbesy
- **Label:** MVP

## User Story

**As a** Café Owner,  
**I want** to view, search, and filter staff accounts associated with my café,  
**so that** I can review their details, staff types, and account statuses and access the relevant account-management actions.

## Trigger

Dashboard → Staff Accounts

## Preconditions

- The Café Owner is signed in.
- The owner account is associated with a café.

## Business Rules

### BR-01 — Café Ownership

- Display only staff accounts associated with the signed-in owner’s café.
- Apply the same café restriction to search, filters, pagination, and account-management actions.
- Prevent access to another café’s staff accounts through direct URLs or requests.

### BR-02 — List Coverage and Ordering

- Include both Active and Inactive staff accounts.
- Include Cashier and Delivery Staff accounts.
- Display each staff account once.
- Display the newest created accounts first.

### BR-03 — Password Confidentiality

- Do not display passwords in the list.
- Do not provide password reveal or copy actions.
- List responses must not expose passwords.

### BR-04 — Search

- Search by part or all of a staff member’s name or email address.
- A record matches if its name or email matches the search text.
- Update results as the owner types.
- An empty search imposes no name or email restriction.
- Search only within the owner’s café.

### BR-05 — Filters

Staff Type options:

- All — default.
- Cashier.
- Delivery Staff.

Account Status options:

- All — default.
- Active.
- Inactive.

Apply search and both filters together. A record must satisfy every applied condition to appear.

### BR-06 — Pagination

- Display up to 25 staff accounts per page.
- Apply pagination after search and filtering.
- Changing search text or either filter returns to the first page.
- Preserve search and filter selections while navigating between pages.
- Maintain newest-created-first ordering across pages.

### BR-07 — Default List State

When the owner opens Staff Accounts, including when returning from Create or Edit:

- Search text is empty.
- Staff Type is All.
- Account Status is All.
- The first page is displayed.
- Display current staff information, ordered by newest creation first.

Do not restore the previous search, filters, or page number.

### BR-08 — Account-management Actions

- Create opens the Create Staff Account form.
- Edit opens the selected staff account’s editing flow.
- Deactivate is available for Active staff accounts only.
- Deactivate opens the separate deactivation flow for the selected account.
- Creation, editing, and deactivation rules are covered by their corresponding stories.

## Acceptance Criteria

### AC-01 — View Staff Accounts

Display only staff accounts belonging to the signed-in owner’s café.

Each record must display:

- Name.
- Email Address.
- Staff Type: Cashier or Delivery Staff.
- Account Status: Active or Inactive.
- Edit action.
- Deactivate action for Active accounts only.

Additional requirements:

- Include Active and Inactive accounts.
- Display each account once.
- Display newest-created accounts first.
- Do not display a Password column or expose password values.

### AC-02 — Search Staff

- Provide a search field for name or email address.
- Support partial and complete matching.
- Update results during typing without requiring a Search action.
- Match against name or email within the owner’s café only.
- Retain both selected filters.
- Return to the first page when search text changes.
- Clearing search removes only the search restriction.

### AC-03 — Filter by Staff Type

Provide:

- All — default.
- Cashier.
- Delivery Staff.

When the selection changes:

- Apply it with the current search and Account Status filter.
- Return to the first page.
- Retain search text and Account Status selection.

### AC-04 — Filter by Account Status

Provide:

- All — default.
- Active.
- Inactive.

When the selection changes:

- Apply it with the current search and Staff Type filter.
- Return to the first page.
- Retain search text and Staff Type selection.

### AC-05 — Combine Search and Filters

- Display only accounts matching the search and both selected filters.
- All imposes no restriction for its respective filter.
- Allow search and either filter to be changed independently.
- Clearing one criterion must not clear the others.
- Retain all criteria when no records match.

### AC-06 — Paginate Results

- Display up to 25 matching accounts per page.
- Display the current page number and total number of pages when results exist.
- Calculate total pages from matching staff accounts belonging to the owner’s café.
- Provide Previous and Next.
- Previous opens the previous page.
- Next opens the next page.
- Disable Previous on the first page.
- Disable Next on the last page.
- Disable both controls when only one page exists.
- Preserve search, filters, and ordering during page navigation.
- When no results exist, do not display active pagination controls or an invalid page count.

### AC-07 — Open Create Staff Account

- Provide a Create action, including when the list is empty.
- Selecting Create opens the Create Staff Account form.
- Account creation is covered by its separate story.

### AC-08 — Open Edit Staff Account

- Provide Edit for each listed staff account.
- Selecting Edit opens the selected account’s editing flow.
- The selected account must belong to the signed-in owner’s café.
- Editing behaviour is covered by its separate story.

### AC-09 — Open Deactivation Flow

- Provide Deactivate for Active accounts only.
- Selecting Deactivate opens the separate deactivation flow for that account.
- Do not offer Deactivate for an Inactive account.
- Confirmation, delivery-order restrictions, status changes, and deactivation errors are covered by the deactivation story.
- Viewing the list alone must not change account status.

### AC-10 — Return from Create or Edit

When the owner returns from either flow:

- Load the current staff information.
- Clear search text.
- Reset Staff Type and Account Status to All.
- Display the first page.
- Order accounts by newest creation first.
- Reflect successfully saved changes.

### AC-11 — Handle Empty Results

If the café has no staff accounts or no accounts match the search and filters:

- Display “No staff accounts found.”
- Keep Create available.
- Retain current search text and filter selections.
- Allow the owner to change or clear criteria.

### AC-12 — Handle Loading, Search, or Filter Failure

If the staff list or updated results cannot be loaded:

- Display “Unable to load staff accounts. Please try again.”
- Provide Retry.
- Retain the search and filter criteria associated with the failed request.
- Retry attempts to load results using those criteria.
- Do not present loading failure as an empty list.
- Do not present earlier results as matching a failed new search or filter request.

### AC-13 — Prevent Cross-café Access

- Prevent the owner from viewing or accessing another café’s staff accounts.
- Apply the restriction to the list, search, filters, pagination, and action destinations.
- Enforce the restriction for direct URLs and requests.
- Do not expose another café’s staff information.

## Acceptance Examples

| Scenario | Expected Result |
| --- | --- |
| Owner opens Staff Accounts | Empty search, both filters set to All, first page, newest accounts first |
| Café has Active and Inactive staff | Display both when Account Status is All |
| Owner enters part of a name | Update matching café staff while typing |
| Owner enters part of an email | Update matching café staff while typing |
| Delivery Staff and Active are selected | Display only active delivery staff matching the search |
| Owner changes search or a filter on page 3 | Return to page 1 of the updated results |
| There are 26 matching accounts | Show 25 on page 1 and 1 on page 2 |
| There are 25 matching accounts | Show one page and disable Previous and Next |
| Café has no staff | Show “No staff accounts found.” and keep Create available |
| No accounts match | Show the empty message and retain search and filters |
| Owner selects Create | Open the creation form |
| Owner selects Edit | Open the selected café staff account’s editing flow |
| Account is Active | Provide Deactivate |
| Account is Inactive | Do not provide Deactivate |
| Owner selects Deactivate | Open the separate deactivation flow for that account |
| Owner returns from Create or Edit | Refresh data and reset search, filters, and pagination |
| List loading fails | Display the error and Retry |
| Owner requests another café’s staff account | Deny access without exposing its information |
| Owner views the list | Do not expose passwords |

## Attachments

| File | Description |
| --- | --- |
| attachments/image-20261010-151915.png | Staff Accounts wireframe |

Review the wireframe against these updated requirements before approval, particularly:

- Name and email search.
- Staff Type and Account Status filters.
- Pagination with 25 records per page.
- Deactivate visibility for Active accounts only.
- Empty and loading-error states.
- No password display.

## Related Requirements

- Café Owner authentication and café association.
- Create Staff Account.
- Edit Staff Account.
- Deactivate Staff Account.
- Staff account types and statuses.

## Out of Scope

- Creating or editing accounts within the list itself.
- Performing deactivation within this story.
- Defining deactivation confirmation or restrictions for assigned delivery orders.
- Reactivating or deleting staff accounts.
- Displaying or revealing passwords.
- Viewing another café’s staff accounts.
- Additional search fields or filters.
- Technical implementation planning and code.