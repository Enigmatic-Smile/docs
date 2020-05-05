# Transactions
The transaction object is the central piece of data of your card-linked application. When a user makes a purchase with a linked card in any of the program's participating locations, Fidel tracks the transaction in the payment card networks and sends it to your server in JSON format through [webhooks](https://docs.fidel.uk/webhooks).

## Transaction object

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
    "logoUrl": "https://coffeebrand.com/logo.png",
    "name": "Coffee Brand"
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

There are two types of transactions: authorisation transactions and clearing transactions. **Authorisation transactions** are processed when the customer is making the payment in-store or online. They trigger the `transaction.auth` webhook event that you can use to notify or reward the user in your application in real-time.

**Clearing transactions** are processed 48-72 hours after the payment is made. Fidel processes clearing transactions triggering the `transaction.clearing` webhook events daily at 12:00 UTC for Mastercard and multiple times per day for Visa and American Express. Only one transaction is sent per event.

You will receive both `transaction.auth` events in real-time and `transaction.clearing` events (in the next 48-72 hours). When clearing transactions are processed, Fidel matches the clearing transaction to the corresponding authorisation transaction if it exists by updating the `cleared` property from `false` to `true`.

We suggest that you use the auth event to notify the user that you registered the transaction and will fulfill the reward when the transaction clears, since the clearing is the confirmation that the transaction has been settled.

Please allow up to 24h after linking a Mastercard card to start receiving real-time authorisation transactions.

## Refund transactions

Refund transactions are processed when a purchased item is returned and the payment is reversed. Refund transactions are a type of clearing transactions that have negative `amount` and trigger the `transaction.clearing` webhook event.

Fidel matches full refund transactions to its original transaction by comparing `cardId`, `locationId`, `merchantId`, `amount` and `datetime`. The refund `transactionId` is set in the original transaction as `refundTransactionId` and the original `transactionId` is set on the refund transaction as `originalTransactionId`.

## Test transactions

For testing purposes, you can use the **API Playground** in test environment to create test transactions and test your application logic.

To create a test transaction you will need a program, a location and a test card linked to the program.  

On the dashboard, go to **API Playground** and click on **Create transaction** from the endpoints menu. The method will be set to POST and the endpoint to **_/transactions/test_**. An editable sample JSON object like the following one will be use to create the transaction.

To create a test transaction, use the dropdown menus to select the Program, Location and Card for the transaction.  These selections will be used to populate the `cardId`, `locationId` and the `amount` in the JSON payload.  You can modify any of the properties in the JSON file (including the amount). If the transaction is created successfully you will see the transaction object in the response body box.

##### Create sample transactions in test mode using the API Playground.

![Create transaction](https://docs.fidel.uk/assets/images/create-transaction.png "Create transaction")

Click **Send** and a test authorisation transaction will be created. If you have created a `transaction.auth` webhook event for this program, the authorisation transaction object will be sent to your webhook URL. Alternatively, you can use the [Create Test Transaction](https://reference.fidel.uk/reference#create-transaction-test) API endpoint to create authorisation test transaction.

In order to clear an authorisation test transaction you can open the transaction options menu in the dashboard and click on 'Clear transaction'. This action will trigger the `transaction.clearing` webhook event.
