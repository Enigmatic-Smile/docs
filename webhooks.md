# Webhooks

Fidel API uses [webhooks](https://en.wikipedia.org/wiki/Webhook) to notify your application when relevant events happen in your account across multiple resources, namely with event types such as `brand.consent`, `card.failed`, `card.linked`, `location.status`, `program.status`, `transaction.auth.qualified`, `transaction.auth`, `transaction.clearing.qualified`, `transaction.clearing`, `transaction.refund.qualified`, `transaction.refund`, `reimbursement.pending`, `reimbursement.issued`, `reimbursement.failed` and `credits.balance.low`.

Fidel API will notify your registered webhook URLs as the event happens, via a HTTP POST request with a signature header for verification, which needs to be received and acknowledged in a timely manner. The HTTP request contains the event object as payload.

For example, when a customer makes a payment with a linked Mastercard card, on a participating location, a `transaction.auth` event is sent in real-time to the specified webhook URL, with a payload that contains the Transaction object.

## Management and behavior

There are two ways you can manage your webhooks, i.e., view, create, update, delete, with the Fidel API. You can create them in the [Fidel Dashboard, under the "Webhooks" page](https://dashboard.fidel.uk/webhooks) or make HTTP requests using the [Webhooks API](https://reference.fidel.uk/reference/create-webhook-brand).

As requirements for creation, Fidel API only accepts HTTPS URLs for webhook endpoints, thus your server must support HTTPS and have a valid certificate.

Fidel API sends the data via HTTP POST in JSON format. It will send test events if your Dashboard is in test mode or if you are using test API keys when registering the webhook URLs. To receive live events, flip your switch on the Dashboard to go live, or create the webhooks using a live API key.

Fidel API has two types of webhooks (brand and program-related), both of which work similarly, but have slightly different requirements to be registered. For customization, we also allow additional HTTP headers to be added to help integrating with your systems.

### Brand-related webhooks

The `brand.consent` webhook only requires an `URL` to register. You can use the [Hooks](https://reference.fidel.uk/reference/create-webhook-brand) endpoint for registering brand webhooks.

Here's an example on how to create one using the Fidel API, with `example.com` as the URL:

```sh
curl -X POST \
  https://api.fidel.uk/v1/hooks \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "event": "brand.consent",
    "url": "https://example.com"
  }'
```

### Program-related webhooks

Program webhooks require a `programId` to be associated with and a `URL` to register. You can register up to 10 webhook URLs per event type for each program. You can use the [Program Hooks](https://reference.fidel.uk/reference/create-webhook-program) endpoint for registering program webhooks. The events that can be registered for a given program are `card.linked`, `card.failed`, `program.status`, `location.status`, `transaction.auth`, `transaction.auth.qualified`, `transaction.clearing`, `transaction.clearing.qualified`, `transaction.refund` and `transaction.refund.qualified`.

Here's an example on how to create a webhook on a Program for the `transaction.auth` event, with `example.com` as the URL:

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "event": "transaction.auth",
    "url": "https://example.com"
  }'
```

## Acknowledging reception

To confirm receipt of a webhook event, your server endpoint should return a <code>200 OK</code> HTTP status code. Any other response, or not providing any response within 20 seconds will be treated as a failure and our system will retry sending the request twice (i.e. three tries in total), with one-minute wait on the second request and two-minute wait on the third (last) attempt.

To avoid timeouts, it is recommended to not run complex and time-consuming logic upon reception of the webhook in order to provide a response back and thus avoid unnecessary retries and potentially duplicate processing of the events.

## Custom request headers

Fidel API allows you to define custom HTTP headers when you register a webhook URL. The custom headers are included in the HTTP POST request headers that are sent to your application.

Custom headers can be defined when creating a new webhook in the Dashboard or by using the [Webhooks API](https://reference.fidel.uk/v1/reference/create-webhook-program) and setting the optional `headers` object with a key-value pair (see example below).

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "event": "transaction.auth",
    "url": "https://example.com",
    "headers": {
      "Custom-Header": "my-custom-header"
    }
  }'
```

To delete custom headers from a registered webhook, use the [Update Hooks](https://reference.fidel.uk/reference/update-webhook) endpoint and send an empty `headers` object.

```sh
curl -X PUT \
  https://api.fidel.uk/v1/hooks/3b4be60b-6596-4b40-ae3d-89b9fdaf132a \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "event": "transaction.auth",
    "url": "https://example.com",
    "headers": {}
  }'
```

A maximum of 5 custom headers per webhook can be defined, and they need to follow strict character validation patterns. The key name must be between 1 and 64 characters and only accepts the Roman alphabet, numbers, dashes and underscores. The value must be between 1 and 1000 characters. Additionally, the HTTP reserved headers are blocklisted and cannot be used for the key name. The full list of blocklisted key names:

```json
[
  "Accept-Charset",
  "Accept-Datetime",
  "Accept-Encoding",
  "Accept-Language",
  "Accept",
  "Access-Control-Request-Headers",
  "Access-Control-Request-Method",
  "Cache-Control",
  "Connection",
  "Content-Length",
  "Content-Type",
  "Cookie",
  "Date",
  "Expect",
  "Fidel-Account",
  "Fidel-Key",
  "Fidel-Live",
  "Fidel-Request-Id",
  "Fidel-User",
  "Forwarded",
  "From",
  "Host",
  "If-Match",
  "If-Modified-Since",
  "If-None-Match",
  "If-Range",
  "If-Unmodified-Since",
  "Max-Forwards",
  "Origin",
  "Pragma",
  "Proxy-Authorization",
  "Range",
  "Referer",
  "TE",
  "Transfer-Encoding",
  "Upgrade",
  "User-Agent",
  "Via",
  "Warning",
  "X-Fidel-Signature",
  "X-Fidel-Timestamp"
]
```

### Brand

A `brand.consent` event is triggered when Brand consent is “Approved”. In the `test` environment, it will immediately trigger after you create a Brand. In the `live` environment, it will trigger when the Brand User approves the consent request.

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
  "logoURL": "https://example.com/"
}
```

### Program

A `program.status` event is triggered in the `live` environment when there are updates for each Location in a Program. In the `test` environment, this webhook doesn't trigger because there is no concept of Location syncing there.

```json
fileName:program.status
{
  "id": "f67b7b16-0e3d-457a-811a-cc21dc8448f3",
  "accountId": "77aa1f5f-7152-45dc-8eb1-cb83355e9cf9",
  "active": true,
  "activeDate": "2018-10-28T09: 33: 30.139Z",
  "created": "2018-10-26T17: 10: 02.509Z",
  "live": true,
  "name": "Starbucks",
  "syncStats": {
    "created": "2020-04-23T11:10:40.054Z",
    "locations": 2,
    "estimatedEndDate": "2020-05-07T11:10:40.054Z",
    "status": "syncing"
  },
  "updated": "2018-10-30T16: 12: 15.604Z"
}
```

### Location

A `location.status` event is triggered when there are updates from a card network for a Location in a Program. In the `test` environment, this webhook triggers three times for each location upon creating a Location, once for each card scheme, with a `location.active` event. In the `live` environment, this would trigger whenever a location has synced successfully for a card scheme, with a `location.active` event. Or whenever a location has failed to sync for a card scheme, with a `location.failed` event.

```json
fileName:location.status
{
    "location": {
        "city": "London",
        "timezone": "Europe/London",
        "mastercard": {
            "estimatedActivationDate": null,
            "clearingTransactionId": null,
            "auth": false,
            "authTransactionId": null,
            "clearing": false,
            "status": "inactive"
        },
        "countryCode": "GBR",
        "activeDate": "2020-08-18T13:40:11.406Z",
        "currency": "GBP",
        "id": "298d9c88-cabe-4583-a54b-574e29b57c84",
        "live": false,
        "address": "1 Main Street",
        "created": "2020-08-18T13:40:11.406Z",
        "postcode": "W1NN3R",
        "searchBy": {
            "merchantIds": {
                "visa": [],
                "mastercard": []
            }
        },
        "accountId": "36081095-2782-4669-8a07-857bbaaeb89b",
        "amex": {
            "estimatedActivationDate": null,
            "clearingTransactionId": null,
            "auth": false,
            "authTransactionId": null,
            "clearing": false,
            "status": "active"
        },
        "visa": {
            "estimatedActivationDate": null,
            "clearingTransactionId": null,
            "auth": false,
            "authTransactionId": null,
            "clearing": false,
            "status": "inactive"
        },
        "brandId": "9cd32c61-43ca-4bb7-8aca-0cf491112c28",
        "preonboard": false,
        "updated": "2020-08-18T13:40:11.406Z",
        "programId": "f2c9719a-6433-4ef4-8401-19d7ebf60ab9",
        "geolocation": {
            "latitude": 51.5138332,
            "longitude": -0.1318224
        }
    },
    "scheme": "amex",
    "status": "location.active"
}
```

### Card

There are two card-related events available: `card.linked` and `card.failed`. They are useful for linking a card if you want to receive the response via your server instead of via the client when using the SDK callbacks.

A `card.linked` event is triggered when a card is successfully linked.

```json
fileName:card.linked
{
  "id": "556f8179-bfdb-4274-852e-ba6e43559590",
  "accountId": "d346d574-d5c2-4a0e-8e02-ffd813fd1a8d",
  "countryCode": "GBR",
  "created": "2018-10-29T17:01:05.013Z",
  "expDate": "2019-10-01T00:00:00.000Z",
  "expMonth": 10,
  "expYear": 2020,
  "firstNumbers": "444400",
  "lastNumbers": "4735",
  "live": false,
  "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
  "scheme": "visa",
  "type": "visa",
  "updated": "2018-10-29T17:01:05.013Z"
}
```

A `card.failed` event is triggered when card linking fails. This payload includes the event name and error message.

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
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "scheme": "mastercard",
    "type": "mastercard",
    "updated": "2018-10-29T17:34:56.380Z"
  },
  "event": "card.failed",
  "message": "Error linking card."
}
```

### Transaction

A `transaction.auth` or **authorisation** transaction event is triggered when a transaction is carried out on a linked card. For example, when a customer is making a payment in-store in real-time. When a customer makes a payment with a linked debit/credit card in an auth-enabled location, the `transaction.auth` webhook is triggered and the transaction object sent to your specified URL in real-time.

```json
fileName:transaction.auth
{
  "id": "7fdfd5d8-9589-402f-8477-4a727ad239a2",
  "accountId": "4ed4b62b-aa4c-43a1-8064-da6d1368e17a",
  "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
  "datetime": "2019-03-12T19:12:01",
  "created": "2019-03-12T19:12:01.744Z",
  "updated": "2019-03-12T19:12:01.744Z",
  "offer": null,
  "approvalCode": "AA00BB",
  "auth": true,
  "authCode": "A73H890",
  "cardPresent": true,
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
    "logoURL": null,
    "metadata": null,
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
      "id": "your-user-id",
      "name": "username"
    }
  },
  "identifiers": {
    "MID": "12345678",
    "mastercardAuthCode": null,
    "mastercardTransactionSequenceNumber": "0000000000000",
    "mastercardRefNumber": "AABBCCDDE",
    "amexApprovalCode": "AA00BB",
    "visaAuthCode": "A73H890"
  }
}
```

A `transaction.clearing` or **clearing** transaction event is triggered when a transaction is settled, usually happens 48 to 72 hours after a payment is made. The Fidel processes for clearing transactions and triggering the `transaction.clearing` webhook events run daily at 12:00 UTC for Mastercard and multiple times per day for Visa and American Express. Only one transaction is sent per event.

```json
fileName:transaction.clearing
{
    "approvalCode": "AA00BB",
    "auth": true,
    "authCode": "A73H890",
    "offer": null,
    "currency": "GBP",
    "id": "8bbbf56b-3819-473b-877d-cf2175f268f4",
    "amount": 10,
    "wallet": null,
    "created": "2020-07-08T17:05:52.567Z",
    "accountId": "36081095-2782-4669-8a07-857bbaaeb89b",
    "cardPresent": false,
    "cleared": true,
    "updated": "2020-07-08T17:20:44.134Z",
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "datetime": "2020-07-08T18:05:52",
    "card": {
        "id": "62744670-f935-4ba3-8e89-be23e31292cf",
        "firstNumbers": "444400",
        "lastNumbers": "4305",
        "scheme": "visa"
    },
    "location": {
        "address": "5 Main St",
        "city": "Bristol",
        "countryCode": "GBR",
        "id": "0ff99cfd-9e8b-48fe-af55-2519ffe1c0a4",
        "geolocation": null,
        "postcode": "BS2 5BL",
        "timezone": "Europe/London",
        "metadata": null
    },
    "brand": {
        "id": "966481e7-dd6f-44d2-83fa-98783aaacf40",
        "name": "Bob's Cafe",
        "logoURL": null,
        "metadata": null
    },
    "identifiers": {
        "MID": "TEST_MID_d74bc1ca-cd6e-409d-ba8e-549a214dfb0a",
        "mastercardTransactionSequenceNumber": null,
        "mastercardRefNumber": null,
        "mastercardAuthCode": null,
        "amexApprovalCode": "AA00BB",
        "visaAuthCode": "A73H890"
    }
}
```

A `transaction.refund` or **refund** transaction event is triggered when a transaction is refunded. A refunded transaction also triggers a **cleared** event, with the `auth` property set to false. The amount on both events is negative. Fidel API tries to identify the initial transaction for which the refund was issued, using `cardId`, `locationId`, `merchantId`, `amount` and `datetime`. If an associated initial transaction is identified, the webhook data contains the `originalTransactionId`. If no initial transaction is identified, the data comes in on both webhooks with a negative amount but no `originalTransactionId` property.

```json
fileName:transaction.refund
{
    "approvalCode": "AA00BB",
    "auth": false,
    "authCode": "A73H890",
    "originalTransactionId": "8bbbf56b-3819-473b-877d-cf2175f268f4",
    "currency": "GBP",
    "id": "5ec08ca8-38c6-42e1-9fa5-32c67e4135b2",
    "amount": -10,
    "wallet": null,
    "created": "2020-07-08T17:23:13.972Z",
    "accountId": "36081095-2782-4669-8a07-857bbaaeb89b",
    "cardPresent": false,
    "cleared": true,
    "updated": "2020-07-08T17:23:13.972Z",
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "datetime": "2020-07-08T18:23:13",
    "card": {
        "id": "62744670-f935-4ba3-8e89-be23e31292cf",
        "firstNumbers": "444400",
        "lastNumbers": "4305",
        "scheme": "visa"
    },
    "location": {
        "address": "5 Main St",
        "city": "Bristol",
        "countryCode": "GBR",
        "id": "0ff99cfd-9e8b-48fe-af55-2519ffe1c0a4",
        "geolocation": null,
        "postcode": "BS2 5BL",
        "timezone": "Europe/London",
        "metadata": null
    },
    "brand": {
        "id": "966481e7-dd6f-44d2-83fa-98783aaacf40",
        "name": "Bob's Cafe",
        "logoURL": null,
        "metadata": null
    },
    "identifiers": {
        "MID": "TEST_MID_d74bc1ca-cd6e-409d-ba8e-549a214dfb0a",
        "mastercardTransactionSequenceNumber": null,
        "mastercardRefNumber": null,
        "amexApprovalCode": null,
        "mastercardAuthCode": null,
        "amexApprovalCode": "AA00BB",
        "visaAuthCode": "A73H890"
    }
}
```

There are three additional transaction webhook events: `transaction.auth.qualified`, `transaction.clearing.qualified` and `transaction.refund.qualified`. They are triggered when an `auth`, `clearing` or a `refund` transaction is qualified for an Offer.

`transaction.auth.qualified` and `transaction.clearing.qualified` events are triggered only if transactions they are inside of the offer's period and pass the set of rules defined in the offer. `transaction.refund.qualified` events might be emitted even when the offer has expired due to the complex matching logic of the refund to the original transaction.

The payload for these events includes the `offer` object with the results of the qualification, and you can read more about it in the [Offers API](/offers/#transaction-qualification) documentation.

```json
"offer": {
            "qualified": true,
            "id": "eeefb94b-d11c-44db-81d7-d86a9fcc4069",
            "message": [],
            "qualificationDate": null,
            "cashback": 2.5,
            "performanceFee": 0.3
         }
```

You can filter transactions from a specific offer by setting the `offerId` optional property when creating a transaction qualification webhook event.

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "event": "transaction.auth.qualified",
    "url": "https://example.com",
    "offerId": "cf22478e-c700-4f31-b75b-38016605e2a3"
  }'
```

`reimbursement.pending`, `reimbursement.issued`, `reimbursement.failed` events are triggered when a reimbursement changes state. The `reimbursement.pending` when the new state is `pending`, `reimbursement.issued` when a new state is `issued`, and `reimbursement.failed` when a new state is `failed`

The payload for these events includes the `reimbursement` object with the up-to-date status. Read more about it in the [Reimbursement page](/reimbursement/#webhook).

```json
{
  "amount": 2.55,
  "created": "2021-09-30T11:11:11.000Z",
  "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
  "currency": "USD",
  "description": "Earned Stars",
  "status": "issued",
  "id": "6c01f956-1f0f-413f-a5db-d1fc8a59ef92",
  "updated": "2021-09-30T11:12:11.000Z",
  "transactionId": "5ec08ca8-38c6-42e1-9fa5-32c67e4135b2",
  "programId": "4a9cef62-dc79-4044-b7aa-34425f753830"
}
```

### Credits

Fidel Credits can be used to reimburse cardholders. Learn more about Fidel Credits on the [Reimbursement page](/reimbursement/#credits) A `credits.balance.low` event is sent when a currency's credit balance drops below 25% of the amount at the time of the previous credits purchase. The payload for these events is the following:

```json
{
  "accountId": "61741c3-3dc9-45f5-8e7c-db1dd649afab",
  "balances": {
    "AUD": 0,
    "CHF": 0,
    "JPY": 0,
    "EUR": 0,
    "GBP": 0,
    "CAD": 0,
    "USD": 249,
    "NZD": 0
  },
  "lastTotalAmount": {
    "AUD": 0,
    "CHF": 0,
    "JPY": 0,
    "EUR": 0,
    "GBP": 0,
    "CAD": 0,
    "USD": 1000,
    "NZD": 0
  },
  "lowCreditsNotificationSent": {
    "AUD": false,
    "CHF": false,
    "JPY": false,
    "EUR": false,
    "GBP": false,
    "CAD": false,
    "USD": true,
    "NZD": false
  }
}
```

## Signatures

If you want to confirm that incoming requests on your webhook URL are coming from the Fidel API, we recommend verifying webhook signatures. We send the `x-fidel-signature` and `x-fidel-timestamp` HTTP headers for each request we make to a webhook URL.

Fidel API generates a unique secret key for each webhook you register. The key is returned in the response's `secretKey` property if you are using the Webhooks API. You can also copy the key from the Fidel Dashboard's Webhooks page by clicking in the **Show Key** button next to your webhook endpoint. To verify a webhook request, generate a signature using the same key that the Fidel API uses and compare that to the value of the `x-fidel-signature` header.

Replay attacks are a common MITM attack vector where a valid payload and its signature is intercepted and re-transmitted. If you want to safeguard against them, you can use the `x-fidel-timestamp` header and confirm that the timestamp is not too old. We recommend you validate the requests in a 5-minute gap. In the case of retries, a new signature and timestamp are generated for each retry request.

The valuation/verification can be conducted as follows:

1. Create a string concatenating the body of the request, the webhook URL and the timestamp value from the `x-fidel-timestamp` header.
2. Double hash the resulting string using the webhook `secretKey` with HMAC-SHA256 and encode it in Base-64.
3. Compare the signature you generated with the signature provided in the `x-fidel-signature` header.

### Example JavaScript implementation

```javascript
/**
  fidelHeaders - x-fidel-signature and x-fidel-timestamp headers
  payload - request payload (body)
  secret - webhook secretKey
  url - webhook URL
*/
function isSignatureValid(fidelHeaders, payload, secret, url) {
  function base64Digest(s) {
    return crypto.createHmac("sha256", secret).update(s).digest("base64");
  }

  /** You can check how much time has passed since the request has been sent */
  /** timestamp - UTC Unix Timestamp (milliseconds) */
  const timestamp = fidelHeaders["x-fidel-timestamp"];
  const content = JSON.stringify(payload) + url + timestamp;

  const signature = base64Digest(base64Digest(content));
  return fidelHeaders["x-fidel-signature"] === signature;
}
```

## Deleting Webhooks

You can delete webhooks in the [Fidel API Dashboard](https://dashboard.fidel.uk/webhooks), or using the API's [Delete Webhook](https://reference.fidel.uk/reference/delete-webhook) endpoint.

```sh
curl -X DELETE \
  https://api.fidel.uk/v1/hooks/b9ef3795-a38f-4ef2-8d8d-293dd7fbe1a7 \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63'
```

## API Reference

If you're looking to find out more about our Webhooks API and how to use it with your application, please visit the [Fidel API Reference](https://reference.fidel.uk/reference/create-webhook-brand).
