# Webhooks
Fidel uses webhooks to notify your application when relevant events happen in your account. This functionality can be used to receive data from events not triggered by direct API requests or to receive the data in a service that is not responsible for making the API request but needs to consume the response.

We will notify your registered webhook URLs as the event happens. For example, when a customer makes a payment with a linked Mastercard on a participating location, a `transaction.auth` event is sent in real-time to the specified webhook URL with the transactional data in the request payload.

## Creating webhooks
Webhooks can be created in the dashboard's webhooks page or by using the [Webhooks API](https://reference.fidel.uk/v1/reference#create-webhook-brand).
You can create up to five webhook URLs for the same event in the same Program. Fidel sends the data via HTTP POST request and will send test or live events depending on your dashboard's environment setting or if you are using test or live API keys. 

Fidel only accepts HTTPS URLs for webhooks endpoints. In order to create webhooks and receive event data, your server must be configured to support HTTPS with a valid certificate.

<div class="info-box">
    <small>Important note</small><br/>
    To confirm receipt of a webhook event, your server endpoint should return a `200` HTTP status code. Any other response will be treated as a failure and we retry the request three times over the next hour with exponential back off.
</div>

## Authentication

To confirm that received events are being sent from Fidel we recommend verifying webhook signatures. That can be done by using the `x-fidel-signature` and `x-fidel-timestamp` HTTP headers. This isn't required, but offers an additional layer of security.

<hr />

A unique secret key is generated for each webhook. The key is returned in the response's `secretKey` property if you are using the Webhooks API. You can also copy the key from the dashboard's webhooks page by clicking in the **Show Key** button next to your webhook endpoint. To verify a webhook request, generate a signature using the same key that Fidel uses and compare that to the value of the `x-fidel-signature` header.

1. Create a string concatenating the body of the request, the webhook URL and the timestamp value from the `x-fidel-timestamp` header.
2. Double hash the resulting string using the webhook key with HMAC-SHA256 and encode it in Base-64.
3. Compare the signature you generated with the signature provided in the `x-fidel-signature` header.

##### Example Javascript implementation

```javascript
/**
  fidelHeaders - x-fidel-signature and x-fidel-timestamp headers
  payload - request payload (body)
  secret - webhook secretKey
  url - webhook URL
*/
function isSignatureValid(fidelHeaders, payload, secret, url) {
  function base64Digest(s) {
    return crypto.createHmac('sha256', secret)
      .update(s)
      .digest('base64');
  }

  /** You might also want to check how much time has passed since the request has been sent */
  /** timestamp - UTC Unix Timestamp (milliseconds) */
  const timestamp = fidelHeaders['x-fidel-timestamp'];
  const content = JSON.stringify(payload) + url + timestamp;

  const signature = base64Digest(base64Digest(content));
  return fidelHeaders['x-fidel-signature'] === signature;
}
```

To prevent replay attacks where a valid payload and it's signature is intercepted and re-transmitted, you can use the `x-fidel-timestamp` header and confirm that the timestamp is not too old. We recommend you validate the requests in a 5 minute gap. In case of retries, a new signature and timestamp are generated for each new request.

<hr>

## Events

### Brand

`brand.consent` event is triggered when the brand consent is approved, in `test` mode it will happen immediately after brand creation and in `live` mode when the Brand User approves the consent.

```json
fileName:brand.consent
{
  "id": "e44a9220-5b46-42cf-a944-31f0674bf8f8",
  "accountId": "1a269963-de11-4ab5-b82d-638a324ff09e",
  "consent": true,
  "created": "2018-10-19T13:29:40.922Z",
  "live": true,
  "name": "Starbucks",
  "updated": "2018-01-20T13:29:40.922Z"
}
```

### Program
`program.status` event is triggered when the program status is updated.

```json
fileName:program.status
{
	"id": "f67b7b16-0e3d-457a-811a-cc21dc8448f3",
	"accountId": "77aa1f5f-7152-45dc-8eb1-cb83355e9cf9",
	"active": true,
	"activeDate": "2018-10-28T09: 33: 30.139Z",
	"created": "2018-10-26T17: 10: 02.509Z",
	"live": true,
	"name": "Starbucks Card Linked",
	"syncStats": {
	  "status": "updating"
	},
	"updated": "2018-10-30T16: 12: 15.604Z"
}
```

### Card
There are two webhooks for card linking if you want to receive the response in your server side instead of client side when using the SDK callbacks.

`card.linked` event is triggered when a card is successfully linked.
```json
fileName:card.linked
{
	"id": "556f8179-bfdb-4274-852e-ba6e43559590",
	"accountId": "d346d574-d5c2-4a0e-8e02-ffd813fd1a8d",
	"countryCode": "GBR",
	"created": "2018-10-29T17:01:05.013Z",
	"expDate": "2019-10-01T00:00:00.000Z",
	"expMonth": 10,
	"expYear": 2019,
	"firstNumbers": "444400",
	"lastNumbers": "4735",
	"live": false,
	"programId": "cc67a36a-bae5-4b91-80f0-1ba9a0851eb5",
	"scheme": "visa",
	"type": "visa",
	"updated": "2018-10-29T17:01:05.013Z"
}
```

`card.failed` event is triggered when card linking fails. This payload includes the event name and error message.

```json
fileName:card.failed
{
  "card": {
    "id": "c3730d4d-9915-4c51-831f-14b8aeeb56b7",
    "accountId": "d346c574-d5c2-4a0e-8e02-ffd713fd1a8d",
    "countryCode": "GBR",
    "created": "2018-10-29T17:34:54.693Z",
    "expDate": "2020-10-01T00:00:00.000Z",
    "expMonth": 10,
    "expYear": 2020,
    "firstNumbers": "528245",
    "lastNumbers": "6015",
    "live": true,
    "programId": "ca9885fc-1f31-4120-8cfe-4c28c3b6cfdb",
    "scheme": "mastercard",
    "type": "mastercard",
    "updated": "2018-10-29T17:34:56.380Z"
  },
  "event": "card.failed",
  "message": "Error linking card."
}
```

### Transaction

**Authorisation** transaction events are triggered while the customer is making the payment in-store in real-time (available on MasterCard and American Express). When a customer makes a payment with a linked Mastercard debit/credit card in an auth-enabled location, a `transaction.auth` event is triggered and the transaction object sent to your specified URL in real-time.

**Clearing** events are triggered when the transaction is settled, usually happens 24-48 hours after the payment has been made. For consistency, Fidel processes clearing transactions triggering the `transaction.clearing` webhook events daily at 12:00 UTC. Only one transaction is sent per event.

```json
fileName:transaction.auth
{
  "id": "7fdfd5d8-9589-402f-8477-4a727ad239a2",
  "accountId": "4ed4b62b-aa4c-43a1-8064-da6d1368e17a",
  "programId": "6e38aa0c-b7ef-46bd-b1bd-c07c648d9cba",
  "datetime": "2019-03-12T19:12:01",
  "created": "2019-03-12T19:12:01.744Z",
  "updated": "2019-03-12T19:12:01.744Z",
  "offer": null,
  "auth": true,
  "cleared": false,
  "amount": 100,
  "currency": "GBP",
  "wallet": null,
  "card": {
    "id": "bc538b71-31c5-4699-840a-6d4a08693314",
    "firstNumbers": "555500",
    "lastNumbers": "5001",
    "scheme": "visa",
    "metadata": {
      "name": "fancy card",
      "id": "card-id"
    }
  },
  "brand": {
    "id": "9d136f2e-df99-4a08-a0a5-3bc1534b7db9",
    "name": "Bob's Cafe",
    "logoUrl": null
  },
  "location": {
    "id": "7a916fbd-70a0-462f-8dbc-bd7dbfbea160",
    "address": "2 Soho Square",
    "city": "London",
    "postcode": "W1D3PX",
    "countryCode": "GBR",
    "timezone": "Europe/London",
    "geolocation": {
      "latitude": 51.5152346,
      "longitude": -0.1310718
    },
    "metadata": {
      "name": "nice place",
      "id": "private-location-id"
    }
  },
  "identifiers": {
    "MID": "merchant ID",
    "mastercardTransactionSequenceNumber": "0000000000000",
    "mastercardRefNumber": "AABBCCDDE",
    "amexApprovalCode": "AA00BB",
    "visaAuthCode": "000000"
  }
}
```

We are working to extend the list of events. If you require any specific event that is not available yet please get in touch on our [Slack channel](https://fidel.uk/join-us-on-slack/) or email at [developer@fidel.uk](mailto:developer@fidel.uk).
