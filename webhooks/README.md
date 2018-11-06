## Webhooks
Fidel uses webhooks to notify your application when relevant events happen in your account. This functionality can be used to receive data from events not triggered by direct API requests or to receive the data in a service that is not responsible for making the API request but needs to consume the response.

We will notify your registered webhook URLs as the event happens. For example, when a customer makes a payment with a linked Mastercard on a participating location, a `transaction.auth` event is sent in real-time to the specified webhook URL with the transactional data in the request payload.

### Creating webhooks
Webhooks can be created in the dashboard's webhooks page or by using the [Webhooks API](https://reference.fidel.uk/v1/reference#create-webhook-brand).
You can create up to five webhook URLs for the same event in the same Program. Fidel sends the data via HTTP POST request and will send test or live events depending on your dashboard's environment setting or if you are using test or live API keys. 

Fidel only accepts HTTPS URLs for webhooks endpoints. In order to create webhooks and receive event data, your server must be configured to support HTTPS with a valid certificate.

<div class="info-box">
    <small>Important note</small><br/>
    To confirm receipt of a webhook event, your server endpoint should return a `200` HTTP status code. Any other response will be treated as a failure and we retry the request three times over the next hour with exponential back off.
</div>

<hr>

# Authentication
To confirm that received events are being sent from Fidel we recommend verifying webhook signatures. That can be done by using the `x-fidel-signature` and `x-fidel-timestamp` HTTP headers. This isn't required, but offers an additional layer of security.

A unique secret key is generated for each webhook. The key is returned in the response's `secretKey` property if you are using the Webhooks API. You can also copy the key from the dashboard's webhooks page by clicking in the **Show Key** button next to your webhook endpoint. To verify a webhook request, generate a signature using the same key that Fidel uses and compare that to the value of the `x-fidel-signature` header.

1. Create a string concatenating the body of the request, the webhook URL and the timestamp value from the `x-fidel-timestamp` header.
2. Double hash the resulting string using the webhook key with HMAC-SHA256 and encode it in Base-64.
3. Compare the signature you generated with the signature provided in the `x-fidel-signature` header.

<br/>
<h5>Example Javascript implementation</h5>

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

<br/>

To prevent replay attacks where a valid payload and it's signature is intercepted and re-transmitted, you can use the `x-fidel-timestamp` header and confirm that the timestamp is not too old. We recommend you validate the requests in a 5 minute gap. In case of retries, a new signature and timestamp are generated for each new request.

<br/>
<hr>

# Events

### Brand
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
<hr>

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
<hr>

### Card
There are two webhooks for card linking if you want to receive the response in your server side instead of client side when using the SDK callbacks.

`card.linked` event is triggered when a card is successfully linked.
```json
fileName:card.linked
{
	"id": "556f8179-bfdb-4274-852e-ba6e43559590",
	"accountId": "d346d574-d5c2-4a0e-8e02-ffd813fd1a8d",
	"cardId": "c600972a-5ee5-42c4-8f42-f1a42a14a289",
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

<br/>

`card.failed` event is triggered when card linking fails. This payload includes the event name and error message.

```json
fileName:card.failed
{
  "card": {
    "id": "c3730d4d-9915-4c51-831f-14b8aeeb56b7",
    "accountId": "d346c574-d5c2-4a0e-8e02-ffd713fd1a8d",
    "cardId": "07c74d71-0e44-45fd-8311-f4e4c41e9e91",
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
<hr>

### Transaction

**Authorisation** transaction events are triggered while the customer is making the payment in-store in real-time (available on MasterCard and American Express). When a customer makes a payment with a linked MasterCard debit/credit card in an auth-enabled location, a `transaction.auth` event is triggered and the transaction object sent to your specified URL in real-time.

<br/>

**Clearing** events are triggered when the transaction is settled, usually happens 24-48 hours after the payment has been made. For consistency, FIDEL API processes clearing transactions triggering the `transaction.clearing` webhook events daily at 12:00 UTC. Only one transaction is sent per event.

```json
fileName:transaction.auth
{
  "id": "ce86c82c-fd7b-4ca4-84cf-88e070dcb40f",
  "accountId": "38599064-f4e9-4cba-88b0-bac11d02a8f5",
  "address": "53 Frith Street",
  "amount": 100,
  "auth": 1,
  "brandId": "ce6ca867-53ed-4744-8262-fb53c515be1b",
  "cardId": "2826d007-ce92-418c-8c99-3c503e764e81",
  "city": "London",
  "cleared": false,
  "countryCode": "GBR",
  "created": "2018-10-19T12:12:00.000Z",
  "currency": "GBP",
  "date": "2018-10-17T10:10:00.000Z",
  "firstNumbers": "444400",
  "lastNumbers": "4010",
  "live": true,
  "locationId": "f9a14399-8f6a-4bbc-8f3e-80547ba5534f",
  "merchantId": "TEST_MID_ccf00f5b-3ab8-4a2b-8a42-5dc03e399667",
  "offer": null,
  "postcode": "W1D 4SN",
  "programId": "daf5a825-1124-4212-bbb7-51b6a519c4ac",
  "scheme": "mastercard",
  "time": "2018-10-17T10:10:00.000Z",
  "type": "mastercard",
  "updated": "2018-10-19T12:12:00.000Z",
  "wallet": "undefined"
}
```
<br/>

We are working to extend the list of events. If you require any specific event that is not available yet please get in touch on our [Slack channel](https://fidel.uk/join-us-on-slack/) or email at [developer@fidel.uk](mailto:developer@fidel.uk).

<br/>


