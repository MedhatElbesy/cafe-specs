# CP-25 — Filter by Location

## 1. User Story

**As a Visitor,** I want to filter the participating cafés by city, so that I can quickly find the cafés available in the city I am interested in.

**Trigger:** Mobile App → Cafés → Filter → City

## 2. Description

The system shall allow visitors to filter the café list by city without requiring registration or sign-in.

At launch, the supported cities are Riyadh and Jeddah. The visitor can select one city at a time. If no city is explicitly selected, the system shall attempt to determine the visitor's city from their location. If the location is unavailable or permission is denied, Riyadh shall be selected by default.

The café list shall remain consistent with CP-122 and CP-23.

## 3. Functional Requirements

### FR-01: Guest Access
- The city filter shall be accessible without registration or sign-in.
- Visitors shall be able to browse cafés and apply or clear the city filter as guests.

### FR-02: Available Cities
- The system shall support Riyadh and Jeddah at launch.
- Only these two cities shall be available for selection.
- Each café shall be associated with a city.

### FR-03: Default City Selection
- When the visitor opens the Cafés screen without a previously selected city, the system shall attempt to determine the visitor's city using the available location service.
- If the visitor's location identifies Riyadh or Jeddah, the system shall select the corresponding city.
- If location permission is denied or the visitor's city cannot be determined, the system shall select Riyadh by default.
- The visitor shall be able to change the selected city manually.

### FR-04: City Selection and Application
- The visitor shall be able to select only one city at a time.
- Selecting a city in the filter shall not apply the filter immediately.
- The system shall apply the selected city filter only after the visitor taps **Apply**.
- After applying the filter, the Cafés screen shall display only cafés associated with the selected city that meet the visibility rules in FR-05.

### FR-05: Café Visibility Rules
The filtered list shall display cafés that:
- Are active.
- Have complete profiles, with all mandatory café profile fields completed.
- Belong to the selected city.

Cafés shall remain visible whether their operating status is `Open` or `Closed`.

The café's operating status shall be determined by its registered business hours at the current time. Closed cafés may be displayed, but their status must be clearly indicated.

Suspended, inactive, or incomplete-profile cafés shall not be displayed as available participating cafés.

These rules shall remain consistent with CP-122 — View Cafés and CP-23 — View Café Details.

### FR-06: Selected City Indicator
- After applying the filter, the selected city shall be clearly displayed on the Cafés screen, such as through a city name or badge.
- The visitor shall be able to reopen the filter and change the selected city.

### FR-07: Clear Filter
- The visitor shall be able to clear the selected city filter.
- After clearing the filter, the system shall display eligible cafés from all supported cities.
- The selected-city indicator shall be removed.

### FR-08: No Results
- If no eligible cafés are available in the selected city, the system shall display:

  `No participating cafés are available in this city right now.`

- The selected city shall remain selected and visible.
- The visitor shall be able to change the city or clear the filter.

### FR-09: Loading Failure
- If the system fails to load the café list, it shall display:

  `Unable to load cafés. Please try again`

- A **Retry** action shall be provided.
- The selected city shall be retained when the visitor retries.

### FR-10: No Internet Connection
- If the visitor has no internet connection while loading or refreshing the café list, the system shall display:

  `No internet connection. Please try again`

- A **Retry** action shall be provided.
- The selected city shall be retained when the visitor retries.

## 4. Acceptance Criteria

**AC-01 — Guest Access**
Given the visitor is not signed in, when they open the Cafés screen and access the city filter, then the filter shall be available without requiring authentication.

**AC-02 — Supported Cities**
Given the visitor opens the city filter, when the available cities are displayed, then Riyadh and Jeddah shall be the only selectable cities.

**AC-03 — Automatic City Selection**
Given no city has been explicitly selected, when the visitor's location identifies Riyadh or Jeddah, then the corresponding city shall be selected automatically.

**AC-04 — Default City**
Given the visitor denies location permission or the location service cannot determine their city, when the Cafés screen initializes, then Riyadh shall be selected by default.

**AC-05 — Single-City Selection**
Given the visitor opens the city filter, when they select a city, then only one city shall be selected at a time.

**AC-06 — Apply Filter**
Given the visitor has selected a city, when they tap Apply, then the system shall display eligible cafés belonging to that city.

**AC-07 — Café Visibility**
Given a city filter is applied, when the café list is displayed, then it shall include active cafés with complete profiles in that city, regardless of whether they are Open or Closed, and exclude suspended, inactive, or incomplete-profile cafés.

**AC-08 — Selected City Indicator**
Given the visitor has applied a city filter, when the Cafés screen is displayed, then the selected city shall be clearly indicated.

**AC-09 — Change City**
Given a city filter has been applied, when the visitor opens the filter and selects another city, then the new city shall replace the previous selection after the visitor taps Apply.

**AC-10 — Clear Filter**
Given a city filter has been applied, when the visitor clears the filter, then the system shall display eligible cafés from all supported cities and remove the selected-city indicator.

**AC-11 — Empty Results**
Given the selected city has no eligible cafés, when the filtered list is loaded successfully, then the system shall display `No participating cafés are available in this city right now.` and retain the selected city.

**AC-12 — Loading Failure**
Given the café list cannot be loaded, when the system encounters a loading error, then it shall display `Unable to load cafés. Please try again`, provide a Retry action, and retain the selected city.

**AC-13 — No Internet**
Given the visitor has no internet connection, when the system attempts to load or refresh the café list, then it shall display `No internet connection. Please try again`, provide a Retry action, and retain the selected city.

## 5. Dependencies

- **CP-122 — View Cafés:** Provides the café list and the café eligibility and visibility rules.
- **CP-23 — View Café Details:** Ensures consistent café operating-status behavior and visibility rules.
- **City Data:** Each café must be associated with a supported city.
- **Location Service:** Used to determine the visitor's city for automatic selection.
- **Network/API Service:** Required to retrieve the latest café list and apply the selected city filter.

## 6. Assumptions and Scope Notes

- The initial supported cities are Riyadh and Jeddah only.
- Riyadh is the default city when the visitor's location is unavailable or permission is denied and no city has been explicitly selected.
- The filter is applied only after the visitor taps Apply.
- The city filter does not change café activation, profile-completeness, or operating-status rules.
- The filter does not require authentication.
- This story covers city-based filtering only; distance-based sorting and other café filters are outside its scope unless defined by another user story.
