# Transactions

Once an offer is active, when a consumer makes a payment with a linked card at a participating location, the transaction workflow is triggered.

## Publisher Responsibilities

Within Offers as a Service, publishers are responsible for submitting transaction data, enrolling cardholders, and issuing rewards in accordance with the offer terms. All submitted transaction data is validated against active offers. Refunds and reversals must not be included, as they cannot be reliably identified by the platform. In cases of partial refunds, publishers should submit the adjusted cashback amount to reflect the corrected transaction value.

All transaction data must be anonymised and exclude any personally identifiable information (PII), including primary account numbers (PAN) and bank account details.

Publishers are also responsible for defining and managing offer targeting rules, such as distinguishing between new and existing cardholders. This is typically based on historical transaction behaviour (e.g. over the previous 3–6 months). Offers should only be delivered to eligible and enrolled consumers who meet the defined targeting criteria.

![OaaS Transaction Flow](https://docs.fidel.uk/assets/images/oaas-transaction-flow.png "OaaS Transaction Flow Diagram")

## Submitting Transactions

To enable transaction processing, Publishers must share 'verified' transactions with Fidel by matching them to the offer ID to the transaction (as well as MID, brand name, address, lat/lang if provided in offer details).

Transactions must be submitted to Fidel using batch files via the SFTP server. Publishers should transmit batch processing files to Fidel on a consistent basis. Those files can be submitted to Fidel in any of the following intervals:

- **Daily** (recommended)
- **Weekly**
- **Monthly**

In order for Fidel to monitor file transfers, publishers should follow the selected schedule. In cases where there is no transaction data, no file needs to be sent.

## File Requirements

Publishers should adhere to the following general requirements when sending files to Fidel:

- File header must be present
- Required fields in the file must be populated

New and established publishers that want to offer a rewards program to the cardholders, must send batch processing files according to the following:

- **File Format:** CSV
- **Property Delimiter:** `|`

Detailed file structures for both the Inbound Transaction Record and Inbound Cardholder Record are documented in the Fidel API documentation, available in the Publisher to Fidel Files (Inbound) section.

## Monitoring & Validation

Once connected to Fidel's SFTP, publishers gain access to the **Data Analytics Platform (Metabase)**, where all submitted transactions can be monitored.

Fidel performs validation on submitted transactions (typically within a few hours). The following reports are generated:

- **Approved Transactions** (transaction distro-only value)
- **Rejected Transactions** (transaction distro-only invalid)

### Rejection Error Codes

If a transaction fails validation, it will be marked as rejected and include an associated error code to help identify the issue. Common rejection reasons include:

- `Duplicated transaction`
- `Failed to identify publisher: Please make sure to include the accountId on the filename`
- `Failed to apply validations: MID not eligible for offer`
- `Failed to apply validations: Offer is not yet live`
- `Failed to apply validations: Offer expired`
- `Failed to apply validations: Offer with ID ${offerId} not found`
- `Failed to apply validations: Transaction currency does not match offer`
- `Failed to apply validations: Cashback does not match offer reward. Calculated ${inputReward}, expected ${expectedReward}`
