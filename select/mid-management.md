# MID Management API

Manage the full lifecycle of Merchant IDs (MIDs) for your card-linked programs. Onboard, offboard, and reassign MIDs to control where you receive transaction data.

---

## Merchant IDs (MIDs)

View and manage Merchant IDs (MIDs) onboarded for your program by selecting a Program from the Programs page. This opens the 'Merchant IDs' tab in the Fidel Dashboard.

<img src="https://docs.fidel.uk/assets/images/merchantids.png" alt="Merchant IDs" style="width: 100%; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;" />

All onboarded MIDs for the selected program are displayed on this page. You can also retrieve this information via the API: [Get MID](https://reference.fidel.uk/reference/get-mid#/) | [List MIDs](https://reference.fidel.uk/reference/list-mids#/)

---

### Onboard MID

Onboard MIDs to existing locations via the [Create MID Request API](https://reference.fidel.uk/reference/create-mid-request#/) or the Fidel Dashboard. Learn more about [what is a MID request](#what-is-a-mid-request?).

#### Dashboard Walkthrough

<video controls style="width: 100%; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;">
 <source src="https://docs.fidel.uk/assets/videos/onboard-mid-walkthrough.mov" type="video/mp4" />
  Your browser does not support the video tag.
</video>

**Step-by-step:**

1. **Navigate to Programs** - Select your program from the Programs page to open the Merchant IDs tab
2. **Initiate onboarding** - Click **+ Onboard MID** in the upper right
3. **Select location** - Choose the brand and location where you want to onboard the MID, then click **Next**
4. **Choose MID origin** - Select the appropriate origin from the dropdown (see table below for descriptions)
5. **Enter MID details** - Select the card network and enter the MID value
6. **Submit** - Click **Done** to create the request

#### MID Origin Types

<table style="border-collapse: collapse; width: 100%; margin-bottom: 16px;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Origin</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Missing transaction lookup</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">A transaction was made at an onboarded location and the MID was identified from the transaction data (most accurate).</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Manual lookup</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Fidel support manually looked up the card network MID for an onboarded location through the card networks.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Brand provided</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The MID was provided directly by the brand/merchant.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><strong>Processor provided</strong></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The MID was provided by the payment processor or acquirer (e.g., Stripe, Adyen).</td>
  </tr>
  <tr>
    <td style="padding: 12px;"><strong>Third-party provided</strong></td>
    <td style="padding: 12px;">A third-party service (e.g., Incubit) sourced the MID for the location.</td>
  </tr>
</tbody>
</table>

> **⏱️ Processing Time:** MID requests are processed asynchronously and typically take up to 7 days to complete due to card network constraints.

---

### Offboard MID

Offboard a MID via the [Create MID Request API](https://reference.fidel.uk/reference/create-mid-request#/) with the action 'offboard', or via the Fidel Dashboard.

#### Dashboard Walkthrough

<video controls style="width: 100%; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;">
 <source src="https://docs.fidel.uk/assets/videos/offboard-mid-walkthrough.mov" type="video/mp4" />
  Your browser does not support the video tag.
</video>

**Step-by-step:**

1. **Locate MID** - Find the MID you want to offboard in the Merchant IDs list
2. **Open actions menu** - Click the three dots next to the relevant MID
3. **Select offboard** - Choose **Offboard MID** from the dropdown menu
4. **Choose reason** - Select the reason for offboarding from the dropdown
5. **Acknowledge impact** - Confirm that you understand you will no longer receive transactions for this MID
6. **Submit** - Click **Confirm** to create the offboard request

> **📋 Next Steps:** This creates a new MID request. See the [MID Requests](#mid-requests) section to track the status.

---

### Reassign MID

Reassign a MID to a different location via the [Create MID Request API](https://reference.fidel.uk/reference/create-mid-request#/) with the action 'reassign', or via the Fidel Dashboard.

#### Dashboard Walkthrough

<video controls style="width: 100%; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;">
 <source src="https://docs.fidel.uk/assets/videos/reassign-mid-walkthrough.mov" type="video/mp4" />
  Your browser does not support the video tag.
</video>

**Step-by-step:**

1. **Locate MID** - Find the MID you want to reassign in the Merchant IDs list
2. **Open actions menu** - Click the three dots next to the relevant MID
3. **Select reassign** - Choose **Reassign MID** from the dropdown menu
4. **Choose reason** - Select the reason for reassigning the MID
5. **Select new location** - Choose the brand and location where the MID should be assigned
6. **Submit** - Click **Done** to create the reassignment request

> **📋 Next Steps:** This creates a new MID request. See the [MID Requests](#mid-requests) section to track the status.

---

## MID Requests

### What is a MID Request?

A MID request is a workflow instruction to the Fidel platform to source, remove, or re-link a card-network MID for a specific merchant location within a program.

**Supported actions map to the MID lifecycle:**

- **✅ onboard**: Ask Fidel to provision or ingest a MID for AMEX, Mastercard, or Visa.
- **❌ offboard**: Remove an existing MID that should no longer be associated with a location, stopping the transaction ingestion pipeline.
- **🔄 reassign**: Move an existing MID from one location to another.

> **📊 Request Tracking:** Every request is tracked independently so you can poll for status, inspect error details, and retrieve the card-network outcome (returned MID identifiers) once processing finishes.

MID requests are displayed under the 'Requests' tab on the Dashboard. All MID requests you create to onboard, offboard, or reassign MIDs will be visible on this tab or can be retrieved via these endpoints: [Get MID Request](https://reference.fidel.uk/reference/get-mid-request#/) | [List MID Requests](https://reference.fidel.uk/reference/list-mid-requests#/)

<img src="https://docs.fidel.uk/assets/images/mid_requests.png" alt="MID requests" style="width: 100%; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;" />

The different statuses of MID requests are defined below:

<img src="https://docs.fidel.uk/assets/images/mid_request_status_definition.png" alt="MID requests status definition" style="width: 100%; max-width: 400px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;" />

### Webhooks

Subscribe to webhooks to receive real-time notifications when a MID request succeeds or fails:

- ✅ `mid-request.succeeded`
- ❌ `mid-request.failed`

You can also view the status on the Dashboard. To understand why a MID request failed, inspect the raw data by clicking on the failed request:

<img src="https://docs.fidel.uk/assets/images/mid_request_detail.png" alt="MID request detail" style="width: 100%; max-width: 600px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 1rem;" />

### Error Codes

These are the error codes for failed MID requests:

<table style="border-collapse: collapse; width: 100%;">
<thead>
  <tr style="background-color: #f8f9fa;">
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Error Code</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Message</th>
    <th style="padding: 12px; text-align: left; border-bottom: 2px solid #dee2e6;">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>card-scheme-mid-not-found</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Merchant not valid</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The card scheme could not locate a valid MID for this merchant.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>program-not-configured</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Client program not configured with card scheme</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The client program has not been configured with the card scheme, so the MID cannot be onboarded.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>card-scheme-shared-mid</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">Shared MID</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The MID provided for onboarding is a 'shared MID'. See 'Shared MIDs' in the FAQs for more information.</td>
  </tr>
  <tr>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;"><code>no-mids-found</code></td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">No MIDs were found at the card scheme</td>
    <td style="padding: 12px; border-bottom: 1px solid #dee2e6;">The MID provided as part of the MID request does not exist at the card network/scheme.</td>
  </tr>
  <tr>
    <td style="padding: 12px;"><code>unexpected-error</code></td>
    <td style="padding: 12px;">Unexpected error</td>
    <td style="padding: 12px;">An unexpected exception occurred during processing.</td>
  </tr>
</tbody>
</table>