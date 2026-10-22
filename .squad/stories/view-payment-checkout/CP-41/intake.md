# CP-41: View Payment Checkout

### User Story

As a registered customer, I want to view a checkout summary of my selected subscription package before paying, so that I can review the package details and total amount before activating my subscription benefits.

### Trigger

Customer Account → Subscription → Subscribe Now

### Description

The system shall display a read-only checkout summary of the selected subscription package before the customer proceeds to payment.

The checkout summary shall include:

* **Package Name:** The name of the selected subscription package.
* **Validity:** 30 calendar days.
* **Daily Entitlement:** One coffee per day.
* **Included Coffee Types:** Display the coffee type if there is only one available type, or display all included types if there are multiple types.
* **Redemption Method:**

  * Without Delivery: In-store redemption.
  * With Delivery: By delivery.
* **Total Amount:** The latest price configured by the Platform Admin, displayed in SAR. The displayed price is final, with no additional delivery charges.

Subscription start and expiry dates shall not be displayed on the checkout screen.

### Payment Notice

The checkout screen shall display the following notices:

* The subscription activates only after successful payment verification.
* Unused daily coffee entitlements do not carry over to the next day.

### Actions

**1. Proceed to Payment**

* The system shall validate the customer's subscription status, package availability, package details, included coffee types, and latest configured price before proceeding.
* The Proceed to Payment button shall be temporarily disabled while validation is in progress.
* If the price changes, the system shall update the displayed price and require the customer to press Proceed to Payment again.
* If both the price and included coffee types change, the system shall update both together and require the customer to review the changes and press Proceed to Payment again.
* If the price changes again during a subsequent validation, the system shall update the price again and require another press.
* If validation fails, the system shall display an error message and allow the customer to retry from the same screen.
* If the package is no longer available, the system shall block proceeding and display an explanatory message with a **Back to Subscription Options** button.
* If all included coffee types become unavailable, the system shall block proceeding and display an explanatory message with a **Back to Subscription Options** button.
* If the customer's session expires, the system shall require the customer to log in again. After successful login, the customer shall return to the Home screen and start the process again.
* When validation succeeds without changes requiring customer review, the system shall proceed to the electronic payment screen.

**2. Change Package**

* The system shall return the customer to the subscription options screen.
* The customer shall select the package and redemption method again.
* The previously selected redemption method shall not be retained automatically.
* If the customer selects Change Package while validation is in progress, the system shall stop the current validation, ignore its result, and return to the subscription options screen.

**3. Cancel Checkout**

* Selecting Cancel shall display a confirmation dialog with:

  * **Continue Checkout:** Close the dialog and remain on the same checkout screen with the same package and redemption method.
  * **Cancel Checkout:** Discard the checkout and return the customer to the Home screen.
* The customer may cancel checkout while validation is in progress. After confirmation, the system shall cancel the checkout, ignore the validation result, and return to the Home screen.

### Error Handling

**1. Active Subscription**

* Customers with an active subscription shall be prevented from accessing checkout.
* The system shall display:
  `You already have an active subscription. You can renew after it expires.`
* The system shall recheck subscription status before proceeding to payment. If the customer has an active subscription, proceeding shall be blocked and the same message displayed.

**2. No Internet Connection**

* If the customer is offline before checkout loads, or the connection is lost while checkout is displayed, the system shall show:
  `No internet connection. Please try again.`
* The customer shall not be able to proceed until the connection is restored.

**3. Server or Data Loading Error**

* If checkout data cannot be loaded or validated due to a server or temporary error, the system shall display:
  `Something went wrong. Please try again.`
* The error message shall disappear when the customer retries and reappear if the retry fails.
* If loading the details of a newly selected package fails, the same error message shall be displayed with an option to retry.
* After a failed validation, the customer shall remain on the same checkout screen and be able to retry.

### Acceptance Criteria

**AC1 — Display Checkout Summary**
Given the customer is logged in and has selected an available subscription package and redemption method, when checkout opens, then the system shall display the package name, 30-calendar-day validity, one-coffee-per-day entitlement, included coffee types, redemption method, and total amount in SAR.

**AC2 — Display Included Coffee Types**
Given the selected package includes one coffee type, when checkout opens, then the system shall display that type. If the package includes multiple types, the system shall display all included types.

**AC3 — Display Subscription Notices**
Given checkout is displayed, then the system shall show the payment activation notice and the daily entitlement non-rollover notice.

**AC4 — Validate Latest Price**
Given checkout is open, when the customer selects Proceed to Payment, then the system shall validate the latest configured price before proceeding.

**AC5 — Handle Price Changes**
Given the configured price changes during validation, when validation completes, then the system shall update the displayed price and require the customer to press Proceed to Payment again.

**AC6 — Handle Multiple Data Changes**
Given both the price and included coffee types change during validation, when validation completes, then the system shall update both in the same checkout screen and require the customer to review the changes and press Proceed to Payment again.

**AC7 — Prevent Duplicate Requests**
Given validation is in progress, when the customer has pressed Proceed to Payment, then the button shall remain disabled until validation completes.

**AC8 — Handle Unavailable Package**
Given the selected package becomes unavailable, when the customer attempts to proceed, then the system shall block proceeding, display an explanatory message, and provide a Back to Subscription Options button.

**AC9 — Handle Unavailable Coffee Types**
Given all coffee types included in the selected package become unavailable, when the customer attempts to proceed, then the system shall block proceeding, display an explanatory message, and provide a Back to Subscription Options button.

**AC10 — Handle Active Subscription**
Given the customer has an active subscription, when the customer attempts to access checkout or proceed to payment, then the system shall block the action and display:
`You already have an active subscription. You can renew after it expires.`

**AC11 — Handle Expired Session**
Given the customer's session expires while checkout is open, when the customer attempts to continue, then the system shall require login. After successful login, the customer shall return to the Home screen.

**AC12 — Handle Internet Connection Errors**
Given the internet connection is unavailable before or during checkout, when the customer attempts to load or proceed, then the system shall display:
`No internet connection. Please try again.`
The customer shall be prevented from proceeding until the connection is restored.

**AC13 — Handle Server and Validation Errors**
Given checkout data loading or validation fails due to a server or temporary error, when the failure occurs, then the system shall display:
`Something went wrong. Please try again.`
The customer shall remain on the current screen and be able to retry.

**AC14 — Change Package**
Given checkout is open, when the customer selects Change Package, then the system shall return to subscription options and require the customer to select a package and redemption method again.

**AC15 — Change Package During Validation**
Given validation is in progress, when the customer selects Change Package, then the system shall stop the current validation, ignore its result, and return to subscription options.

**AC16 — Cancel Checkout**
Given checkout is open, when the customer selects Cancel, then the system shall display a confirmation dialog with Continue Checkout and Cancel Checkout options.

**AC17 — Cancel During Validation**
Given validation is in progress, when the customer confirms Cancel Checkout, then the system shall cancel checkout, ignore the validation result, and return the customer to the Home screen.

**AC18 — Retry Failed Request**
Given a temporary error message is displayed, when the customer retries, then the system shall hide the error while retrying and display it again if the retry fails.

### Wireframe Updates Required

* Replace the displayed currency with SAR.
* Display the one-coffee-per-day entitlement.
* Display the included coffee type or types.
* Display the selected redemption method.
* Add the payment activation and non-rollover notices.
* Add the Cancel action and its confirmation dialog.
* Change the primary action label to **Proceed to Payment**.
* Do not display subscription start or expiry dates on checkout.
* Ensure the displayed total amount reflects the latest configured price.
