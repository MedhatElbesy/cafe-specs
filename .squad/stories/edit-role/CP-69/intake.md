> **Fetched from jira:** [CP-69](https://yomnaarafahammad-1791369924187.atlassian.net/browse/CP-69)  
> *Fetched 2026-10-20T22:09:04.752Z. Edit the sections below as needed; the planner reads this file verbatim.*


## Source — work item (from tracker)

**Title:** Edit Role  
**Type:** Story  
**Status:** Idea  
**Assignee:** Sara Salah El-Din  
**Labels:** MVP, Sprint1

### Description

Epic: Administration (
    
                
            
            CP-49
        
                                                    Idea
            
)

Feature: RBAC (
    
                
            
            CP-52
        
                                                    Idea
            
)

User Story: Edit Role (
    
                
            
            CP-69
        
                                                    Idea
            
)

Description: “As a Platform Admin, I want to edit an existing role, so that I can keep its name and description accurate.”

Trigger: Login -> Dashboard -> Roles & Permissions -> Roles -> Edit

Acceptance Criteria:

The edit form must include the following fields, pre-filled with the current data:

	Role Name
	
		A text input field that
		
			Accepts letters (Uppercase/Lowercase), spaces, multiple words with spaces and numbers.

			Mandatory.

			Must be unique; if the name has been used before, an error message appears.

		
		

	
	

	Description
	
		A multi-line text input field.

		Optional.

	
	

	Actions:
	
		Save:
		
			The system validates the fields before saving.

			If a mandatory field (Role Name) is left empty or cleared, the system does not save the changes, keeps the admin on the edit form, and displays the error message “Please fill required field” under the field.

			If the Role Name already exists, the system does not save the changes and displays the error message “Role name already exists”.

			If all validations pass, the system saves the changes and returns to the Roles list.

		
		

		Cancel: The system discards all changes and returns to the Roles list.

		Back:
		
			If no changes were made, the system returns to the Roles list.

			If there are unsaved changes, the system displays a confirmation dialog: “You have unsaved changes. Are you sure you want to leave?” with two options:
			
				Leave: The system discards the changes and returns to the Roles list.

				Stay: The system closes the dialog and keeps the admin on the edit form.

			
			

		
		

	
	

Handling Errors:

	If a mandatory field is left empty, the message “Please fill required field” appears.

	If the Role Name contains invalid characters, the system does not create the role and displays the error message “The system does not save the changes,  Role name must contain only letters (Uppercase/Lowercase) , numbers and spaces”.

	If the name already exists, the message “Role name already exists” appears.

	Upon successful save, the application displays “Role updated successfully”.

	If saving fails, the message “Something went wrong. Please try again.” appears.

### Attachments

None.

---
# Story intake

Fill this template for each story you want planned. Keep it copy-paste-friendly: the planner reads **this file and the files in `attachments/`**, nothing else.

- Folder: `.squad/stories/edit-role/CP-69/intake.md`
- Binaries (screenshots, PDFs, exports): put them in `attachments/` next to this file and list them below.
- Do **not** rely on external links (tracker URLs, wiki, chat) — the planner cannot open them. Paste the content you want considered.

This is **not** an implementation prompt. It is the input to the plan-generation meta-prompt bundled with squad-kit (`generate-plan.md` in the installed package).

---

## Feature

- **Feature name (display):**
- **Feature slug (folder under `plans/`):** `edit-role`

## Tracker (metadata only)

- **Tracker type:** `jira`
- **Work item id:** `CP-69` *(used in filenames and plan tables; fill manually if empty)*
- **Work item type:** `Story`
- **Status:** `Idea`
- **Assignee:** `Sara Salah El-Din`
- **Labels:** `MVP, Sprint1`

External tracker links are **not** followed by the planner. Keep the id for naming and traceability only.

---

## Title

*(Paste the work item title verbatim. Prefilled when `squad new-story` fetched from a tracker.)*

```
Edit Role
```

---

## Description

*(Paste the full work item description. Prefilled when fetched from a tracker.)*

```
Epic: Administration (
    
                
            
            CP-49
        
                                                    Idea
            
)

Feature: RBAC (
    
                
            
            CP-52
        
                                                    Idea
            
)

User Story: Edit Role (
    
                
            
            CP-69
        
                                                    Idea
            
)

Description: “As a Platform Admin, I want to edit an existing role, so that I can keep its name and description accurate.”

Trigger: Login -> Dashboard -> Roles & Permissions -> Roles -> Edit

Acceptance Criteria:

The edit form must include the following fields, pre-filled with the current data:

	Role Name
	
		A text input field that
		
			Accepts letters (Uppercase/Lowercase), spaces, multiple words with spaces and numbers.

			Mandatory.

			Must be unique; if the name has been used before, an error message appears.

		
		

	
	

	Description
	
		A multi-line text input field.

		Optional.

	
	

	Actions:
	
		Save:
		
			The system validates the fields before saving.

			If a mandatory field (Role Name) is left empty or cleared, the system does not save the changes, keeps the admin on the edit form, and displays the error message “Please fill required field” under the field.

			If the Role Name already exists, the system does not save the changes and displays the error message “Role name already exists”.

			If all validations pass, the system saves the changes and returns to the Roles list.

		
		

		Cancel: The system discards all changes and returns to the Roles list.

		Back:
		
			If no changes were made, the system returns to the Roles list.

			If there are unsaved changes, the system displays a confirmation dialog: “You have unsaved changes. Are you sure you want to leave?” with two options:
			
				Leave: The system discards the changes and returns to the Roles list.

				Stay: The system closes the dialog and keeps the admin on the edit form.

			
			

		
		

	
	

Handling Errors:

	If a mandatory field is left empty, the message “Please fill required field” appears.

	If the Role Name contains invalid characters, the system does not create the role and displays the error message “The system does not save the changes,  Role name must contain only letters (Uppercase/Lowercase) , numbers and spaces”.

	If the name already exists, the message “Role name already exists” appears.

	Upon successful save, the application displays “Role updated successfully”.

	If saving fails, the message “Something went wrong. Please try again.” appears.
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
