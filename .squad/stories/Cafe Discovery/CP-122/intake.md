# CP-122 — View Cafes

## 1. User Story

**As a Guest,** I want to view the list of participating cafés, so that I can discover cafés available for use.

## 2. Trigger

Mobile App → Cafés

## 3. Preconditions

- The Guest opens the Cafés screen.
- Location permission is required to retrieve the user's location and sort cafés by distance.
- Café profile information, mandatory fields, business hours, and operational status are configured in the system.

## 4. Business Rules

**BR-01 — Café Eligibility**

The system must display only cafés that meet all the following conditions:

- The café is active.
- The café profile is complete, meaning all mandatory fields defined in the café profile are completed.

Suspended, inactive, or incomplete-profile cafés must not be displayed.

**BR-02 — Café Status**

Each displayed café must show the status `open/closed`.

**BR-03 — Café Sorting**

The system must sort cafés by their distance from the user's current location, from nearest to farthest.

**BR-04 — Location Permission**

If the Guest denies location permission or the system cannot determine the user's location, the system must request that the Guest enable location access and must not display the café list until the location is available.

**BR-05 — Latest Data**

The system must retrieve the latest café data from the server whenever the Guest opens the Cafés screen or manually refreshes it.

## 5. Acceptance Criteria

### AC-01 — Display Participating Cafés

**Given** the Guest has opened the Cafés screen and the system has successfully retrieved the user's location,

**When** the system successfully retrieves the café list,

**Then** the system must display only cafés that are active and have complete profiles.

### AC-02 — Display Café Information

**Given** a café meets all display eligibility criteria,

**When** the café appears in the list,

**Then** its card must display:

- Café image.
- Café name.
- Café address.
- Status: `Open/Closed`.

### AC-03 — Sort Cafés by Distance

**Given** the system has successfully determined the user's location and retrieved eligible cafés,

**When** the café list is displayed,

**Then** cafés must be ordered from nearest to farthest based on their distance from the user's location.

### AC-04 — Handle Missing Location

**Given** the Guest has denied location permission or the system cannot determine the user's location,

**When** the Guest opens or refreshes the Cafés screen,

**Then** the system must request that the Guest enable location access and must not display the café list until the location is available.

### AC-05 — Handle an Empty Café List

**Given** the user's location has been determined successfully and the system has successfully retrieved the latest café data,

**When** no cafés meet the eligibility criteria,

**Then** the system must display:

`No cafés are currently available.`

### AC-06 — Open Café Details

**Given** the café list is displayed,

**When** the Guest taps anywhere on a café card,

**Then** the system must navigate to the Café Details screen defined in CP-23 for the selected café.

### AC-07 — Refresh Café List

**Given** the Guest is on the Cafés screen,

**When** the Guest opens the screen or manually requests a refresh,

**Then** the system must retrieve the latest café data from the server and update the displayed list according to the business rules.

### AC-08 — Handle Loading Failure

**Given** the system cannot load the café list because the request fails,

**When** the loading failure occurs,

**Then** the system must display:

`Unable to load cafés. Please try again`

**And** provide a `Retry` button that allows the Guest to attempt loading the list again.

### AC-09 — Handle No Internet Connection

**Given** the Guest has no internet connection,

**When** the Guest opens or refreshes the Cafés screen,

**Then** the system must display:

`No internet connection. Please try again`

**And** provide a `Retry` button that allows the Guest to attempt loading the list again.

## 6. Dependencies

- **CP-23 — Café Details:** Destination screen opened when the Guest selects a café card.
- **Café Profile Configuration:** Defines mandatory profile fields, registered business hours, and café status.
- **Location Services:** Required to determine the user's location and sort cafés by distance.

## 7. Out of Scope

- Displaying inactive, suspended, or incomplete-profile cafés.
- Displaying the café list when the user's location is unavailable.
- Defining the detailed layout or content of the Café Details screen, which belongs to CP-23.
- Defining a fallback sorting method when location access is unavailable.
