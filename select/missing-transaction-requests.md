# Missing Transaction Request API

Automate the process of investigating transactions that weren't captured by your integration. Instead of manual submissions, programmatically trigger look-ups with card networks and receive transaction data directly in the API response or via webhooks.

---

## Overview

When a transaction occurs at an enrolled merchant but isn't automatically tracked by your integration, you can use the Missing Transaction Request API to search for it. This is useful for:

- **🔍 Transaction recovery**: Find transactions that may have been missed due to timing or technical issues
- **✅ Data verification**: Confirm transaction details with card network data
- **🏪 MID discovery**: Identify new Merchant IDs that need to be onboarded

> **💡 Tip:** If a missing transaction lookup returns a new Merchant ID, you can onboard it using the [MID Request API](https://docs.fidelapi.com/docs/select/mid-management) or the Dashboard's MID Management tab.

---

## API Endpoints

### Create a Missing Transaction Request

Use this endpoint to initiate a search for a transaction that was not automatically tracked.

**Endpoint:** `POST /missing-transaction-requests` | [API Reference](https://reference.fidel.uk/reference/create-missing-transaction-request)

#### Required Parameters

<table style="border-collapse: collapse; width: 100%; margin-bottom: 16px;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Parameter</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Type</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>cardId</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">String</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The unique identifier of the enrolled card.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>locationId</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">String</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The ID of the location where the transaction occurred.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>transactionDate</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">String</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The date the transaction took place (ISO 8601 format).</td>
  </tr>
  <tr>
    <td style="padding: 12px;"><code>amount</code></td>
    <td style="padding: 12px;">Number</td>
    <td style="padding: 12px;">The exact value of the transaction.</td>
  </tr>
</tbody>
</table>

#### Response Details

If successful, the API returns a Transaction Object containing:

- **Transaction data** shared by the card network
- **Transaction Type** (Visa only): Indicates if the transaction is `settlement` or `authorization`
- **Merchant ID (MID)**: The card network's identifier for that merchant

> **💡 Note:** For Visa transactions, the `vmid` and `vsid` fields may be missing if the MID has not been assigned yet. In this case, the `networkStatus` will be `MID-NOT-ASSIGNED`.

> **🏪 Next Step for Onboarding:** If you discover a new Merchant ID through a missing transaction lookup, use the [MID Request endpoint](https://reference.fidel.uk/reference/get_programsprogramidmidsmidid-3) or the Dashboard's MID Management tab to onboard it.

#### Transaction Date Limits

The transaction date must be within a certain time window, which varies by card network:

<table style="border-collapse: collapse; width: 100%; margin-bottom: 16px;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Card Network</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Maximum Days in the Past</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Visa</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">90 days</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Mastercard</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">90 days</td>
  </tr>
  <tr>
    <td style="padding: 12px;">Amex</td>
    <td style="padding: 12px;">30 days</td>
  </tr>
</tbody>
</table>

---

### Get a Missing Transaction Request

Retrieve the details and current status of a specific request.

**Endpoint:** `GET /missing-transaction-requests/{id}` | [API Reference](https://reference.fidel.uk/reference/get_missing-transaction-requests-missingtransactionrequestid)

---

### List Missing Transaction Requests

View all requests created within your program.

**Endpoint:** `GET /missing-transaction-requests` | [API Reference](https://reference.fidel.uk/reference/list-missing-transaction-request)

---

## Request Lifecycle & Statuses

When a request is created, it moves through several stages based on timing and network feedback:

<table style="border-collapse: collapse; width: 100%; margin-bottom: 16px;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Status</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Pending</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The transaction occurred less than 4 days ago. Processing is paused until the 4-day window passes.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Processing</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The request has been submitted to the card network and is awaiting a response.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Successful</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Transactions were found and the data has been returned.</td>
  </tr>
  <tr>
    <td style="padding: 12px;"><strong>Failed</strong></td>
    <td style="padding: 12px;">The card network could not find a matching transaction for the details provided.</td>
  </tr>
</tbody>
</table>

> **⏱️ Processing Time:** Requests remain in `Pending` status for 4 days after the transaction date before being submitted to the card network. This delay ensures transaction data has been fully processed by all parties.

---

## Webhooks

To avoid constant polling, subscribe to these webhook events to receive real-time updates when a request reaches a terminal state:

<table style="border-collapse: collapse; width: 100%; margin-bottom: 16px;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Event</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>missing-transaction-request.succeeded</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Triggered when transaction results are found by the card network.</td>
  </tr>
  <tr>
    <td style="padding: 12px;"><code>missing-transaction-request.failed</code></td>
    <td style="padding: 12px;">Triggered when no results are found by the card network.</td>
  </tr>
</tbody>
</table>

### Example Webhook Payload (Successful)

```json
fileName:missing-transaction-request-succeeded.json
{
  "id": "317e0700-2a9e-44d3-9380-6c06c6ef3593",
  "accountId": "f7a83b85-2452-4715-806c-35be51b6515b",
  "programId": "a7ce6bed-efad-4582-b9c2-a583185fa39e",
  "brandId": "50c28205-ff3c-440e-8e27-42b7770d8dd9",
  "locationId": "ed8ef30e-5e33-4bf8-93eb-aaa8ee8c2de4",
  "cardId": "bc538b71-31c5-4699-820a-6d4a08693314",
  "cardLastNumbers": "4242",
  "amount": 50.99,
  "transactionDate": "2025-10-02",
  "scheme": "visa",
  "live": true,
  "status": "successful",
  "estimatedCompletionDate": "2025-10-10T16:57:44.230Z",
  "created": "2025-10-06T16:57:44.230Z",
  "updated": "2025-10-06T16:57:55.033Z",
  "result": {
    "transactions": [
      {
        "amount": 50.99,
        "date": "2025-10-02",
        "networkStatus": "SUCCEED",
        "merchantCity": "London",
        "merchantName": "Test Merchant",
        "type": "settlement",
        "vmid": "789012",
        "vsid": "6543211012",
        "visaStoreName": "Test Store"
      }
    ]
  }
}
```

### Example Webhook Payload (Failed)

```json
fileName:missing-transaction-request-failed.json
{
  "id": "317e0700-2a9e-44d3-9380-6c06c6ef3593",
  "accountId": "f7a83b85-2452-4715-806c-35be51b6515b",
  "programId": "a7ce6bed-efad-4582-b9c2-a583185fa39e",
  "brandId": "50c28205-ff3c-440e-8e27-42b7770d8dd9",
  "locationId": "ed8ef30e-5e33-4bf8-93eb-aaa8ee8c2de4",
  "cardId": "bc538b71-31c5-4699-820a-6d4a08693314",
  "cardLastNumbers": "4242",
  "amount": 50.99,
  "transactionDate": "2025-10-02",
  "scheme": "visa",
  "live": true,
  "status": "failed",
  "estimatedCompletionDate": "2025-10-10T16:57:44.230Z",
  "created": "2025-10-06T16:57:44.230Z",
  "updated": "2025-10-06T16:57:55.033Z",
  "result": {
    "error": {
      "code": "transaction-not-found",
      "message": "No matching transactions found for the provided details"
    }
  }
}
```

---

## Dashboard Support

> **🚀 Coming Soon:** Dashboard support for Missing Transaction Requests is currently in development. Please watch this space for updates on the dashboard extension.

---

## Test Mode

In test mode (non-live environment), Missing Transaction Requests are processed immediately without contacting card networks and without the 4-day pending period. You can use specific amount values to simulate different outcomes:

### Success Pattern

Any amount **not listed below** will result in an **immediate success** with mock transaction data.

### Failure Patterns

Use these specific amounts to simulate **immediate failure** scenarios:

<table style="border-collapse: collapse; width: 100%; margin-bottom: 16px;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Amount</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Error Code</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>10.01</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>transaction-not-found</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">There were no transactions on the card network for the request.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>10.02</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>card-not-found</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Card does not exist on the card network.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>10.03</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>card-not-linked-to-program</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Card does not exist on the program.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>10.04</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>program-not-found</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The program does not exist on the card network.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>10.05</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>network-request-failed</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The request to the network has failed.</td>
  </tr>
  <tr>
    <td style="padding: 12px;"><code>10.06</code></td>
    <td style="padding: 12px;"><code>unexpected-error</code></td>
    <td style="padding: 12px;">An unexpected error occurred during processing.</td>
  </tr>
</tbody>
</table>

> **💡 Tip:** Test mode is useful for integration testing and validating your webhook handlers without waiting for the 4-day pending period or card network responses.
