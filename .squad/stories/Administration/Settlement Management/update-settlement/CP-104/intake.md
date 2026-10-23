# Functional Specification — CP-104: Update Settlement

## 1. Story Identification

| Attribute | Details |
|---|---|
| Story ID | CP-104 |
| Story Title | Update Settlement |
| Epic | Administration (CP-49) |
| Feature | Admin Platform (CP-50) → Settlement Management (CP-101) |
| Work Item Type | Story |
| Jira Status | Idea |
| Assignee | Medhat Elbesy |
| Labels | MVP, Sprint1 |
| Primary Actor | Platform Admin |
| Related Stories | CP-102 — View Settlement; CP-103 — View One Settlement |
| Specification Status | Draft — Pending Business Approval |

## 2. User Story

**As a** Platform Admin,  
**I want to** record full payment of a Café's outstanding receivables,  
**So that** the system reflects the payment made outside the platform and clears the covered outstanding amount.

## 3. Trigger

Platform Dashboard → Settlement Management → View Details → Update Settlement

The Platform Admin opens the Settlement Recording Form from CP-103 — View One Settlement.

The system identifies the selected Café and displays its full outstanding amount.

## 4. Preconditions

1. The Platform Admin is authenticated.
2. The Platform Admin has Update Settlement permission.
3. The selected Café exists.
4. The selected Café has an outstanding amount greater than zero when the form is opened.
5. The system can identify the selected Café independently of other Cafés.
6. The system can retrieve the latest saved outstanding receivables for the selected Café.
7. The system can identify the individual unsettled receivables covered by the proposed settlement.
8. The system can determine the full outstanding amount using the recorded receivable amounts and applicable financial calculation rules.
9. The external payment has been made before the Platform Admin records it in the system.

**Permission Consideration**

Update Settlement permission is separate from View Settlements permission.

CP-103 requires View Settlements permission to display settlement details. CP-104 requires Update Settlement permission to open and submit the Settlement Recording Form.

The system must revalidate Update Settlement permission when the form opens and when the settlement is submitted.

**Financial Consideration**

CP-104 records an external payment; it does not transfer money.

The settlement must cover the entire outstanding amount represented by the form's receivable snapshot. Partial settlement is not supported.

Before recording the settlement, the system must verify that the exact outstanding receivables and their amounts have not changed.

## 5. Fields and Validation

### 5.1 Settlement Recording Form

| Element | Type | Required Behavior |
|---|---|---|
| Café Name | Read-only text | Displays the selected Café |
| Settlement Amount | Read-only monetary value | Displays the full outstanding amount |
| Payment Date | Date field | Mandatory; initially empty; today or a past date only |
| Transfer Proof Image | Image upload | Mandatory; JPG/PNG only; maximum 5 MB |
| Save | Action | Validates the form and opens confirmation |
| Cancel | Action | Returns to CP-103 without recording a settlement |
| Confirmation Dialog | Dialog | Requests confirmation before recording |
| Confirm | Dialog action | Starts settlement recording after confirmation |
| Confirmation Cancel | Dialog action | Closes the dialog and preserves entered values |
| Reload | Recovery action | Reloads outstanding receivables after a detected change |

**Form Behavior**

1. The system shall display the selected Café's name and full outstanding amount.
2. Café Name and Settlement Amount shall be read-only.
3. Payment Date and Transfer Proof Image shall be mandatory.
4. Save shall validate the entered values before displaying the Confirmation Dialog.
5. The system shall not record a settlement merely because Save was selected.
6. Recording shall begin only after the Platform Admin selects Confirm.
7. The system shall enforce Update Settlement permission and the financial validation rules before recording.
8. The system shall not allow partial settlement.

### 5.2 Café Name

| Attribute | Specification |
|---|---|
| Field | Café Name |
| Type | Read-only text |
| Source | Selected Café |
| Required | Yes |
| Editable | No |
| Entity Scope | Individual Café |

**Behavior**

1. The system shall display the Café selected in CP-103.
2. The field shall not be editable.
3. The settlement shall apply only to the selected Café.
4. Cafés sharing the same owner or brand shall remain independent.
5. The system shall not combine receivables belonging to different Cafés.
6. A suspended Café shall remain eligible for settlement when all confirmed conditions are satisfied.

### 5.3 Settlement Amount

| Attribute | Specification |
|---|---|
| Field | Settlement Amount |
| Type | Read-only monetary value |
| Source | Full outstanding amount for the selected Café |
| Editable | No |
| Partial Amount Entry | Not supported |
| Currency | SAR |
| Display Precision | Two decimal places |
| Validation | Must represent the full outstanding amount |

**Behavior**

1. The system shall display the full outstanding amount associated with the selected Café's unsettled receivables.
2. The amount shall be read-only.
3. The Platform Admin shall not enter or modify the settlement amount.
4. Partial settlement shall not be supported.
5. The displayed amount shall be based on the applicable receivables and transaction-specific rates established by CP-102 and CP-103.
6. Previously settled receivables shall be excluded.
7. The amount shall be displayed in SAR with two decimal places.
8. The exact calculation rounding rule remains pending Finance confirmation.
9. Before recording, the system shall verify that the outstanding receivables and their amounts still match the snapshot represented by the form.
10. If the current outstanding amount is zero, the system shall prevent settlement recording.

### 5.4 Payment Date

| Attribute | Specification |
|---|---|
| Field | Payment Date |
| Type | Date input |
| Required | Yes |
| Default Value | Empty |
| Meaning | Date the external payment was made |
| Allowed Values | Today or any past date |
| Disallowed Values | Future dates |
| Required Error | `Please fill required field` |
| Future Date Error | `Payment date cannot be in the future` |

**Behavior**

1. Payment Date shall be empty when the form first opens.
2. The Platform Admin shall select or enter the date on which the external payment was made.
3. Payment Date shall be mandatory.
4. Today's date shall be accepted.
5. Any past date shall be accepted.
6. Future dates shall be rejected.
7. If Payment Date is missing, the system shall display `Please fill required field` beneath the field.
8. If Payment Date is in the future, the system shall display `Payment date cannot be in the future`.
9. The system shall prevent the Confirmation Dialog from opening while Payment Date validation fails.
10. The date used to determine today shall follow a business-confirmed timezone, which remains pending clarification.

### 5.5 Transfer Proof Image

| Attribute | Specification |
|---|---|
| Field | Transfer Proof Image |
| Type | Image upload |
| Required | Yes |
| Purpose | Evidence/reference of the external payment |
| Accepted Formats | JPG, PNG |
| Maximum File Size | 5 MB |
| Required Error | `Please fill required field` |
| Invalid Format Error | `Only JPG and PNG images are allowed.` |
| Invalid Size Error | `Image size must not exceed 5 MB.` |

**Behavior**

1. The system shall provide a mandatory Transfer Proof Image upload field.
2. The field shall accept JPG and PNG image files only.
3. The maximum permitted file size shall be 5 MB.
4. The system shall reject files in unsupported formats.
5. The system shall reject files exceeding the confirmed maximum size.
6. If no image has been provided, the system shall display `Please fill required field` beneath the field.
7. For an unsupported format, the system shall display `Only JPG and PNG images are allowed.`
8. For a file exceeding 5 MB, the system shall display `Image size must not exceed 5 MB.`
9. The system shall prevent the Confirmation Dialog from opening while image validation fails.
10. The accepted Transfer Proof Image shall be recorded with the settlement.
11. The image shall remain available for retry when settlement saving fails.
12. Reloading the form after an outstanding-receivables change shall clear the selected image.

### 5.6 Save Action

| Attribute | Specification |
|---|---|
| Action | Save |
| Type | Form action |
| Initial Operation | Validate required fields and input rules |
| Successful Validation | Open Confirmation Dialog |
| Failed Validation | Display applicable field errors |
| Settlement Recording | Not performed until Confirm |

**Behavior**

1. Selecting Save shall initiate validation of Payment Date and Transfer Proof Image.
2. The system shall validate mandatory fields.
3. The system shall validate the Payment Date against the no-future-date rule.
4. The system shall validate the image format and maximum file size.
5. If validation fails, the system shall display the applicable error messages.
6. The system shall not display the Confirmation Dialog until the form passes validation.
7. If validation succeeds, the system shall display the Confirmation Dialog.
8. Selecting Save alone shall not create a Settlement Record or change any receivable status.

### 5.7 Confirmation Dialog

| Attribute | Specification |
|---|---|
| Component | Confirmation Dialog |
| Trigger | Successful Save validation |
| Confirmation Message | `Confirm recording full payment of amount to café name?` |
| Displayed Values | Full Settlement Amount and selected Café Name |
| Actions | Confirm, Cancel |
| Confirm | Initiates settlement recording |
| Cancel | Closes dialog and returns to the populated form |

**Behavior**

1. The system shall display the Confirmation Dialog only after successful form validation.
2. The dialog shall identify the selected Café and full settlement amount.
3. The confirmation text shall follow the Jira wording, substituting the applicable amount and Café name.
4. Selecting Cancel shall close the dialog.
5. Confirmation Cancel shall preserve the entered Payment Date and Transfer Proof Image.
6. Confirmation Cancel shall not create a Settlement Record.
7. Confirmation Cancel shall not change receivable balances or statuses.
8. Selecting Confirm shall initiate the authorized settlement recording process.
9. Confirm shall be disabled while settlement recording is being processed.

### 5.8 Outstanding Receivables Revalidation

| Attribute | Specification |
|---|---|
| Validation | Outstanding Receivables Revalidation |
| Reference | Snapshot represented by the loaded form |
| Comparison | Exact receivable identities and their amounts |
| Total-Only Comparison | Insufficient |
| Timing | Before settlement is recorded |
| Changed Error | `Outstanding receivables have changed. Reload and verify the payment amount before recording settlement.` |
| Zero Outstanding Error | `No outstanding amount to settle` |
| Changed Result | No settlement recorded |

**Behavior**

1. The system shall retain the receivable snapshot associated with the loaded form.
2. Before recording settlement, the system shall retrieve or verify the latest outstanding receivables.
3. The system shall compare the exact outstanding receivables and their amounts against the form snapshot.
4. Matching only the Total Outstanding Amount shall not be sufficient.
5. If the outstanding receivables or their amounts have changed, the system shall block settlement recording.
6. The system shall display `Outstanding receivables have changed. Reload and verify the payment amount before recording settlement.`
7. The Platform Admin shall reload the latest outstanding receivables and verify the amount against the actual external payment before attempting to record settlement again.
8. If no outstanding amount remains, the system shall display `No outstanding amount to settle`.
9. No Settlement Record shall be created when revalidation fails.
10. No covered receivables shall be marked as settled when revalidation fails.
11. The system shall not automatically record a settlement against a changed receivable set.

### 5.9 Reload After Outstanding Change

| Attribute | Specification |
|---|---|
| Action | Reload |
| Availability | Recovery after outstanding-receivables change |
| Financial Data | Reload latest outstanding receivables and amount |
| Payment Date | Cleared |
| Transfer Proof Image | Cleared |
| Required Admin Action | Review latest amount and re-enter payment details |

**Behavior**

1. Reload shall retrieve the latest outstanding receivables for the selected Café.
2. The system shall update the read-only Settlement Amount to reflect the refreshed outstanding amount.
3. The system shall clear Payment Date.
4. The system shall clear Transfer Proof Image.
5. The Platform Admin shall review the refreshed amount against the actual external payment.
6. The Platform Admin shall enter Payment Date and upload Transfer Proof Image again before a new submission.
7. If the refreshed outstanding amount is zero, the system shall not permit settlement recording.
8. Reload shall not itself create a Settlement Record or change receivable statuses.

### 5.10 Settlement Recording and Atomic Save

| Attribute | Specification |
|---|---|
| Operation | Record Full Settlement |
| Payment Type | External payment already made |
| Settlement Scope | Entire validated outstanding receivable set |
| Record Creation | Settlement Record |
| Receivable Update | Mark all covered receivables as settled |
| Consistency | Atomic — all changes succeed or none are saved |
| Partial Settlement | Not allowed |

**Behavior**

1. After Confirm, the system shall enforce Update Settlement permission.
2. The system shall revalidate the exact outstanding receivables and their amounts.
3. If validation succeeds, the system shall record the full settlement.
4. The system shall save the Settlement Record and mark all covered receivables as settled as one atomic operation.
5. The system shall not save a Settlement Record without successfully updating all covered receivables.
6. The system shall not mark covered receivables as settled without successfully recording the corresponding settlement.
7. If any required part of the operation fails, none of the settlement-related changes shall be saved.
8. The system shall prevent the same settlement from being recorded twice.
9. Newly created receivables after a successful settlement shall remain outstanding.
10. The operation shall not initiate a money transfer.

### 5.11 Settlement Audit Data

| Data Element | Required Behavior |
|---|---|
| Café | Record the selected Café |
| Full Amount | Record the full settled amount |
| Payment Date | Record the date of the external payment |
| Transfer Proof Image | Record the uploaded payment evidence |
| Covered Receivables | Record the receivables included in the settlement |
| Recording Admin | Record the Platform Admin who recorded the settlement |
| Recording Timestamp | Record when the settlement was recorded |

**Behavior**

1. The system shall record all seven confirmed data elements.
2. The recorded Café shall be the selected Café.
3. The recorded amount shall equal the full amount successfully settled.
4. The recorded Covered Receivables shall correspond to the exact receivables validated and settled.
5. The Recording Admin shall identify the authorized Platform Admin who completed the operation.
6. The Recording Timestamp shall represent the system-recorded time of the settlement.
7. The Transfer Proof Image shall be associated with the recorded settlement.
8. Audit data and the covered receivable updates shall remain consistent with the atomic settlement operation.

### 5.12 Duplicate Submission Prevention

| Attribute | Specification |
|---|---|
| Risk | Repeated Confirm or duplicate requests |
| UI Protection | Disable Confirm during Processing |
| Server Protection | Prevent duplicate settlement recording |
| Expected Result | One settlement for the validated receivable set |

**Behavior**

1. The system shall disable Confirm while settlement recording is processing.
2. The system shall prevent repeated submissions from recording the same settlement twice.
3. Duplicate requests shall not create duplicate Settlement Records.
4. Duplicate requests shall not cause the same covered receivables to be settled more than once.
5. Server-side protection shall apply independently of the Confirm button's disabled state.
6. The specific technical idempotency mechanism shall be defined during technical design.

### 5.13 Successful Settlement

| Attribute | Specification |
|---|---|
| Condition | Settlement saved successfully |
| Exact Message | `Settlement recorded successfully` |
| Navigation | Return to CP-103 |
| Financial Data | Refresh latest outstanding amounts |
| Covered Receivables | Settled |

**Behavior**

1. After successful atomic settlement recording, the system shall display `Settlement recorded successfully`.
2. The system shall return the Platform Admin to CP-103 — View One Settlement for the selected Café.
3. CP-103 shall refresh the Café's latest outstanding amounts.
4. The covered receivables shall no longer contribute to outstanding totals.
5. Any newly created unsettled receivables shall remain outstanding.
6. If no outstanding receivables remain, CP-103 shall display the zero-outstanding state.
7. Update Settlement shall reflect the refreshed outstanding balance and applicable permission conditions.

### 5.14 Form Cancel

| Attribute | Specification |
|---|---|
| Action | Cancel |
| Location | Settlement Recording Form |
| Destination | CP-103 — View One Settlement |
| Settlement Record | Not created |
| Receivable Changes | None |

**Behavior**

1. Selecting Cancel from the form shall return the Platform Admin directly to CP-103.
2. The system shall not create a Settlement Record.
3. The system shall not mark receivables as settled.
4. The system shall not change outstanding balances as a result of Cancel.
5. Cancel from the form shall be distinguished from Cancel within the Confirmation Dialog.

### 5.15 Save Failure

| Attribute | Specification |
|---|---|
| Condition | Settlement recording fails |
| Exact Message | `Unable to record settlement. Please try again.` |
| Partial Changes | Not allowed |
| Payment Date | Retained |
| Transfer Proof Image | Retained |
| Retry | Permitted, subject to revalidation |

**Behavior**

1. If settlement recording fails, the system shall display `Unable to record settlement. Please try again.`
2. The system shall not retain partial settlement-related changes.
3. The system shall preserve the entered Payment Date.
4. The system shall preserve the uploaded Transfer Proof Image.
5. The Platform Admin shall be able to retry the recording workflow.
6. Any subsequent recording attempt shall remain subject to permission validation, outstanding-receivables revalidation, and duplicate prevention.
7. The system shall not report successful settlement recording when the atomic operation has failed.

### 5.16 Zero Outstanding State

| Attribute | Specification |
|---|---|
| Condition | No outstanding amount remains before recording |
| Exact Message | `No outstanding amount to settle` |
| Settlement Record | Not created |
| Receivable Updates | None |
| Zero-Value Settlement | Not allowed |

**Behavior**

1. If the latest outstanding amount is zero, the system shall prevent settlement recording.
2. The system shall display `No outstanding amount to settle`.
3. No Settlement Record shall be created.
4. No receivable status shall be changed.
5. The system shall not record a zero-value settlement.

### 5.17 Suspended Café Settlement

| Attribute | Specification |
|---|---|
| Café Status | Suspended |
| Settlement Allowed | Yes, when conditions are met |
| Required Outstanding | Greater than zero |
| Required Permission | Update Settlement |
| Settlement Type | Full settlement only |

**Behavior**

1. Suspension alone shall not prevent settlement recording.
2. The system shall allow full settlement for a suspended Café with a positive outstanding amount when the Platform Admin has Update Settlement permission.
3. All standard field validation, financial revalidation, atomic save, and duplicate-prevention rules shall apply.
4. The system shall not combine the suspended Café's receivables with another Café's receivables.

### 5.18 Permission Enforcement

| Attribute | Specification |
|---|---|
| Protected Function | Update Settlement |
| Required Permission | Update Settlement |
| Form Opening | Permission revalidated |
| Confirm / Save | Permission revalidated |
| Direct URL | Protected |
| Direct Request / API | Protected |
| Unauthorized Recording | Prohibited |
| Exact Unauthorized Message | Not specified |

**Behavior**

1. The system shall require Update Settlement permission to access the Settlement Recording Form.
2. The system shall revalidate Update Settlement permission when opening the form.
3. The system shall revalidate Update Settlement permission when Confirm initiates settlement recording.
4. If permission has been revoked, the system shall prevent recording.
5. Users without Update Settlement permission shall not bypass authorization through a direct URL.
6. Users without Update Settlement permission shall not bypass authorization through a direct request.
7. Unauthorized requests shall not create Settlement Records or modify receivables.
8. The exact unauthorized-access message remains unspecified.

## 6. Business Rules

**BR-01 — External Payment Recording**

The system shall record an external payment that has already been made. It shall not transfer money.

**BR-02 — Selected Café Scope**

A settlement shall apply only to the selected Café.

**BR-03 — Independent Café Entity**

Each Café shall be treated as an independent entity, even when Cafés share an owner or brand.

**BR-04 — Full Settlement Only**

The system shall support full settlement of outstanding receivables only. Partial settlement shall not be supported.

**BR-05 — Read-Only Settlement Amount**

The full Settlement Amount shall be displayed as read-only.

**BR-06 — Read-Only Café Name**

The selected Café Name shall be displayed as read-only.

**BR-07 — Mandatory Payment Date**

Payment Date shall be mandatory and initially empty.

**BR-08 — Payment Date Range**

Payment Date shall accept today or any past date and shall reject future dates.

**BR-09 — Mandatory Transfer Proof**

Transfer Proof Image shall be mandatory.

**BR-10 — Accepted Image Formats**

Transfer Proof Image shall accept JPG and PNG only.

**BR-11 — Image Size Limit**

Transfer Proof Image shall not exceed 5 MB.

**BR-12 — Field Validation Before Confirmation**

Save shall validate the required fields and input rules before opening the Confirmation Dialog.

**BR-13 — Confirmation Required**

The system shall request confirmation before recording a settlement.

**BR-14 — Confirmation Cancel**

Cancel within the Confirmation Dialog shall close the dialog, retain entered values, and make no financial changes.

**BR-15 — Recording After Confirm**

The settlement recording operation shall begin only after Confirm is selected.

**BR-16 — Exact Receivable Revalidation**

Before recording settlement, the system shall compare the exact outstanding receivables and their amounts with the snapshot represented by the form.

**BR-17 — Total-Only Comparison Prohibited**

Matching Total Outstanding Amount alone shall not be sufficient to authorize settlement recording.

**BR-18 — Outstanding Changed Restriction**

If outstanding receivables or their amounts have changed, the system shall prevent recording and require the Platform Admin to reload and verify the payment amount.

**BR-19 — Reload After Change**

Reload shall retrieve the latest outstanding amount and receivables and clear Payment Date and Transfer Proof Image.

**BR-20 — Zero Outstanding Restriction**

If no outstanding amount remains, the system shall not record a settlement.

**BR-21 — Atomic Settlement**

Saving the Settlement Record and marking all covered receivables as settled shall be atomic.

**BR-22 — No Partial Financial Changes**

If settlement recording fails, none of the settlement-related changes shall be saved.

**BR-23 — Duplicate Prevention**

The system shall prevent repeated submissions from recording the same settlement twice.

**BR-24 — Confirm Processing State**

Confirm shall be disabled while settlement recording is processing.

**BR-25 — Server-Side Duplicate Protection**

Duplicate prevention shall be enforced on the server independently of the user interface.

**BR-26 — Settlement Audit Data**

The system shall record Café, Full Amount, Payment Date, Transfer Proof Image, Covered Receivables, Recording Admin, and Recording Timestamp.

**BR-27 — Covered Receivable Settlement**

After successful recording, all covered receivables shall be marked as settled.

**BR-28 — Newly Created Receivables**

Receivables created after successful settlement shall remain outstanding.

**BR-29 — Successful Recording Feedback**

The system shall display `Settlement recorded successfully` after successful recording.

**BR-30 — Success Navigation**

After successful recording, the system shall return to CP-103 and refresh the selected Café's outstanding amounts.

**BR-31 — Save Failure Recovery**

When saving fails, the system shall retain Payment Date and Transfer Proof Image for retry without retaining partial financial changes.

**BR-32 — Form Cancel**

Cancel from the Settlement Recording Form shall return to CP-103 without recording payment or changing receivables.

**BR-33 — Suspended Café Eligibility**

A suspended Café shall remain eligible for full settlement when its outstanding amount is greater than zero and the Platform Admin has Update Settlement permission.

**BR-34 — Update Permission**

Update Settlement permission shall be required to open and submit the Settlement Recording Form.

**BR-35 — Permission Revalidation**

Update Settlement permission shall be revalidated when the form opens and when settlement recording is initiated.

**BR-36 — Unauthorized Access Protection**

Users without Update Settlement permission shall not record settlements through the UI, direct URL, or direct requests.

**BR-37 — Previously Settled Receivables**

Previously settled receivables shall not be included in the current settlement.

**BR-38 — Currency**

Settlement Amount shall be displayed in SAR with two decimal places.

**BR-39 — Finance Rounding Dependency**

The calculation rounding rule shall follow the Finance-approved rule, which remains pending confirmation.

**BR-40 — Previously Recorded Settlement Editing**

Editing an existing recorded settlement shall not be supported by CP-104.

## 7. Acceptance Criteria

### AC-01 — Open Settlement Recording Form

**Given** the Platform Admin is authenticated,  
**And** has Update Settlement permission,  
**And** the selected Café has an outstanding amount greater than zero,  
**When** the Platform Admin selects Update Settlement from CP-103,  
**Then** the system shall open the Settlement Recording Form for the selected Café.

### AC-02 — Display Selected Café Name

**Given** the Settlement Recording Form opens successfully,  
**When** the form is displayed,  
**Then** the selected Café Name shall be displayed as read-only.

### AC-03 — Display Full Settlement Amount

**Given** the selected Café has outstanding receivables,  
**When** the form opens,  
**Then** the system shall display the full outstanding amount as read-only,  
**And** shall not allow the Platform Admin to enter a partial amount.

### AC-04 — Payment Date Empty by Default

**Given** the Settlement Recording Form is opened,  
**When** the Payment Date field is displayed,  
**Then** it shall be empty by default.

### AC-05 — Require Payment Date

**Given** Payment Date is empty,  
**When** the Platform Admin selects Save,  
**Then** the system shall display `Please fill required field` beneath Payment Date,  
**And** shall not open the Confirmation Dialog.

### AC-06 — Accept Today's Payment Date

**Given** the Platform Admin selects today's date,  
**When** Payment Date is validated,  
**Then** the date shall be accepted.

### AC-07 — Accept Past Payment Date

**Given** the Platform Admin selects a past date,  
**When** Payment Date is validated,  
**Then** the date shall be accepted.

### AC-08 — Reject Future Payment Date

**Given** the Platform Admin selects a future date,  
**When** Payment Date is validated,  
**Then** the system shall display `Payment date cannot be in the future`,  
**And** shall prevent confirmation.

### AC-09 — Require Transfer Proof Image

**Given** no Transfer Proof Image has been provided,  
**When** the Platform Admin selects Save,  
**Then** the system shall display `Please fill required field` beneath the image field.

### AC-10 — Accept JPG Image

**Given** the Platform Admin uploads a JPG image not exceeding 5 MB,  
**When** the image is validated,  
**Then** the image shall be accepted.

### AC-11 — Accept PNG Image

**Given** the Platform Admin uploads a PNG image not exceeding 5 MB,  
**When** the image is validated,  
**Then** the image shall be accepted.

### AC-12 — Reject Unsupported Image Format

**Given** the Platform Admin uploads an unsupported file format,  
**When** the image is validated,  
**Then** the system shall display `Only JPG and PNG images are allowed.`

### AC-13 — Reject Oversized Image

**Given** the Platform Admin uploads an image exceeding 5 MB,  
**When** the image is validated,  
**Then** the system shall display `Image size must not exceed 5 MB.`

### AC-14 — Validate Before Confirmation

**Given** one or more required fields are invalid,  
**When** the Platform Admin selects Save,  
**Then** the system shall display the applicable validation errors,  
**And** shall not open the Confirmation Dialog.

### AC-15 — Display Confirmation Dialog

**Given** Payment Date and Transfer Proof Image pass validation,  
**When** the Platform Admin selects Save,  
**Then** the system shall display the Confirmation Dialog identifying the full amount and selected Café.

### AC-16 — Cancel Confirmation

**Given** the Confirmation Dialog is displayed,  
**When** the Platform Admin selects Cancel within the dialog,  
**Then** the dialog shall close,  
**And** the form shall retain Payment Date and Transfer Proof Image,  
**And** no settlement shall be recorded.

### AC-17 — Confirm Settlement Recording

**Given** the Confirmation Dialog is displayed,  
**When** the Platform Admin selects Confirm,  
**Then** the system shall initiate permission and financial revalidation before recording the settlement.

### AC-18 — Revalidate Exact Receivables

**Given** the form was loaded with a specific set of outstanding receivables and amounts,  
**When** settlement recording is initiated,  
**Then** the system shall compare the latest outstanding receivable identities and amounts against the form snapshot.

### AC-19 — Reject Changed Receivable Set With Same Total

**Given** the receivable set has changed since the form was loaded,  
**And** the Total Outstanding Amount remains unchanged,  
**When** the Platform Admin confirms settlement,  
**Then** the system shall reject recording because the exact receivable set differs.

### AC-20 — Handle Changed Outstanding Receivables

**Given** the outstanding receivables or their amounts have changed,  
**When** the Platform Admin confirms settlement,  
**Then** the system shall display `Outstanding receivables have changed. Reload and verify the payment amount before recording settlement.`,  
**And** shall not create a Settlement Record.

### AC-21 — Reload Changed Outstanding

**Given** the outstanding-receivables change error is displayed,  
**When** the Platform Admin selects Reload,  
**Then** the system shall retrieve the latest outstanding receivables and amount,  
**And** shall clear Payment Date,  
**And** shall clear Transfer Proof Image.

### AC-22 — Require Re-entry After Reload

**Given** the Platform Admin has reloaded changed outstanding receivables,  
**When** the refreshed form is displayed,  
**Then** the Platform Admin shall be required to review the amount and re-enter the mandatory payment details before resubmission.

### AC-23 — Block Zero Outstanding Settlement

**Given** the latest outstanding amount is zero,  
**When** settlement recording is attempted,  
**Then** the system shall display `No outstanding amount to settle`,  
**And** shall not create a Settlement Record.

### AC-24 — Record Full Settlement Atomically

**Given** the Platform Admin is authorized,  
**And** the form is valid,  
**And** the exact outstanding receivables and amounts remain unchanged,  
**When** the Platform Admin confirms settlement,  
**Then** the system shall save the Settlement Record and mark all covered receivables as settled as one atomic operation.

### AC-25 — Prevent Partial Financial Changes

**Given** a required part of settlement recording fails,  
**When** the atomic operation cannot complete,  
**Then** no settlement-related changes shall be saved.

### AC-26 — Record Settlement Audit Data

**Given** settlement recording succeeds,  
**When** the Settlement Record is created,  
**Then** it shall include the Café, Full Amount, Payment Date, Transfer Proof Image, Covered Receivables, Recording Admin, and Recording Timestamp.

### AC-27 — Disable Confirm During Processing

**Given** settlement recording is processing,  
**When** the Platform Admin views the Confirmation Dialog,  
**Then** Confirm shall be disabled until processing completes.

### AC-28 — Prevent Duplicate Settlement Recording

**Given** repeated requests attempt to record the same settlement,  
**When** the system processes those requests,  
**Then** it shall prevent duplicate Settlement Records and duplicate settlement of covered receivables.

### AC-29 — Display Settlement Success

**Given** the settlement is recorded successfully,  
**When** recording completes,  
**Then** the system shall display `Settlement recorded successfully`.

### AC-30 — Return to Refreshed CP-103

**Given** settlement recording succeeds,  
**When** the success workflow completes,  
**Then** the system shall return to CP-103 for the selected Café,  
**And** refresh its outstanding amounts.

### AC-31 — Exclude Settled Receivables After Success

**Given** covered receivables have been successfully settled,  
**When** CP-103 refreshes,  
**Then** those receivables shall no longer contribute to the outstanding amount.

### AC-32 — Preserve Newly Created Receivables

**Given** new receivables are created after successful settlement,  
**When** CP-103 retrieves the latest outstanding amount,  
**Then** the new unsettled receivables shall remain outstanding.

### AC-33 — Cancel Settlement Form

**Given** the Platform Admin is viewing the Settlement Recording Form,  
**When** the Platform Admin selects Cancel,  
**Then** the system shall return directly to CP-103,  
**And** shall not record a settlement or modify receivables.

### AC-34 — Handle Save Failure

**Given** the Platform Admin confirms settlement,  
**When** saving fails,  
**Then** the system shall display `Unable to record settlement. Please try again.`,  
**And** shall not retain partial settlement-related changes.

### AC-35 — Retain Inputs After Save Failure

**Given** settlement saving fails,  
**When** the error is displayed,  
**Then** Payment Date and Transfer Proof Image shall remain available for retry.

### AC-36 — Revalidate On Retry

**Given** a settlement recording attempt failed,  
**When** the Platform Admin retries,  
**Then** the system shall revalidate authorization and outstanding receivables before recording.

### AC-37 — Allow Suspended Café Settlement

**Given** the selected Café is suspended,  
**And** its outstanding amount is greater than zero,  
**And** the Platform Admin has Update Settlement permission,  
**When** the Platform Admin records a valid full settlement,  
**Then** the system shall allow the settlement subject to all standard validations.

### AC-38 — Revalidate Permission When Opening Form

**Given** the Platform Admin attempts to open the Settlement Recording Form,  
**When** access is requested,  
**Then** the system shall verify Update Settlement permission.

### AC-39 — Revalidate Permission On Confirm

**Given** the Platform Admin opened the form while authorized,  
**But** Update Settlement permission was subsequently revoked,  
**When** the Platform Admin confirms settlement,  
**Then** the system shall prevent recording.

### AC-40 — Deny Unauthorized Direct URL Access

**Given** a user lacks Update Settlement permission,  
**When** the user attempts to access the Settlement Recording Form through a direct URL,  
**Then** the system shall deny access.

### AC-41 — Deny Unauthorized Direct Request

**Given** a user lacks Update Settlement permission,  
**When** the user attempts to record settlement through a direct request,  
**Then** the system shall reject the request,  
**And** shall not create a Settlement Record or modify receivables.

### AC-42 — Do Not Transfer Money

**Given** the Platform Admin records a settlement,  
**When** recording succeeds,  
**Then** the system shall record the external payment only,  
**And** shall not initiate a money transfer.

### AC-43 — Exclude Previously Settled Receivables

**Given** the selected Café has previously settled receivables,  
**When** the system determines the amount covered by the current settlement,  
**Then** previously settled receivables shall be excluded.

### AC-44 — Preserve Café Independence

**Given** multiple Cafés share an owner or brand,  
**When** the Platform Admin records a settlement for one Café,  
**Then** only that Café's covered receivables shall be settled.

## 8. Acceptance Examples

| ID | Input / Scenario | Expected Result |
|---|---|---|
| EX-01 | Authorized Admin opens Update Settlement for Café A with SAR 500.00 outstanding | Form opens for Café A |
| EX-02 | Café A selected | Café Name displays Café A and is read-only |
| EX-03 | Full outstanding = SAR 500.00 | Settlement Amount displays SAR 500.00 and is read-only |
| EX-04 | Admin attempts to enter SAR 200.00 as partial settlement | Partial amount entry is not supported |
| EX-05 | Form initially opens | Payment Date is empty |
| EX-06 | Payment Date missing | `Please fill required field` |
| EX-07 | Payment Date is today | Accepted |
| EX-08 | Payment Date is a past date | Accepted |
| EX-09 | Payment Date is a future date | `Payment date cannot be in the future` |
| EX-10 | Transfer Proof Image missing | `Please fill required field` |
| EX-11 | JPG image, 2 MB | Accepted |
| EX-12 | PNG image, 5 MB | Accepted |
| EX-13 | PDF uploaded | `Only JPG and PNG images are allowed.` |
| EX-14 | JPG image, 6 MB | `Image size must not exceed 5 MB.` |
| EX-15 | Invalid Payment Date and valid image | Save displays field error; no Confirmation Dialog |
| EX-16 | Valid Payment Date and valid image | Save opens Confirmation Dialog |
| EX-17 | Confirmation Dialog displayed | Café and full amount shown |
| EX-18 | Admin selects Cancel in Confirmation Dialog | Dialog closes; entered values retained; no settlement |
| EX-19 | Admin selects Confirm | Permission and financial revalidation begin |
| EX-20 | Snapshot contains T-001 SAR 200 and T-002 SAR 300; current receivables match | Financial revalidation passes |
| EX-21 | Snapshot contains T-001 SAR 200 and T-002 SAR 300; current set contains T-002 SAR 300 and T-003 SAR 200 | Revalidation fails despite unchanged SAR 500 total |
| EX-22 | Outstanding receivables changed | Confirmed Jira changed-outstanding error displayed; no settlement |
| EX-23 | Admin selects Reload after changed-outstanding error | Latest amount loaded; Payment Date and image cleared |
| EX-24 | Reload completes | Admin must review amount and re-enter payment details |
| EX-25 | Outstanding becomes SAR 0.00 before recording | `No outstanding amount to settle`; no settlement |
| EX-26 | Authorized valid settlement of SAR 500.00 succeeds | Settlement Record saved; covered receivables settled |
| EX-27 | Settlement Record save succeeds but receivable update fails | Atomic operation fails; no partial changes retained |
| EX-28 | Receivable update succeeds but Settlement Record save fails | Atomic operation fails; no partial changes retained |
| EX-29 | Settlement successfully recorded | All seven audit data elements recorded |
| EX-30 | Confirm is processing | Confirm disabled |
| EX-31 | Two duplicate requests attempt same settlement | Only one settlement may be recorded |
| EX-32 | Settlement succeeds | `Settlement recorded successfully` |
| EX-33 | Settlement succeeds and no new receivables exist | Return to CP-103; Outstanding = SAR 0.00 |
| EX-34 | New receivable created after successful settlement | New receivable remains outstanding |
| EX-35 | Admin selects Form Cancel | Return to CP-103; no financial changes |
| EX-36 | Save fails | `Unable to record settlement. Please try again.` |
| EX-37 | Save fails after valid form entry | Payment Date and image retained |
| EX-38 | Admin retries after Save failure | Permission and receivable revalidation performed again |
| EX-39 | Suspended Café has SAR 500.00 outstanding and authorized Admin | Full settlement permitted |
| EX-40 | Admin lacks Update Settlement permission when opening form | Access denied |
| EX-41 | Admin's Update Permission revoked after form opens | Confirm cannot record settlement |
| EX-42 | Unauthorized direct URL access | Access denied |
| EX-43 | Unauthorized direct API request | No Settlement Record or receivable changes |
| EX-44 | Café A and Café B share owner | Settlement of Café A does not affect Café B |
| EX-45 | External payment already made | Recording documents payment; no platform money transfer |
| EX-46 | Previously settled receivable exists | Excluded from current settlement |
| EX-47 | Valid Transfer Proof Image uploaded, then outstanding change triggers Reload | Uploaded image cleared |
| EX-48 | Valid Payment Date entered, then outstanding change triggers Reload | Payment Date cleared |

*All financial amounts and transaction identifiers in these examples are illustrative. The examples do not establish a calculation rounding method.*

## 9. Attachments and Related Requirements

### 9.1 Attachments

| Attachment | Description |
|---|---|
| `attachments/image-20261010-151322.png` | CP-104 Jira attachment — Update Settlement wireframe |
| CP-104 Jira Story | Original Update Settlement requirements |
| Confirmed Decisions D-01 to D-20 | Business clarification decisions used for this specification |

**Wireframe Alignment**

1. Rename Transfer Reference to Transfer Proof Image.
2. Display JPG/PNG-only and 5 MB maximum requirements.
3. Display Payment Date empty by default.
4. Show required-field and future-date validation messages.
5. Show image format and size validation messages.
6. Display Confirmation Dialog after successful form validation.
7. Distinguish Confirmation Cancel from Form Cancel.
8. Show disabled Confirm during Processing.
9. Show changed-outstanding error and Reload.
10. Clear Payment Date and image after Reload.
11. Show zero-outstanding error state.
12. Show Save Failure state with retained inputs.
13. Show Permission Revoked state.
14. Show Success Message and return to CP-103.

### 9.2 Related Requirements

| Reference | Name | Relationship |
|---|---|---|
| CP-49 | Administration | Parent Epic |
| CP-50 | Admin Platform | Parent Feature |
| CP-101 | Settlement Management | Settlement Management Feature |
| CP-102 | View Settlement | Settlement Management list |
| CP-103 | View One Settlement | Source page and return destination |
| CP-104 | Update Settlement | Current User Story |
| CP-52 | RBAC | Update Settlement permission management |
| ID not provided | Coffee Receivables | Financial source records |
| ID not provided | Delivery Receivables | Financial source records |

### 9.3 Dependencies

1. **CP-103 Navigation:** CP-103 must pass the selected Café's identity to the Settlement Recording Form.
2. **Independent Café Records:** The system must distinguish each Café independently, regardless of shared ownership or branding.
3. **Outstanding Receivables:** The system must retrieve the latest unsettled receivables for the selected Café.
4. **Receivable Snapshot:** The system must identify the exact receivables and amounts represented by the form.
5. **Financial Revalidation:** The system must detect changes to receivable identities or amounts before recording.
6. **Atomic Persistence:** Settlement Record creation and covered receivable status updates must succeed or fail together.
7. **Duplicate Prevention:** The system must prevent duplicate settlement recording, including repeated requests.
8. **Authorization:** Update Settlement permission must be enforced when opening the form and when recording.
9. **Audit Data:** The system must associate the seven confirmed audit data elements with the settlement.
10. **Image Handling:** The system must support validated JPG/PNG proof images up to 5 MB.
11. **CP-103 Refresh:** After successful recording, CP-103 must display refreshed outstanding amounts.
12. **Finance Rounding:** The exact calculation rounding rule must be confirmed by Finance before finalizing financial implementation.

### 9.4 Pending Confirmations

| ID | Area | Pending Item |
|---|---|---|
| P-01 | Finance Rounding | Exact calculation rounding rule |
| P-02 | Payment Date Timezone | Timezone used to determine whether a Payment Date is in the future |
| P-03 | Duplicate Protection Design | Technical implementation of server-side idempotency and concurrent request handling |
| P-04 | Unauthorized Feedback | Exact message or screen used when Update Settlement permission is missing or revoked |

These items are not treated as confirmed business rules beyond the behavior explicitly defined in this specification.

## 10. Out of Scope

The following functionality is excluded from CP-104:

1. Transferring money to a Café through the platform.
2. Processing bank transfers or external payment transactions.
3. Supporting partial settlements.
4. Editing a previously recorded settlement.
5. Deleting a previously recorded settlement.
6. Displaying Settlement History.
7. Modifying Café receivable rates.
8. Creating Coffee Receivables.
9. Creating Delivery Receivables.
10. Creating or editing Café records.
11. Modifying payment gateway integrations.
12. Defining the technical idempotency mechanism.
13. Defining database schemas or API contracts.
14. Defining the Finance rounding method before confirmation.
15. Combining settlements across independent Cafés.
16. Adding optional settlement notes or additional audit fields not included in the confirmed requirements.
17. Defining a settlement approval workflow.
18. Initiating any new external payment as part of settlement recording.

---
