# CP-29 — View Subscription Options

## 1. User Story

**As a** Guest,  
**I want to** view the available subscription options without creating an account,  
**So that** I can compare them and choose the option that best meets my needs.

## 2. Trigger

Mobile App → Subscription

## 3. Business Rules

### BR-01 — Subscription Options Visibility
- The system must display subscription options that have been configured by the Platform Admin.
- Options that have not been configured must not be displayed.
- If a configured option is temporarily unavailable, its card must remain visible with a clear indication that the option is unavailable.
- The Guest may open the details page of an unavailable option to review its information, but must not be allowed to start the subscription process for that option.

### BR-02 — Subscription Card Content
Each configured subscription option card must display the following fields in the same order:
1. Option name.
2. Price in SAR.
3. Subscription validity: “30 calendar days”.
4. Daily entitlement: one cup per day of the specified coffee defined by the Platform Admin.

The specified coffee is the same for all subscribers.

### BR-03 — Price Updates
- When the subscription options page is opened or refreshed, each card must display the latest configured price available from the Platform Admin.
- If the price changes after the options page has loaded, the card may continue displaying the price already shown until the page is refreshed or reopened.
- When the Guest opens an option's details page, that page must display the latest configured price, even if it differs from the price previously displayed on the card.

### BR-04 — Option Selection and Navigation
- The entire subscription option card must be clickable.
- Selecting the Without Delivery card must open the details page defined in US-31.
- Selecting the With Delivery card must open the details page defined in US-30.
- Opening an unavailable option's details page is permitted for information purposes.
- Starting a subscription for an unavailable option must be prevented according to the applicable details-page and subscription-process rules.

### BR-05 — No Configured Options
If no subscription options have been configured, the system must display:

`Subscription options are currently unavailable.`

No subscription option cards should be displayed in this state.

### BR-06 — Guest Access
- Guests must be able to open the subscription options page without creating an account or logging in.
- Viewing and comparing the option cards must not require authentication.
- Authentication requirements for subscribing are handled by the respective subscription details and subscription process stories.

## 4. Acceptance Criteria

### AC-01 — Display Configured Subscription Options

**Given** the Guest opens the Subscription page  
**When** subscription options are loaded successfully  
**Then** the system must display the configured subscription option cards  
**And** each card must represent its corresponding option: Without Delivery or With Delivery  
**And** options that have not been configured must not be displayed.

### AC-02 — Display Consistent Card Information

**Given** one or more subscription options are configured  
**When** the option cards are displayed  
**Then** each card must show:
- Option name.
- Price in SAR.
- Validity of 30 calendar days.
- Daily entitlement of one cup per day of the specified coffee.

**And** all cards must display these fields in the same order to support comparison.

### AC-03 — Display the Latest Configured Price

**Given** the Platform Admin has configured a price for a subscription option  
**When** the Guest opens or refreshes the Subscription page  
**Then** the corresponding card must display the latest configured price in SAR.

### AC-04 — Open Subscription Details

**Given** a subscription option card is displayed  
**When** the Guest selects any part of the card  
**Then** the system must open the details page corresponding to the selected option:
- With Delivery → US-30.
- Without Delivery → US-31.

### AC-05 — Display the Latest Price on the Details Page

**Given** the Guest has viewed an option card  
**And** the configured price has changed since the card was displayed  
**When** the Guest opens the option's details page  
**Then** the details page must display the latest configured price, even if it differs from the price shown on the card.

### AC-06 — Display an Unavailable Option

**Given** a subscription option has been configured but is temporarily unavailable  
**When** the Guest opens the Subscription page  
**Then** the option's card must remain visible  
**And** the card must clearly indicate that the option is unavailable.

### AC-07 — Open Details for an Unavailable Option

**Given** a configured subscription option is unavailable  
**When** the Guest selects its card  
**Then** the system must allow the Guest to open its details page and review its information  
**But** the Guest must not be allowed to start the subscription process for that unavailable option.

### AC-08 — No Configured Subscription Options

**Given** no subscription options have been configured by the Platform Admin  
**When** the Guest opens the Subscription page and the options are loaded successfully  
**Then** no option cards must be displayed  
**And** the system must display:

`Subscription options are currently unavailable.`

### AC-09 — Subscription Options Loading Failure

**Given** the subscription options cannot be loaded due to a loading failure other than loss of internet connectivity  
**When** the Guest opens the Subscription page  
**Then** the system must display:

`Unable to load subscription options. Please try again.`

### AC-10 — No Internet Connection

**Given** the device has no internet connection  
**When** the Guest attempts to load the subscription options  
**Then** the system must display:

`No internet connection. Please try again.`

## 5. Error and Empty-State Messages

| Scenario | Required message |
|---|---|
| No configured subscription options | `Subscription options are currently unavailable.` |
| Subscription options fail to load | `Unable to load subscription options. Please try again.` |
| No internet connection | `No internet connection. Please try again.` |

An unavailable configured option must have a clear unavailability indicator. The exact wording for that indicator has not been specified.

## 6. Dependencies

### Platform Admin Configuration
The page depends on the Platform Admin for:
- Configured subscription options.
- Current price for each option.
- The specified daily coffee type.
- Availability status of each option.

### US-30 — View Details of Subscription with Delivery
The With Delivery card must navigate to the corresponding details page, which governs the With Delivery subscription's details and availability rules.

### US-31 — View Details of Subscription without Delivery
The Without Delivery card must navigate to the corresponding details page, which governs the Without Delivery subscription's details and availability rules.

### Subscription Process
The subscription process must prevent a Guest from starting a subscription for an option that is unavailable.

## 7. Scope Boundaries

This story covers:
- Displaying configured subscription options.
- Presenting comparable option-card information.
- Displaying current configured prices when the page is opened or refreshed.
- Navigating from an option card to its corresponding details page.
- Displaying unavailable options and the no-options empty state.
- Handling subscription-options loading failures and lack of internet connectivity.

This story does not define:
- The detailed content or behavior of the With Delivery or Without Delivery details pages.
- Registration, login, payment, or subscription confirmation flows.
- Daily order fulfillment or coffee redemption.
- The exact wording or visual design of the unavailable-option indicator.
- Technical implementation details or architecture.

