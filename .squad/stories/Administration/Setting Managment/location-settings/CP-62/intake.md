# CP-62 — Set Location Settings

## Feature

- Feature name: Setting Management
- Feature slug: location-settings
- Project: Rashfa
- Epic: Administration — CP-49
- Parent feature: Setting Management — CP-60

## Tracker

- Tracker type: jira
- Work item ID: CP-62
- Work item type: Story
- Tracker status at import: Idea
- Assignee at import: Maram Atef Saeed

## User Story

As a Platform Admin,
I want to configure the maximum distance in kilometres for café
discovery and delivery selection,
so that subscribers can find eligible cafés within the configured range.

## Trigger

Dashboard → Settings → Location Settings

## Preconditions

- The Platform Admin is signed in.
- The Platform Admin has permission to manage Location Settings.

## Business Rules

BR-01: The location range is measured in kilometres.

BR-02: The range must be a positive whole number.
The minimum valid value is 1 km.
No business-defined maximum applies.

BR-03: Before any admin configuration, the effective range is 10 km.

BR-04: Distance is measured from the subscriber’s current location.

BR-05: A successfully saved change applies to subsequent café
discovery and delivery selection operations.

BR-06: Existing delivery orders are not affected by range changes.

BR-07: Being within the configured range does not override
other café eligibility rules.

## Acceptance Criteria

### AC-01 — View Current Range

- Display the effective location range in kilometres.
- Before any admin configuration, display 10 km and identify it
  as the default value.
- Display the name of the admin who last successfully updated
  the setting, where available.
- Do not attribute the initial default value to an admin.

### AC-02 — Enter a New Range

- Allow an authorized Platform Admin to enter a new range.
- Keep Save disabled when the entered value is numerically
  equal to the current effective range.

### AC-03 — Validate the Range

- Accept positive whole numbers only.
- Reject empty values, zero, negative values, fractional values,
  and non-numeric input.
- Prevent invalid values from being saved.
- Display an inline validation message explaining that the
  value must be a whole number greater than zero.

### AC-04 — Save Successfully

- When the admin saves a valid, changed value, persist it as
  the new range.
- Display a success message.
- Update the Current Location Range card with the saved value.
- Update “Last updated by” to the signed-in admin.
- Apply the new range to subsequent discovery and delivery
  selection operations.
- Keep existing delivery orders unaffected.

### AC-05 — Handle Save Failure

- Display an error message if saving fails.
- Preserve the previously effective range.
- Do not change “Last updated by”.
- Allow the admin to retry saving.

### AC-06 — Leave Without Saving

- If the admin leaves the page without saving, discard any unsaved changes without displaying a confirmation dialog.
- Keep the effective range and “Last updated by” unchanged.
- When the admin returns, display the current effective range.

## Acceptance Examples

| Current range | Entered value        | Expected result                                   |
| ------------- | -------------------- | ------------------------------------------------- |
| Default 10 km | 10                   | Save remains disabled                             |
| 10 km         | 5                    | Save succeeds; effective range becomes 5 km       |
| 10 km         | 1                    | Save succeeds; effective range becomes 1 km       |
| 10 km         | 0                    | Validation error; range remains 10 km             |
| 10 km         | -5                   | Validation error; range remains 10 km             |
| 10 km         | 2.5                  | Validation error; range remains 10 km             |
| 10 km         | Empty                | Validation error; range remains 10 km             |
| 10 km         | abc                  | Validation error; range remains 10 km             |
| 10 km         | 20, but saving fails | Error shown; range remains 10 km; retry available |

## Attachments

| File                                                                | Description                           |
| ------------------------------------------------------------------- | ------------------------------------- |
| attachments/Location settings — current range, Save disabled@2x.png | Current range and disabled Save state |
| attachments/Location settings — save failure@2x.png                 | Save failure state                    |

The attachments must be checked against these updated requirements.
Any differences must be resolved before approving the specification.

## Related Requirements

- Café discovery.
- Delivery café selection.
- Admin permissions.
- Café eligibility rules.

## Additional Business Rules

BR-08: Distance is the direct geographical distance between
the subscriber’s current coordinates and the café’s coordinates,
not road or travel distance.

BR-09: Cafés at a distance less than or equal to the effective
range are within the geographical scope.

## Location Precondition

- The subscriber’s current location is available before café
  discovery or delivery selection.
- Handling unavailable subscriber location is outside this story’s scope.

## Out of Scope

- Changing existing delivery orders.
- Changing other café eligibility rules.
- Selecting implementation technologies.
- Technical implementation planning and code.
