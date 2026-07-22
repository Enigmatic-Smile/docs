# List MID Insights

The List MID Insights endpoint provides programmatic clarity into the health, status, and validation details of your onboarded Merchant Identification Numbers (MIDs) across card networks. As part of our Phase 2 rollout, this endpoint enables advanced programmatic querying to proactively monitor your transaction ingestion pipelines.

---

## Overview

The List MID Insights endpoint allows you to query and monitor the full lifecycle of your onboarded MIDs. Key capabilities include:

- **🔍 Health monitoring**: Track the current status of every onboarded MID across card networks
- **📊 Network activation details**: View scheme-specific identifiers (Visa, Mastercard, Amex) for each MID
- **🔗 Transaction linkage**: Identify the first and last transactions seen on each MID for your program
- **📅 Date filtering**: Filter MID insights by last seen transaction date ranges
- **🏷️ Origin tracing**: Understand how each MID was discovered or sourced
- **📄 Paginated results**: Efficiently retrieve large sets of MID insights using cursor-based pagination

---

## Availability & Requirements

> **⚠️ Important Eligibility Notice:** This endpoint is exclusively available to **Premium Tier** customers.

- **Activation**: This feature is not enabled by default. It is only available to Premium Tier clients. To unlock access for your program, please reach out directly to your assigned Customer Success Manager (CSM).
- **Dashboard Support**: This release represents the programmatic API phase. Full self-service visualization and dashboard functionality for MID Insights will be available in the Fidel Dashboard by August 2026.

---

## API Endpoints

### List MID Insights

Retrieve a paginated list of MID insights for a program, including health, status, and validation details across card networks.

**Endpoint:** `GET /programs/{programId}/mid-insights` | [API Reference](https://reference.fidel.uk/reference/list-mid-insights)

#### Request Parameters

##### Headers

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Header Name</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Type</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Required</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>Authorization</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>String</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Your live secret API key (Bearer SK_KEY).</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Yes</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>Fidel-Version</code></td>
    <td style={{'padding': '12px'}}>String</td>
    <td style={{'padding': '12px'}}>The targeted API version (e.g., <code>2024-11-25</code>).</td>
    <td style={{'padding': '12px'}}>Yes</td>
  </tr>
</tbody>
</table>

##### Path Parameters

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Parameter</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Type</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Required</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px'}}><code>programId</code></td>
    <td style={{'padding': '12px'}}>uuid</td>
    <td style={{'padding': '12px'}}>The id of the program.</td>
    <td style={{'padding': '12px'}}>Yes</td>
  </tr>
</tbody>
</table>

##### Query Parameters

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Parameter</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Type</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Required</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>brandId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>uuid</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Brand unique identifier.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>id</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>uuid</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mid insight unique identifier.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>limit</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Numerical digit from 1 up to 100.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>locationId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>uuid</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Location unique identifier.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>midId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>uuid</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Merchant unique identifier.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>lastSeenTransactionDateFrom</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>date, date-time</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Filter MID insights with a last seen transaction date on or after this value for your program. Accepts ISO 8601 date or date-time format.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>lastSeenTransactionDateTo</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>date, date-time</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Filter MID insights with a last seen transaction date on or before this value for your program. Accepts ISO 8601 date or date-time format.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>origin</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>enum</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Filters MID insights by how the MID was sourced or discovered for your program. Accepted values: <code>address-match</code>, <code>brand-provided-mid</code>, <code>customer-provided-mid</code>, <code>manual-mid-lookup</code>, <code>missing-transaction-lookup</code>, <code>processor-provided-mid</code>, <code>system-mid-lookup</code>, <code>system-provided-mid</code>, <code>third-party-provided-mid</code>.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>seNumber</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Amex Service Establishment number (Merchant ID). Must be exactly 10 numeric digits.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>mcAcquiringMid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Mastercard Acquiring Merchant ID.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>mcLocationId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Mastercard Location ID.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>visaAcquiringMid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Visa Acquiring Merchant ID (CAID).</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>visaBin</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Visa Bank Identification Number.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>vmid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Visa Merchant ID. Must be 6 to 8 numeric digits.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>vsid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The Visa Store ID.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>select</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>enum</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Select specific response mode. Use <code>count</code> to return only the total count without items.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>sort</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>enum</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Field to sort results by. Accepted values: <code>created</code>, <code>onboardedAt</code>, <code>offboardedAt</code>, <code>lastSeenTransactionDate</code>, <code>sourcedAt</code>, <code>eventTs</code>, <code>updated</code>, <code>origin</code>, <code>status</code>, <code>scheme</code>.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>order</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>enum</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Sort order direction. Accepted values: <code>asc</code>, <code>desc</code>.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>start</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Base64-encoded pagination cursor for the next page of results. Obtained from the <code>last</code> field of a previous response.</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>No</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>status</code></td>
    <td style={{'padding': '12px'}}>enum</td>
    <td style={{'padding': '12px'}}>Filter by MID insight status. Accepted values: <code>onboarded</code>, <code>offboarded</code>.</td>
    <td style={{'padding': '12px'}}>No</td>
  </tr>
</tbody>
</table>

---

## Response Object

The endpoint returns a JSON object containing a paginated array of MID insights. Each entry details network activation statuses and diagnostic messages to help you resolve network mismatches quickly.

The top-level response contains:

- **`count`** — Total number of matching MID insights
- **`items`** — Array of MID insight records (see fields below)
- **`last`** — Pagination cursor for the next page of results (pass as `start` in your next request)

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Field</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Type</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Required</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>brandId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The brand (merchant) this MID insight belongs to.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>cardNetworkMerchantName</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Merchant name as registered with the card network.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>created</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Timestamp when the MID insight record was created.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>id</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Unique identifier of this MID insight record.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>locationId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The location this MID is associated with.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>mcAcquiringMid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mastercard acquiring MID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>mcLocationId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mastercard location identifier.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>midId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The MID record this insight is linked to.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>midRequestId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The MID request that originated this MID, if applicable.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>offboardedAt</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Timestamp when the MID was offboarded from the card network.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>onboardedAt</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Timestamp when the MID was onboarded to the card network.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>origin</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>How the MID was sourced (e.g. manual, automatic). See <a href="#mid-origin-types">MID Origin Types</a>.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>programId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The program this MID insight belongs to.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>seNumber</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Service Establishment number (Amex merchant identifier).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>sourcedAt</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Timestamp when the MID was initially sourced or discovered.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>scheme</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Card network scheme (e.g. <code>visa</code>, <code>mastercard</code>, <code>amex</code>).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>status</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Required</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Current status of the MID insight (e.g. <code>onboarded</code>, <code>offboarded</code>, <code>sourced</code>).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>visaAcquiringMid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa acquiring MID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>visaBin</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Bank Identification Number.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>vmid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Merchant ID (assigned by Visa).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>vsid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Store ID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>lastSeenTransactionDate</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Date of the most recent transaction seen on this MID for your program.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>lastSeenTransactionId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>ID of the most recent transaction seen on this MID for your program.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>firstTransactionClearingId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>string</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Optional</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>ID of the first clearing transaction on this MID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>firstTransactionAuthId</code></td>
    <td style={{'padding': '12px'}}>string</td>
    <td style={{'padding': '12px'}}>Optional</td>
    <td style={{'padding': '12px'}}>ID of the first authorisation transaction on this MID for your program.</td>
  </tr>
</tbody>
</table>

---

## MID Origin Types

The `origin` field describes how each MID was discovered or sourced for a location.

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Origin</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Response Field Value</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Address match</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>address-match</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>MID was identified as part of searching the address automatically on the card network and matching MIDs based on brand name and address details provided by the client on location level.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Missing transaction lookup</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>missing-transaction-lookup</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>A transaction was made at an onboarded location and the MID was identified from the transaction data (most accurate).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Manual lookup</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>manual-mid-lookup</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Fidel support manually looked up the card network MID for an onboarded location through the card networks.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Brand provided</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>brand-provided-mid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The MID was provided directly by the brand or merchant.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Customer provided</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>customer-provided-mid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The client added the MID to the location or to a MID request for onboarding.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Processor provided</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>processor-provided-mid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The MID was provided by the payment processor or acquirer (e.g., Stripe, Adyen).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Third-party provided</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>third-party-provided-mid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>A third-party service (e.g., Incubit) sourced the MID for the location.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}>System MID look-up</td>
    <td style={{'padding': '12px'}}><code>system-mid-lookup</code></td>
    <td style={{'padding': '12px'}}>A Mastercard CAID (Card Acceptor ID) triggered the system to look up the Visa MIDs/CAIDs through the Mastercard CAID.</td>
  </tr>
</tbody>
</table>

---

## Scheme-Specific Fields

Depending on the card network (`scheme`) of a MID insight, different identifier fields will be populated in the response.

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Field</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Scheme</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>seNumber</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Amex</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Amex MID number (Service Establishment number).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>mcAcquiringMid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mastercard</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mastercard Acquiring Merchant ID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>mcLocationId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mastercard</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Mastercard Location ID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>visaAcquiringMid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Acquiring Merchant ID (CAID).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>visaBin</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Bank Identification Number.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>vmid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Merchant ID (assigned by Visa).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>vsid</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Visa Store ID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>cardNetworkMerchantName</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>All schemes</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Merchant name as registered with the card network at the scheme for this MID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>lastSeenTransactionDate</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>All schemes</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Date of the last transaction tracked by Fidel for your program on this MID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>lastSeenTransactionId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>All schemes</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Last tracked Fidel transaction ID on this MID for your program (confirms whether the MID is still active).</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>firstTransactionClearingId</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>All schemes</td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>First Fidel clearing transaction ID tracked on this MID.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>firstTransactionAuthId</code></td>
    <td style={{'padding': '12px'}}>All schemes</td>
    <td style={{'padding': '12px'}}>First Fidel authorisation transaction ID tracked on this MID for your program.</td>
  </tr>
</tbody>
</table>

---

## Implementation Example

```curl
curl --request GET \
  --url 'https://api.fidel.uk/v1/programs/programId/mid-insights?sort=created&order=desc' \
  --header 'accept: application/json' \
  --header 'Authorization: Bearer clsk_live_...' \
  --header 'Fidel-Version: 2024-11-25'
```

---

## Error Codes

<table style={{borderCollapse: 'collapse', 'width': '100%', marginBottom: '16px'}}>
<thead>
  <tr style={{backgroundColor: '#f8f9fa'}}>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>HTTP Status</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Error Code</th>
    <th style={{'padding': '12px', textAlign: 'left', borderBottom: '2px solid #dee2e6'}}>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>401</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>unauthorized</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The API key provided is invalid or expired.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>403</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>tier_restricted</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>Feature locked. Your account is not configured for Premium tier access.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>404</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}><code>resource_not_found</code></td>
    <td style={{'padding': '12px', borderBottom: '1px solid #dee2e6'}}>The requested <code>programId</code> could not be found.</td>
  </tr>
  <tr>
    <td style={{'padding': '12px'}}><code>429</code></td>
    <td style={{'padding': '12px'}}><code>rate_limit_exceeded</code></td>
    <td style={{'padding': '12px'}}>Request volume limits exceeded. Implement request back-off mechanisms.</td>
  </tr>
</tbody>
</table>
