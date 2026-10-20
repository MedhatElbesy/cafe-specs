> **Fetched from jira:** [CP-68](https://yomnaarafahammad-1791369924187.atlassian.net/browse/CP-68)  
> *Fetched 2026-10-20T22:06:12.731Z. Edit the sections below as needed; the planner reads this file verbatim.*


## Source — work item (from tracker)

**Title:** View Role  
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

User Story: View Role (
    
                
            
            CP-68
        
                                                    Idea
            
)

Description: “As a Platform Admin, I want to view the list of roles and their details, so that I know what access each role gives.”

Trigger: Login → Dashboard -> Roles & Permissions -> Roles

Acceptance Criteria:

	The list screen displays roles in a table with the following columns:
	
		Role Name

		Description

		Number of Permissions

		Number of Assigned Users

	
	

	A search field is available to search by name.

	Actions:
	
		View Details: tapping a role opens the details screen, which displays the role data, its assigned permissions, and its linked users (read-only).

		Create Role: opens 
    
                
            
            CP-67
        
                                                    Idea
            
.

		Edit: opens 
    
                
            
            CP-69
        
                                                    Idea
            
.

		Delete: opens 
    
                
            
            CP-70
        
                                                    Idea
            
.

		Manage Permissions: Opens 
    
                
            
            CP-75
        
                                                    Idea
            
.

		Back: The system returns to the previous screen (Roles & Permissions).

	
	

Handling Errors:

	If there are no roles or the search returns no results, the message “No results found” appears.

	If the data fails to load, the message “Unable to load data. Please try again.” appears with a Retry button.

### Attachments

None.

---
# Story intake

Fill this template for each story you want planned. Keep it copy-paste-friendly: the planner reads **this file and the files in `attachments/`**, nothing else.

- Folder: `.squad/stories/view-role/CP-68/intake.md`
- Binaries (screenshots, PDFs, exports): put them in `attachments/` next to this file and list them below.
- Do **not** rely on external links (tracker URLs, wiki, chat) — the planner cannot open them. Paste the content you want considered.

This is **not** an implementation prompt. It is the input to the plan-generation meta-prompt bundled with squad-kit (`generate-plan.md` in the installed package).

---

## Feature

- **Feature name (display):**
- **Feature slug (folder under `plans/`):** `view-role`

## Tracker (metadata only)

- **Tracker type:** `jira`
- **Work item id:** `CP-68` *(used in filenames and plan tables; fill manually if empty)*
- **Work item type:** `Story`
- **Status:** `Idea`
- **Assignee:** `Sara Salah El-Din`
- **Labels:** `MVP, Sprint1`

External tracker links are **not** followed by the planner. Keep the id for naming and traceability only.

---

## Title

*(Paste the work item title verbatim. Prefilled when `squad new-story` fetched from a tracker.)*

```
View Role
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

User Story: View Role (
    
                
            
            CP-68
        
                                                    Idea
            
)

Description: “As a Platform Admin, I want to view the list of roles and their details, so that I know what access each role gives.”

Trigger: Login → Dashboard -> Roles & Permissions -> Roles

Acceptance Criteria:

	The list screen displays roles in a table with the following columns:
	
		Role Name

		Description

		Number of Permissions

		Number of Assigned Users

	
	

	A search field is available to search by name.

	Actions:
	
		View Details: tapping a role opens the details screen, which displays the role data, its assigned permissions, and its linked users (read-only).

		Create Role: opens 
    
                
            
            CP-67
        
                                                    Idea
            
.

		Edit: opens 
    
                
            
            CP-69
        
                                                    Idea
            
.

		Delete: opens 
    
                
            
            CP-70
        
                                                    Idea
            
.

		Manage Permissions: Opens 
    
                
            
            CP-75
        
                                                    Idea
            
.

		Back: The system returns to the previous screen (Roles & Permissions).

	
	

Handling Errors:

	If there are no roles or the search returns no results, the message “No results found” appears.

	If the data fails to load, the message “Unable to load data. Please try again.” appears with a Retry button.
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
