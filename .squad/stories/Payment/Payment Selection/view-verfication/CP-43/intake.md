# CP-43 — View Verification

## Source work item

* **Tracker:** CP-43
* **Title:** View Verfication
* **Feature:** Payment Management
* **Epic:** Payment
* **Intake path:** `.squad/stories/view-verfication/CP-43/intake.md`

## Story intake

### Feature

**Payment Management — View Verification**

### Tracker metadata

* **Story ID:** CP-43
* **Story Type:** User Story
* **Actor:** Registered Customer
* **Status:** As defined in the tracker.

### Title

**View Verification**

### Description

As a registered customer, I want to view the verification status of my payment, so that I know whether my payment was successful and my subscription is being activated.

### Trigger

Customer App → Subscription → Checkout → Payment → Pay

The verification screen opens automatically after the customer submits a payment.

### Acceptance criteria

**AC1 — Display Verification Status**

* After the customer submits a payment, the system displays the current transaction status.
* While the payment result is not confirmed, the screen displays a loading indicator and the message: `We are verifying your payment`.
* The screen displays the transaction reference and transaction creation date/time in Saudi Arabia time (KSA).
* The transaction status is determined using the trusted final payment status.

**AC2 — Successful Payment**

* When the payment is confirmed successful, display: `Payment verified successfully`.
* Display the payment amount in SAR, transaction reference, and transaction creation date/time in KSA.
* Display the `Continue` button.
* Automatically navigate to the Subscription Activation screen after 3 seconds if the customer has not tapped `Continue`.
* If the customer taps `Continue`, cancel the automatic navigation to prevent duplicate navigation.
* Hide `Try Again` after payment success.
* Do not submit another payment for the same successful transaction.

**AC3 — Failed Payment**

* When the payment is confirmed failed, display: `Payment was not completed. Your subscription was not activated.`
* Display the transaction reference and transaction creation date/time in KSA.
* Display the `Try Again` button.
* Allow the customer to start another payment attempt after the previous transaction is confirmed failed.
* Allow repeated attempts when subsequent transactions also fail.

**AC4 — Continue Action**

* When the customer taps `Continue` after successful payment, open the Subscription Activation screen.
* The system must not charge the customer again.
* The same transaction must not activate the subscription more than once.

**AC5 — Try Again Action**

* When the customer taps `Try Again` after confirmed payment failure, return to the Payment page.
* Retain the previously selected subscription package.
* Do not allow a new payment attempt while the previous transaction remains in `Verifying`.

**AC6 — Back to Home**

* Display `Back to Home` only while the payment status is `Verifying` or `Failed`.
* When selected, return the customer to the Home screen.
* If the transaction is still being verified, continue verification in the background.
* After payment success, proceed to Subscription Activation instead of displaying `Back to Home`.

**AC7 — Delayed Verification**

* If verification takes more than 30 seconds, display: `Your payment is still being verified. We will update you here once it is confirmed.`
* Allow the customer to leave the screen and return later.
* Do not treat a delayed response alone as a payment failure.

**AC8 — No Internet Connection**

* If the internet connection is lost during verification, display: `No internet connection. Please try again.`
* When connectivity is restored, check the existing transaction status before treating the payment as failed or allowing another payment attempt.

**AC9 — Unable to Retrieve Status**

* If the system cannot retrieve the payment status, display: `Unable to verify your payment status. Please try again.`
* Display a `Retry` button.
* Each tap on `Retry` initiates a new status-check request.
* A status-check request must not create a new payment transaction.

**AC10 — Conflicting Payment Results**

* If the payment gateway response and callback provide conflicting results, keep the transaction in `Verifying`.
* Establish the trusted final transaction status before activating the subscription.
* Do not activate the subscription based on an unconfirmed success result.

**AC11 — Customer Returns to the App**

* If the customer leaves the application while verification is in progress, continue verification in the background.
* When the customer returns, display the latest status of the existing transaction.
* Do not assume the transaction is still in its previous state without checking its current status.

**AC12 — Payment Fails While Customer Is Away**

* If the payment is confirmed failed while the customer is away from the application, send the notification: `Payment failed. Please try again.`
* When the customer returns, display the current transaction status.
* Allow another payment attempt after the failure is confirmed.

**AC13 — Payment Succeeds While Customer Is Away**

* If payment succeeds while the customer is away, proceed with subscription activation in the background.
* Once activation succeeds, send the notification: `Your subscription is now active!`
* Do not require the customer to submit another payment.

**AC14 — Delayed Subscription Activation**

* If payment succeeds but subscription activation is delayed, retain the successful payment status.
* Retry subscription activation in the background.
* Do not charge the customer again for the same successful transaction.
* Once activation succeeds, allow the customer to access the Subscription Activation screen.

**AC15 — Activation Screen Failure**

* If the Subscription Activation screen fails to open after successful payment, retry opening it.
* Retain the successful payment status.
* Do not submit another payment or activate the same subscription more than once.

**AC16 — Duplicate Payment Callback**

* If the payment gateway sends the same successful callback more than once, process the transaction idempotently.
* Repeated callbacks must not cause duplicate subscription activation or additional charges.

**AC17 — Prevent Duplicate Payment Attempts**

* While a transaction remains in `Verifying`, prevent another payment attempt for the same package.
* Allow another attempt only after the previous transaction is confirmed failed.

**AC18 — Transaction Details**

* Display the current transaction only.
* Display the transaction reference, transaction creation date/time in KSA, and payment amount in SAR when available.
* Retain the transaction creation timestamp regardless of whether the transaction eventually succeeds or fails.
* Viewing historical transactions is outside the scope of this story.

### Attachments

* No attachments specified in the available story details.

### Dependencies

* **CP-42 — Confirm Payment**
* **Receive Activation** user story
* Payment gateway response and callback
* Transaction status retrieval
* Subscription activation service
* Notification service

### Extra notes

* Payment verification and subscription activation are separate operations.
* The system must not activate a subscription while payment remains unverified.
* A subscription QR code must not be issued before successful payment and subscription activation.
* The trusted final payment status must be established before the system treats the transaction as successful or failed.
* A successful payment must not be charged again because of delayed activation or a navigation error.
* Duplicate callbacks must not result in duplicate activation.
* The current screen displays only the current transaction, not a history of previous transactions.
* The selected verification delay is 30 seconds.
* The selected automatic navigation delay is 3 seconds.
* Payment failure and successful activation notifications are required according to the decisions made for this story.

### Technical hints

* **Language:** TypeScript (as indicated in the source hints; confirm with the implementation team).
* **Root path:** `.` (confirm with the implementation team).
* Use the payment gateway's trusted transaction status to resolve verification.
* Status-check requests must remain separate from payment submission requests.
* Ensure transaction processing and subscription activation are idempotent.
* Background verification, activation retries, and notifications depend on the available backend services and integration capabilities.

### Out of scope

* Viewing historical payment transactions.
* Implementing or changing the payment gateway itself.
* Defining payment methods or package selection, which belong to their respective stories.
* Defining the subscription activation screen's full behavior, which belongs to the Receive Activation story.
* Implementing subscription QR-code usage and redemption.
* Defining backend infrastructure or notification-provider configuration beyond the integration needed by this story.
