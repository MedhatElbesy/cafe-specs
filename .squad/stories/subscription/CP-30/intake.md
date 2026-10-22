# CP-30 — View Details of Subscription with Delivery

## 1. User Story

**As a** Guest,  
**I want to** view the details of the With Delivery subscription option,  
**So that** I can understand what it includes and decide whether to subscribe.

## 2. Trigger

Mobile App → Subscription → With Delivery

## 3. Business Rules

**BR-01 — Subscription Price**
- The system must display the latest configured subscription price in SAR.
- The price must reflect the latest configuration available when the details page is opened or refreshed.
- Before proceeding with the subscription, the system must revalidate the current price.
- If the price has changed, the system must display the updated price and notify the user so they can review it before confirming the subscription.

**BR-02 — Subscription Validity**
- The subscription is valid for 30 calendar days.
- The activation date is Day 1.
- The subscription expires at the end of Day 30, according to Saudi Arabia local time.

**BR-03 — Daily Coffee Entitlement**
- The subscriber is entitled to one specified coffee per day.
- The coffee type is defined by the Platform Admin and is the same for all subscribers.
- Unused daily entitlements do not carry over to the following day.

**BR-04 — Delivery and Pickup**
- The subscriber must select a café for each daily order.
- For each daily order, the subscriber may choose either:
  - Delivery.
  - Pickup from the selected café.
- When delivery is selected, the system must display the saved delivery address and allow the subscriber to confirm or change it.
- Café selection and fulfillment choices are made as part of the daily ordering process, not on the subscription details page.

**BR-05 — Terms and Conditions**
- The system must display the applicable Terms and Conditions configured by the Platform Admin.
- The displayed terms must reflect the latest approved configuration when the details page is opened or refreshed.
- The page is not required to update its displayed terms automatically while it remains open.

**BR-06 — Subscription Availability**
- The system must respect the current availability status configured for the With Delivery subscription option.
- If the option is unavailable, the system must prevent the user from proceeding with the subscription.
- The system must hide the subscription action and display the following message:

  `This subscription option is currently unavailable.`

- Availability must be revalidated when the user attempts to proceed with the subscription.

## 4. Acceptance Criteria

### AC-01 — Display Subscription Details

**Given** the Guest opens the With Delivery subscription details page  
**When** the page loads successfully  
**Then** the system must display:
- The latest configured price in SAR.
- Subscription validity of 30 calendar days.
- The specified daily coffee entitlement.
- Delivery and pickup information.
- The applicable Terms and Conditions.
- The available actions according to the subscription's current availability.

### AC-02 — Display Latest Configured Information

**Given** the Platform Admin has updated the subscription price, specified coffee, or Terms and Conditions  
**When** the Guest opens or refreshes the details page  
**Then** the page must display the latest applicable configured information.

### AC-03 — Subscribe Now for a Guest

**Given** the user is not authenticated  
**When** the user clicks **Subscribe Now** while the option is available  
**Then** the system must:
1. Display the following eligibility message exactly as specified in the original requirement:

   `The subscription is available for registers only, Please Sign up`

2. Direct the user to the appropriate authentication flow:
   - Registration if the user does not have an account.
   - Login if the user already has an account.
3. Preserve the user's selection of With Delivery.

**And** after successful authentication, the user must be able to continue the With Delivery subscription process.

### AC-04 — Return After Canceling Authentication

**Given** the user has started registration or login after selecting With Delivery  
**When** the user cancels or exits the authentication process without completing it  
**Then** the system must return the user to the With Delivery details page  
**And** preserve the With Delivery selection  
**And** revalidate availability when the user attempts to proceed again.

### AC-05 — Revalidate Price Before Proceeding

**Given** the Guest has opened the With Delivery details page  
**And** the configured price has changed since the page was opened  
**When** the user clicks **Subscribe Now**  
**Then** the system must retrieve or validate the current price  
**And** display the updated price and notify the user of the change  
**And** allow the user to review the updated price before confirming the subscription.

### AC-06 — Prevent Subscription When Unavailable

**Given** the With Delivery subscription option is unavailable  
**When** the Guest opens the details page or attempts to proceed  
**Then** the system must prevent subscription progression  
**And** hide the subscription action  
**And** display:

`This subscription option is currently unavailable.`

### AC-07 — Back to Options

**Given** the Guest is viewing the With Delivery details page  
**When** the Guest clicks **Back to Options**  
**Then** the system must return the Guest to the subscription options page defined in US-29  
**And** display the options according to the latest Platform Admin configuration, including unavailable options according to their availability rules.

### AC-08 — Subscription Details Loading Failure

**Given** the system cannot load the subscription details  
**When** the loading attempt fails for a reason other than loss of internet connectivity  
**Then** the system must display:

`Unable to load subscription details. Please try again.`

**And** the user must not be allowed to proceed with the subscription using unvalidated details.

### AC-09 — No Internet Connection

**Given** the device has no internet connection  
**When** the Guest attempts to load or proceed with the subscription details  
**Then** the system must display:

`No internet connection. Please try again.`

**And** the user must not be allowed to proceed without the required validation.

### AC-10 — Daily Coffee Entitlement

**Given** the subscriber has an active With Delivery subscription  
**When** a new subscription day begins  
**Then** the subscriber is entitled to one specified coffee for that day  
**And** any unused entitlement from the previous day must not be carried forward.

### AC-11 — Daily Order Fulfillment

**Given** the subscriber places a daily coffee order under an active With Delivery subscription  
**When** the subscriber proceeds with the order  
**Then** the subscriber must select a café for that order  
**And** choose either delivery or pickup  
**And**, if delivery is selected, be able to confirm or change the saved delivery address.

## 5. Error Messages

| Scenario | Message |
|---|---|
| Subscription details cannot be loaded | `Unable to load subscription details. Please try again.` |
| No internet connection | `No internet connection. Please try again.` |
| Subscription option unavailable | `This subscription option is currently unavailable.` |
| Guest attempts to subscribe | `The subscription is available for registers only, Please Sign up` |

## 6. Dependencies

- **Platform Admin configuration:** Subscription price, specified coffee, Terms and Conditions, and availability status.
- **US-29 — Subscription Options:** Return navigation and display of currently configured subscription options.
- **Registration and Login:** Authentication flow must preserve the selected With Delivery option and allow the user to continue after successful authentication.
- **Subscription Process:** Must validate availability and price before allowing subscription progression.
- **Daily Ordering Process:** Café selection, delivery/pickup selection, saved-address confirmation or modification, and daily entitlement fulfillment.

## 7. Scope Boundaries

- This story covers viewing the With Delivery subscription details and navigating into or out of the subscription flow.
- Selecting a café, choosing delivery or pickup, and confirming or changing the delivery address occur in the daily ordering process.
- Detailed rules for recording daily entitlement redemption and changing fulfillment choices after an order has been created are outside this story unless explicitly included in the daily ordering requirements.
- No additional payment, refund, cancellation, or technical implementation behavior is assumed by this specification.