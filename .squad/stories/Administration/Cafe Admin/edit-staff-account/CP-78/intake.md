# Functional Specification — CP-78: Edit Staff Account

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-78 |
| Story Title | Edit Staff Account |
| Epic | Administration (CP-49) |
| Feature | Café Platform (CP-51) → Admin (CP-96) |
| User Role | Café Owner |
| Priority / Label | MVP |
| Status | Draft — Pending Review |
| Reference | Jira CP-78, Edit Staff Account Wireframe |
| Confirmed Decisions | D-01 to D-20 |

## 2. User Story

**As a** Café Owner,  
**I want to** edit a staff account,  
**So that** I can keep the staff member's details, password, and staff type up to date.

## 3. Trigger

Dashboard → Staff Accounts → Edit

The Café Owner selects **Edit** for an existing staff account.

## 4. Preconditions

1. The Café Owner is authenticated and authorized to manage staff accounts for the relevant Café.
2. The selected staff account exists and belongs to that Café.
3. The system can retrieve the staff account's current information.
4. The system can validate email uniqueness across staff accounts on the platform.
5. The system can retrieve the staff member's currently assigned delivery orders when a Delivery-to-Cashier change is requested.

## 5. Fields and Validation

### 5.1 Name

| Attribute | Specification |
|---|---|
| Field Type | Text input |
| Required | Yes |
| Initial Value | Current staff name |
| Accepted Characters | Arabic letters, English letters, and spaces |
| Validation | Must not be empty; numbers are not accepted |
| Error Message | `Please fill required field` |

**Behavior**

- The field shall display the current staff name.
- The Café Owner may change the name or retain the existing value.
- Multiple-word names shall be supported.
- Invalid values shall prevent Update.

### 5.2 Email Address

| Attribute | Specification |
|---|---|
| Field Type | Email input |
| Required | Yes |
| Initial Value | Current email address |
| Format | Valid email format |
| Uniqueness | Across all staff accounts on the platform |
| Comparison | Case-insensitive; excludes the current account |

**Validation Messages**

| Condition | Error Message |
|---|---|
| Empty email | `Please fill required field` |
| Invalid format | `Please enter a valid email address` |
| Email already used by another staff account | `Email already exists` |

**Behavior**

- The Café Owner may update or retain the current email.
- Keeping the existing email, including changing its letter case, shall not trigger a duplicate error from the current account.
- Email uniqueness shall be validated before saving.

### 5.3 New Password

| Attribute | Specification |
|---|---|
| Field Type | Masked password input |
| Required | No |
| Initial Value | Empty |
| Minimum Length | 8 characters |
| Complexity | At least one uppercase letter and one special character |
| Maximum Length | No additional maximum business rule confirmed |
| Blank Value | Existing password remains unchanged |

**Validation Message**

`Password must contain at least 8 characters, one uppercase letter, and one special character.`

**Behavior**

- The existing password shall never be displayed.
- A valid new password shall replace the existing password after successful Update.
- Subsequent sign-ins shall require the new password.
- Changing the password shall not automatically terminate existing sessions.

### 5.4 Confirm New Password

| Attribute | Specification |
|---|---|
| Field Type | Masked password input |
| Required | Only when New Password is entered |
| Initial Value | Empty |
| Validation | Exact, case-sensitive match with New Password |

**Validation Messages**

| Condition | Error Message |
|---|---|
| New Password entered; confirmation empty | `Please confirm your new password` |
| Passwords do not match exactly | `Passwords do not match` |
| Confirmation entered; New Password empty | `Please enter a new password` — displayed beneath New Password |

**Behavior**

- Both password fields may remain empty to retain the existing password.
- Password mismatch or incomplete password entry shall block Update.

### 5.5 Staff Type

| Attribute | Specification |
|---|---|
| Field Type | Dropdown |
| Required | Yes |
| Initial Value | Current staff type |
| Available Options | Cashier, Delivery |
| Empty Value Error | `Please fill required field` |

**Behavior**

- The Café Owner may retain the existing type or select another type.
- A successful type change shall apply the fixed permissions of the new type and remove permissions exclusive to the previous type.
- Updated permissions shall apply to existing sessions.

**Delivery-to-Cashier Restriction**

When changing a staff member from Delivery to Cashier:

- The system shall check currently assigned orders when Update is submitted.
- The change shall be blocked if any assigned order has status **New, Preparing, or Out for Delivery**.
- The change shall be allowed once no such orders remain assigned, including when orders have reached **Delivered**, **Not Responding**, or have been reassigned.
- The restriction shall not prevent updating other fields while retaining Staff Type as Delivery.

**Error Message**

`Cannot change staff type while delivery orders are in progress. Complete or reassign the orders first.`

### 5.6 Update

| Attribute | Specification |
|---|---|
| Action | Update |
| Validation | All applicable field and business validations |
| No Changes | Update is allowed |
| Success Message | `Staff account updated successfully` |
| Success Navigation | Staff Accounts List |

**Behavior**

- The system shall validate the entered values, Café ownership, and applicable delivery-order restrictions before saving.
- The system shall update the existing account without creating another account.
- The system shall preserve the current Active/Inactive status.
- If no fields were changed, Update shall still be allowed and shall display the success message.
- If account data changed in another session after the form was opened, the system shall block Update and require Reload.
- On successful Update, the system shall return to the Staff Accounts List with updated details.
- If validation fails, no changes shall be saved.

### 5.7 Cancel

| Attribute | Specification |
|---|---|
| Action | Cancel |
| Navigation | Remain on Edit Staff Account page |
| Entered Values | Retained |
| Save Changes | No |

**Behavior**

Selecting Cancel shall not save any changes. The Café Owner shall remain on the edit page, with all currently entered values retained.

### 5.8 Error Handling

| Scenario | System Behavior |
|---|---|
| Required field missing | Display `Please fill required field` beneath the affected field |
| Invalid email | Display `Please enter a valid email address` |
| Duplicate email | Display `Email already exists` |
| Invalid password | Display the specified password complexity message |
| Missing password confirmation | Display `Please confirm your new password` |
| Password mismatch | Display `Passwords do not match` |
| Confirmation without new password | Display `Please enter a new password` beneath New Password |
| Delivery-to-Cashier restriction fails | Display the specified ongoing-orders error; do not save |
| Update operation fails | Display `Unable to update account. Please try again.`; save no changes; retain entered values |
| Staff account loading fails | Display `Unable to load staff account. Please try again.` with Retry; do not show an editable form with unverified data |
| Account modified in another session | Block Update and require Reload |
| Unauthorized Café/account access | Deny the operation and prevent account modification |

The system shall enforce Café ownership and authorization on both form access and Update, including direct URL and API requests.

## 6. Business Rules

| Rule ID | Business Rule |
|---|---|
| BR-01 | Only an authorized Café Owner may edit staff accounts belonging to their Café. |
| BR-02 | Editing shall update the existing staff account without creating a new account. |
| BR-03 | Name, Email Address, and Staff Type are mandatory; existing valid values may be retained. |
| BR-04 | Name shall accept Arabic letters, English letters, and spaces. |
| BR-05 | Email shall be valid and unique across all platform staff accounts, using case-insensitive comparison and excluding the current account. |
| BR-06 | Existing passwords shall never be displayed; leaving New Password empty shall retain the current password. |
| BR-07 | A new password shall contain at least eight characters, one uppercase letter, and one special character. |
| BR-08 | Password confirmation shall be required when a new password is entered and shall match exactly. |
| BR-09 | Entering password confirmation without a new password shall block Update. |
| BR-10 | A successfully changed password shall apply to subsequent sign-ins without automatically terminating existing sessions. |
| BR-11 | Staff Type shall be Cashier or Delivery, with fixed permissions associated with each type. |
| BR-12 | A successful Staff Type change shall apply the new fixed permissions and remove permissions exclusive to the previous type, including in existing sessions. |
| BR-13 | Delivery-to-Cashier changes shall be blocked while the staff member has assigned orders in New, Preparing, or Out for Delivery. |
| BR-14 | The ongoing-order restriction shall be revalidated at Update submission. |
| BR-15 | Completed or reassigned orders shall not block Delivery-to-Cashier changes when no restricted assigned orders remain. |
| BR-16 | The Delivery-to-Cashier restriction shall not block unrelated edits that retain Staff Type as Delivery. |
| BR-17 | All applicable validations shall pass before account changes are saved. |
| BR-18 | Update shall be allowed even when no fields have been modified. |
| BR-19 | Updates based on outdated account data shall be blocked until Reload. |
| BR-20 | The existing Active/Inactive account status shall remain unchanged. |
| BR-21 | Successful Update shall display the specified success message and return to the Staff Accounts List. |
| BR-22 | Cancel shall save nothing, retain entered values, and keep the Café Owner on the edit page. |
| BR-23 | A failed Update shall save no changes and shall retain entered values for retry. |
| BR-24 | A loading failure shall prevent editing unverified data and provide Retry. |
| BR-25 | Café ownership and authorization shall be enforced at form access and Update, including direct requests. |

## 7. Acceptance Criteria

### AC-01 — Load Staff Account

**Given** an authorized Café Owner selects Edit for a staff account belonging to their Café,  
**When** the Edit Staff Account page loads successfully,  
**Then** the system displays the current Name, Email Address, and Staff Type,  
**And** does not display the existing password.

### AC-02 — Edit Staff Name

**Given** the Café Owner is editing a staff account,  
**When** a valid name containing Arabic or English letters and spaces is entered,  
**Then** the system accepts the value.

**And** if Name is empty or contains unsupported characters, the system prevents Update.

### AC-03 — Validate Email

**Given** the Café Owner enters an email address,  
**When** Update is submitted,  
**Then** the system validates its format and case-insensitive uniqueness across all platform staff accounts, excluding the current account.

**And** the system displays the corresponding validation message if the value is invalid or duplicated.

### AC-04 — Retain Existing Password

**Given** New Password and Confirm New Password are empty,  
**When** Update succeeds,  
**Then** the existing password remains unchanged.

### AC-05 — Change Password

**Given** the Café Owner enters a new password meeting the required complexity,  
**And** the confirmation matches exactly,  
**When** Update succeeds,  
**Then** the new password replaces the previous password,  
**And** subsequent sign-ins require the new password,  
**And** existing sessions are not automatically terminated solely because of the password change.

### AC-06 — Validate Password Confirmation

**Given** the Café Owner enters a new password or confirmation,  
**When** the required counterpart is missing or the values do not match exactly,  
**Then** the system displays the corresponding validation message,  
**And** prevents Update.

### AC-07 — Change Staff Type

**Given** the Café Owner selects a different Staff Type,  
**When** Update succeeds,  
**Then** the system applies the fixed permissions of the new type,  
**And** removes permissions exclusive to the previous type,  
**And** applies the changes to existing sessions.

### AC-08 — Block Delivery-to-Cashier With Ongoing Orders

**Given** the staff member is Delivery and has an assigned order in New, Preparing, or Out for Delivery,  
**When** the Café Owner selects Cashier and submits Update,  
**Then** the system blocks the change,  
**And** displays `Cannot change staff type while delivery orders are in progress. Complete or reassign the orders first.`,  
**And** saves no changes.

### AC-09 — Allow Delivery-to-Cashier After Order Resolution

**Given** all relevant orders are Delivered, Not Responding, or reassigned,  
**And** no restricted assigned orders remain,  
**When** the Café Owner submits a valid Delivery-to-Cashier change,  
**Then** the system allows the update.

### AC-10 — Revalidate Orders at Submission

**Given** a Delivery Staff account has no ongoing orders when the form opens,  
**But** an ongoing order is assigned before Update,  
**When** the Café Owner submits a Delivery-to-Cashier change,  
**Then** the system blocks the change.

### AC-11 — Allow Unrelated Staff Edits

**Given** a Delivery Staff member has ongoing assigned orders,  
**When** the Café Owner updates Name, Email, or Password without changing Staff Type,  
**Then** the ongoing-order restriction does not block an otherwise valid update.

### AC-12 — Update Without Changes

**Given** the Café Owner has not modified any field,  
**When** Update is submitted with valid existing values,  
**Then** the system accepts the submission,  
**And** displays `Staff account updated successfully`.

### AC-13 — Prevent Concurrent Overwrite

**Given** account data changed in another session after the edit form loaded,  
**When** the Café Owner submits Update using outdated data,  
**Then** the system blocks Update,  
**And** requires Reload before another attempt.

### AC-14 — Successful Update

**Given** all applicable validations pass,  
**When** the Café Owner submits Update,  
**Then** the system updates the existing account,  
**And** preserves its Active/Inactive status,  
**And** displays `Staff account updated successfully`,  
**And** returns to the Staff Accounts List with updated details.

### AC-15 — Cancel Editing

**Given** the Café Owner has entered changes,  
**When** Cancel is selected,  
**Then** no changes are saved,  
**And** the Café Owner remains on the Edit Staff Account page,  
**And** all entered values are retained.

### AC-16 — Handle Update Failure

**Given** the Café Owner submits an update,  
**When** the update operation fails,  
**Then** the system displays `Unable to update account. Please try again.`,  
**And** saves no changes,  
**And** retains entered values for retry.

### AC-17 — Handle Loading Failure

**Given** the staff account data cannot be loaded,  
**When** loading fails,  
**Then** the system displays `Unable to load staff account. Please try again.` with Retry,  
**And** does not display an editable form containing unverified data.

### AC-18 — Enforce Café Ownership

**Given** a staff account does not belong to the Café managed by the requesting Café Owner,  
**When** the Café Owner attempts to access or update that account, including through a direct URL or API request,  
**Then** the system denies the operation,  
**And** prevents account modification.

## 8. Acceptance Examples

| Example ID | Scenario / Input | Expected Result |
|---|---|---|
| EX-01 | Name = `Omar Ali` | Accepted |
| EX-02 | Name = `عمر علي` | Accepted |
| EX-03 | Name is empty | `Please fill required field` |
| EX-04 | Name = `Omar123` | Rejected |
| EX-05 | Existing email is retained | Accepted without duplicate error |
| EX-06 | Another account uses the same email with different letter case | `Email already exists` |
| EX-07 | Email = `invalid-email` | `Please enter a valid email address` |
| EX-08 | Both password fields empty | Existing password retained |
| EX-09 | New Password = `Password@1`, confirmation matches | Accepted |
| EX-10 | New Password = `password@1` | Password complexity error |
| EX-11 | New Password entered, confirmation empty | `Please confirm your new password` |
| EX-12 | New Password empty, confirmation entered | `Please enter a new password` |
| EX-13 | Password and confirmation differ in letter case | `Passwords do not match` |
| EX-14 | Delivery → Cashier with an assigned Preparing order | Update blocked |
| EX-15 | Delivery → Cashier after all ongoing orders are reassigned | Update allowed |
| EX-16 | New ongoing order assigned before Update | Type change blocked |
| EX-17 | Delivery Staff with ongoing orders updates Name only | Update allowed if otherwise valid |
| EX-18 | Staff Type changes successfully | New fixed permissions apply to existing sessions |
| EX-19 | Update submitted without field changes | Success message displayed |
| EX-20 | Account modified by another session | Update blocked; Reload required |
| EX-21 | Update succeeds for an Inactive account | Account remains Inactive |
| EX-22 | Cancel after entering changes | No save; remain on form; values retained |
| EX-23 | Update fails | Error displayed; no save; values retained |
| EX-24 | Account loading fails | Error and Retry; no unverified editable form |
| EX-25 | Attempt to edit another Café's staff account | Access denied |

*Example values are illustrative test inputs, not additional business rules.*

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Reference | Description |
|---|---|
| CP-78 Jira Story | Original requirements and error handling |
| Edit Staff Account Wireframe | Form layout, fields, and actions |
| D-01 to D-20 | Confirmed clarification decisions |

### 9.2 Related Requirements

| Reference | Description |
|---|---|
| CP-49 | Administration Epic |
| CP-51 | Café Platform Feature |
| CP-96 | Admin |
| Staff Accounts List | Entry point and successful-update destination |
| Staff Account Status Management | Separate activation/deactivation functionality |
| Delivery Order Management | Assigned-order status validation |
| Staff Type Permissions | Fixed permissions for Cashier and Delivery |

### 9.3 Pending Clarifications

| ID | Area | Pending Item |
|---|---|---|
| P-01 | Concurrent Update | Exact error message when account data has changed |
| P-02 | Technical Design | Mechanism for detecting concurrent modifications |
| P-03 | Name Validation | Whether hyphens, apostrophes, or other punctuation should be accepted |

The confirmed validation scope remains Arabic letters, English letters, and spaces unless P-03 is clarified.

## 10. Out of Scope

- Creating or deleting staff accounts.
- Activating or deactivating staff accounts.
- Editing staff accounts belonging to another Café.
- Creating or modifying fixed Staff Type permissions.
- Assigning custom permissions to individual staff accounts.
- Creating, completing, or reassigning delivery orders.
- Editing Café Status or applying unrelated Café Status error rules.
- Automatically terminating existing sessions solely because the password changed.
- Defining password storage, hashing, database schema, or API implementation details.
- Defining the technical mechanism for concurrent-update detection.
- Introducing additional Staff Types beyond Cashier and Delivery.
- Introducing unconfirmed password maximum-length or name punctuation requirements.

---
