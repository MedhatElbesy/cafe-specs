# CP-113 — Create Café

## Feature

- **Project:** Rashfa
- **Epic:** Registration & Access
- **Parent Feature:** Café Registration — CP-4
- **Feature Slug:** create-cafe

## Tracker

- **Tracker Type:** Jira
- **Work Item ID:** CP-113
- **Work Item Type:** Story
- **Status at Import:** Idea
- **Assignee at Import:** Tarek Bugshan
- **Label:** MVP

## User Story

**As a** Platform Admin,  
**I want** to create a Café Owner account and an associated initial café record, specifying login credentials and agreed coffee and delivery rates,  
**so that** the owner can sign in and complete their café information, and the platform can calculate the café’s receivables using the agreed rates.

## Trigger

Platform Admin Dashboard → Café Accounts → Create Café Owner Account

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to create café accounts.

## Fields and Validation

| Field            | Required | Rules                                                                                      |
| ---------------- | -------- | ------------------------------------------------------------------------------------------ |
| Email Address    | Yes      | Must have a valid email format and must not already be associated with an existing account |
| Password         | Yes      | At least 8 characters, including at least one uppercase letter and one special character   |
| Confirm Password | Yes      | Must match Password                                                                        |
| Coffee Price     | Yes      | Numeric value greater than zero, in SAR, with no more than two decimal places              |
| Delivery Price   | Yes      | Numeric value greater than zero, in SAR, with no more than two decimal places              |

## Business Rules

### BR-01 — Authorized Creation

Only Platform Admins with permission to create café accounts can perform this action.

The restriction applies to navigation, direct URLs, and requests.

### BR-02 — Email Uniqueness

The owner’s email address must not already be associated with an existing account.

Duplicate email addresses prevent account creation.

### BR-03 — Login Credentials

- The Platform Admin enters the owner’s password during creation.
- Confirm Password must match Password.
- The Platform Admin delivers the email address and password to the owner outside the system.
- This flow does not send automated login instructions, temporary passwords, or password-setup links.

### BR-04 — Agreed Rates

- Coffee Price is the amount owed to the café for each eligible cup recorded for settlement.
- Delivery Price is the amount owed to the café for each eligible delivery recorded for settlement.
- These values are café receivable rates, not subscriber package prices.
- Both rates are mandatory and greater than zero.
- Both rates use SAR and allow no more than two decimal places.

### BR-05 — Linked Account and Café

Successful creation must produce:

- One Café Owner account.
- One initial café record linked to that account.
- The agreed coffee and delivery rates associated with that café.

The owner completes the café’s remaining information after signing in.

### BR-06 — Complete Creation

- Do not report success unless the owner account, linked café record, and agreed rates have all been saved.
- A failed creation must not leave an incomplete owner-and-café registration.
- Repeated submission of the same creation request must not create duplicate accounts or café records.

### BR-07 — Post-creation Navigation

After successful creation:

- Display a success message.
- Return the Platform Admin to the Café Accounts list.
- Make the newly created account available in that list.

### BR-08 — Operational Eligibility

Creating the account does not override the café approval, activation, or opening rules.

Subscriber redemption remains subject to the applicable café eligibility rules.

## Acceptance Criteria

### AC-01 — Display Creation Form

The screen must display:

- Email Address.
- Password.
- Confirm Password.
- Coffee Price.
- Delivery Price.
- Create Account action.

All fields are mandatory.

Display SAR as the currency for Coffee Price and Delivery Price.

Password and Confirm Password must be masked during entry.

### AC-02 — Validate Required Fields

When Create Account is selected:

- Validate all required fields.
- Identify each missing field.
- Display “Required Field(s) Missing! Please complete all required fields.”
- Keep the admin on the form.
- Prevent creation until the required fields are completed correctly.

### AC-03 — Validate Email Address

If the email format is invalid:

- Display “Invalid Email Address! Please enter a valid email address.”
- Identify the Email Address field.
- Prevent account creation.

If the email is already associated with an existing account:

- Display “Email Address Already Exists! Please use a different email address.”
- Identify the Email Address field.
- Prevent account creation.

Keep the admin on the form in both cases.

### AC-04 — Validate Password

Reject a password that:

- Has fewer than 8 characters.
- Does not contain an uppercase letter.
- Does not contain a special character.

Display an inline message:

“Password must contain at least 8 characters, one uppercase letter, and one special character.”

Prevent account creation until the password is valid.

### AC-05 — Validate Password Confirmation

If Confirm Password does not match Password:

- Display “Passwords do not match.”
- Identify the Confirm Password field.
- Prevent account creation.
- Keep the admin on the form.

### AC-06 — Validate Coffee and Delivery Prices

Validate each price independently.

Reject:

- Empty values.
- Zero.
- Negative values.
- Non-numeric values.
- Values with more than two decimal places.

For an invalid non-empty value, display an inline message identifying the affected field:

“Enter an amount greater than zero with no more than two decimal places.”

Do not silently round an invalid value to make it acceptable.

Prevent creation until both prices are valid.

### AC-07 — Create Successfully

When all fields are valid and the email is unique:

- Create the Café Owner account using the entered email and password.
- Create the associated initial café record.
- Save Coffee Price and Delivery Price against that café.
- Display “Café account created successfully.”
- Return the admin to the Café Accounts list.
- Display the newly created account in the list.

The owner can subsequently use the supplied credentials in the café login flow and complete their café information.

### AC-08 — Deliver Credentials Outside the System

- Do not send an automated email containing login instructions.
- Do not generate or send a password-setup link as part of this flow.
- The Platform Admin is responsible for delivering the credentials to the owner outside the system.

### AC-09 — Handle Creation Failure

If creation fails:

- Do not display a success message.
- Display “Unable to create café account. Please try again.”
- Keep the admin on the creation form.
- Allow the admin to retry.
- Do not leave an incomplete owner-and-café registration.

### AC-10 — Prevent Duplicate Submission

- Prevent repeated clicks while a creation request is being processed.
- Repeated submission must not result in duplicate owner accounts or café records.

### AC-11 — Prevent Unauthorized Access

- Deny access to admins without permission to create café accounts.
- Apply this restriction to direct URLs and requests.
- Do not create an owner account or café record through an unauthorized request.

## Acceptance Examples

| Scenario                                 | Expected Result                                                                       |
| ---------------------------------------- | ------------------------------------------------------------------------------------- |
| All fields are valid and email is unique | Create the owner account, linked café, and rates; show success and return to the list |
| Email is empty                           | Show required-field validation; prevent creation                                      |
| Email format is invalid                  | Show invalid-email validation; prevent creation                                       |
| Email already exists                     | Show duplicate-email validation; prevent creation                                     |
| Password has 7 characters                | Reject the password                                                                   |
| Password lacks an uppercase letter       | Reject the password                                                                   |
| Password lacks a special character       | Reject the password                                                                   |
| Confirm Password differs from Password   | Show “Passwords do not match.”                                                        |
| Coffee Price is 5                        | Accept the price                                                                      |
| Delivery Price is 7.50                   | Accept the price                                                                      |
| Either price is 0 or negative            | Reject the affected price                                                             |
| Either price has three decimal places    | Reject the affected price without silently rounding                                   |
| Delivery Price is empty                  | Show required-field validation; prevent creation                                      |
| Creation fails                           | Show an error, remain on the form, and allow retry                                    |
| Admin submits repeatedly                 | Do not create duplicate accounts or cafés                                             |
| Creation succeeds                        | Do not send an automated credentials email or setup link                              |
| Admin lacks permission                   | Deny creation                                                                         |

## Attachments

| File                                                              | Description                 |
| ----------------------------------------------------------------- | --------------------------- |
| attachments/CP 4 Cafe Registration - CP 113 Cafe Registration.png | Café registration wireframe |

The wireframe must be reviewed against these updated requirements, particularly:

- Password and Confirm Password fields.
- Correct email validation.
- Coffee Price and Delivery Price validation.
- Removal of automated email delivery from this flow.

## Related Requirements

- Café Owner login.
- Café information completion.
- Café approval, activation, and opening rules.
- Coffee and delivery settlement calculations.
- Admin permissions.
- Café Accounts list.

## Out of Scope

- Automated delivery of login credentials.
- Password-setup links.
- Password reset and first-login password-change behaviour.
- Login validation and login error handling.
- Completing the café’s name, map location, and operating hours.
- Creating café staff accounts.
- Editing agreed rates after creation.
- Executing settlement payments.
- Technical implementation planning and code.
