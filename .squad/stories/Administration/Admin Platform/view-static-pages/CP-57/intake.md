# CP-57 — View Static Pages

## Feature

- **Project:** Rashfa
- **Epic:** Administration — CP-49
- **Parent Feature:** Admin Platform — CP-50
- **Feature Slug:** view-static-pages

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-57
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Maram Atef Saeed

## User Story

**As a** Platform Admin,  
**I want** to view predefined static pages and their currently published Arabic content,  
**so that** I can review the information available to subscribers.

## Trigger

Dashboard → Static Pages → Select Page

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to view static pages.

## Business Rules

### BR-01 — Predefined Pages

Display the following pages in this fixed order:

1. Terms and Conditions — الشروط والأحكام.
2. Privacy Policy — سياسة الخصوصية.
3. About Us — من نحن.
4. Contact Us — تواصل معنا.
5. FAQ — الأسئلة الشائعة.

All five page records remain visible even when they have no published content.

Search and pagination are not required.

### BR-02 — Published Content

- Display only the currently published title and content.
- Display the same published content version available to subscribers in the app.
- Do not display unpublished edits or drafts as published content.
- Opening or viewing a page must not modify its content or publication state.

### BR-03 — Content Language

- Static-page content is Arabic only.
- Display Arabic content using right-to-left direction.
- Preserve published formatting, paragraph order, and FAQ question-and-answer associations.
- A content-language selector is not required.

### BR-04 — Last Updated Information

- Last Updated Date and Last Updated By refer to the latest update of the currently published version.
- Unpublished edits must not change the displayed publication metadata.
- Display “—” for an unavailable date or admin name independently.
- Do not invent metadata when no publication or update has occurred.
- Display dates using Asia/Riyadh, Saudi Arabia time (UTC+3).

### BR-05 — FAQ Content

- Display published questions with their corresponding answers as a list.
- Preserve the same question order used in the subscriber app.
- Do not display unpublished FAQ entries.
- If no published FAQ entries exist, display the standard unpublished-content message.

### BR-06 — View and Edit Permissions

- Viewing requires View Static Pages permission.
- Editing requires Edit Static Pages permission.
- Admins with viewing permission can read content without editing permission.
- Show Edit only to admins with the required editing permission.
- Edit opens the corresponding page’s separate editing flow.
- Showing Edit does not make the viewing screen editable.
- Enforce permissions for navigation, direct URLs, and requests.

## Acceptance Criteria

### AC-01 — View Static Pages List

Display all five predefined pages in the order specified in BR-01.

Each record must display:

- Page Title.
- Last Updated Date.
- Name of the Admin Who Last Updated the Published Page.

Requirements:

- Keep pages with no published content in the list.
- Display “—” for unavailable update metadata.
- Do not provide search or pagination.

### AC-02 — View Published Page Content

When the admin selects a page:

- Open the selected page.
- Display its currently published Arabic title and content.
- Display the same published version available in the subscriber app.
- Preserve the published content’s formatting.
- Display Arabic text right-to-left.
- Do not expose unpublished changes.

### AC-03 — View FAQ

When the admin selects FAQ:

- Display the published questions and answers as a list.
- Keep each answer associated with its question.
- Preserve the question order used in the subscriber app.
- Display the content right-to-left.

### AC-04 — Handle Unpublished Content

If a selected static page has no published content:

- Display “No content published yet”.
- Keep the selected page identifiable.
- Do not display draft content as a substitute.
- Keep Back to Static Pages available.
- Show Edit only if the admin has editing permission.

If FAQ has no published questions and answers, apply the same behaviour.

### AC-05 — Display Last Updated Information

- Display the latest update date and admin name associated with the currently published version.
- Use Asia/Riyadh for the date.
- If either value is unavailable, display “—” for that value.
- An unpublished edit must not replace the displayed metadata of the published version.

### AC-06 — Enforce View-only Access

For an admin with viewing permission but without editing permission:

- Allow access to the list and published content.
- Do not display Edit.
- Do not allow content modification.
- Prevent access to editing through direct URLs or requests.

### AC-07 — Open Editing Flow

For an admin with viewing and editing permissions:

- Display Edit for the selected page.
- Selecting Edit opens that page’s separate editing flow.
- Support access to editing for pages with or without published content.
- Keep editing, saving, and publishing behaviour outside this story.

### AC-08 — Return to Static Pages

Provide Back to Static Pages from each page view.

Selecting it returns to the predefined list in its fixed order.

### AC-09 — Handle List Loading Failure

If the static-pages list cannot be loaded:

- Display “Unable to load static pages. Please try again.”
- Provide Retry.
- Retry attempts to load the list again.
- Do not present a loading failure as an empty list.

### AC-10 — Handle Content Loading Failure

If the selected page’s content cannot be loaded:

- Display “Unable to load page content. Please try again.”
- Provide Retry for the selected page.
- Keep Back to Static Pages available.
- Do not display “No content published yet” unless the system has confirmed that no published content exists.

### AC-11 — Prevent Unauthorized Access

- Deny list and page access without View Static Pages permission.
- Enforce restrictions for navigation, direct URLs, and requests.
- Do not expose restricted content through unauthorized requests.
- Enforce editing permission independently of viewing permission.

## Acceptance Examples

| Scenario                                        | Expected Result                                                           |
| ----------------------------------------------- | ------------------------------------------------------------------------- |
| Admin opens Static Pages                        | Display all five pages in the fixed order without search or pagination    |
| A page has no published content                 | Keep its record visible in the list                                       |
| Admin opens a published page                    | Display the same published Arabic content available in the subscriber app |
| Unpublished edits exist                         | Continue displaying the current published version                         |
| Admin opens FAQ                                 | Display published questions and answers in the app’s order                |
| FAQ has no published entries                    | Display “No content published yet”                                        |
| Update date or admin name is unavailable        | Display “—” for the unavailable value                                     |
| A draft is changed                              | Do not change the displayed metadata of the published version             |
| Admin has viewing permission only               | Allow viewing and hide Edit                                               |
| Admin has viewing and editing permissions       | Show Edit and open the selected page’s editing flow                       |
| Selected page has no content and admin can edit | Display the empty state and Edit                                          |
| Content loading fails                           | Show the loading error and Retry, not the unpublished-content message     |
| Admin selects Back to Static Pages              | Return to the fixed list                                                  |
| Admin lacks viewing permission                  | Deny access                                                               |

## Attachments

| File                                                              | Description                                         |
| ----------------------------------------------------------------- | --------------------------------------------------- |
| attachments/FAQ — list layout, view-only access@2x.png            | Published FAQ list and view-only access             |
| attachments/Static page — no content published yet@2x.png         | Unpublished-content state                           |
| attachments/Static page — published content, with Edit@2x (1).png | Published content with permission-based Edit action |

Review the wireframes against these requirements before approval, particularly:

- Arabic content and right-to-left display.
- Published-version metadata.
- Permission-based Edit visibility.
- Loading errors versus unpublished-content states.

## Related Requirements

- Static-page editing and publishing.
- Subscriber app static-page display.
- FAQ content and ordering.
- Admin roles and permissions.

## Out of Scope

- Editing, saving, or publishing content within this viewing flow.
- Creating or deleting predefined page types.
- Managing FAQ entries or their order.
- Draft previews and version-history browsing.
- English content and language selection.
- Search and pagination.
- Technical implementation planning and code.
