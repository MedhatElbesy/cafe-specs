# CP-105 — Cafe Staff Member Scan QR

## 1. User Story

**As a** Café Cashier or Delivery Staff member,  
**I want to** scan and verify a subscriber’s QR code,  
**So that** I can complete an eligible in-café redemption or confirm the handover of an assigned delivery order without duplicate processing or incorrect subscription deductions.

## 2. Trigger

Dashboard → Orders Screen → Scan QR Code

## 3. Preconditions

1. The staff member is logged in.
2. The staff member is assigned to the branch where the operation is being performed.
3. The branch is currently open.
4. The QR code is valid and can be verified through CP-106.
5. The subscriber has an active subscription with a remaining balance.
6. For delivery handover, the delivery order must:
   - Be assigned to the currently logged-in Delivery Staff member.
   - Belong to the relevant branch.
   - Be in a status that allows handover confirmation.
   - Not have been delivered previously.

## 4. Functional Requirements

### 4.1 Scan and Verify QR Code

1. The system must allow authorized Cashier and Delivery Staff members to scan a subscriber’s QR code from the Orders screen.
2. The system must validate the QR code through **CP-106 — Verify QR**.
3. The system must verify the staff member’s branch assignment and the branch’s current operating status.
4. The system must apply the eligibility requirements relevant to the selected operation:
   - **In-café redemption:** The subscriber must have an active subscription with a remaining balance.
   - **Delivery handover:** The subscriber must have an active subscription with a remaining balance, and the assigned delivery order must be valid and eligible for handover.
5. A delivery order that is invalid, already delivered, or not assigned to the current Delivery Staff member must be rejected.
6. The system must not allow an ongoing delivery order to be redeemed as an in-café order by a Cashier. However, this restriction must not prevent the assigned Delivery Staff member from completing the handover of that same order.

### 4.2 In-Café Redemption — Cashier

1. After successful QR verification, the system must display the result and provide a confirmation action.
2. The system must not deduct a cup before the Cashier presses **Confirm**.
3. After confirmation, the system must deduct exactly one cup from the subscriber’s remaining subscription balance.
4. The redemption must be marked as completed only after the operation succeeds.
5. The QR code must be hidden in the subscriber’s app only after the redemption has completed successfully.
6. The system must display a success message specific to in-café redemption.

### 4.3 Delivery Handover — Delivery Staff

1. After successful QR verification and delivery-order validation, the system must provide a confirmation action.
2. The system must not deduct an additional cup when confirming delivery handover.
3. The system must update the associated delivery order status to **Delivered** only after successful confirmation.
4. The system must display a success message specific to delivery handover.
5. The QR code must be hidden in the subscriber’s app only after the handover operation has completed successfully.
6. The operation must be accepted only when the order is assigned to the current Delivery Staff member, belongs to the relevant branch, and is eligible for handover.

### 4.4 Failed Verification or Confirmation

1. If QR verification fails, the system must display the specific rejection reason returned by CP-106.
2. If the subscription is inactive or has no remaining balance, the operation must be rejected.
3. If delivery-order validation fails, the operation must be rejected with a reason-specific error.
4. If confirmation fails after the staff member presses **Confirm**:
   - No cup must be deducted.
   - The delivery order status must not be changed to Delivered.
   - The system must display an error explaining the failure reason.
   - The scan screen must remain available for another attempt.
5. A failed operation must not cause the QR code to be hidden in the subscriber’s app.

### 4.5 Duplicate Processing Prevention

1. The system must prevent an already completed redemption or delivery handover from being processed again.
2. Repeated confirmation or rescanning of the same completed operation must not cause an additional deduction or duplicate handover.
3. The system must display a message indicating that the operation has already been completed.
4. If a previous attempt’s result is unclear, retry handling must ensure that an operation that already succeeded is not executed a second time.

### 4.6 Branch and Staff Restrictions

1. Staff members must not perform the operation for a branch other than the branch to which they are assigned.
2. If the branch is closed, QR scanning and confirmation must be blocked for both Cashier and Delivery Staff.
3. The system must reject operations that do not satisfy the staff, branch, subscription, QR, or order eligibility requirements.

## 5. Acceptance Criteria

### AC1 — Successful In-Café Redemption

**Given** a logged-in Cashier assigned to an open branch, a valid QR code, and a subscriber with an active subscription and remaining balance,  
**When** the Cashier scans the QR code and presses Confirm,  
**Then** the system deducts exactly one cup, marks the redemption as completed, displays the in-café success message, and the QR code is hidden in the subscriber’s app after successful completion.

### AC2 — Successful Delivery Handover

**Given** a logged-in Delivery Staff member assigned to an open branch, a valid QR code, an active subscription with remaining balance, and an eligible delivery order assigned to that staff member,  
**When** the staff member scans the QR code and presses Confirm,  
**Then** the system marks the order as Delivered, displays the delivery-handover success message, does not deduct an additional cup, and hides the QR code in the subscriber’s app after successful completion.

### AC3 — Invalid QR Code

**Given** an invalid QR code,  
**When** the staff member scans it,  
**Then** the system rejects the operation, displays the specific reason returned by CP-106, performs no deduction or order-status change, and keeps the scan screen available.

### AC4 — Inactive Subscription or No Remaining Balance

**Given** a subscriber whose subscription is inactive or whose remaining balance is zero,  
**When** the staff member attempts either operation,  
**Then** the system rejects the operation, displays the relevant rejection reason, performs no deduction or delivery-status change, and does not hide the QR code.

### AC5 — Ongoing Delivery Order and In-Café Redemption

**Given** a subscriber has an ongoing delivery order,  
**When** a Cashier attempts an in-café redemption,  
**Then** the system rejects the operation and displays:

`This Subscriber has an ongoing delivery order and cannot redeem in-cafe.`

No cup is deducted, and the scan screen remains available.

### AC6 — Delivery Order Not Eligible for Handover

**Given** a delivery order is not assigned to the current Delivery Staff member, belongs to another branch, is not in an eligible status, or has already been delivered,  
**When** the staff member attempts to confirm handover,  
**Then** the system rejects the operation, displays a reason-specific error, performs no deduction, and does not change the order status.

### AC7 — Branch Is Closed

**Given** the branch is closed,  
**When** either a Cashier or Delivery Staff member attempts to scan or confirm a QR operation,  
**Then** the system blocks the operation and does not deduct a cup or change the delivery-order status.

### AC8 — Staff Assigned to Another Branch

**Given** a staff member attempts to perform an operation at a branch to which they are not assigned,  
**When** the operation is initiated,  
**Then** the system rejects the operation without deduction or order-status change.

### AC9 — Confirmation Fails

**Given** QR verification has succeeded but the operation fails after the staff member presses Confirm,  
**When** the failure occurs,  
**Then** the system displays a reason-specific error, performs no deduction or delivery-status change, does not hide the QR code, and keeps the scan screen available.

### AC10 — Duplicate Operation

**Given** the redemption or delivery handover has already completed successfully,  
**When** the same operation is submitted or scanned again,  
**Then** the system prevents duplicate processing, does not deduct another cup or repeat the handover, and displays a message indicating that the operation has already been completed.

### AC11 — QR Visibility

**Given** a QR operation has been initiated,  
**When** verification succeeds but confirmation has not completed successfully,  
**Then** the QR code remains visible in the subscriber’s app.

**And** the QR code is hidden only after the relevant operation completes successfully.

## 6. Dependencies

- **CP-100 — Staff Scan:** Staff QR scanning feature.
- **CP-106 — Verify QR:** QR validation and specific rejection reasons.
- Subscription service: subscription status and remaining balance.
- Staff and branch management: staff assignment and branch operating status.
- Delivery order management: assignment, eligibility, status validation, and transition to Delivered.
- Redemption processing: cup deduction, operation completion, and duplicate prevention.
- Subscriber app: QR visibility update after successful completion.

## 7. Business Rules

1. In-café redemption deducts one cup only after successful confirmation.
2. Delivery handover does not deduct an additional cup.
3. Both operation types require an active subscription with a remaining balance.
4. Only staff assigned to the relevant branch may perform the operation.
5. Both operation types are blocked when the branch is closed.
6. Delivery handover is restricted to an eligible order assigned to the current Delivery Staff member.
7. Failed or duplicate operations must not cause an additional deduction or an invalid order-status transition.
8. The subscriber’s QR code is hidden only after successful completion.
