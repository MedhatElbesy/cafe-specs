# Functional Specification — CP-58: Edit Static Pages

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-58 |
| Story Title | Edit Static Pages |
| Epic | Administration (CP-49) |
| Feature | Admin Platform (CP-50) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Maram Atef Saeed |
| Labels | Not specified in Jira |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** edit and publish static-page content from the dashboard,  
**So that** subscribers can access up-to-date legal and informational content without requiring a new app release.

The functionality shall support all existing Static Pages, including Terms and Conditions and FAQ.

For applicable non-FAQ pages, the Platform Admin shall be able to edit the Page Title and rich-text Content.

For the FAQ page, the Platform Admin shall be able to add, edit, delete, and reorder FAQ items.

Successfully saved changes shall be published immediately and made available to subscribers when they next open the relevant page.

## 3. Trigger

Dashboard → Static Pages → Select Page → Edit

1. The Platform Admin navigates to Static Pages from the dashboard.
2. The Platform Admin selects an existing Static Page.
3. The Platform Admin selects Edit.
4. The system opens the corresponding Editor.
5. Where published content exists, the Editor displays the currently published Page Title and Content.
6. For the FAQ page, the Editor provides the applicable FAQ item management controls.
7. If the page has no published content, the Editor opens with empty content and displays the confirmed empty-content message.

## 4. Preconditions

1. The user is authenticated in the Admin Platform.
2. The user is a Platform Admin with the required permission to edit Static Pages.
3. The Static Pages functionality is available in the Admin Platform.
4. The Static Page to be edited already exists.
5. The Platform Admin has selected the relevant page and initiated Edit.
6. The system can identify the selected page and its currently published content, if any.

**Page Type Consideration**

The editing interface shall support the applicable content structure for the selected Static Page.

The supplied wireframes establish two illustrated editing experiences:

- Terms and Conditions: Page Title and rich-text Content.
- FAQ: FAQ item management, including Question, Answer, and ordering controls.

The specific layouts for other existing Static Page types are not defined by the supplied wireframes.

**Publication Consideration**

Save is the action used to save and publish changes. There is no separate Draft or Approval workflow within CP-58.

## 5. Fields and Validation

### 5.1 Static Page Editor — General Elements

| Element | Type | Required Behavior |
|---|---|---|
| Selected Page | Page context | Identifies the Static Page being edited |
| Page Title | Text input | Displays the existing published title and supports editing |
| Content | Rich-text editor | Displays existing published content and supports the confirmed formatting capabilities |
| Save | Button | Saves and immediately publishes valid changes |
| Cancel | Action | Initiates leave-editor behavior, including confirmation when unsaved changes exist |
| Navigation Controls | Navigation actions | Allow leaving the Editor, subject to unsaved-changes confirmation |
| Validation Messages | Inline feedback | Identify missing required fields |
| Publication Failure Dialog | Error dialog | Displays the confirmed failure message and provides OK |

The FAQ page additionally supports the FAQ-specific controls defined in Section 5.4.

### 5.2 Page Title

| Attribute | Specification |
|---|---|
| Field | Page Title |
| Type | Text input |
| Mandatory | Yes |
| Initial Value | Currently published Page Title, if available |
| Editable | Yes |
| Empty Value | Invalid |
| Whitespace-only Value | Invalid |
| Validation Timing | On field blur and Save |
| Error Display | Inline |
| Exact Error Message | `Page Title is required.` |

**Validation Behavior**

1. The system shall pre-fill Page Title with the currently published value when one exists.
2. The Platform Admin shall be able to edit the title.
3. Page Title shall be mandatory.
4. A blank or whitespace-only Page Title shall be considered empty.
5. When the field loses focus, the system shall validate whether it contains a non-empty value.
6. When Save is selected, the system shall validate Page Title.
7. If Page Title is empty, the system shall display the inline message `Page Title is required.`.
8. Publication shall be prevented while Page Title is invalid.
9. A change to Page Title shall count as a publishable change.
10. Restoring Page Title to its originally loaded value shall remove that field's contribution to the unsaved-changes state.

No maximum character length or additional character-format rule has been confirmed.

### 5.3 Rich-Text Content

| Attribute | Specification |
|---|---|
| Field | Content |
| Type | Rich-text editor |
| Mandatory | Yes for applicable page content |
| Initial Value | Currently published Content, if available |
| Editable | Yes |
| Empty Value | Invalid |
| Whitespace-only Value | Invalid |
| Validation Timing | On field blur and Save |
| Error Display | Inline |
| Exact Error Message | `Content is required.` |

**Supported Formatting**

The rich-text editor shall support the following five capabilities:

1. Headings
2. Bold text
3. Bulleted lists
4. Numbered lists
5. Hyperlinks

**Content Behavior**

1. The Editor shall display the currently published Content when available.
2. The Platform Admin shall be able to edit the Content.
3. Content shall be mandatory for the applicable rich-text page editor.
4. Empty or whitespace-only Content shall be invalid.
5. The system shall validate Content on field blur and Save.
6. If Content is empty, the system shall display `Content is required.` inline.
7. Invalid Content shall prevent publication.
8. The editor shall support the five confirmed formatting capabilities.
9. Supported formatting shall be preserved when the published content is displayed in the Admin Platform.
10. Supported formatting shall also be preserved when the published content is displayed in the subscriber app.
11. Changes to Content, including its supported formatting, shall be considered publishable changes.
12. Rich-text editing capabilities beyond the five specified capabilities are not required by CP-58.

No additional hyperlink-validation rules or rich-text size limits have been confirmed.

### 5.4 FAQ Editor

The FAQ Editor shall allow the Platform Admin to manage FAQ items through the Admin Platform.

Each FAQ item shall contain a Question and an Answer.

The Editor shall support adding, editing, deleting, and reordering FAQ items.

#### 5.4.1 FAQ Fields

| Element | Type | Mandatory | Validation / Behavior |
|---|---|---|---|
| Question | Plain-text single-line input | Yes | Must contain a non-empty value |
| Answer | Plain-text multiline field | Yes | Must contain a non-empty value |
| Add FAQ Item | Action | N/A | Adds an FAQ item to the pending editor content |
| Delete FAQ Item | Action | N/A | Removes the item from the pending editor content |
| Reorder FAQ Item | Action | N/A | Changes the item's pending display order |
| FAQ Items Section | Collection | At least one item required | Publication is blocked when no FAQ items exist |

#### 5.4.2 Question Validation

| Attribute | Specification |
|---|---|
| Field | Question |
| Type | Plain-text single-line |
| Mandatory | Yes |
| Empty Value | Invalid |
| Whitespace-only Value | Invalid |
| Validation Timing | On blur and Save |
| Error Display | Inline |
| Exact Error Message | `Question is required.` |

**Behavior**

1. Each FAQ item shall have a Question.
2. The Question shall use a plain-text single-line input.
3. An empty or whitespace-only Question shall be invalid.
4. The system shall validate the Question on field blur and Save.
5. If the Question is empty, the system shall display `Question is required.` beneath or near the incomplete field.
6. Publication shall be prevented while any FAQ item has an invalid Question.

#### 5.4.3 Answer Validation

| Attribute | Specification |
|---|---|
| Field | Answer |
| Type | Plain-text multiline |
| Mandatory | Yes |
| Empty Value | Invalid |
| Whitespace-only Value | Invalid |
| Validation Timing | On blur and Save |
| Error Display | Inline |
| Exact Error Message | `Answer is required.` |

**Behavior**

1. Each FAQ item shall have an Answer.
2. The Answer shall use a plain-text multiline field.
3. An empty or whitespace-only Answer shall be invalid.
4. The system shall validate the Answer on field blur and Save.
5. If the Answer is empty, the system shall display `Answer is required.` beneath or near the incomplete field.
6. Publication shall be prevented while any FAQ item has an invalid Answer.
7. Rich-text formatting is not required for FAQ Answers under the confirmed wireframe-based decision.

#### 5.4.4 FAQ Minimum Item Count

| Attribute | Specification |
|---|---|
| Minimum FAQ Items | One |
| Minimum Valid Content | One complete Question and Answer |
| Zero Items | Invalid |
| Validation Timing | Save |
| Error Display | Inline near FAQ Items |
| Exact Error Message | `At least one FAQ item is required.` |

**Behavior**

1. The FAQ page shall contain at least one complete FAQ item before publication.
2. Each retained FAQ item shall contain a valid Question and Answer.
3. If the Platform Admin removes all FAQ items and selects Save, the system shall prevent publication.
4. The system shall display `At least one FAQ item is required.` near the FAQ Items section.
5. Adding at least one complete FAQ item shall satisfy the minimum-item requirement.
6. The minimum-item rule is a confirmed clarification added to the original Jira requirements.

#### 5.4.5 FAQ Add, Edit, Delete, and Reorder

1. The Platform Admin shall be able to add FAQ items.
2. The Platform Admin shall be able to edit existing FAQ Questions and Answers.
3. The Platform Admin shall be able to delete FAQ items from the pending editor content.
4. The Platform Admin shall be able to reorder FAQ items.
5. FAQ additions, edits, deletions, and reordering shall remain pending until Save/Publish succeeds.
6. These changes shall not update the subscriber-facing published FAQ content before successful publication.
7. After successful publication, the subscriber app shall display the published FAQ items in the saved order.
8. If publication fails or the changes are discarded, the previously published FAQ content and ordering shall remain unchanged.
9. Changes to FAQ items or their ordering shall count as publishable changes.

### 5.5 Save Button State

| Editor State | Save Button | Expected Behavior |
|---|---|---|
| No publishable changes | Disabled | No Save action available |
| Publishable changes with valid data | Enabled | Save validates and publishes if successful |
| Publishable changes with invalid data | Enabled | Save validates, displays inline errors, and blocks publication |
| All changes reverted to original values | Disabled | No unsaved changes remain |
| FAQ item added, edited, deleted, or reordered | Enabled while the changes remain | Save validates and publishes if valid |
| Publication failure | Editor remains open | Unsaved edits remain available |

**Save State Rules**

1. Save shall remain disabled when the current publishable content matches the originally loaded version.
2. Save shall become enabled when a publishable change exists.
3. Save shall remain enabled when changes exist even if the changed content contains invalid required-field values.
4. Selecting Save with invalid data shall trigger validation and prevent publication.
5. If all changes are reverted, Save shall become disabled again.
6. For the FAQ page, additions, edits, deletions, and ordering changes shall be included in change detection.
7. A field interaction that does not result in a difference from the originally loaded content shall not independently establish an unsaved change.

### 5.6 Validation and Publication

| Attribute | Specification |
|---|---|
| Action | Save |
| Purpose | Validate, save, and publish changes |
| Validation Timing | On Save, in addition to field blur |
| Publication Timing | Immediately after successful Save |
| Separate Draft Workflow | No |
| Separate Approval Workflow | No |
| Success Navigation | Updated Page Details |
| Success Feedback | Success message |
| Invalid Data | Publication blocked |

**Behavior**

1. Selecting Save shall validate the applicable required fields.
2. For rich-text pages, Page Title and Content shall be mandatory.
3. For FAQ items, each Question and Answer shall be mandatory.
4. The FAQ page shall contain at least one complete FAQ item.
5. If validation fails, the system shall prevent saving and publishing.
6. The system shall display the applicable inline validation messages.
7. If validation passes and Save succeeds, the system shall save and immediately publish the updated content.
8. The system shall display a success message.
9. The system shall show the updated published content on Page Details.
10. The published version shall become available to subscribers when they next open the relevant page.

**Wireframe Reference — Success Message**

The supplied success wireframe displays:

`Changes saved and published`

This text is a wireframe reference. Its exact wording has not been separately confirmed as a business decision.

### 5.7 Unsaved Changes and Navigation

| Attribute | Specification |
|---|---|
| Applies To | Cancel and all navigation attempts that leave the Editor |
| Condition | Current publishable content differs from the originally loaded version |
| Confirmation | Required when unsaved changes exist |
| Dialog Title | `Discard unsaved changes?` — wireframe reference |
| Action 1 | Discard changes |
| Action 2 | Continue editing |

**Behavior**

1. The system shall compare the current publishable content with the originally loaded version.
2. The comparison shall include the applicable Page Title, Content, FAQ items, and FAQ ordering.
3. If no unsaved changes exist, the Platform Admin may leave the Editor without an unsaved-changes confirmation.
4. If unsaved changes exist, selecting Cancel shall display the unsaved-changes confirmation.
5. The same confirmation shall apply to other navigation attempts that leave the Editor, including applicable Back, breadcrumb, and sidebar navigation.
6. The confirmation shall provide **Discard changes** and **Continue editing**.
7. Selecting Discard changes shall discard all unsaved edits and proceed with the requested navigation away from the Editor.
8. Selecting Continue editing shall close the confirmation and keep the Editor open with all unsaved edits preserved.
9. Discarding edits shall not change the previously published version.
10. If the Platform Admin restores all content to its originally loaded state, the system shall treat the Editor as having no unsaved changes.

### 5.8 Publication Failure

| Attribute | Specification |
|---|---|
| Condition | Save/Publish fails |
| Feedback Type | Error dialog |
| Exact Message | `Something went wrong. Please try again.` |
| Action | OK |
| Navigation After OK | Remain on Editor |
| Unsaved Edits | Preserved |
| Previously Published Content | Unchanged |
| Last Updated | Unchanged |
| Updated By | Unchanged |

**Behavior**

1. If Save or publication fails, the system shall display an error dialog.
2. The dialog shall contain the exact message `Something went wrong. Please try again.`.
3. The dialog shall provide OK.
4. Selecting OK shall close the error dialog.
5. The Platform Admin shall remain on the Editor.
6. The current unsaved edits shall remain available for correction or retry.
7. The previously published version shall remain unchanged.
8. The system shall not treat the failed operation as a successful publication.
9. The Last Updated date/time and Updated By information shall remain unchanged.
10. The subscriber app shall continue to access the previously published version.

### 5.9 No Published Content

| Attribute | Specification |
|---|---|
| Condition | Selected Static Page has no published content |
| Editor Access | Allowed |
| Content State | Empty |
| Exact Message | `No content has been published` |
| Publication Requirement | Valid content required |

**Behavior**

1. The system shall allow the Platform Admin to open the Editor for a Static Page with no published content.
2. The Editor shall indicate the absence of published content.
3. The system shall display `No content has been published`.
4. The content fields shall initially be empty where no published values exist.
5. The Platform Admin shall be able to enter the applicable page content.
6. Publication shall be prevented until the applicable required fields and FAQ requirements are satisfied.
7. Once valid changes are successfully saved, the content shall be published immediately.

### 5.10 Last Updated and Updated By

| Attribute | Specification |
|---|---|
| Last Updated | Date and time of latest successful publication |
| Updated By | Name of the Platform Admin who successfully published the changes |
| Update Trigger | Successful publication |
| Display Locations | Static Pages List and Page Details |
| Failed Publication | No metadata changes |
| FAQ-only Changes | Metadata updated after successful publication |

**Behavior**

1. After each successful publication, the system shall record the name of the Platform Admin who published the changes.
2. The system shall record the date and time of the successful publication.
3. The latest successful publication information shall be displayed on Page Details.
4. The latest successful publication information shall also be displayed in the Static Pages List.
5. Successfully published FAQ additions, edits, deletions, or reordering shall update the publication metadata.
6. Failed publication attempts shall not update Last Updated or Updated By.
7. Discarded changes shall not update publication metadata.

The exact date/time format and timezone are not specified in the confirmed requirements.

### 5.11 Subscriber Visibility and Published Version Consistency

| Attribute | Specification |
|---|---|
| Publication | Immediate after successful Save |
| Subscriber Availability | When subscriber next opens the relevant page |
| App Update Required | No |
| Version Consistency | Admin Platform and subscriber app display the same published version |
| Formatting | Supported formatting preserved |
| FAQ Order | Saved published order |

**Behavior**

1. Successfully saved changes shall become the published version immediately.
2. When a subscriber next opens the relevant Static Page, the subscriber app shall make the latest published content available.
3. Subscribers shall not need to install a new app release to access the latest published content.
4. The Admin Platform and subscriber app shall display the same published content version.
5. Supported rich-text formatting shall be preserved across both interfaces.
6. Published FAQ items shall appear in the saved order.
7. Failed publication attempts shall not replace the previously published content.

## 6. Business Rules

**BR-01 — Authorized Editing**

The system shall provide Edit Static Pages functionality to Platform Admins with the required permission.

**BR-02 — Supported Pages**

The functionality shall support all existing Static Pages, including Terms and Conditions and FAQ.

**BR-03 — Entry Point**

The Platform Admin shall access the Editor through Dashboard → Static Pages → Select Page → Edit.

**BR-04 — Published Content Pre-Fill**

The Editor shall display the currently published Page Title and Content when available.

**BR-05 — Page Title Required**

Page Title shall be mandatory for applicable page editing.

**BR-06 — Content Required**

Rich-text Content shall be mandatory for applicable non-FAQ page editing.

**BR-07 — Whitespace Validation**

Whitespace-only required-field values shall be treated as empty.

**BR-08 — Validation Timing**

Required-field validation shall occur on field blur and Save.

**BR-09 — Page Title Error**

An empty Page Title shall display `Page Title is required.` inline.

**BR-10 — Content Error**

Empty rich-text Content shall display `Content is required.` inline.

**BR-11 — Supported Rich-Text Formatting**

The editor shall support Headings, Bold text, Bulleted lists, Numbered lists, and Hyperlinks.

**BR-12 — Formatting Preservation**

Supported formatting shall be preserved when published content is displayed in the Admin Platform and subscriber app.

**BR-13 — FAQ Item Management**

The FAQ Editor shall support adding, editing, deleting, and reordering FAQ items.

**BR-14 — FAQ Question Type**

FAQ Question shall be a plain-text single-line field.

**BR-15 — FAQ Answer Type**

FAQ Answer shall be a plain-text multiline field.

**BR-16 — FAQ Question Required**

Each FAQ item shall contain a non-empty Question.

**BR-17 — FAQ Answer Required**

Each FAQ item shall contain a non-empty Answer.

**BR-18 — FAQ Question Error**

An empty FAQ Question shall display `Question is required.` inline.

**BR-19 — FAQ Answer Error**

An empty FAQ Answer shall display `Answer is required.` inline.

**BR-20 — FAQ Minimum Item Count**

At least one complete FAQ item shall be required before FAQ publication.

**BR-21 — Zero FAQ Items Error**

If the FAQ page contains no FAQ items, the system shall display `At least one FAQ item is required.` near the FAQ Items section when Save validation occurs.

**BR-22 — FAQ Validation Scope**

All FAQ items shall be validated before publication.

**BR-23 — Pending FAQ Changes**

FAQ additions, edits, deletions, and reordering shall remain unpublished until Save succeeds.

**BR-24 — FAQ Published Order**

Successfully published FAQ items shall be displayed in the saved order.

**BR-25 — Save Disabled Without Changes**

Save shall remain disabled when no publishable content has changed.

**BR-26 — Save Enabled With Changes**

Save shall become enabled when publishable content differs from the originally loaded version.

**BR-27 — Save Enabled With Invalid Changes**

Save shall remain enabled when changes exist even if required fields are invalid.

**BR-28 — Validation Before Publication**

Selecting Save shall validate applicable required fields and block publication when validation fails.

**BR-29 — Reverted Changes**

When all edits are reverted to the originally loaded version, Save shall become disabled and the Editor shall be treated as having no unsaved changes.

**BR-30 — Change Detection Scope**

Change detection shall include the applicable Page Title, Content, FAQ items, and FAQ ordering.

**BR-31 — Immediate Publication**

A successful Save shall immediately publish the updated content.

**BR-32 — No Separate Approval**

CP-58 shall not require a separate Draft or Approval workflow before publication.

**BR-33 — Successful Publication Feedback**

After successful publication, the system shall display a success message and show the updated content on Page Details.

**BR-34 — Navigation Confirmation**

When unsaved changes exist, Cancel and all other navigation attempts that leave the Editor shall require confirmation.

**BR-35 — Discard Changes**

Selecting Discard changes shall discard unsaved edits and complete the requested navigation away from the Editor.

**BR-36 — Continue Editing**

Selecting Continue editing shall close the confirmation and preserve unsaved edits in the Editor.

**BR-37 — No Confirmation Without Changes**

When the Editor contains no unsaved changes, leaving the Editor shall not require an unsaved-changes confirmation.

**BR-38 — Publication Failure Message**

A failed Save/Publish operation shall display `Something went wrong. Please try again.` in an error dialog with OK.

**BR-39 — Publication Failure Navigation**

After acknowledging the publication failure, the Platform Admin shall remain on the Editor with unsaved edits preserved.

**BR-40 — Published Content Preservation**

Failed publication and discarded edits shall leave the previously published version unchanged.

**BR-41 — No Published Content State**

If a Static Page has no published content, the Editor shall open with empty content and display `No content has been published`.

**BR-42 — Initial Publication Validation**

A page with no published content shall require valid applicable content before publication.

**BR-43 — Publication Metadata**

Every successful publication shall record the publishing admin's name and the publication date/time.

**BR-44 — Publication Metadata Display**

The latest successful publication information shall appear on Page Details and in the Static Pages List.

**BR-45 — FAQ Publication Metadata**

Successfully published FAQ-only changes, including ordering changes, shall update publication metadata.

**BR-46 — Failed Publication Metadata**

Failed publication attempts shall not update Last Updated or Updated By.

**BR-47 — Subscriber Content Availability**

The latest successfully published content shall be available when subscribers next open the relevant page.

**BR-48 — No App Release Dependency**

Subscribers shall not need an app update to access the latest published content.

**BR-49 — Published Version Consistency**

The Admin Platform and subscriber app shall display the same published content version.

**BR-50 — FAQ Plain-Text Restriction**

The confirmed FAQ Question and Answer editing controls shall use plain text rather than the non-FAQ rich-text editor.

## 7. Acceptance Criteria

### AC-01 — Open Static Page Editor

**Given** the user is a Platform Admin with the required permission,  
**And** the user has selected an existing Static Page,  
**When** the user selects Edit,  
**Then** the system shall open the Editor for that page.

### AC-02 — Display Published Content

**Given** the selected page has published content,  
**When** the Editor opens,  
**Then** the system shall pre-fill the applicable Page Title and Content with the currently published values.

### AC-03 — Support All Existing Static Pages

**Given** the Admin Platform contains existing Static Pages,  
**When** the authorized Platform Admin selects one of those pages for editing,  
**Then** the system shall provide the applicable editing functionality for that page.

### AC-04 — Validate Empty Page Title

**Given** the Platform Admin is editing a page with a Page Title field,  
**When** the Page Title is empty and loses focus,  
**Then** the system shall display `Page Title is required.` inline.

### AC-05 — Validate Empty Content

**Given** the Platform Admin is editing a page with required rich-text Content,  
**When** the Content is empty and loses focus,  
**Then** the system shall display `Content is required.` inline.

### AC-06 — Reject Whitespace-only Page Title

**Given** the Platform Admin enters only spaces in Page Title,  
**When** validation occurs,  
**Then** the system shall treat Page Title as empty,  
**And** display `Page Title is required.`.

### AC-07 — Reject Whitespace-only Content

**Given** the Platform Admin enters only whitespace in required Content,  
**When** validation occurs,  
**Then** the system shall treat Content as empty,  
**And** display `Content is required.`.

### AC-08 — Validate Required Fields on Save

**Given** one or more required fields are empty,  
**And** publishable changes exist,  
**When** the Platform Admin selects Save,  
**Then** the system shall display the applicable inline validation messages,  
**And** prevent publication.

### AC-09 — Support Rich-Text Formatting

**Given** the Platform Admin is editing rich-text Content,  
**When** the Platform Admin uses the supported formatting controls,  
**Then** the editor shall support Headings, Bold text, Bulleted lists, Numbered lists, and Hyperlinks.

### AC-10 — Preserve Published Formatting

**Given** formatted Content has been successfully published,  
**When** the published page is displayed in the Admin Platform or subscriber app,  
**Then** the supported formatting shall be preserved.

### AC-11 — Add FAQ Item

**Given** the Platform Admin is editing the FAQ page,  
**When** the Platform Admin adds an FAQ item,  
**Then** the Editor shall include the new item in the pending FAQ content.

### AC-12 — Edit FAQ Item

**Given** an FAQ item exists in the Editor,  
**When** the Platform Admin changes its Question or Answer,  
**Then** the Editor shall retain the changed value as an unsaved edit.

### AC-13 — Delete FAQ Item

**Given** an FAQ item exists in the Editor,  
**When** the Platform Admin deletes the item,  
**Then** the item shall be removed from the pending FAQ content,  
**And** the published FAQ content shall remain unchanged until Save succeeds.

### AC-14 — Reorder FAQ Items

**Given** the FAQ Editor contains multiple items,  
**When** the Platform Admin changes their order,  
**Then** the pending FAQ order shall change,  
**And** the published order shall remain unchanged until Save succeeds.

### AC-15 — Validate Empty FAQ Question

**Given** a FAQ item has an empty Question,  
**When** the Question loses focus or Save validation occurs,  
**Then** the system shall display `Question is required.` inline.

### AC-16 — Validate Empty FAQ Answer

**Given** a FAQ item has an empty Answer,  
**When** the Answer loses focus or Save validation occurs,  
**Then** the system shall display `Answer is required.` inline.

### AC-17 — Reject Whitespace-only FAQ Fields

**Given** a FAQ Question or Answer contains only whitespace,  
**When** validation occurs,  
**Then** the system shall treat that field as empty,  
**And** display its applicable required-field error.

### AC-18 — Validate All FAQ Items

**Given** the FAQ Editor contains multiple items,  
**And** one or more items have incomplete Questions or Answers,  
**When** the Platform Admin selects Save,  
**Then** the system shall identify the incomplete fields using inline errors,  
**And** prevent publication.

### AC-19 — Reject Zero FAQ Items

**Given** the Platform Admin has removed all FAQ items,  
**When** the Platform Admin selects Save,  
**Then** the system shall prevent publication,  
**And** display `At least one FAQ item is required.` near the FAQ Items section.

### AC-20 — Allow FAQ Publication With Complete Items

**Given** the FAQ page contains at least one complete FAQ item,  
**And** all applicable required content is valid,  
**And** publishable changes exist,  
**When** the Platform Admin selects Save and publication succeeds,  
**Then** the updated FAQ content shall be published.

### AC-21 — Display FAQ Items in Saved Order

**Given** the Platform Admin successfully publishes reordered FAQ items,  
**When** a subscriber next opens the FAQ page,  
**Then** the subscriber app shall display the published FAQ items in the saved order.

### AC-22 — Disable Save Without Changes

**Given** the Editor's current publishable content matches the originally loaded version,  
**When** the Platform Admin views Save,  
**Then** Save shall be disabled.

### AC-23 — Enable Save After a Change

**Given** the Platform Admin changes publishable content,  
**When** the current content differs from the originally loaded version,  
**Then** Save shall become enabled.

### AC-24 — Keep Save Enabled With Invalid Changes

**Given** publishable changes exist,  
**And** a required field is invalid,  
**When** the Platform Admin views Save,  
**Then** Save shall remain enabled,  
**And** selecting it shall trigger validation and block publication.

### AC-25 — Disable Save After Reverting Changes

**Given** the Platform Admin changes publishable content,  
**When** the Platform Admin restores all content to its originally loaded state,  
**Then** Save shall become disabled.

### AC-26 — Detect FAQ Ordering Changes

**Given** the Platform Admin changes only the order of FAQ items,  
**When** the current order differs from the originally loaded order,  
**Then** Save shall become enabled.

### AC-27 — Save and Publish Immediately

**Given** valid publishable changes exist,  
**When** the Platform Admin selects Save and the operation succeeds,  
**Then** the system shall save and immediately publish the updated content.

### AC-28 — Display Successful Publication

**Given** Save/Publish succeeds,  
**When** publication completes,  
**Then** the system shall display a success message,  
**And** show the updated published content on Page Details.

### AC-29 — Cancel With Unsaved Changes

**Given** the Platform Admin has unsaved changes,  
**When** the Platform Admin selects Cancel,  
**Then** the system shall display an unsaved-changes confirmation.

### AC-30 — Confirm Other Leave-Editor Navigation

**Given** the Platform Admin has unsaved changes,  
**When** the Platform Admin attempts to leave the Editor using an applicable navigation action,  
**Then** the system shall display the same unsaved-changes confirmation.

### AC-31 — Discard Unsaved Changes

**Given** the unsaved-changes confirmation is displayed,  
**When** the Platform Admin selects Discard changes,  
**Then** the system shall discard the unsaved edits,  
**And** complete the requested navigation away from the Editor,  
**And** retain the previously published content.

### AC-32 — Continue Editing

**Given** the unsaved-changes confirmation is displayed,  
**When** the Platform Admin selects Continue editing,  
**Then** the confirmation shall close,  
**And** the Editor shall remain open,  
**And** all unsaved edits shall be preserved.

### AC-33 — Leave Editor Without Changes

**Given** the Editor contains no unsaved changes,  
**When** the Platform Admin selects Cancel or another action that leaves the Editor,  
**Then** the system shall allow navigation without an unsaved-changes confirmation.

### AC-34 — Reverted Changes Do Not Trigger Confirmation

**Given** the Platform Admin made edits and later restored all content to its originally loaded state,  
**When** the Platform Admin attempts to leave the Editor,  
**Then** the system shall not display an unsaved-changes confirmation.

### AC-35 — Publication Failure Message

**Given** the Platform Admin selects Save,  
**When** saving or publishing fails,  
**Then** the system shall display an error dialog containing `Something went wrong. Please try again.`,  
**And** provide OK.

### AC-36 — Preserve Edits After Publication Failure

**Given** the publication failure dialog is displayed,  
**When** the Platform Admin selects OK,  
**Then** the dialog shall close,  
**And** the Platform Admin shall remain on the Editor,  
**And** the unsaved edits shall be preserved.

### AC-37 — Preserve Published Content After Failure

**Given** a page already has published content,  
**When** a subsequent Save/Publish attempt fails,  
**Then** the previously published content shall remain unchanged.

### AC-38 — Open Editor Without Published Content

**Given** a Static Page has no published content,  
**When** the Platform Admin opens its Editor,  
**Then** the Editor shall open with empty content,  
**And** display `No content has been published`.

### AC-39 — Validate Initial Publication

**Given** a Static Page has no published content,  
**When** the Platform Admin attempts to publish incomplete required content,  
**Then** the system shall prevent publication,  
**And** display the applicable validation errors.

### AC-40 — Record Successful Publication Metadata

**Given** a Platform Admin successfully publishes changes,  
**When** publication completes,  
**Then** the system shall record the publishing admin's name,  
**And** the successful publication date and time.

### AC-41 — Display Publication Metadata

**Given** a page has been successfully published,  
**When** the Platform Admin views Page Details or the Static Pages List,  
**Then** the system shall display the latest successful publication information.

### AC-42 — Update Metadata After FAQ-only Changes

**Given** the Platform Admin changes only FAQ items or their order,  
**When** the changes are successfully published,  
**Then** Last Updated and Updated By shall reflect that successful publication.

### AC-43 — Preserve Metadata After Failure

**Given** a page has existing publication metadata,  
**When** a subsequent Save/Publish attempt fails,  
**Then** Last Updated and Updated By shall remain unchanged.

### AC-44 — Publish Content to Subscriber App

**Given** the Platform Admin successfully publishes changes to a Static Page,  
**When** a subscriber next opens that page,  
**Then** the subscriber app shall make the latest published content available.

### AC-45 — No App Update Required

**Given** the Platform Admin has successfully published updated Static Page content,  
**When** a subscriber opens the relevant page using the existing app,  
**Then** the latest published content shall be available without requiring a new app release.

### AC-46 — Maintain Published Version Consistency

**Given** a Static Page has been successfully published,  
**When** its published content is displayed in the Admin Platform and subscriber app,  
**Then** both interfaces shall display the same published content version.

### AC-47 — Do Not Publish Pending FAQ Changes

**Given** the Platform Admin has added, edited, deleted, or reordered FAQ items,  
**And** Save has not succeeded,  
**When** a subscriber opens the FAQ page,  
**Then** the subscriber shall continue to see the previously published FAQ content and order.

### AC-48 — Do Not Update Metadata After Discard

**Given** the Platform Admin has unsaved changes,  
**When** the Platform Admin discards those changes,  
**Then** the previously published content shall remain unchanged,  
**And** Last Updated and Updated By shall remain unchanged.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Authorized Platform Admin selects Edit | Selected Static Page Editor opens |
| EX-02 | Page has published content | Published title and content are pre-filled |
| EX-03 | Admin selects Terms and Conditions | Applicable rich-text editor opens |
| EX-04 | Admin selects FAQ | FAQ item management controls are available |
| EX-05 | Page Title left empty | `Page Title is required.` |
| EX-06 | Page Title contains spaces only | Treated as empty |
| EX-07 | Required Content left empty | `Content is required.` |
| EX-08 | Required Content contains whitespace only | Treated as empty |
| EX-09 | Admin leaves invalid required field | Inline validation appears |
| EX-10 | Admin selects Save with invalid required fields | Publication blocked; inline errors displayed |
| EX-11 | Admin applies Heading formatting | Heading formatting supported |
| EX-12 | Admin applies Bold formatting | Bold formatting supported |
| EX-13 | Admin creates bulleted list | Bulleted list supported |
| EX-14 | Admin creates numbered list | Numbered list supported |
| EX-15 | Admin inserts hyperlink | Hyperlink formatting supported |
| EX-16 | Formatted content published | Formatting preserved in Admin Platform and subscriber app |
| EX-17 | Admin adds FAQ item | Item added to pending FAQ content |
| EX-18 | Admin edits FAQ Question | Pending Question updated |
| EX-19 | Admin edits FAQ Answer | Pending Answer updated |
| EX-20 | Admin deletes FAQ item | Pending item removed; published content unchanged |
| EX-21 | Admin reorders FAQ items | Pending order updated |
| EX-22 | FAQ Question empty | `Question is required.` |
| EX-23 | FAQ Answer empty | `Answer is required.` |
| EX-24 | FAQ Question or Answer contains spaces only | Treated as empty |
| EX-25 | Multiple FAQ items have missing fields | All incomplete fields identified; publication blocked |
| EX-26 | Admin deletes all FAQ items | `At least one FAQ item is required.` |
| EX-27 | FAQ contains one complete item | Minimum FAQ item requirement satisfied |
| EX-28 | FAQ reorder successfully published | Subscriber app displays saved order |
| EX-29 | No changes made | Save disabled |
| EX-30 | Page Title changed | Save enabled |
| EX-31 | Rich-text Content changed | Save enabled |
| EX-32 | FAQ item added, edited, or deleted | Save enabled |
| EX-33 | FAQ order changed | Save enabled |
| EX-34 | Changed content is invalid | Save enabled; publication blocked on validation |
| EX-35 | All changes reverted | Save disabled |
| EX-36 | Valid changes successfully saved | Content immediately published |
| EX-37 | Successful publication | Success message and updated Page Details shown |
| EX-38 | Cancel with unsaved changes | Unsaved-changes confirmation displayed |
| EX-39 | Breadcrumb navigation with unsaved changes | Unsaved-changes confirmation displayed |
| EX-40 | Sidebar navigation with unsaved changes | Unsaved-changes confirmation displayed |
| EX-41 | Select Discard changes | Changes discarded; requested navigation completed |
| EX-42 | Select Continue editing | Editor remains open; edits preserved |
| EX-43 | Leave Editor without changes | No confirmation required |
| EX-44 | Leave Editor after reverting all changes | No confirmation required |
| EX-45 | Save/Publish fails | `Something went wrong. Please try again.` |
| EX-46 | Select OK after publication failure | Remain on Editor; edits preserved |
| EX-47 | Publication fails | Previously published content unchanged |
| EX-48 | Page has no published content | `No content has been published` displayed |
| EX-49 | Initial publication attempted with missing fields | Publication blocked |
| EX-50 | Successful publication | Publishing admin name recorded |
| EX-51 | Successful publication | Publication date/time recorded |
| EX-52 | View Page Details after publication | Latest publication information displayed |
| EX-53 | View Static Pages List after publication | Latest publication information displayed |
| EX-54 | FAQ-only changes published | Last Updated and Updated By updated |
| EX-55 | Publication fails | Last Updated and Updated By unchanged |
| EX-56 | Admin discards edits | Publication metadata unchanged |
| EX-57 | Subscriber opens updated page after publication | Latest published content available |
| EX-58 | Subscriber uses existing app version | No app release required |
| EX-59 | Admin Platform and subscriber app display published page | Same published version displayed |
| EX-60 | FAQ changes remain unsaved | Subscriber app retains previously published FAQ items and order |

## 9. Attachments and Related Requirements

### 9.1 Attachments

The original Jira intake lists the following attachments:

| Attachment | Purpose |
|---|---|
| `After Save — success message, Last Updated@2x.png` | Successful publication feedback and Last Updated information |
| `Editor — discard unsaved changes@2x.png` | Unsaved-changes confirmation and navigation actions |
| `Editor — opened, pre-filled, Save disabled@2x.png` | Initial Editor state with published content |
| `Editor — required field validation@2x.png` | Required-field validation presentation |
| `Editor — save failure@2x.png` | Publication failure feedback |
| `FAQ editor — add, edit, delete, reorder@2x.png` | FAQ management controls and item ordering |

The user also supplied wireframe screenshots during the clarification process.

**Wireframe wording that is not separately confirmed as an exact business message:**

- `Discard unsaved changes?`
- `Changes saved and published`

The confirmed navigation actions and their behavior remain binding regardless of these unconfirmed exact message texts.

### 9.2 Related Requirements

| Reference | Requirement / Feature | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-50 | Admin Platform | Parent Feature |
| CP-58 | Edit Static Pages | Current User Story |
| CP-52 | RBAC | Related permission-management feature; CP-58 requires an authorized Platform Admin |

### 9.3 Dependencies

1. **Platform Admin Authorization:** The Edit action depends on the Platform Admin having the required permission.
2. **Static Pages List:** The workflow depends on an existing Static Pages area from which the Platform Admin can select a page.
3. **Page Details:** The workflow depends on the selected page's details being available for opening the Editor and displaying successful publication results.
4. **Existing Published Content:** Where a published version exists, the Editor depends on its current title and content for pre-filling and change detection.
5. **Page Content Structure:** The applicable editing controls depend on whether the selected page uses rich-text content, FAQ items, or another existing page structure.
6. **Rich-Text Formatting:** The Editor must support and preserve the five confirmed formatting capabilities for applicable content.
7. **FAQ Item Management:** FAQ publication depends on complete Question and Answer values and at least one complete FAQ item.
8. **Published Version Availability:** The subscriber app depends on the latest successfully published Static Page content being available when the page is next opened.
9. **Publication Metadata:** The Static Pages List and Page Details depend on the latest successful publication information.
10. **Unsaved-Changes Detection:** Navigation confirmation and Save state depend on comparing current publishable content with the originally loaded version.

## 10. Out of Scope

The following are not included in CP-58:

1. Creating or deleting Static Pages.
2. Creating new Static Page types.
3. Defining layouts for existing page types not covered by the supplied wireframes.
4. Defining Page Title, Content, Question, or Answer maximum character limits.
5. Adding rich-text capabilities beyond Headings, Bold text, Bulleted lists, Numbered lists, and Hyperlinks.
6. Defining additional hyperlink validation or URL-format restrictions.
7. Supporting rich-text formatting within the confirmed plain-text FAQ Question and Answer fields.
8. Publishing an FAQ page with zero FAQ items.
9. Publishing invalid or incomplete required content.
10. Implementing a separate Save as Draft action.
11. Implementing a publication approval workflow.
12. Scheduling publication for a future date.
13. Defining publication version history or rollback functionality.
14. Defining technical synchronization mechanisms between the Admin Platform and subscriber app.
15. Requiring subscribers to install a new app release to access published content.
16. Defining publication timestamp formatting or timezone rules.
17. Defining concurrent-editing or conflict-resolution behavior when multiple admins edit the same page.
18. Defining additional publication failure categories or error messages.
19. Defining APIs, database schemas, storage formats, or technical implementation details.
20. Defining audit logging beyond the confirmed Last Updated and Updated By publication information.

---

