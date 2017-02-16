## Webhooks
**FIDEL API** uses webhooks to send transactional data and other relevant events to your server. For example, when a customer successfully links a card through the web form or mobile SDKs, we create a `card.link.success` event that will post the created card object to a URL specified by you.
Currently, we have two groups of events, transaction and card related events. Under transactions events we have authorisation and clearing.

<h5>Webhooks event structure.</h5>

![Webhooks diagram](/assets/images/webhooks-diagram.png "Webhooks diagram")

<br/>

# Transaction Events

#### Authorisation

**Authorisation events** are triggered while the customer is making the payment in-store in real-time (currently authorisation events only available on MasterCard). When a customer makes a payment with a linked MasterCard debit/credit card in a program location, a `transaction.auth` event is triggered and the transaction object sent to your specified URL in real-time.

<br/>

#### Clearing

**Clearing events** are triggered when the transaction is settled, usually happens 24-48 hours after the payment has been made.

Under clearing there are two separate events, `transaction.clearing.success` and `transaction.clearing.update`. The success event is triggered when the transaction is successfully settled (e.g. an authorisation was sent to charge the card for £5 and the same amount was successfully charged from the card). On the other hand, the update event is triggered when the clearing amount does not match the authorisation amount (e.g. the card was authorised for £5 but it was only charged £2). In this case, a `transaction.clearing.update` is triggered upon receiving the settled transaction with the final amount charged.

<br/>

# Card Events
There are four card related events that you can be notified about. Two are triggered when the customer is linking the card on the web or mobile. The other two when a linked card is about to expire or is already expired.

When the user links the card by sending the card number and expiry date, FIDEL API tokenises and sends this information directly to Visa or MasterCard depending on the card type. If the card is successfully linked, a `card.link.success` event is triggered and the newly created card object sent to the specified URL. If an error occurs, a `card.link.error` is sent instead.

You can be notified when a card is one month from expiring by creating a `card.link.expiring` webhook. Also, you can be notified of all linked card that have expired by creating a `card.link.expired` webhook. The expiry webhooks are scheduled to run daily at 12:00pm UTC.

We are looking to extend  the list of events in the future. If you require any specific event that is not available yet please speak with us on the ~Slack channel~ or email us at developer@fidel.uk.

<br/>

# Create Webhook

Work in progress...

<h5>Create webhook image</h5>

<br/>

___

### Help?
If you have any questions please contact us by [email](mailto:developer@fidel.uk) or talk with our developers in the [Slack channel](fidel.uk).

<br/>
