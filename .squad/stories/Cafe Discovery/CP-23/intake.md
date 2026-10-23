# CP-23 — View Café Details

## 1. User Story

**As a Guest,** I want to view the details of a participating café branch, so that I can decide whether the branch is suitable and available before visiting it.

## 2. Trigger

Mobile App → Cafés → Select Café → Café Details

## 3. Preconditions

- The Guest can access the Café Details screen without registration or sign-in.
- The selected café branch has a registered profile.
- The system can retrieve the latest available café information and operational status when an internet connection is available.

## 4. Business Rules

### BR-01 — Café Eligibility

A café branch may be displayed as an available participating café only if:

- Its operational status is active.
- Its profile is complete, meaning all mandatory fields defined in the café profile are completed.

Suspended, inactive, or incomplete-profile cafés must not be displayed as available participating cafés.

### BR-02 — Café Operating Status

The system must display the branch's current operating status as one of the following:

- `Open`
- `Closed`

The operating status must be determined according to the branch's registered business hours and the current time.

No separate participating-status field is displayed on the Café Details screen.

### BR-03 — Closed Branches

A closed branch may remain visible for informational purposes.

However, the `Request Order` action must not be available for use while the branch is closed.

### BR-04 — Latest Status

Whenever the Guest opens or refreshes the Café Details screen, the system must retrieve the latest available café information and operating status from the server.

### BR-05 — Guest Access

The Café Details screen must be accessible without registration or sign-in.

The `Request Order` action is available only to subscribers with an active subscription.

### BR-06 — Location Information

The screen must display the café's registered address as text.

When the Guest selects `View on Map / Get Directions`, the system must use the supported external map service to display the café's registered geographical location.

### BR-07 — Request Order

The system must display the `Request Order` action to subscribers who have an active subscription.

The action must be unavailable while the selected café branch is closed.

The order-creation flow is handled by CP-134.

## 5. Acceptance Criteria

### AC-01 — Display Café Details

**Given** the Guest selects an eligible participating café branch,

**When** the Café Details screen opens,

**Then** the system must display:

- Café name.
- Café address.
- Café operating status (`Open` or `Closed`).
- The `View on Map / Get Directions` action.

### AC-02 — Display Current Operating Status

**Given** the Guest opens or refreshes the Café Details screen,

**When** the latest café information is retrieved successfully,

**Then** the system must display the branch's current operating status according to its registered business hours.

**And** the status must be displayed as either `Open` or `Closed`.

### AC-03 — Handle Closed Branches

**Given** the selected café branch is closed,

**When** the Guest opens its details,

**Then** the Café Details screen may remain accessible for informational purposes.

**And** the system must display the status `Closed`.

**And** the `Request Order` action must be unavailable.

### AC-04 — Handle Ineligible Café Branches

**Given** a café branch is suspended, inactive, or has an incomplete profile,

**When** the system determines which branches can be displayed as available participating cafés,

**Then** the branch must not be displayed as an available participating café.

### AC-05 — Access Without Registration

**Given** the user is a Guest who has not registered or signed in,

**When** the Guest opens the details of an eligible café branch,

**Then** the system must allow access to the Café Details screen without requiring registration or sign-in.

### AC-06 — Display Request Order for Eligible Subscribers

**Given** the user is a subscriber with an active subscription,

**And** the selected café branch is open,

**When** the subscriber opens the Café Details screen,

**Then** the system must display the `Request Order` action.

**And** selecting the action must initiate the order-creation flow defined in CP-134.

### AC-07 — Restrict Request Order for Closed Branches

**Given** the selected café branch is closed,

**When** a subscriber opens the Café Details screen,

**Then** the `Request Order` action must be unavailable.

### AC-08 — View Café Location on a Map

**Given** the café's registered geographical location is available,

**When** the Guest selects `View on Map / Get Directions`,

**Then** the system must use the supported external map service to display the café's registered location.

### AC-09 — Handle Unavailable Café Location

**Given** the system cannot retrieve the café's registered geographical location,

**When** the Guest attempts to view the location,

**Then** the system must display:

`Location is currently unavailable.`

**And** the Café Details screen must remain accessible.

### AC-10 — Handle External Map Service Failure

**Given** the external map service is temporarily unavailable,

**When** the Guest attempts to view the café's location,

**Then** the system must display:

`Map service is temporarily unavailable.`

**And** the Café Details screen must remain accessible.

### AC-11 — Handle No Internet Connection

**Given** there is no internet connection,

**When** the Guest opens or refreshes the Café Details screen,

**Then** the system must display:

`No internet connection. Please try again`

### AC-12 — Return to Café List

**Given** the Guest is viewing the Café Details screen,

**When** the Guest selects `Back`,

**Then** the system must return the Guest to the café list.

## 6. Error Messages

| Scenario | Message |
|---|---|
| Café location cannot be retrieved | `Location is currently unavailable.` |
| External map service is temporarily unavailable | `Map service is temporarily unavailable.` |
| No internet connection | `No internet connection. Please try again` |

## 7. Dependencies

- **CP-122 — View Cafes:** Provides the café list and must support displaying eligible branches that are either `Open` or `Closed`.
- **CP-134 — Request Order:** Defines the order-creation flow initiated by an eligible subscriber.
- **Café Profile Configuration:** Defines mandatory profile fields, registered address, geographical location, and business hours.
- **External Map Service:** Displays the café's registered geographical location and directions.

## 8. Out of Scope

- The detailed order-creation process covered by CP-134.
- Displaying suspended, inactive, or incomplete-profile cafés as available participating cafés.
- A separate participating-status field on the Café Details screen.
- Order creation while the café branch is closed.
- The internal implementation of the external map service.
