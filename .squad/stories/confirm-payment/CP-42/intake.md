**# CP-42: Confirm Payment**

**### User Story**

As a registered customer, I want to confirm payment by selecting a suitable payment method, so that my subscription is activated and I can start redeeming my daily coffee.

**### Trigger**

Customer Account → Subscription → Subscribe Now → Proceed to Payment

**### Description**

The system shall display the electronic payment screen where the customer can select a supported payment method, agree to the Terms & Conditions and Privacy Policy, and complete payment for the selected subscription.

The system shall display all electronic payment methods supported by the configured payment gateway. Cash shall not be available.

The payment amount shall be calculated and validated by the system and shall not rely on an amount supplied or modified by the customer.

**### Payment Methods**

* The system shall display all electronic payment methods supported by the configured payment gateway.

* The customer shall select one payment method before proceeding with payment.

* Cash shall not be displayed as a payment method.

* If no payment method is selected, the system shall display:

  `Please select a payment method`

* The Pay button shall remain disabled until the customer selects a payment method and agrees to the Terms & Conditions and Privacy Policy.

**### Card Details**

* When a card payment method is selected, the customer shall enter the required card details through the payment gateway's secure page or form.

* Card details shall be handled through the payment gateway's secure payment interface.

**### Terms & Conditions and Privacy Policy**

* The system shall display the checkbox text:

  `I agree to the Terms & Conditions and Privacy Policy`

* The checkbox shall be unchecked by default.

* The Terms & Conditions and Privacy Policy links shall open their published content within the app.

* The customer must agree to the terms before proceeding with payment.

* If the checkbox is unchecked, the system shall display:

  `Please agree to the Terms & Conditions to continue`

* The system shall record the customer's consent and the time at which consent was given.

**### Actions**

****1. Pay****

* When the customer selects a payment method, agrees to the terms, and presses Pay, the system shall send a payment request to the payment gateway.

* The payment request shall include the payment amount calculated by the system and a unique transaction reference.

* The system shall prevent duplicate payment requests for the same payment attempt.

* The Pay button shall remain disabled while the payment request is being submitted to prevent repeated submissions.

* While payment is being verified, the system shall display a loading indicator and:

  `We are verifying your payment`

* The system shall verify the final payment status using the payment gateway's confirmation mechanisms before activating the subscription.

****2. Successful Payment****

* After successful payment has been verified, the system shall activate the subscription for 30 calendar days.

* The system shall trigger the electronic invoice.

* The system shall display the Subscription Confirmation screen with the subscription QR code only after the subscription has been successfully activated.

* If the payment gateway response and callback provide conflicting results, the system shall verify the final payment status before activating the subscription.

* A missing response or callback shall not be treated as proof of successful payment.

****3. Failed Payment****

* If the payment gateway confirms that payment has failed, the system shall display a message indicating that the payment failed.

* The customer shall be able to return to the payment page and initiate a new payment attempt.

* A failed payment shall not activate the subscription or display the subscription QR code.

* A new payment attempt shall not cause the previous attempt to be treated as successful.

* The exact failure message and retry limits are TBD.

****4. Pending Payment****

* If payment is pending or has not yet been confirmed, the system shall display:

  `We are verifying your payment`

* The system shall continue checking the payment status automatically until a final result is received.

* The QR code shall remain hidden while payment is pending or unconfirmed.

* If payment remains pending for an extended period, the system shall continue checking its status automatically. The exact timeout duration and handling of indefinitely pending payments are TBD.

****5. Cancel Payment****

* If the customer selects Cancel before the payment request is submitted, the system shall discard the current payment flow and return the customer to the Home screen.

* Once the payment request has been submitted, the customer shall not be allowed to cancel the payment flow until the transaction result is determined.

* Leaving the payment screen or returning to Home shall not automatically be treated as cancellation of the financial transaction.

* If payment succeeds after the customer leaves the payment screen, the system shall process the verified payment result and activate the subscription.

* If payment fails, the system shall not activate the subscription.

****6. Subscription Activation Failure****

* If payment succeeds but subscription activation fails, the system shall retry subscription activation.

* The system shall not ask the customer to pay again solely because subscription activation failed.

* The system shall prevent duplicate subscription activation for the same successful payment.

* The Subscription Confirmation screen and QR code shall only be displayed after successful subscription activation.

**### Error Handling**

****1. Payment Method Validation****

* If the customer attempts to proceed without selecting a payment method, the system shall display:

  `Please select a payment method`

****2. Terms and Conditions Validation****

* If the customer attempts to proceed without checking the Terms & Conditions and Privacy Policy checkbox, the system shall display:

  `Please agree to the Terms & Conditions to continue`

****3. Payment Failure****

* If the payment gateway confirms that payment has failed, the system shall display a payment failure message.

* The customer shall be able to return to the payment page and initiate a new payment attempt.

****4. Pending or Unconfirmed Payment****

* If payment has not been confirmed, the system shall display:

  `We are verifying your payment`

* The system shall not activate the subscription or display the QR code until payment is verified successfully.

****5. No Internet Connection****

* If there is no internet connection, the system shall display:

  `No internet connection. Please try again.`

* The system shall not treat an unverified payment as successful because of a connection failure.

* After the connection is restored, the payment status shall be verified before the customer is allowed to initiate another payment attempt for the same unresolved transaction.

****6. Duplicate Payment Requests****

* The system shall prevent duplicate payment requests for the same payment attempt.

* Repeated Pay actions or request retries shall not result in duplicate payment processing or duplicate subscription activation.

****7. Payment Successful but Subscription Activation Failed****

* If payment is verified successfully but subscription activation fails, the system shall retry activation without asking the customer to pay again.

* The system shall not display the Subscription Confirmation screen or QR code until activation succeeds.

**### Acceptance Criteria**

****AC1 — Display Payment Methods****

Given the customer has reached the payment screen, when the screen loads, then the system shall display all electronic payment methods supported by the configured payment gateway and shall not display cash.

****AC2 — Require Payment Method Selection****

Given the payment screen is displayed, when the customer attempts to proceed without selecting a payment method, then the system shall display:

`Please select a payment method`

The Pay button shall remain disabled until a payment method is selected and the required consent is provided.

****AC3 — Secure Card Details****

Given the customer selects a card payment method, when card details are required, then the system shall use the payment gateway's secure page or form to collect them.

****AC4 — Require Terms Agreement****

Given the payment screen is displayed, then the Terms & Conditions and Privacy Policy checkbox shall be unchecked by default, and the published content shall open within the app when its links are selected.

****AC5 — Validate Terms Agreement****

Given the customer has not agreed to the Terms & Conditions and Privacy Policy, when the customer attempts to proceed, then the system shall display:

`Please agree to the Terms & Conditions to continue`

The system shall not proceed with payment until consent is provided.

****AC6 — Record Consent****

Given the customer agrees to the Terms & Conditions and Privacy Policy, when consent is submitted, then the system shall record the customer's agreement and the time of consent.

****AC7 — Initiate Payment****

Given the customer has selected a payment method and agreed to the terms, when the customer presses Pay, then the system shall send a payment request containing the system-calculated payment amount and a unique transaction reference.

****AC8 — Prevent Duplicate Payment Requests****

Given a payment attempt has already been submitted, when the customer repeats the Pay action or the request is retried, then the system shall prevent duplicate processing for that payment attempt.

****AC9 — Display Payment Verification Status****

Given a payment request has been submitted and its final status has not been confirmed, when the system is verifying the payment, then the system shall display a loading indicator and:

`We are verifying your payment`

****AC10 — Activate Subscription After Verified Payment****

Given the payment gateway confirms successful payment, when the system verifies the final payment status, then the system shall activate the subscription for 30 calendar days and trigger the electronic invoice.

****AC11 — Display Subscription Confirmation****

Given payment has been verified successfully and the subscription has been activated, when the activation succeeds, then the system shall display the Subscription Confirmation screen with the subscription QR code.

****AC12 — Handle Pending Payment****

Given payment is pending or unconfirmed, when the system checks the payment status, then the system shall continue checking automatically, display the verification message, and keep the QR code hidden until payment is confirmed and the subscription is activated.

****AC13 — Handle Failed Payment****

Given the payment gateway confirms that payment has failed, when the system receives the result, then the system shall display a payment failure message, shall not activate the subscription, and shall allow the customer to return to the payment page and initiate a new attempt.

****AC14 — Handle Conflicting Payment Results****

Given the payment gateway response and callback provide conflicting results, when the system processes the payment result, then the system shall verify the final payment status before activating the subscription.

****AC15 — Handle Missing Payment Confirmation****

Given the payment response or callback is missing, when the system cannot confirm the final payment status, then the system shall not treat the payment as successful and shall not activate the subscription or display the QR code.

****AC16 — Cancel Before Payment Submission****

Given the customer is on the payment screen and no payment request has been submitted, when the customer selects Cancel, then the system shall discard the payment flow and return the customer to Home.

****AC17 — Prevent Cancellation After Submission****

Given a payment request has been submitted, when the customer attempts to cancel the payment flow, then the system shall prevent cancellation until the transaction result is determined.

****AC18 — Handle Internet Connection Errors****

Given the internet connection is unavailable, when the customer attempts to submit or verify payment, then the system shall display:

`No internet connection. Please try again.`

The system shall not treat an unverified payment as successful.

****AC19 — Recover from Subscription Activation Failure****

Given payment has been verified successfully but subscription activation fails, when the system handles the failure, then the system shall retry activation without asking the customer to pay again and shall display the confirmation screen and QR code only after activation succeeds.

****AC20 — Trigger Electronic Invoice****

Given payment has been verified successfully, when the system processes the successful payment, then the system shall trigger the electronic invoice. Displaying or downloading the invoice for the customer is not required by this story.

**### Dependencies**

* A configured payment gateway supporting electronic payment methods.

* Payment gateway responses and callbacks for payment verification.

* A system-calculated subscription price.

* A mechanism to prevent duplicate payment processing.

* A subscription activation mechanism.

* An electronic invoice generation mechanism.

* Published Terms & Conditions and Privacy Policy content.

* A QR code generation or retrieval mechanism for an activated subscription.

**### Technical Hints**

* **Primary language:** TypeScript (listed in the existing technical hints; implementation context has not been independently verified).

* **Repository root:** `.`

* The payment gateway provider and provider-specific implementation details are TBD.

* Payment status must be verified before subscription activation.

* Duplicate payment processing and duplicate subscription activation must be prevented.

* Subscription activation retries must not require another customer payment.

**### Out of Scope**

* Cash payments.

* Selecting or implementing a specific payment gateway provider.

* Defining the exact payment failure message and retry limits.

* Defining the timeout duration for pending payments.

* Displaying or downloading the electronic invoice for the customer.

* Subscription redemption rules beyond activation and displaying the confirmation QR code.
