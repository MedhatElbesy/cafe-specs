# CP-55 — View Cafés Profile

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-50
- **Feature Slug:** view-cafe-profile

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-55
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed

## User Story

**As a** Platform Admin,  
**I want** to view, search, and filter registered cafés and open their profiles,  
**so that** I can quickly find a café and access its information.

## Trigger

Dashboard → Cafés

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view cafés.

## Business Rules

### BR-01 — Access Permissions

Only Platform Admins with permission to view cafés can access the list, search, and filters.

This restriction applies to navigation, direct URLs, and requests.

### BR-02 — List Coverage and Ordering

- Include all registered cafés, including initial café records whose information is incomplete.
- Display each café once as a separate record.
- Display the newest registered cafés first.
- Display “—” for missing information, including café name, owner name, or address.
- An incomplete record must still provide View Profile for the corresponding café.

### BR-03 — Participation and Operating Statuses

Participation statuses are:

- Active.
- Suspended.

Operating statuses are:

- Open.
- Closed.

When a café is suspended, its operating status automatically becomes Closed.

The list displays the resulting statuses. This page does not provide actions to suspend cafés or change operating status.

### BR-04 — Search

- Search by café name using partial matching.
- Update results as the admin types.
- An empty search displays all cafés matching the selected filters.
- A café without a name remains visible when the search is empty and the filters match.
- A café without a name does not match a non-empty café-name search.

### BR-05 — Combined Filters

Provide independent Participation Status and Operating Status filters.

Apply the search and both filters together. A record must satisfy every selected condition to appear.

All means no restriction for that particular filter.

### BR-06 — Pagination

- Display up to 25 cafés per page.
- Apply pagination after search and filtering.
- Changing search text or either filter returns to the first page.
- Preserve search text and both filter selections while navigating between pages.

### BR-07 — Default List State

When the admin opens the list, including when returning from a café profile:

- Search text is empty.
- Participation Status is All.
- Operating Status is All.
- The first page is displayed.
- The newest registered cafés appear first.

Do not restore previous search text, filter selections, or page number.

### BR-08 — Read-only List

This page displays café information and provides navigation to café profiles. It does not allow editing café information or statuses.

## Acceptance Criteria

### AC-01 — View Registered Cafés

Each record must display:

- Café Name.
- Owner Name.
- Address.
- Participation Status: Active or Suspended.
- Operating Status: Open or Closed.
- View Profile action.

Additional requirements:

- Include cafés with incomplete profile information.
- Display “—” for missing information.
- Display each café once.
- Display the newest registered cafés first.
- Display current participation and operating statuses.
- A suspended café must display Closed as its operating status.

### AC-02 — Search Cafés

- Allow the admin to search by part or all of a café name.
- Update matching results during typing without requiring a Search action.
- Return to the first page whenever search text changes.
- Retain both selected filters while searching.
- Clearing the search displays all cafés matching the selected filters.

### AC-03 — Filter by Participation Status

Provide:

- All — default.
- Active.
- Suspended.

When the selection changes:

- Apply it with the existing search and Operating Status filter.
- Return to the first page.
- Retain the search text and Operating Status selection.

### AC-04 — Filter by Operating Status

Provide:

- All — default.
- Open.
- Closed.

When the selection changes:

- Apply it with the existing search and Participation Status filter.
- Return to the first page.
- Retain the search text and Participation Status selection.

### AC-05 — Apply Search and Filters Together

- Display only cafés matching the search text and both selected filters.
- Participation Status = All does not restrict participation status.
- Operating Status = All does not restrict operating status.
- Selecting Suspended and Open returns no matching cafés because suspension automatically closes the café.

### AC-06 — Paginate Results

- Display up to 25 matching records per page.
- Display the current page number and total number of pages when results exist.
- Calculate total pages from the matching results.
- Next opens the next page.
- Previous opens the previous page.
- Disable Previous on the first page.
- Disable Next on the last page.
- If only one page exists, disable both controls.
- Retain search text and both filters during page navigation.
- If no results exist, do not display active pagination controls or an invalid page count.

### AC-07 — Open Café Profile

- Provide View Profile for each café, including incomplete records.
- Selecting it opens the selected café’s profile.
- The displayed profile must correspond to that café.
- Profile details are covered by CP-125 — View Café Profile Details.

### AC-08 — Return from Café Profile

When the admin returns to the Cafés list:

- Clear the search text.
- Reset both filters to All.
- Display the first page.
- Display the newest registered cafés first.

### AC-09 — Handle Empty Results

If no cafés are registered or no cafés match the search and selected filters:

- Display “No cafés found.”
- Retain the current search text and both selected filters.
- Allow the admin to change or clear the search and change either filter.

### AC-10 — Handle Loading Failure

If the café list cannot be loaded:

- Display “Cafés could not be loaded. Please try again.”
- Provide a Retry action.
- Retain the search text and both selected filters.
- Retry attempts to load the list using the retained search and filters.
- Do not display a loading failure as an empty result.

### AC-11 — Prevent Unauthorized Access

- Deny access to admins without permission to view cafés.
- Apply the restriction to navigation, direct URLs, and requests.
- Do not expose café records to unauthorized users.

## Acceptance Examples

| Scenario                                          | Expected Result                                                               |
| ------------------------------------------------- | ----------------------------------------------------------------------------- |
| Admin opens Cafés                                 | Empty search, both filters set to All, first page, newest registrations first |
| Café profile is incomplete                        | Include the café and display “—” for missing information                      |
| Café has no name and search is empty              | Include it if the selected filters match                                      |
| Café has no name and search is non-empty          | Exclude it from café-name search results                                      |
| Admin enters part of a café name                  | Update matching results while typing                                          |
| Admin selects Active and Open                     | Display only active, open cafés matching the search                           |
| Café is suspended                                 | Display Suspended and Closed                                                  |
| Admin selects Suspended and Open                  | Display “No cafés found.”                                                     |
| Admin changes search or either filter on page 3   | Return to page 1 of the updated results                                       |
| There are 26 matching cafés                       | Display 25 on page 1 and 1 on page 2                                          |
| There are 25 matching cafés                       | Display one page and disable Previous and Next                                |
| Admin selects View Profile for an incomplete café | Open that café’s profile                                                      |
| Admin returns from a profile                      | Reset search, filters, and page to defaults                                   |
| No cafés match                                    | Display the empty-state message and retain search and filters                 |
| Loading fails                                     | Display the loading error and Retry; retain search and filters                |
| Admin lacks permission                            | Deny access without exposing café records                                     |

## Attachments

| File                                                   | Description                              |
| ------------------------------------------------------ | ---------------------------------------- |
| attachments/Cafés — list, search and filter@2x (1).png | Cafés list, search, and filter wireframe |

The wireframe must be reviewed against these updated requirements before approval.

## Related Requirements

- CP-113 — Create Café.
- CP-125 — View Café Profile Details.
- Café profile completion.
- Café participation and suspension rules.
- Café opening and closing rules.
- Admin permissions.

## Out of Scope

- Creating café accounts.
- Editing café or owner information.
- Suspending or reactivating cafés from this page.
- Manually changing operating status from this page.
- Additional search fields or filters.
- Preserving list state when returning from a café profile.
- Café profile details, covered by CP-125.
- Technical implementation planning and code.
