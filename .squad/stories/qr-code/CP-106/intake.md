# CP-106 — Verify QR

## 1. User Story

**As a** Cafe Staff Member (Cashier / Delivery Staff),  
**I want** to scan a subscriber’s QR code and verify their subscription eligibility and daily redemption status,  
**So that** only eligible subscribers can proceed with the appropriate redemption or delivery handover flow.

## 2. Trigger

Dashboard → Scan Screen → Scan QR Code (CP-105) → QR Verification (CP-106)

## 3. Description

The system verifies the scanned QR code, retrieves the associated subscriber and subscription records, and validates the subscriber’s eligibility.

For delivery orders, the system also validates the relationship between the subscriber and the order, the order’s eligibility for handover, the assigned delivery staff member, the relevant branch, and whether the order has already been delivered.

CP-106 is responsible for verification only. It must not record a cup deduction, redemption, or consumption. The result is returned to CP-105, which handles the subsequent operation.

## 4. Acceptance Criteria

### AC1 — Read and Validate QR Code

- The system must read the QR code scanned by the logged-in Cafe Staff Member.
- The system must identify the subscriber associated with the QR code.
- If the QR code is invalid, unrecognized, or cannot be associated with a subscriber, the system must reject the verification.
- The system must display the following message:

  `Invalid QR code.`

- No redemption or consumption must be recorded.

### AC2 — Retrieve Subscriber and Subscription Data

- The system must retrieve the subscriber and subscription records associated with the scanned QR code.
- The system must verify that the subscription is active and within its 30-calendar-day validity period.
- The system must not require a separate check of the subscription’s remaining overall cup balance as part of CP-106 verification.

### AC3 — Verify Active Subscription

- If the subscriber has an active subscription within its validity period, the system proceeds to the daily redemption validation.
- If the subscriber has never subscribed or the subscription has expired, the system must reject verification.
- The system must display the following message in either case:

  `This Subscriber does not have an active subscription.`

- No redemption or consumption must be recorded.

### AC4 — Verify Daily Redemption Status

- The system must verify whether the subscriber has already redeemed the cup for the current day.
- If the subscriber has already redeemed today's cup, the system must reject verification and return the following message:

  `Today's cup has already been redeemed.`

- If the subscription is active and today's cup has not been redeemed, the system must return `Valid`.
- The verification must not deduct a cup or record consumption.

### AC5 — Verify Branch and Staff Eligibility

- The system must verify that the logged-in staff member is assigned to the relevant branch.
- The system must verify that the relevant branch is open.
- If either condition is not satisfied, verification must be rejected with an appropriate reason.
- No redemption, consumption, or delivery status change must occur following a rejected verification.

### AC6 — Validate Delivery Order

When the QR verification is initiated for a delivery handover, the system must verify that:

- The order is associated with the scanned subscriber.
- The order belongs to the relevant branch.
- The order is assigned to the currently logged-in Delivery Staff member.
- The order is eligible for handover according to its current status.
- The order has not already been delivered.

If any validation fails, the system must reject the verification and return an appropriate rejection reason.

No cup deduction, consumption, or order status change must occur during CP-106 verification.

### AC7 — Return Verification Result to CP-105

- If all applicable verification checks pass, CP-106 must return `Valid` to CP-105.
- If verification fails, CP-106 must return a rejection result with the corresponding reason.
- CP-106 must not duplicate subscriber, subscription, or order data in its response. CP-105 is responsible for retrieving any additional data it needs separately.
- CP-105 is responsible for proceeding with the appropriate operation after receiving the verification result.

### AC8 — Prevent Concurrent Duplicate Redemption

- The system must prevent multiple concurrent operations from successfully redeeming the same subscriber’s daily cup.
- If one operation completes the daily redemption before another operation can proceed, the subsequent operation must be rejected because today's cup has already been redeemed.
- The implementation mechanism for preventing concurrent duplicate redemption will be determined during technical design.
- CP-106 must not record a cup deduction or consumption as part of this protection.

## 5. Business Rules

1. A subscription must be active and within its 30-calendar-day validity period.
2. A subscriber may redeem no more than one cup per day.
3. An active subscription with no redemption recorded for the current day may return `Valid`, regardless of the remaining overall subscription cup balance.
4. CP-106 performs verification only; it does not deduct cups or record consumption.
5. Delivery handover requires the relevant order, branch, and staff validations to pass.
6. The branch must be open, and the logged-in staff member must be assigned to that branch.
7. Concurrent duplicate daily redemptions must be prevented.
8. CP-105 remains responsible for the subsequent redemption or delivery handover operation.

## 6. Dependencies

- **CP-100 — Staff Scan**
- **CP-105 — Scan QR / Redemption Flow**
- Subscription Management
- Subscriber Management
- Staff and Branch Management
- Daily Redemption Tracking
- Delivery Order Management

## 7. Out of Scope

- Recording cup deductions or consumption.
- Completing an in-cafe redemption.
- Confirming delivery handover or changing an order to `Delivered`.
- Defining the technical mechanism for concurrency control.
- Returning full subscriber, subscription, or order details in the verification response.