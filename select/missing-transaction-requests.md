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

**Endpoint:** `POST /cards/{cardId}/missing-transaction-requests` | [API Reference](https://reference.fidel.uk/reference/create-missing-transaction-request)

#### Required Parameters

<table style={{'border-collapse': 'collapse', 'width': '100%', 'margin-bottom': '16px'}}>
<thead>
  <tr style={{'background-color': '#f8f9fa'}}>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Parameter</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Type</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>cardId</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>String</td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The unique identifier of the enrolled card.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>locationId</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>String</td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The ID of the location where the transaction occurred.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>transactionDate</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>String</td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The date the transaction took place (ISO 8601 format).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>amount</code></td>
    <td style={{'padding': '12px'}}>Number</td>
    <td style={{'padding': '12px'}}>The exact value of the transaction.</td>
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

<table style={{'border-collapse': 'collapse', 'width': '100%', 'margin-bottom': '16px'}}>
<thead>
  <tr style={{'background-color': '#f8f9fa'}}>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Card Network</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Maximum Days in the Past</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Visa</td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>90 days</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Mastercard</td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>90 days</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}>Amex</td>
    <td style={{'padding': '12px'}}>30 days</td>
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

**Endpoint:** `GET /programs/{programId}/missing-transaction-requests` | [API Reference](https://reference.fidel.uk/reference/list-missing-transaction-request)

---

## Request Lifecycle & Statuses

When a request is created, it moves through several stages based on timing and network feedback:

<table style={{'border-collapse': 'collapse', 'width': '100%', 'margin-bottom': '16px'}}>
<thead>
  <tr style={{'background-color': '#f8f9fa'}}>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Status</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><strong>Pending</strong></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The transaction occurred less than 4 days ago. Processing is paused until the 4-day window passes.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><strong>Processing</strong></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The request has been submitted to the card network and is awaiting a response.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><strong>Successful</strong></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Transactions were found and the data has been returned.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><strong>Failed</strong></td>
    <td style={{'padding': '12px'}}>The card network could not find a matching transaction for the details provided.</td>
  </tr>
</tbody>
</table>

> **⏱️ Processing Time:** Requests remain in `Pending` status for 4 days after the transaction date before being submitted to the card network. This delay ensures transaction data has been fully processed by all parties.

---

## Webhooks

To avoid constant polling, subscribe to these webhook events to receive real-time updates when a request reaches a terminal state:

<table style={{'border-collapse': 'collapse', 'width': '100%', 'margin-bottom': '16px'}}>
<thead>
  <tr style={{'background-color': '#f8f9fa'}}>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Event</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>missing-transaction-request.succeeded</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Triggered when transaction results are found by the card network.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>missing-transaction-request.failed</code></td>
    <td style={{'padding': '12px'}}>Triggered when no results are found by the card network.</td>
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
        "matchScore": 85,
        "type": "settlement",
        "vmid": "789012",
        "vsid": "6543211012",
        "visaStoreName": "Test Store"
      }
    ]
  }
}
```

> **💡 Note:** See the [Match Score](#match-score) section below for details on how the `matchScore` field works and how to use it to select the best match.

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

## Match Score

When you submit a request for a missing transaction, the API evaluates the transaction details you provided against the data received from the card network. Each result includes a `matchScore` to quantify that relationship.

![Match Score in Matched Transactions](https://docs.fidel.uk/assets/images/mtr-match-score.png "Match Score displayed on matched transactions")

*Screenshot: Matched transactions modal showing match score percentages for each result, helping identify the best match*

- **Ranking**: Results are ranked in descending order, with the most likely match appearing first.
- **Use Case**: This is particularly useful for automating the selection of a **Merchant ID** when multiple similar transactions are found for a single request.

### Field Specification

The `matchScore` field is included in the response object for each transaction result:

<table style={{'border-collapse': 'collapse', 'width': '100%', 'margin-bottom': '16px'}}>
<thead>
  <tr style={{'background-color': '#f8f9fa'}}>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Property</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Detail</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><strong>Type</strong></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Number</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><strong>Range</strong></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>0–100</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><strong>Description</strong></td>
    <td style={{'padding': '12px'}}>A score indicating the likelihood that the returned transaction is the one you're looking for. A higher score means a stronger match based on the provided details.</td>
  </tr>
</tbody>
</table>

### Implementation Example

In a scenario where a single request yields two potential matches, your integration can use the `matchScore` to decide which record to process based on the higher score.

```json
{
  "result": {
    "transactions": [
      {
        "amount": 10.17,
        "date": "2026-02-09",
        "networkStatus": "SUCCEED",
        "merchantCity": "Goleta",
        "merchantName": "Cake Corporation",
        "matchScore": 98,
        "type": "authorization",
        "vmid": "14246971590487O",
        "vsid": "27315004037738S",
        "visaStoreName": "Jeannine's Baking Co."
      },
      {
        "amount": 10.17,
        "date": "2026-02-09",
        "networkStatus": "SUCCEED",
        "merchantCity": "Goleta",
        "merchantName": "Cake Corporation",
        "matchScore": 26,
        "type": "settlement",
        "vmid": "17132168905952S",
        "vsid": "22361983717297O",
        "visaStoreName": "Jeannine's Baking Co."
      }
    ]
  }
}
```

In this example, the authorization transaction with a score of **98** is the most likely match and should be prioritized over the settlement transaction scoring **26**.

---

## Dashboard Support

The Missing Transaction Requests feature is fully available in the Fidel Dashboard, providing a user-friendly interface for creating, tracking, and managing your transaction lookup requests without writing code.

### Video Walkthrough

<video controls style={{'width': '100%', 'border-radius': '8px', 'box-shadow': '0 2px 8px rgba(0,0,0,0.1)', 'margin-bottom': '1rem'}}>
 <source src="https://docs.fidel.uk/assets/videos/mtr_dashboard_walkthrough-match-score.mp4" type="video/mp4" />
  Your browser does not support the video tag.
</video>

---

### Dashboard Features

The dashboard interface provides several powerful capabilities:

#### 🔍 **Direct Network Lookup**
Submit requests directly from the dashboard UI to query card networks for missing transactions.

#### 📊 **Real-time Status Tracking**
Monitor the progress of all your requests with clear visual status indicators in a centralized list view.

#### ✅ **Automatic Validation**
The creation form validates card IDs and location IDs in real-time, showing matched card and location details as you type.

#### 🎯 **Matched Transaction Display**
When transactions are found, click on successful requests to view detailed results including merchant identifiers (MIDs), amounts, dates, and network settlement data.

#### 📤 **Bulk Export**
Export your missing transaction requests and results in CSV or JSON format for reporting and analysis.

#### 🔄 **MID Onboarding Integration**
Directly onboard merchant identifiers (MIDs) from matched transactions to streamline your location setup—no need to switch between tools.

#### 🕐 **90-Day Retention**
Requests are automatically retained for transactions within the past 90 days, ensuring clean and relevant data.

---

### How to Access the Dashboard

1. **Navigate**: From the main dashboard menu, click on **Transactions** → **Missing Requests**
2. **Direct URL**: Navigate to `/missing-transactions` in your dashboard
3. **Sidebar**: The feature appears as a nested item under the Transactions section when viewing transaction-related pages

![Missing Transaction Requests List View](https://docs.fidel.uk/assets/images/mtr-list.png "MTR List view")

*Screenshot: Missing Transaction Requests list page showing requests with status indicators, filters, and action buttons*

---

### Creating a Request in the Dashboard

#### Prerequisites
Before creating a request, you'll need:
- ✅ Valid **Card ID** (must be a card linked to your program)
- ✅ **Transaction date** (within the past 3 months for Visa/Mastercard, 30 days for Amex)
- ✅ **Transaction amount** (exact amount in the card's currency)
- ✅ **Location ID** (Fidel location where the transaction occurred)

#### Step-by-Step Guide

**1. Open the Create Form**

Click the **"Create Missing Transaction Request"** button in the top-right corner of the Missing Transaction Requests page.

![Create Request Form](https://docs.fidel.uk/assets/images/mtr-create.png "Create Request Form")

*Screenshot: Create Missing Transaction Request drawer form with card ID, transaction date, amount, and location ID fields*

**2. Enter Card Details**

- **Card ID**: Paste or type the full Card ID (UUID format)
- **Real-time validation**: As you type, the system validates the card
- **Visual confirmation**: When valid, you'll see card details (scheme icon and last 4 digits)
- **Error handling**: Invalid card IDs display an error message immediately

**3. Select Transaction Date**

- **Date picker**: Choose the date when the transaction occurred
- **Constraint**: Only dates within the allowed timeframe are selectable (see Transaction Date Limits above)
- **Format**: DD/MM/YYYY
- **Help text**: "Lookup is available only for transactions from the past 3 months"

> **Important:** The transaction must be at least 4 days old. Newer transactions will remain in "Pending" status until the 4-day window passes.

**4. Enter Transaction Amount**

- **Format**: Numerical value (e.g., 45.99 or 100)
- **Currency**: Automatically determined based on the card's country
- **Validation**: Amount must be ≥ 0

**5. Provide Location ID**

- **Location ID**: Enter the Fidel Location ID where the transaction took place
- **Real-time validation**: System checks if the location exists in your program
- **Visual confirmation**: When valid, displays:
  - Brand name
  - Location address
  - City, postal code, and country

**6. Submit the Request**

Click **"Submit"** to create the request. You'll see a success notification, and the request will appear in your list.

---

### Viewing and Managing Requests

#### Understanding the Request List

The main page displays all your missing transaction requests in a table with the following columns:

| Column | Description |
|--------|-------------|
| **Date Added** | When the request was created |
| **Status** | Current state of the request (see statuses below) |
| **Last 4** | Last 4 digits of the card |
| **Transaction Date** | Date of the transaction being searched |
| **Amount** | Transaction amount |
| **Brand** | Brand associated with the location |
| **Location** | Location where the transaction occurred |

#### Status Indicators

Each request displays a color-coded status tag. Click **"View status definitions"** in the top-right corner for a comprehensive reference.

![Status Definition Helper](https://docs.fidel.uk/assets/images/mtr-status-def.png "Status Definition Helper")
*Screenshot: Status definitions popup showing explanations for Pending, Processing, Successful, and Failed statuses*

---

### Viewing Matched Transactions

When a request reaches **Successful** status:

**1. View Match Count**

The status tag displays the number of matched transactions (e.g., "Successful - 2 matches").

**2. Open Matched Transactions Modal**

Click on the **Successful status tag** to open a detailed modal showing all found transactions.

![Matched Transactions Modal](https://docs.fidel.uk/assets/images/mtr-matched.png "Matched Transactions Modal")
*Screenshot: Matched transactions modal displaying found transactions with MID details and onboarding options*

**3. Transaction Details**

For each matched transaction, you'll see:
- **Transaction date and time**
- **Transaction amount** (in the card's currency)
- **Merchant information** (name and city, if available)
- **Network status** (succeeded, mid-not-assigned)
- **Merchant Identifiers** by card scheme:
  - **Mastercard**: Location ID, Acquiring MID, SE Number
  - **Visa**: MID, SID, Acquiring MID, BIN, Store Name
  - **Amex**: SE Number

**4. Onboard MIDs**

Select individual transactions using checkboxes, then click **"Onboard Selected MIDs"** to:
- Create MID requests for the selected merchant identifiers
- Automatically link them to the location
- Navigate to the MID requests page to track onboarding progress

> **Tip:** You can select multiple transactions across different card schemes and onboard them in a single action.

---

### Filtering and Search

Quickly find specific requests using the **Filter Box**:

#### Available Filters
- **Request ID**: Search by exact request ID
- **Brand**: Filter by brand
- **Location**: Filter by specific location
- **Card**: Filter by card ID

#### Filter Controls
- **Apply filters**: Type or select values, then click outside the filter box to apply
- **Clear filters**: Click "Clear All" to reset and view all requests
- **Empty state**: When no results match, you'll see "No missing transaction requests matched your current filters"

---

### Exporting Data

Export your missing transaction requests for reporting, compliance, or analysis:

#### How to Export

**1. Click Export Button**

Located in the top-right corner, next to the Status Definitions link.

**2. Configure Export**

- **File Type**: Choose CSV or JSON format
- **Email Addresses** (Optional): Enter up to 5 email addresses to receive the export
  - If blank, the file is sent to your account's primary email
  - Each email address on a new line

**3. Submit Export Job**

Click **"Export"** to queue the export job.

#### What Gets Exported

All missing transaction requests matching your current filters, including:
- Request details (ID, status, created/updated dates)
- Card information (ID, last 4 digits, scheme)
- Transaction details (date, amount)
- Location and brand information
- Matched transaction results (for successful requests)
- Error messages (for failed requests)

#### Receiving the Export

- ✉️ You'll receive an email with a download link when the export is ready
- 📋 The Job ID is automatically copied to your clipboard for tracking
- ⏱️ Large exports may take a few minutes to process

---

### Dashboard Best Practices

#### For Successful Lookups:
- ✅ Use exact transaction amounts (including decimals)
- ✅ Verify the transaction date is correct
- ✅ Ensure the card was active at the time of the transaction
- ✅ Confirm the location ID matches where the transaction occurred

#### Troubleshooting Failed Requests:
- ❌ Double-check all transaction details for accuracy
- ❌ Verify the card was linked to your program on the transaction date
- ❌ Consider that the transaction might not have been processed by the network
- ❌ Review the error message on the Failed status tag for specific guidance

---

## Test Mode

In test mode (non-live environment), Missing Transaction Requests are processed immediately without contacting card networks and without the 4-day pending period. You can use specific amount values to simulate different outcomes:

### Success Pattern

Any amount **not listed below** will result in an **immediate success** with mock transaction data.

### Failure Patterns

Use these specific amounts to simulate **immediate failure** scenarios:

<table style={{'border-collapse': 'collapse', 'width': '100%', 'margin-bottom': '16px'}}>
<thead>
  <tr style={{'background-color': '#f8f9fa'}}>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Amount</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Error Code</th>
    <th style={{'padding': '12px', 'text-align': 'left', 'border-bottom': '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>10.01</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>transaction-not-found</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>There were no transactions on the card network for the request.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>10.02</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>card-not-found</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Card does not exist on the card network.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>10.03</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>card-not-linked-to-program</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>Card does not exist on the program.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>10.04</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>program-not-found</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The program does not exist on the card network.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>10.05</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}><code>network-request-failed</code></td>
    <td style={{'padding': '12px', 'border-bottom': '1px solid #dee2e6'}}>The request to the network has failed.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>10.06</code></td>
    <td style={{'padding': '12px'}}><code>unexpected-error</code></td>
    <td style={{'padding': '12px'}}>An unexpected error occurred during processing.</td>
  </tr>
</tbody>
</table>

> **💡 Tip:** Test mode is useful for integration testing and validating your webhook handlers without waiting for the 4-day pending period or card network responses.
