# Functional Specification — CP-31

## 1. Story Information

- **Story ID:** CP-31
- **Title:** View details of subscription without delivery
- **Feature:** Subscription
- **Feature Slug:** `subscription`
- **Tracker:** Jira
- **Work Item Type:** Story
- **Status:** Idea
- **Assignee:** Fardous Gamal

## 2. User Story

As a Guest, I want to view the details of subscription Without Delivery option, so that I can understand what it includes and decide whether to subscribe.

## 3. Trigger

Mobile App → Subscription → Without Delivery card.

The Guest selects the Without Delivery card from the subscription options page (US-29) to view its details.

## 4. Preconditions

1. The Guest can access the subscription options page without creating an account.
2. The Without Delivery subscription option is configured and available for display.
3. The subscription price and applicable terms and conditions have been configured by the Platform Admin.
4. The specified coffee type has been defined by the Platform Admin.
5. The user can navigate back to the subscription options page (US-29).

If the option is unavailable, the system must display the specified unavailability message and must not allow the Guest to proceed with that option.

## 5. Fields and Display Requirements

| Field | Required behavior |
|---|---|
| Subscription Price | Display the configured price in SAR. The displayed price must reflect the latest configured price when the details page is opened or refreshed. |
| Subscription Validity | Display the validity period as “30 calendar days”. |
| Daily Coffee Entitlement | Display the daily entitlement and the name of the specified coffee configured by the Platform Admin. |
| Daily Entitlement Rule | Communicate that the entitlement is one specified coffee per day and unused daily entitlement does not carry over to the following day. |
| Redemption Method | Explain that the subscriber redeems the daily cup at a participating café by showing the QR code to the cashier. |
| Delivery Availability | Clearly state that delivery is not included in the Without Delivery option. |
| Terms and Conditions | Display the latest published terms and conditions applicable to this subscription option. |

## 6. Business Rules

**BR-01 — Price Currency**

The subscription price must be displayed in Saudi Arabian Riyals (SAR).

**BR-02 — Price Freshness**

The price displayed on the details page must reflect the latest configured price when the page is opened or refreshed. The price does not need to update automatically while the page remains open.

**BR-03 — Price Change During Subscription Initiation**

If the price changes between viewing the details page and confirming the subscription, the user must be shown the updated price and informed that the price has changed. The user must be able to review the updated price before confirming the subscription.

The final confirmation flow is a dependency of the subscription process and must be aligned with its requirements.

**BR-04 — Subscription Validity**

The subscription validity period is 30 calendar days.

The activation date is counted as Day 1. The subscription expires at the end of Day 30 according to Saudi Arabia time.

**BR-05 — Specified Coffee**

The Platform Admin defines one specified coffee type for the Without Delivery option. The same specified coffee type applies to all subscribers using this option.

The details page must display the coffee name. Displaying an additional coffee description is not required.

**BR-06 — Daily Entitlement**

The subscriber is entitled to one specified coffee per day.

Unused daily entitlement expires at the end of the applicable day and cannot be carried over to the following day.

**BR-07 — Redemption Method**

The subscriber redeems the daily cup at a participating café by showing the QR code to the cashier.

Delivery is not included in this subscription option.

**BR-08 — Terms and Conditions**

The details page must display the latest published terms and conditions configured by the Platform Admin for the Without Delivery option.

Whether explicit acceptance is mandatory, and where that acceptance occurs, is determined by the subscription process requirements rather than this story.

**BR-09 — Subscribe Now**

When the option is available, selecting Subscribe Now initiates the subscription process with Without Delivery preselected.

If the Guest is not authenticated, the user must proceed to registration or login before continuing. After successful authentication, the Without Delivery selection must be retained.

**BR-10 — Availability Revalidation**

If the option becomes unavailable after the details page has been opened, the system must prevent the user from proceeding when Subscribe Now is selected and display the specified unavailability message.

The Subscribe Now action must be hidden when the option is known to be unavailable on the details page.

**BR-11 — Back to Options**

Selecting Back to Options returns the user to the subscription options page (US-29), where the options displayed reflect the platform's current configuration.

**BR-12 — Authentication Cancellation**

If the user cancels registration or login, the user must return to the Without Delivery details page with the option selection retained.

Availability must be rechecked when the user attempts to subscribe again.

**BR-13 — Error Handling**

The system must distinguish between the following conditions:

- No internet connection.
- Failure to load subscription details despite internet connectivity.
- Subscription option unavailable.

The corresponding messages are specified in Section 8.

## 7. Acceptance Criteria

### AC-01 — Display Subscription Price

**Given** the Guest opens the Without Delivery details page and the option is available,

**When** the details are displayed,

**Then** the system displays the configured subscription price in SAR.

**And** the displayed price reflects the latest configured price at the time the page is opened.

### AC-02 — Refresh Subscription Price

**Given** the subscription price has changed in the Platform Admin configuration,

**When** the Guest reopens or refreshes the details page,

**Then** the latest configured price is displayed.

**And** the page is not required to update the price automatically while it remains open.

### AC-03 — Handle Price Changes Before Confirmation

**Given** the Guest has viewed the subscription price,

**And** the price changes before subscription confirmation,

**When** the updated price is presented during the subscription process,

**Then** the system displays the new price and informs the user that the price has changed.

**And** the user can review the updated price before confirming the subscription.

### AC-04 — Display Subscription Validity

**Given** the Guest opens the Without Delivery details page,

**When** the subscription details are displayed,

**Then** the validity period is displayed as “30 calendar days”.

**And** the activation date is counted as Day 1.

**And** the subscription expires at the end of Day 30 according to Saudi Arabia time.

### AC-05 — Display Specified Coffee and Daily Entitlement

**Given** the specified coffee has been configured by the Platform Admin,

**When** the Guest views the subscription details,

**Then** the system displays the name of the specified coffee.

**And** the system communicates that the entitlement is one specified coffee per day.

**And** the system communicates that unused daily entitlement does not carry over to the following day.

### AC-06 — Display Redemption Method and Delivery Exclusion

**Given** the Guest views the Without Delivery details page,

**When** the subscription information is displayed,

**Then** the system explains that the daily cup can be redeemed at a participating café by showing the QR code to the cashier.

**And** the system clearly states that delivery is not included.

### AC-07 — Display Current Terms and Conditions

**Given** the Platform Admin has published terms and conditions for the Without Delivery option,

**When** the Guest opens the details page,

**Then** the system displays the latest published terms and conditions applicable to the option.

### AC-08 — Proceed with Without Delivery

**Given** the subscription option is available,

**When** the Guest selects Subscribe Now,

**Then** the subscription process is initiated with Without Delivery selected.

### AC-09 — Require Authentication Before Continuing

**Given** the Guest is not authenticated,

**When** the Guest selects Subscribe Now,

**Then** the user is directed to registration or login.

**And** after successful authentication, the user can continue with Without Delivery selected.

### AC-10 — Return After Authentication Cancellation

**Given** the Guest has been directed to registration or login,

**When** the Guest cancels that process,

**Then** the user returns to the Without Delivery details page.

**And** the Without Delivery selection is retained.

**And** availability is rechecked when the user attempts to subscribe again.

### AC-11 — Return to Subscription Options

**Given** the Guest is viewing the Without Delivery details page,

**When** the Guest selects Back to Options,

**Then** the system returns the user to US-29.

**And** the subscription options displayed reflect the platform's current configuration.

### AC-12 — Handle an Unavailable Option

**Given** the Without Delivery option is unavailable,

**When** the details page is displayed,

**Then** the system displays the message:

“This subscription option is currently unavailable.”

**And** the Subscribe Now action is hidden.

### AC-13 — Revalidate Availability Before Proceeding

**Given** the option was available when the details page was opened,

**But** the option becomes unavailable before the Guest proceeds,

**When** the Guest selects Subscribe Now,

**Then** the system prevents the user from continuing with that option.

**And** the system displays:

“This subscription option is currently unavailable.”

### AC-14 — Handle Failure to Load Details

**Given** an internet connection is available,

**When** the system fails to load the subscription details,

**Then** the system displays:

“Unable to load subscription details. Please try again.”

### AC-15 — Handle No Internet Connection

**Given** the device has no internet connection,

**When** the Guest attempts to load the subscription details,

**Then** the system displays:

“No internet connection. Please try again.”

### AC-16 — Navigate Away After Unavailability

**Given** the Without Delivery option is unavailable,

**When** the Guest selects Back to Options,

**Then** the system returns the Guest to US-29.

**And** the options displayed reflect the platform's current configuration.

## 8. Acceptance Examples

| Scenario | Expected result |
|---|---|
| The configured price is SAR 120 when the page opens. | The page displays SAR 120. |
| The price changes to SAR 150 and the Guest refreshes the page. | The page displays SAR 150. |
| The price changes after the Guest viewed SAR 120 but before subscription confirmation. | The user is informed of the change and can review the updated price before confirmation. |
| The subscription is activated on 1 November. | 1 November is Day 1; the subscription expires at the end of Day 30 in Saudi Arabia time. |
| The user has not redeemed the daily cup on a particular day. | That day's unused entitlement does not carry over to the next day. |
| The option becomes unavailable while the details page is open. | Proceeding is prevented and the unavailability message is displayed. |
| The Guest cancels login after selecting Subscribe Now. | The Guest returns to the details page with Without Delivery retained. |
| The details request fails while internet connectivity is available. | The load-failure message is displayed. |
| The device has no internet connection when the Guest attempts to load details. | The no-internet message is displayed. |
| The Guest selects Back to Options. | The Guest returns to US-29 and sees options based on current platform configuration. |

## 9. Attachments and Related Requirements

### Attachments

The intake references the following files:

1. `attachments/Grayscale Coffee Subscription Wireframe.png`
2. `attachments/Grayscale Subscription UX Wireframe Flow.png`

The images were referenced in the supplied intake, but their visual content has not been reviewed as part of this specification.

### Related Requirements

- **US-29:** Subscription options cards. Provides the entry point to the details page and the return destination for Back to Options.
- **Subscription process:** Owns the subscription confirmation flow, final price review, and any mandatory terms acceptance.
- **Registration/Login flow:** Owns authentication while preserving the user's selected subscription option when returning to the details page.

No additional Jira IDs have been assumed.

## 10. Remaining Open Questions

1. **Price-change handling:** The subscription process must define the precise point at which the updated price is shown and reviewed. The user must be informed of the change before confirming the subscription.
2. **Availability response:** The business requirement defines the message and prevents proceeding when the option becomes unavailable. The exact destination after displaying this message, other than the explicit Back to Options action, remains unspecified.
3. **Wireframe alignment:** The referenced wireframes need to be reviewed to confirm that the page layout, labels, and navigation are consistent with the agreed requirements.
4. **Terms acceptance:** Whether acceptance is mandatory is outside CP-31 and must be confirmed by the subscription process requirements.

These open questions do not change the confirmed requirements above but must be resolved by the relevant owners where applicable.

## 11. Out of Scope

The following are outside the scope of CP-31 unless separately specified:

- Registration and login functionality itself.
- Subscription purchase, payment processing, and payment failure handling.
- Final subscription confirmation and the business rules governing the subscription transaction.
- QR code generation, validation, and café redemption processing.
- Café participation management.
- Platform Admin configuration screens for prices, coffee types, availability, and terms and conditions.
- Enforcement of daily entitlement limits during redemption.
- Delivery ordering, fulfillment, and delivery fees.
- Defining or enforcing mandatory terms acceptance within the subscription process.
- Technical implementation details, including APIs, database schemas, system architecture, and code.

---

**Specification status:** Consolidated draft based on the confirmed business decisions. Wireframe alignment and the identified cross-story dependencies remain to be verified before the specification can be considered fully validated.