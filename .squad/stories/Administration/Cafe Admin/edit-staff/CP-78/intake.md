> **Fetched from jira:** [CP-78](https://yomnaarafahammad-1791369924187.atlassian.net/browse/CP-78)  
> *Fetched 2026-10-21T19:59:28.154Z. Edit the sections below as needed; the planner reads this file verbatim.*


## Source — work item (from tracker)

**Title:** Edit Staff  
**Type:** Story  
**Status:** Idea  
**Assignee:** Medhat Elbesy  
**Labels:** MVP

### Description

Epic: Administration (
    
                
            
            CP-49
        
                                                    Idea
            
)

Feature: Cafe Platform (
    
                
            
            CP-51
        
                                                    Idea
            
) → Admin (
    
                
            
            CP-96
        
                                                    Idea
            
) 

User Story: Edit Staff (
    
                
            
            CP-78
        
                                                    Idea
            
)

Description:

As an Cafe Owner 

I want to edit staff account 

So that i can keep the staff member’s details, password, and staff type up to date. 

	Trigger:

Dashboard >> Staff Accounts >> Edit

	Acceptance Criteria:

The edit form must display the account’s current data

	
	
		Name
		
			Displays the current name

			A text input field, that only accepts letters

			Updating this field is optional. The current value can be retained, but the field must not be empty.

		
		

		Email Address
		
			Displays the current email.

			Updating this field is optional. The current value can be retained, but the field must not be empty.

			Must have a valid email format

			If the email format is invalid, display “Please enter a valid email address”.

			Must be Unique across all staff accounts on platform

			If the email belongs to another staff account, display “Email already exists”.

			Keeping the account’s current email must not trigger a duplicate-email error. Uniqueness validation must exclude the account being edited and ignore letter case.

		
		

		Password
		
			The Café Owner can set a new password for the staff account.

			The existing password must not be displayed.

			Optional; leaving it empty keeps the current password unchanged.

			If entered, must contain at least 8 characters, including at least one uppercase letter and one special character.

			Input must be masked.

		
		

		Password Confirmation
		
			Mandatory when a new password is entered.

			Must match the new Password.

			Input must be masked.

		
		

		Type:
		
			Displays the current staff type.

			A drop down list that contains 2 values:
			
				Cashier

				Delivery

			
			

			Updating this field is optional. The current value can be retained, but the field must not be empty.

			Changing the type applies the fixed permissions of the new type and removes permissions exclusive to the previous type, including for existing sessions.

			A Delivery Staff account cannot be changed to Cashier while it has assigned orders in New, Preparing, or Out for Delivery.

			The role can be changed after those orders reach Delivered or Not Responding, or are reassigned to another delivery staff member.

		
		

	
	

	Actions:
	
		Update:
		
			The system validates all fields and the staff-type change restriction before saving.

			If validation fails, no changes are saved.

			The system updates the existing account without creating a new account.

			If a new password is provided, it replaces the current password. Subsequent sign-ins must use the new password.

			Account status remains unchanged; activation and deactivation are handled separately.

			Upon success, display “Staff account updated successfully”.

			Return to the Staff Accounts list with the updated details.

		
		

		Cancel:
		
			No changes are saved.

			The user remains on the edit page.

			All currently entered values remain in the form.

		
		

	
	

Handling Errors:

	If Name, Email Address, or Type is cleared, display *“Please fill required field”* beneath the affected field and do not save any changes.

	If the password does not meet the requirements, display “Password must contain at least 8 characters, one uppercase letter, and one special character.”

	If Password Confirmation does not match Password, display “Passwords do not match”.

	If the update fails, display “Unable to update account. Please try again.” No changes are saved, and the entered values remain in the form for retry.

	If a staff-type change is blocked by assigned ongoing orders, display “Cannot change staff type while delivery orders are in progress. Complete or reassign the orders first.”

	If loading fails, display “Unable to load staff account. Please try again.” and provide a Retry action.

	The system must prevent editing staff accounts belonging to another café, including through a direct URL or request.

	If a new password is entered without Password Confirmation, display “Please confirm your new password”.

	If the current status cannot be loaded, display “Unable to load café status. Please try again.” and provide a Retry action.

	If updating fails, display “Unable to update café status. Please try again.” The previously saved status must remain unchanged.

	The system must prevent unauthorized status updates, including through a direct request.

### Attachments

| File | Size | Status |
| ---- | ---- | ------ |
| `attachments/image-20261010-152001.png` | 62 KB | downloaded |

---
# Story intake

Fill this template for each story you want planned. Keep it copy-paste-friendly: the planner reads **this file and the files in `attachments/`**, nothing else.

- Folder: `.squad/stories/edit-staff/CP-78/intake.md`
- Binaries (screenshots, PDFs, exports): put them in `attachments/` next to this file and list them below.
- Do **not** rely on external links (tracker URLs, wiki, chat) — the planner cannot open them. Paste the content you want considered.

This is **not** an implementation prompt. It is the input to the plan-generation meta-prompt bundled with squad-kit (`generate-plan.md` in the installed package).

---

## Feature

- **Feature name (display):**
- **Feature slug (folder under `plans/`):** `edit-staff`

## Tracker (metadata only)

- **Tracker type:** `jira`
- **Work item id:** `CP-78` *(used in filenames and plan tables; fill manually if empty)*
- **Work item type:** `Story`
- **Status:** `Idea`
- **Assignee:** `Medhat Elbesy`
- **Labels:** `MVP`

External tracker links are **not** followed by the planner. Keep the id for naming and traceability only.

---

## Title

*(Paste the work item title verbatim. Prefilled when `squad new-story` fetched from a tracker.)*

```
Edit Staff
```

---

## Description

*(Paste the full work item description. Prefilled when fetched from a tracker.)*

```
Epic: Administration (
    
                
            
            CP-49
        
                                                    Idea
            
)

Feature: Cafe Platform (
    
                
            
            CP-51
        
                                                    Idea
            
) → Admin (
    
                
            
            CP-96
        
                                                    Idea
            
) 

User Story: Edit Staff (
    
                
            
            CP-78
        
                                                    Idea
            
)

Description:

As an Cafe Owner 

I want to edit staff account 

So that i can keep the staff member’s details, password, and staff type up to date. 

	Trigger:

Dashboard >> Staff Accounts >> Edit

	Acceptance Criteria:

The edit form must display the account’s current data

	
	
		Name
		
			Displays the current name

			A text input field, that only accepts letters

			Updating this field is optional. The current value can be retained, but the field must not be empty.

		
		

		Email Address
		
			Displays the current email.

			Updating this field is optional. The current value can be retained, but the field must not be empty.

			Must have a valid email format

			If the email format is invalid, display “Please enter a valid email address”.

			Must be Unique across all staff accounts on platform

			If the email belongs to another staff account, display “Email already exists”.

			Keeping the account’s current email must not trigger a duplicate-email error. Uniqueness validation must exclude the account being edited and ignore letter case.

		
		

		Password
		
			The Café Owner can set a new password for the staff account.

			The existing password must not be displayed.

			Optional; leaving it empty keeps the current password unchanged.

			If entered, must contain at least 8 characters, including at least one uppercase letter and one special character.

			Input must be masked.

		
		

		Password Confirmation
		
			Mandatory when a new password is entered.

			Must match the new Password.

			Input must be masked.

		
		

		Type:
		
			Displays the current staff type.

			A drop down list that contains 2 values:
			
				Cashier

				Delivery

			
			

			Updating this field is optional. The current value can be retained, but the field must not be empty.

			Changing the type applies the fixed permissions of the new type and removes permissions exclusive to the previous type, including for existing sessions.

			A Delivery Staff account cannot be changed to Cashier while it has assigned orders in New, Preparing, or Out for Delivery.

			The role can be changed after those orders reach Delivered or Not Responding, or are reassigned to another delivery staff member.

		
		

	
	

	Actions:
	
		Update:
		
			The system validates all fields and the staff-type change restriction before saving.

			If validation fails, no changes are saved.

			The system updates the existing account without creating a new account.

			If a new password is provided, it replaces the current password. Subsequent sign-ins must use the new password.

			Account status remains unchanged; activation and deactivation are handled separately.

			Upon success, display “Staff account updated successfully”.

			Return to the Staff Accounts list with the updated details.

		
		

		Cancel:
		
			No changes are saved.

			The user remains on the edit page.

			All currently entered values remain in the form.

		
		

	
	

Handling Errors:

	If Name, Email Address, or Type is cleared, display *“Please fill required field”* beneath the affected field and do not save any changes.

	If the password does not meet the requirements, display “Password must contain at least 8 characters, one uppercase letter, and one special character.”

	If Password Confirmation does not match Password, display “Passwords do not match”.

	If the update fails, display “Unable to update account. Please try again.” No changes are saved, and the entered values remain in the form for retry.

	If a staff-type change is blocked by assigned ongoing orders, display “Cannot change staff type while delivery orders are in progress. Complete or reassign the orders first.”

	If loading fails, display “Unable to load staff account. Please try again.” and provide a Retry action.

	The system must prevent editing staff accounts belonging to another café, including through a direct URL or request.

	If a new password is entered without Password Confirmation, display “Please confirm your new password”.

	If the current status cannot be loaded, display “Unable to load café status. Please try again.” and provide a Retry action.

	If updating fails, display “Unable to update café status. Please try again.” The previously saved status must remain unchanged.

	The system must prevent unauthorized status updates, including through a direct request.
```

---

## Acceptance criteria

*(Checklist, bullets, Gherkin, etc. Prefilled for Azure DevOps when the work item has acceptance criteria.)*

```

```

---

## Attachments

Place files in `attachments/` next to this `intake.md`, then list them here so the planner knows what to open.

| File (relative to this folder) | What it is |
| ------------------------------ | ---------- |
| *(e.g. `attachments/flow.png`)* | *(e.g. UX flow)* |

*(Add rows per file. If none, write "None.")*

---

## Dependencies

- **Blocked by / related ids:** (tracker ids only; optional short note)
- **Depends on code areas or other stories:**

## Extra notes (optional)

- Anything not captured above (e.g. chat context) — keep short.

## Technical hints (optional)

- APIs, screens, services already discussed. Repos/roots: `.`. Primary language: `typescript`.

## Out of scope

- What this story explicitly does **not** cover:
