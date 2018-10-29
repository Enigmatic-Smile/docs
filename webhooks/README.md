## Webhooks
Webhooks are used to send transactional data and other relevant events to your server. For example, when a customer makes a payment with a linked Mastercard, a `transaction.auth` is sent in real-time to the specified webhooks linked to that program. Check below the webhook events available.

<h5>Webhooks events</h5>

![Webhooks diagram](https://docs.fidel.uk/assets/images/webhooks_diagram.png "Webhooks diagram")

<br/>

# Brand
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
	"updated": "2018-01-20T13:29:40.922Z",
}
```

# Program
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

# Card
There are two webhooks for card linking if you want to receive the response in your server side instead of client side when using the SDK callbacks.

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
	"updated": "2018-10-29T17:01:05.013Z",
}
```

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
    "updated": "2018-10-29T17:34:56.380Z",
  },
  "event": "card.failed",
  "message": "Error linking card."
}
```


# Transaction

**Authorisation** transaction events are triggered while the customer is making the payment in-store in real-time (available on MasterCard and American Express). When a customer makes a payment with a linked MasterCard debit/credit card in an auth-enabled location, a `transaction.auth` event is triggered and the transaction object sent to your specified URL in real-time.


<br/>

**Clearing** events are triggered when the transaction is settled, usually happens 24-48 hours after the payment has been made. For consistency, FIDEL API processes clearing transactions triggering the `transaction.clearing` webhook events daily at 12:00 UTC. Only one transaction is sent per event.

<br/>

We are looking to extend  the list of events in the future. If you require any specific event that is not available yet please speak with us on the [Slack channel](https://fidel-developers-slack-invites.herokuapp.com/) or email us at [developer@fidel.uk](mailto:developer@fidel.uk).

<br/>

# Create Webhook

To create a new webhook, go to **Webhooks** page on the dashboard, click **Add Webhook**, enter the URL where you want to receive the event's payload, select the program and the event. From the **Webhhoks** page you can also delete and edit any of the existing webhooks.

