## Webhooks
**FIDEL API** uses webhooks to send transactional data and other relevant events to your server. For example, when a customer makes a payment with a linked Mastercard, a `transaction.auth` is sent in real-time to the specified webhooks linked to that program. Currently, there are two groups of events, transaction and card related events.

<h5>Webhooks events</h5>

![Webhooks diagram](https://docs.fidel.uk/assets/images/webhooks-diagram.png "Webhooks diagram")

<br/>

# Brand Events

`brand.consent` event is triggered when the brand consent is approved, in `test` mode it will happen immediately after brand creation and in `live` mode when Fidel verifies the merchant consent.

<br/>

# Program Events

`program.status` event is triggered when either program becomes `active` or program `sync` status change.

<br/>

# Transaction Events

#### Authorisation

**Authorisation events** are triggered while the customer is making the payment in-store in real-time (currently authorisation events are only available on MasterCard). When a customer makes a payment with a linked MasterCard debit/credit card in a program location, a `transaction.auth` event is triggered and the transaction object sent to your specified URL in real-time.

<br/>

#### Clearing

**Clearing events** are triggered when the transaction is settled, usually happens 24-48 hours after the payment has been made. For consistency, FIDEL API processes Visa and Mastercard cleared transactions triggering the `transaction.clearing` webhook events daily at 12:00 UTC.

<br/>

# Card Events
We are working on a few card events to give you more data about the card lifecycle.

You can be notified when a card is one month from expiring by creating a `card.link.expiring` webhook. Also, you can be notified of all expired cards by creating a `card.link.expired` webhook. These events will be triggered monthly on the first day of the month so you can notify your users to update their cards to continue to receive rewards.

We are looking to extend  the list of events in the future. If you require any specific event that is not available yet please speak with us on the [Slack channel](https://fidel-developers-slack-invites.herokuapp.com/) or email us at [developer@fidel.uk](mailto:developer@fidel.uk).

<br/>

# Create Webhook

To create a new webhook, go to **Webhooks** page on the dashboard, click **Add Webhook**, enter the URL where you want to receive the event's payload, select the program and the event. From the **Webhhoks** page you can also delete and edit any of the existing webhooks.
