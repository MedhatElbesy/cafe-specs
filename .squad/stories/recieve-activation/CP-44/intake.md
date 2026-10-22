> **Fetched from jira:** [CP-44](https://yomnaarafahammad-1791369924187.atlassian.net/browse/CP-44)
> *Fetched 2026-10-22. Edit the sections below as needed; the planner reads this file verbatim.*

---

## Source — work item (from tracker)

**Title:** Receive Activation

**Type:** Story

**Status:** Idea

**Assignee:** nada.ashraf.1.2.3.4.9

### Description

"As a Customer, I want to receive confirmation that my subscription is activated, so that I can start redeeming my daily coffee immediately."

**Trigger:**

Payment Verification → Payment Verified Successfully → Subscription Activated.

**Acceptance Criteria:**

The Subscription Activation screen must be displayed only after the payment has been verified as successful and the subscription has been activated. The screen must display the following information:

**Activation Message**

* The message "Subscription activated successfully".

**Subscription Details**

* Selected package: With Delivery or Without Delivery.
* Activation date: today (Saudi Arabia time), counted as day one.
* Expiry date: the end of day 30 (Saudi Arabia time).
* Daily entitlement status: "Today's coffee is available".

**QR Code**

* The subscriber's QR code is displayed immediately, since today's entitlement is available.
* It is the same QR code shown in Profile → View Daily Entitlement QR Code (CP-17).

**Electronic Invoice**

* The system must trigger the electronic invoice for the successful purchase in accordance with applicable ZATCA requirements.
* Invoice processing runs in the background and does not block activation.
* If invoice processing fails, the system retries in the background without blocking activation.

**Actions**

**Done:**

* The system closes the activation screen and returns the customer to the Home screen.

---

# Story intake

Fill this template for each story you want planned. Keep it copy-paste-friendly: the planner reads **this file and the files in `attachments/`**, nothing else.

* Folder: `.squad/stories/recieve-activation/CP-44/intake.md`
* Binaries (screenshots, PDFs, exports): put them in `attachments/` next to this file and list them below.
* Do **not** rely on external links (tracker URLs, wiki, chat) — the planner cannot open them. Paste the content you want considered.

---

## Feature

* **Feature name (display):** Subscription Activation
* **Feature slug (folder under `plans/`):** `recieve-activation`

## Tracker (metadata only)

* **Tracker type:** `jira`
* **Work item id:** `CP-44`
* **Work item type:** `Story`
* **Status:** `Idea`
* **Assignee:** `nada.ashraf.1.2.3.4.9`
* **Labels:** `None`

External tracker links are not followed by the planner. Keep the ID for naming and traceability only.

---

## Title

```text
Receive Activation
```

---

## Description

```text
As a Customer, I want to receive confirmation that my subscription is activated, so that I can start redeeming my daily coffee immediately.

Trigger:
Payment Verification → Payment Verified Successfully → Subscription Activated.

The Subscription Activation screen must be displayed only after the payment has been verified as successful and the subscription has been activated.

The screen must display the following information:

Activation Message:
- Display the message "Subscription activated successfully".

Subscription Details:
- Selected package: With Delivery or Without Delivery.
- Activation date: the current date in Saudi Arabia time, counted as day one.
- Expiry date: the end of the 30th calendar day, calculated using Saudi Arabia time.
- Daily entitlement status: "Today's coffee is available".

QR Code:
- Display the subscriber's QR code immediately after successful subscription activation because today's entitlement is available.
- Use the same QR code as the one displayed in Profile → View Daily Entitlement QR Code (CP-17).
- Do not create a separate QR code specifically for the activation screen.

Electronic Invoice:
- Trigger the electronic invoice process for the successful purchase in accordance with applicable ZATCA requirements.
- Process invoice generation in the background.
- Invoice processing must not block subscription activation or prevent the activation screen from being displayed.
- If invoice processing fails, retry the process in the background.

Actions:
- Display a Done button.
- When the customer taps Done, close the activation screen and return the customer to the Home screen.
```

---

## Acceptance criteria

### AC1 — Activation screen eligibility

* The Subscription Activation screen must only be displayed after payment has been verified as successful and the subscription has been activated.
* Failed, pending, or unverified payments must not lead to the activation screen.

### AC2 — Activation confirmation

* The screen must display the exact message: "Subscription activated successfully".

### AC3 — Selected package

* The screen must display the package purchased by the customer.
* The package type must be identified as either With Delivery or Without Delivery.
* The displayed package must match the successfully activated subscription.

### AC4 — Activation date

* The screen must display the subscription activation date using Saudi Arabia time.
* The activation date must correspond to the actual subscription activation date.
* The activation date counts as day one of the subscription.

### AC5 — Expiry date

* The screen must display the subscription expiry date calculated using Saudi Arabia time.
* The activation date is day one, and the subscription remains valid through the end of the 30th calendar day.

### AC6 — Daily entitlement status

* The screen must display the status: "Today's coffee is available".
* This status must correspond to the activated subscription's current daily entitlement.

### AC7 — QR code availability

* The subscriber's QR code must be displayed immediately after subscription activation.
* The QR code must correspond to the customer's active subscription and today's available entitlement.

### AC8 — QR code consistency

* The activation screen must use the same QR code referenced in Profile → View Daily Entitlement QR Code (CP-17).
* The system must not generate a separate QR code specifically for the activation screen.
* The QR code must follow the existing entitlement validation rules.

### AC9 — Electronic invoice initiation

* After a successful purchase, the system must trigger the electronic invoice process in accordance with applicable ZATCA requirements.

### AC10 — Background invoice processing and retry

* Invoice processing must run in the background.
* Delays or failures in invoice processing must not block subscription activation or prevent the activation screen from being displayed.
* If invoice processing fails, the system must retry the process in the background.
* Invoice retries must not cause duplicate customer charges or duplicate invoices.

### AC11 — Done action

* The activation screen must display a Done button.
* When the customer taps Done, the system must close the activation screen and return the customer to the Home screen.
* This action must not create another payment or subscription.

### AC12 — Subscription data consistency

* The selected package, activation date, expiry date, entitlement status, and QR code displayed on the screen must correspond to the same activated subscription.
* The screen must not indicate that today's coffee is available before the subscription is activated.

---

## Attachments

None.

---

## Dependencies

* **Blocked by / related IDs:**

  * **CP-17 — View Daily Entitlement QR Code:** The activation screen must display the same QR code referenced in CP-17.
  * **Payment Verification:** Successful payment verification is required before the subscription activation screen is displayed.

* **Depends on code areas or other stories:**

  * Subscription activation and status management.
  * Subscription package details.
  * Daily entitlement status and QR code functionality referenced by CP-17.
  * Electronic invoice processing in accordance with applicable ZATCA requirements.
  * Home screen navigation.

The exact implementation ownership of these dependencies should be confirmed during planning rather than assumed.

---

## Extra notes (optional)

* Saudi Arabia time must be used for subscription activation and expiry calculations.
* The activation date counts as day one of the 30-day subscription.
* The expiry date represents the end of the 30th calendar day.
* The activation screen must display the QR code referenced by CP-17.
* Invoice processing must be independent of the customer-facing activation flow.
* Invoice failures must trigger background retries without blocking activation.
* Do not assume additional story dependencies unless they are confirmed by the project requirements.

---

## Technical hints (optional)

* **Repository root:** `.`
* **Primary language:** TypeScript.
* Reuse existing subscription activation and entitlement mechanisms wherever available.
* Reuse the existing QR code functionality referenced by CP-17.
* Use the `Asia/Riyadh` time zone for activation and expiry date calculations.
* Trigger electronic invoice processing asynchronously.
* Implement invoice retries without duplicating invoices or customer charges.
* Keep subscription activation and invoice processing separate so invoice delays do not block the activation flow.
* Inspect the existing codebase and related stories before introducing new services or assumptions.

---

## Out of scope

* Selecting or purchasing a subscription package.
* Payment checkout and payment verification implementation.
* Viewing historical transactions or invoices.
* Changing subscription packages after activation.
* Redeeming coffee through the QR code; redemption follows the existing entitlement flow.
* Creating a separate QR code specifically for the activation screen.
* Implementing the Profile QR code screen covered by CP-17.
* Implementing a complete invoice-management system beyond triggering and retrying invoice processing for the successful purchase.
