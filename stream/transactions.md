# Transactions

The transaction object is the central piece of data in your card-linked application. When a user makes a purchase with a linked card, Fidel API captures the transaction event in real time. Fidel API then sends the data to your server in JSON format through [webhooks](doc:webhooks). This allows you to build customized experiences that react to users' real-time transactions.

This page explains how transactions can be collected and processed with the Fidel API platform.

## Transaction Event Types

In the [life cycle of a transaction](/stream/#transaction-life-cycle), there are multiple transaction events. One transaction event occurs at authorization time. Another transaction event occurs when the transaction has cleared, meaning the funds were successfully moved. If a payment gets reimbursed, that results in a third event. These are distinct events, and Fidel API processes all of them, even if you're not registering webhooks to listen for all event types.

Fidel API currently supports three types of transactions: authorization transactions, clearing transactions and refund transactions.

### Authorization Transactions

Authorization transactions are processed when a purchase is made on a linked card at an auth-enabled location (either in-store or online). For example, when a customer makes a purchase at an auth-enabled location with their linked card, the `transaction.auth` webhook will be triggered and the transaction object will be sent to your specified URL in real time.

### Clearing Transactions

Clearing transactions - also known as settled transactions - are processed when a payment transaction settles. This usually happens 48 to 72 hours after a payment is made. Clearing transactions trigger the `transaction.clearing` webhook event. The processes run multiple times per day for Visa. Only one transaction is sent per event.

### Refund Transactions

Refund transactions are processed when a payment is refunded, i.e., when a purchased item is returned and the payment reverses. A refunded transaction triggers two webhook events: `transaction.clearing` and `transaction.refund`, with the `auth` property set to `false`. The amount on both events is negative. Fidel API tries to identify the initial transaction for which the refund was issued using `cardId`, `locationId`, `merchantId`, `amount` and `datetime`. If an associated initial transaction is identified, the webhook data contains the `originalTransactionId`. If no initial transaction is identified, the data comes in on both webhooks with a negative amount but no `originalTransactionId` property.

The original transaction has a `refundTransactionId` property set to the `transactionId` of the refunded transaction. Updates on the original transaction will not trigger a webhook event.

You will receive both `transaction.auth` events in real time and `transaction.clearing` events (in the next 48 to 72 hours). When clearing transactions are processed, Fidel API matches the clearing transaction to the corresponding authorization transaction if it exists by updating the `cleared` property from `false` to `true`.

If you need the updated information about the original transaction, you can retrieve it using the [Get Transaction](https://transaction-stream.fidel.uk/reference/get-transaction) endpoint, with the `originalTransactionId` from the refunded transaction object.

## Transaction Object

The transaction object contains the following data:

| **Field name**                                                                  | **Description**                                                                                                                                                                                                                                                                                                                                  | **Type**                                                            |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| `accountId`                                                                     | The unique identifier of the user account at Fidel API.                                                                                                                                                                                                                                                                                          | string                                                              |
| `amount`                                                                        | The amount of the transaction in the currency it was charged in. In the case of international transactions, the transaction has both `amount` and `billingAmount` fields filled in and their value may differ due to the conversion.                                                                                                             | number                                                              |
| `auth`                                                                          | Indicates whether the transaction was authorized by the issuing bank.                                                                                                                                                                                                                                                                            | boolean                                                             |
| `authCode`                                                                      | Unique code that authorizes a transaction, sent by the issuer/the PCN on the issuer's behalf when approving a transaction. It mirrors the `identifiers.visaAuthCode` property. You can use the authorization code in combination with the card number to match an authorization event and a clearing event when the transaction ID is different. | string (or null)                                                    |
| `billingAmount`                                                                 | The amount in the currency of the country of issuance, converted from the original purchase currency. In the case of international transactions, the transaction has both `amount` and `billingAmount` fields filled in and their value may differ due to the conversion.                                                                        | number (or null)                                                    |
| `billingCurrency`                                                               | The currency of the country of issuance.                                                                                                                                                                                                                                                                                                         | string in ISO 4217 format (or null)                                 |
| `brand.name`                                                                    | The name of the merchant where the purchase was made.                                                                                                                                                                                                                                                                                            | string                                                              |
| `card.firstNumbers`                                                             | The first numbers of the card, used for helping users identify their cards.                                                                                                                                                                                                                                                                      | string                                                              |
| `card.id`                                                                       | The tokenized card identifier used by Fidel API.                                                                                                                                                                                                                                                                                                 | string                                                              |
| `card.lastNumbers`                                                              | The last numbers of the card, used for helping users identify their cards.                                                                                                                                                                                                                                                                       | string                                                              |
| `card.scheme`                                                                   | The payment network (Visa).                                                                                                                                                                                                                                                                                                                      | string                                                              |
| `cardPresent`                                                                   | Not in use                                                                                                                                                                                                                                                                                                                                       | null                                                                |
| `cleared`                                                                       | Indicates whether the transaction was cleared.                                                                                                                                                                                                                                                                                                   | boolean                                                             |
| `created`                                                                       | The timestamp in UTC format when the transaction object was created in the Fidel API database.                                                                                                                                                                                                                                                   | string, timestamp in UTC format                                     |
| `currency`                                                                      | The original currency of the purchase.                                                                                                                                                                                                                                                                                                           | string in [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) format |
| `datetime`                                                                      | The timestamp (in the local timezone of the merchant/acquirer) of the transaction.                                                                                                                                                                                                                                                               | string, timestamp in UTC format                                     |
| `id`                                                                            | The unique identifier of the transaction.                                                                                                                                                                                                                                                                                                        | string                                                              |
| `identifiers.amexApprovalCode`                                                  | Not in use in Transaction Stream programs.                                                                                                                                                                                                                                                                                                       | null                                                                |
| `identifiers.mastercardAuthCode`                                                | Not in use in Transaction Stream programs.                                                                                                                                                                                                                                                                                                       | null                                                                |
| `identifiers.mastercardRefNumber`                                               | Not in use in Transaction Stream programs.                                                                                                                                                                                                                                                                                                       | null                                                                |
| `identifiers.mastercardTransactionSequenceNumber`                               | Not in use in Transaction Stream programs.                                                                                                                                                                                                                                                                                                       | null                                                                |
| `identifiers.visaAuthCode`                                                      | Unique code that is sent along with the authorization of the transaction by Visa. It mirrors the `authCode` property.                                                                                                                                                                                                                            | string (or null)                                                    |
| `location`                                                                      | Details of the merchant where the purchase was made:                                                                                                                                                                                                                                                                                             |
| `location.city`, `location.countryCode`, `location.postcode`, `location.state`. | string (city, postcode, and state can be null)                                                                                                                                                                                                                                                                                                   |
| `merchantCategoryCode`                                                          | The merchant category code (MCC) provided by the card network. It is used to classify businesses by the types of goods provided or services rendered.                                                                                                                                                                                            | string                                                              |
| `programId`                                                                     | The identifier of the Fidel API program that this transaction is linked to.                                                                                                                                                                                                                                                                      | string                                                              |
| `programType`                                                                   | The type of the Fidel API program that this transaction is linked to (`transaction-stream`).                                                                                                                                                                                                                                                     | string                                                              |
| `updated`                                                                       | The date and time when this transaction was last time updated (authorized/cleared/reimbursed).                                                                                                                                                                                                                                                   | string, timestamp in UTC format                                     |
| `wallet`                                                                        | Not in use.                                                                                                                                                                                                                                                                                                                                      | null                                                                |

Example of a transaction in JSON format:

```json
{
  "accountId": "f61741c3-3dc9-45f5-8e7c-123456789012",
  "amount": 486,
  "auth": true,
  "authCode": "00856I",
  "billingAmount": 4.43,
  "billingCurrency": "USD",
  "brand": {
    "name": "My Coffee Company"
  },
  "card": {
    "firstNumbers": "123456",
    "id": "159cad70-8187-4d01-8b67-123456789012",
    "lastNumbers": "1234",
    "scheme": "visa"
  },
  "cardPresent": null,
  "cleared": true,
  "created": "2021-08-05T20:10:31.746Z",
  "currency": "JPY",
  "datetime": "2021-08-05T20:10:28",
  "id": "a6e9759c-525b-4942-aa56-123456789012",
  "identifiers": {
    "amexApprovalCode": null,
    "mastercardAuthCode": null,
    "mastercardRefNumber": null,
    "mastercardTransactionSequenceNumber": null,
    "visaAuthCode": "00856I"
  },
  "location": {
    "countryCode": "JPN",
    "postcode": "340-0144",
    "state": "SAITAMA"
  },
  "merchantCategoryCode": "4816",
  "merchantDescriptor": "My Coffee Company",
  "programId": "9ba1adb2-22e8-4c7e-b100-123456789012",
  "programType": "transaction-stream",
  "updated": "2021-08-11T11:31:18.543Z",
  "wallet": null
}
```

## Creating a Test Transaction on the Dashboard

For testing purposes, you can use the [**API Playground**](https://dashboard.fidel.uk/playground) in the Fidel API test environment to create test transactions and test your application logic. Alternatively, you can use the [Create Test Transaction](https://reference.fidel.uk/reference/create-transaction-test) endpoint to create authorization test transactions.

![](https://docs.fidel.uk/assets/images/create-test-transaction.png")

To create a test transaction, you will need to have a program and a test card already linked to it.
Then, follow these steps to create a test transaction:

1. On the Fidel API dashboard go to the [Playground](https://dashboard.fidel.uk/playground) option in the navigation menu.
2. Click on `Create Transaction`. The method will be set to POST and the endpoint to `/transactions/test`.
3. Select a program and a card. This selection will be used to populate the `cardId` in the JSON payload. You can modify any of the properties in the request payload (including the `cardId`).
4. Click `Run` and a test authorization transaction will be created.

If the transaction is created successfully, you will see the transaction object in the `Response` box. If you have registered a `transaction.auth` webhook event for this program, the authorization transaction object will be sent to your webhook URL as well.

### Clearing Test Transactions Using the Dashboard

To clear an authorization transaction, you can navigate to the program, and then to the [Transaction](https://dashboard.fidel.uk/transactions) option in the Dashboard navigation menu. You'll see all your transactions listed there. Find the one you want to change the status of, click on the three dots on the right side of it. A popup menu will appear. Click on the 'Clear' option. This action will change the status of the transaction from `auth` to `cleared`, and trigger the `transaction.clearing` webhook event.

## Testing Transactions in a Live Environment

### How to Collect Test Data

After going live, you can link any eligible card to a program, and make regular purchases to test your system.

### How to Receive the Data Collected

You can check the transactions of a specific program on the Fidel API Dashboard.

You can also retrieve the data using the Transactions API. In addition, you can create webhooks to receive transaction events.

## Unsupported Transactions

The following types of transactions are not supported:

- Encryption verification transactions
- Transactions that don't travel through the network (e.g some recurring payments, some subscriptions of digital services) \*\*

\*\* Specific scenarios are geographically dependent.

## For Developers

### Creating a Test Transaction Using the API

To create a test transaction, you will need to have a program and a test card already linked to it. Then, you can create a transaction the following way:

```sh
curl -X POST \
https://api.fidel.uk/v1/transactions/test \
-H 'content-type: application/json' \
-H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
-d '{
    "amount": 10,
    "cardId": "bc538b71-31c5-4699-820a-6d4a08693314",
    "currency": "USD"
}'
```

### API Reference

To find out more about our Transaction Stream API and how to use it with your application, check the [API Reference](https://transaction-stream.fidel.uk/reference/introduction-1).
