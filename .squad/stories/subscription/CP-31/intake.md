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

As a Guest, I want to view the details of the Without Delivery subscription option, so that I can understand what it includes and decide whether to subscribe.

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
| Terms and Conditions | Display the latest published terms and conditions applicable to this