# Transactions

The Fidel API `Transaction` object is the central piece of data in your card-linked application. When a user makes a purchase with a linked card in a program participating brand location, Fidel captures the transaction event in real-time. The Fidel API then sends the data to your server in JSON format through [webhooks](/webhooks).

One transaction event occurs at authorisation time. The other transaction event occurs when the transaction has cleared. These are two distinct events, and Fidel processes both of them, even if you're not registering webhooks to listen for both event types. Because we bill based on transaction events, and we process both of them, you'll get charged for both of them, regardless of the number or type of webhooks you have registered with Fidel.

## Transaction Object

| **Field name** | **Description** | **Type** |
| --- | ------ | --- |
| `accountId` | The unique identifier of the user account at Fidel API. | string |
| `amount` | The amount of the transaction in the currency it was charged in. | number |
| `approvalCode` | Unique code that is sent along with the authorization of the transaction by Amex. It mirrors the identifiers.amexApprovalCode property. | string (or null) |
| `auth` | Indicates whether Fidel API received an authorization event for this transaction. | boolean |
| `authCode` | Unique code that is sent along with the authorization of the transaction by Visa and Mastercard. It is sent by the issuer/the PCN on the issuer's behalf when approving a transaction. It mirrors the identifiers.mastercardAuthCode or identifiers.visaAuthCode properties, depending on the issuing card for the transaction. | string (or null) |
| `brand.id` | The identifier of the brand at Fidel API. | string |
| `brand.logoURL` | The URL of the logo for the brand. | string (or null) |
| `brand.name` | The name of the merchant where the purchase was made. | string |
| `brand.metadata` | Customized information you added to the brand. | object (or null) |
| `card.firstNumbers` | The first numbers of the card, used for helping users identify their cards. | string |
| `card.id` | The tokenized card identifier created by Fidel API. | string |
| `card.lastNumbers` | The last numbers of the card, used for helping users identify their cards. | string |
| `card.metadata` | Customized information you added to the card. | object (or null) |
| `card.scheme` | The payment network (Visa, Mastercard or Amex). | string |
| `cardPresent` | Indicates whether the transaction was in store (as opposed to online). Only available in Visa transactions, and only if the transaction data received from Visa contains this information. In test transactions, this field is always null. | boolean (or null) |
| `cleared` | Indicates whether Fidel API received a clearing event for this transaction. | boolean |
| `created` | The time when the transaction object was created in the Fidel API database. | string, in ISO format |
| `currency` | The currency of the purchase. | string, in ISO 4217 format |
| `datetime` | The time of the transaction (local time of the merchant). | string, in ISO format |
| `id` | The unique identifier of the transaction. | string |
| `identifiers` | See the definition of the identifiers object below. | object |
| `location.address` | The street address of the location where the purchase was made. | string |
| `location.city` | The city of the location where the purchase was made. | string |
| `location.countryCode` | The country code of the location where the purchase was made. | string, Alpha3 format of [ISO 3166-1](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes) |
| `location.` `geolocation.latitude` | The latitude of the location where the purchase was made. | number |
| `location.` `geolocation.longitude` | The longitude of the location where the purchase was made. | number |
| `location.id` | The identifier of the location where the purchase was made. | string |
| `location.metadata` | Custom metadata that you added to the location. | object (or null) |
| `location.postcode` | The postcode of the location where the purchase was made. | string |
| `location.timezone` | The time zone of the location where the purchase was made. | string |
| `merchantCategoryCode` | The merchant category code (MCC) provided by the card network. It is used to classify businesses by the types of goods provided or services rendered. May not be present in all transactions. | string |
| `offer.cashback` | The amount of money that can be refunded to the cardholder according to the offer. | number |
| `offer.id` | The identifier of the offer. The location where the transaction was made is linked to the offer. If the transaction qualifies for more than one offer, the one with highest reward for the cardholder is added to the transaction. If none of the offers apply, the most recently created is added to the transaction. | string |
| `offer.message` | If the transaction doesn't qualify for the offer, the message field explains why. | string (or null) |
| `offer.performanceFee` | Fee paid to Fidel API for the service. | number |
| `offer.qualificationDate` | The date when the offer will become qualified, depending on the return period. If the offer has a return period, transactions will only qualify for the offer after the return period has passed. | string |
| `offer.qualified` | Indicates whether the transaction is qualified for the offer. | boolean |
| `programId` | The identifier of the Fidel API program that this transaction is linked to. | string |
| `refundTransactionId` | The identifier of the transaction that was refunded by this transaction, if applies. | string |
| `updated` | The date and time when this transaction was last time updated (authorized/cleared). | string, in ISO format |
| `wallet` | This property has been deprecated since August 2019 because of privacy concerns, and will always return `null` on transactions created after that date. If you're retrieving a transaction that was created before August 2019, the property could be one of: `"apple-pay"`, `"google-pay"`, or `"samsung-pay"`. | null |

The `identifiers` object includes the following properties, all of them being of string type or null:

- `identifiers.amexApprovalCode`: Unique code that is sent along with the authorization of the transaction by Amex. It mirrors the approvalCode property.
- `identifiers.mastercardAuthCode`: Unique code that is sent along with the authorization of the transaction by Mastercard. It mirrors the authCode property.
- `identifiers.mastercardRefNumber`: Unique identifier of the transaction provided by Mastercard. It is generated by Mastercard when the authorization request is received from the issuer. This is used by Fidel API to try to match a given clearing transaction with the corresponding authorization transaction.
- `identifiers.mastercardTransactionSequenceNumber`: Transaction sequence number provided by Mastercard. This is generated by Mastercard to identify the sequence of the transaction for a given PAN in a calendar day. Available only in clearing transactions.
- `identifiers.MID`: Card terminal identifier in a brand's location that may or may not be unique. MIDs can be different between auth and clearing.
- `identifiers.visaAuthCode`: Unique code that is sent along with the authorization of the transaction by Visa. It mirrors the authCode property.

```json
summary:Example of a transaction in JSON format (API version is 2022-07-13)
fileName:transaction.json
{
  "id": "7fdfd5d8-9589-402f-8477-4a727ad138a2",
  "accountId": "4ed4b72b-aa4c-43a1-8054-da6d1368e17a",
  "amount": 100,
  "approvalCode": "AA00BB",
  "auth": true,
  "authCode": "A73H890",
  "cardPresent": false,
  "cleared": false,
  "created": "2019-04-09T16:00:00.644Z",
  "currency": "GBP",
  "datetime": "2019-04-10T15:59:30",
  "programId": "6e38aa0c-b7ef-46bd-b1bd-c07c647d9cba",
  "refundTransactionId": null,
  "updated": "2019-04-09T16:00:00.644Z",
  "wallet": null,
  "brand": {
    "id": "9d136f2e-df99-4a08-a0a5-3bc1534b7db8",
    "logoURL": "https://example.com/logo.png",
    "name": "Coffee Brand",
    "metadata": null
  },
  "card": {
    "id": "bc538b71-31c5-4699-820a-6d4a08693314",
    "firstNumbers": "555500",
    "lastNumbers": "5001",
    "scheme": "visa",
    "metadata": {
      "id": "00012345"
    }
  },
  "identifiers": {
    "amexApprovalCode": "AA00BB",
    "mastercardAuthCode": null,
    "mastercardRefNumber": "AABBCCDDE",
    "mastercardTransactionSequenceNumber": "0000001234567",
    "MID": "8552067328",
    "visaAuthCode": "A73H890"
  },
  "location": {
    "id": "7a916fbd-70a0-462f-8dbc-bd7dbfbea140",
    "address": "53 Frith Street",
    "city": "London",
    "countryCode": "GBR",
    "postcode": "W1D 4SN",
    "timezone": "Europe/London",
    "geolocation": {
      "latitude": 51.513716,
      "longitude": -0.13202
    },
    "metadata": {
      "id": "0001234567",
      "name": "Coffee Brand HQ"
    }
  },
  "offer": {
    "qualified": true,
    "id": "eeefb94b-d11c-44db-81d7-d86a9fcc4069",
    "message": [],
    "qualificationDate": null,
    "cashback": 2.5,
    "performanceFee": 0.3
  },
}
```

The Fidel API currently supports three types of transactions: authorisation transactions, clearing transactions and refund transactions.

**Authorisation transactions** are processed when a purchase registers on a linked card. For example, when a customer makes a payment in-store or online in real time. When a customer makes a payment with a linked debit/credit card in an auth-enabled location, the `transaction.auth` webhook is also triggered and the transaction object sent to your specified URL in real time.

**Clearing transactions**, also known as “settled transaction”, are processed when a payment transaction settles, usually happens 48 to 72 hours after a payment registers. The Fidel processes for clearing transactions are also triggering the `transaction.clearing` webhook events. The processes run daily at 12:00 UTC for Mastercard and multiple times per day for Visa and American Express. Only one transaction is sent per event.

**Refund transactions** are processed when a payment is refunded, usually when a purchased item is returned, and the payment reverses. A refunded transaction triggers two webhook events, `transaction.clearing` and `transaction.refund`, with the `auth` property set to false. The amount on both events is negative. The Fidel API tries to identify the initial transaction for which the refund was issued, using `cardId`, `locationId`, `merchantId`, `amount` and `datetime`. If an associated initial transaction is identified, the webhook data contains the `originalTransactionId`. If no initial transaction is identified, the data comes in on both webhooks with a negative amount but no `originalTransactionId` property.

The original transaction has a new `refundTransactionId` property, set to the `transactionId` of the refunded transaction. Updates on the original transaction will not trigger a webhook event.

You will receive both `transaction.auth` events in real-time and `transaction.clearing` events (in the next 48 to 72 hours). When clearing transactions are processed, the Fidel API matches the clearing transaction to the corresponding authorisation transaction if it exists by updating the `cleared` property from `false` to `true`.

If you need the updated information about the original transaction, you can retrieve it using our Transactions API, with the `originalTransactionId` from the refunded Transaction object.

```sh
curl -X GET \
  https://api.fidel.uk/v1/transactions/a375e18f-0678-40fa-aa8b-4875e2146437 \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>'
```

We suggest that you use the auth event to notify the user that you registered the transaction and will fulfil the reward when the transaction clears. The clearing event is the confirmation that the transaction has been settled.

<div class="info-box">
  <small>Note</small><br/>
  Please allow up to 24h after linking a Mastercard card to start receiving real-time authorisation transactions.
</div>

## Test Transactions

For testing purposes, you can use the [**API Playground**](https://dashboard.fidel.uk/playground) in the Fidel Dashboard test environment to create test transactions and test your application logic. Alternatively, you can use the [Create Test Transaction](https://reference.fidel.uk/reference/create-transaction-test) API endpoint to create authorisation test transaction. You can see an example implementation for creating and clearing test transactions via the API in our [sample application on GitHub](https://github.com/FidelLimited/fidel-api-sample-app/blob/main/server/controllers/transactions.js).

To create a test transaction, you will need a [Program](/programs), a [Location](/locations) and a test [Card](/cards) linked to the program.

##### Create Test Transactions Using the API

```sh
curl -X POST \
  https://api.fidel.uk/v1/transactions/test \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "amount": 10,
    "cardId": "bc538b71-31c5-4699-820a-6d4a08693314",
    "locationId": "7a916fbd-70a0-462f-8dbc-bd7dbfbea140"
  }'
```

##### Create Test Transactions Using the API Playground

On the Fidel Dashboard, go to the [**Playground**](https://dashboard.fidel.uk/playground) option in the navigation menu. Click on the transactions **/create** link, located on the left side, in the **Endpoints** drawer menu. The method will be set to POST and the endpoint to **_/transactions/test_**. An editable sample JSON object like the following one will be used to create the transaction.

![Create transaction](https://docs.fidel.uk/assets/images/create-transaction.png "Create transaction")

To create a test transaction, use the dropdown menus to select the Program, Location and Card for the transaction. These selections will be used to populate the `cardId`, `locationId` and the `amount` in the JSON payload. You can modify any of the properties in the JSON file (including the amount).

Click **Run** and a test authorisation transaction will be created. If the transaction is created successfully, you will see the transaction object in the Response body box. If you have registered a `transaction.auth` webhook event for this program, the authorisation transaction object will be sent to your webhook URL as well.

##### Clear Test Transactions Using the Dashboard

To clear an authorisation test transaction, you can navigate to the [Transaction](https://dashboard.fidel.uk/transactions) option in the Dashboard navigation menu. You'll see all your transactions listed there. Find the one you want to change the status of, click on the three dots on the right side of it. A popup menu will appear, and you should click on the 'Clear' option. This action will change the status of the transaction from `auth` to `cleared`, and trigger the `transaction.clearing` webhook event.

## API Reference

To find out more about our Transactions API and how to use it with your application, please visit the [Fidel API Reference](https://reference.fidel.uk/reference/create-transaction-test).
