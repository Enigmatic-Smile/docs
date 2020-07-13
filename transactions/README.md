# Transactions
The Fidel API `Transaction` object is the central piece of data in your card-linked application. When a customer makes a purchase with a linked card in any of the program's participating locations, Fidel tracks the transaction in the payment card networks. The Fidel API sends the data to your server in JSON format through [webhooks](/webhooks).


## Transaction Object

##### API version from `2019-03-05`

```json
fileName:transaction.json
{
  "id": "7fdfd5d8-9589-402f-8477-4a727ad138a2",
  "accountId": "4ed4b72b-aa4c-43a1-8054-da6d1368e17a",
  "amount": 100,
  "auth": true,
  "cleared": false,
  "created": "2019-04-09T16:00:00.644Z",
  "currency": "GBP",
  "datetime": "2019-04-10T15:59:30",
  "offer": null,
  "programId": "6e38aa0c-b7ef-46bd-b1bd-c07c647d9cba",
  "refundTransactionId": null,
  "updated": "2019-04-09T16:00:00.644Z",
  "wallet": "apple-pay",
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
      "id": "00012345",
      "name": "Joseph Cooper"
    }
  },
  "identifiers": {
    "amexApprovalCode": "AA00BB",
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
  }
}
```
> The wallet property could be one of: `"apple-pay" | "google-pay" | "samsung-pay"`.

<details>
  <summary style="margin-bottom: 30px;">JSON Response on API versions to 2018-08-16</summary>
<div class="code-box"><strong>transaction.json</strong><pre><code class="language-json hljs">{
  <span class="hljs-attr">"id"</span>: <span class="hljs-string">"7fdfd5d8-9589-402f-8477-4a727ad239a2"</span>,
  <span class="hljs-attr">"accountId"</span>: <span class="hljs-string">"4ed4b62b-aa4c-43a1-8064-da6d1368e17a"</span>,
  <span class="hljs-attr">"programId"</span>: <span class="hljs-string">"6e38aa0c-b7ef-46bd-b1bd-c07c648d9cba"</span>,
  <span class="hljs-attr">"brandId"</span>: <span class="hljs-string">"9d136f2e-df99-4a08-a0a5-3bc1534b7db9"</span>,
  <span class="hljs-attr">"locationId"</span>: <span class="hljs-string">"7a916fbd-70a0-462f-8dbc-bd7dbfbea160"</span>,
  <span class="hljs-attr">"cardId"</span>: <span class="hljs-string">"bc538b71-31c5-4699-840a-6d4a08693314"</span>,
  <span class="hljs-attr">"amount"</span>: <span class="hljs-number">100</span>,
  <span class="hljs-attr">"currency"</span>: <span class="hljs-string">"GBP"</span>,
  <span class="hljs-attr">"countryCode"</span>: <span class="hljs-string">"GBR"</span>,
  <span class="hljs-attr">"scheme"</span>: <span class="hljs-string">"visa"</span>,
  <span class="hljs-attr">"firstNumbers"</span>: <span class="hljs-string">"555500"</span>,
  <span class="hljs-attr">"lastNumbers"</span>: <span class="hljs-string">"5001"</span>,
  <span class="hljs-attr">"address"</span>: <span class="hljs-string">"2 Soho Square"</span>,
  <span class="hljs-attr">"postcode"</span>: <span class="hljs-string">"W1D3PX"</span>,
  <span class="hljs-attr">"city"</span>: <span class="hljs-string">"London"</span>,
  <span class="hljs-attr">"merchantId"</span>: <span class="hljs-string">"12345"</span>,
  <span class="hljs-attr">"live"</span>: <span class="hljs-literal">false</span>,
  <span class="hljs-attr">"auth"</span>: <span class="hljs-literal">true</span>,
  <span class="hljs-attr">"cleared"</span>: <span class="hljs-literal">true</span>,
  <span class="hljs-attr">"time"</span>: <span class="hljs-string">"2017-03-02T19:12:01.743Z"</span>,
  <span class="hljs-attr">"date"</span>: <span class="hljs-string">"2017-03-02T19:12:01.743Z"</span>,
  <span class="hljs-attr">"created"</span>: <span class="hljs-string">"2017-03-02T19:12:01.744Z"</span>,
  <span class="hljs-attr">"updated"</span>: <span class="hljs-string">"2017-03-02T19:12:01.744Z"</span>,
  <span class="hljs-attr">"offer"</span>: <span class="hljs-literal">null</span>,
  <span class="hljs-attr">"medatada"</span>: {
    <span class="hljs-attr">"id"</span>: <span class="hljs-string">"your-unique-id"</span>,
    <span class="hljs-attr">"property"</span>: <span class="hljs-string">"value"</span>
  }
}</code><span class="line-numbers-rows" aria-hidden="true"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span></pre></div>
</details>

The Fidel API currently supports three types of transactions: authorisation transactions, clearing transactions and refund transactions.

**Authorisation transactions** are processed when a purchase registers on a linked card. For example, when a customer is making a payment in-store or online in real-time. When a customer makes a payment with a linked debit/credit card in an auth-enabled location, the `transaction.auth` webhook is also triggered and the transaction object sent to your specified URL in real-time.

**Clearing transactions**, also known as “settled transaction”, are processed when a payment transaction settles, usually happens 48 to 72 hours after a payment registers. The Fidel processes for clearing transactions are also triggering the `transaction.clearing` webhook events. The processes run daily at 12:00 UTC for Mastercard and multiple times per day for Visa and American Express. Only one transaction is sent per event.

**Refund transactions** are processed when a payment is refunded, usually when a purchased item is returned, and the payment reverses. A refunded transaction triggers two webhook events, `transaction.clearing` and `transaction.refund`, with the `auth` property set to false. The amount on both events is negative. The Fidel API tries to identify the initial transaction for which the refund was issued, using `cardId`, `locationId`, `merchantId`, `amount` and `datetime`. If an associated initial transaction is identified, the webhook data contains the `originalTransactionId`. If no initial transaction is identified, the data comes in on both webhooks with a negative amount but no `originalTransactionId` property.

The original transaction has a new `refundTransactionId` property, set to the `transactionId` of the refunded transaction. Updates on he original transaction will not trigger a webhook event.

You will receive both `transaction.auth` events in real-time and `transaction.clearing` events (in the next 48 to 72 hours). When clearing transactions are processed, the Fidel API matches the clearing transaction to the corresponding authorisation transaction if it exists by updating the `cleared` property from `false` to `true`.

If you need the updated information about the original transaction, you can retrieve it using our Transactions API, with the `originalTransactionId` from the refunded Transaction object.

```sh
curl -X GET \
  https://api.fidel.uk/v1/transactions/a375e18f-0678-40fa-aa8b-4875e2146437 \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63'
```

We suggest that you use the auth event to notify the user that you registered the transaction and will fulfil the reward when the transaction clears. The clearing event is the confirmation that the transaction has been settled.

<div class="info-box">
  <small>Note</small><br/>
  Please allow up to 24h after linking a Mastercard card to start receiving real-time authorisation transactions.
</div>


## Test Transactions

For testing purposes, you can use the [**API Playground**](https://dashboard.fidel.uk/playground) in the Fidel Dashboard test environment to create test transactions and test your application logic. Alternatively, you can use the [Create Test Transaction](https://reference.fidel.uk/reference#create-transaction-test) API endpoint to create authorisation test transaction.

To create a test transaction, you will need a [Program](/programs), a [Location](/locations) and a test [Card](cards) linked to the program.

##### Create Test Transactions Using the API.

```sh
curl -X POST \
  https://api.fidel.uk/v1/transactions/test \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
  -d '{
    "amount": 10,
    "cardId": "bc538b71-31c5-4699-820a-6d4a08693314",
    "locationId": "7a916fbd-70a0-462f-8dbc-bd7dbfbea140"
  }'
```

##### Create Test Transactions Using the API Playground.

On the Fidel Dashboard, go to the [**Playground**](https://dashboard.fidel.uk/playground) option in the navigation menu. Click on **Create transaction** link, located on the left side, in the **END POINTS** menu. The method will be set to POST and the endpoint to **_/transactions/test_**. An editable sample JSON object like the following one will be used to create the transaction.

![Create transaction](https://docs.fidel.uk/assets/images/create-transaction.png "Create transaction")

To create a test transaction, use the dropdown menus to select the Program, Location and Card for the transaction. These selections will be used to populate the `cardId`, `locationId` and the `amount` in the JSON payload.  You can modify any of the properties in the JSON file (including the amount).

Click **Send** and a test authorisation transaction will be created. If the transaction is created successfully, you will see the transaction object in the response body box. If you have registered a `transaction.auth` webhook event for this program, the authorisation transaction object will be sent to your webhook URL as well.

##### Clear Test Transactions Using the Dashboard.

To clear an authorisation test transaction, you can navigate to the [Transaction](https://dashboard.fidel.uk/transactions) option in the Dashboard navigation menu. You'll see all your transactions listed there. Find the one you want to change the status of, click on the three dots on the right side of it. A popup menu will appear, and you should click on the 'Clear transaction' option. This action will change the status of the transaction from `auth` to `cleared`, and trigger the `transaction.clearing` webhook event.

## API Reference

If you're looking to find out more about our Transactions API and how to use it with your application, please visit the [Fidel API Reference](https://reference.fidel.uk/reference#create-transaction-test).
