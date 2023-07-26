# Webhooks

Fidel API uses [webhooks](https://en.wikipedia.org/wiki/Webhook) to automatically notify your application when relevant events occur in your account. You can utilize this functionality to receive data from events not triggered by direct API requests, or to access the data in a service that is not responsible for making the API request but needs to consume the response.

There are several webhooks available to use with Fidel API, each corresponding to an event happening in your account: `card.linked`, `card.failed`, `card.data.sharing.started`, `card.data.sharing.ended`, `card.verification.started`, `card.verification.time.exhausted`, `card.verification.failed`, `transaction.auth`, `transaction.clearing`, and `transaction.refund`. Each of them requires an HTTPS URL to be registered.

Fidel API will notify your registered webhook URLs as the event happens via a HTTP POST request. The HTTP request payload contains the event object. For example, when a customer makes a payment with a linked card, a `transaction.auth` event is sent in real time to the specified webhook URL. The HTTP request payload contains the transaction object.

## Creating Webhooks

Fidel API only accepts HTTPS URLs for webhook endpoints. Your webhook server must support HTTPS and have a valid certificate.

##### Return a 200 status code

To confirm receipt of a webhook event, your server endpoint should return a <code>200 OK</code>  HTTP status code. Any other response or not receiving any response within 20 seconds will be treated as a failure and the API will retry sending the request twice (i.e. three tries in total), with one-minute wait on the second request and two-minute wait on the third (last) attempt. You can see an example implementation for handling webhooks in the [sample application on GitHub](https://github.com/FidelLimited/fidel-api-sample-app/blob/main/server/routes/webhooks.js).

Fidel API sends the data via HTTP POST in JSON format. It will send test events if your Dashboard is in test mode or if you are using test API keys when registering the webhook URLs. To receive live events, get in touch with Fidel API's support team or create the webhooks using a live API key.

There are two ways you can register webhooks with Fidel API. You can create them in the [Fidel API Dashboard, under the "Webhooks" page](https://dashboard.fidel.uk/webhooks) or make HTTP requests using the [Webhooks API](https://reference.fidel.uk/reference/create-webhook-program).

Webhooks are created at the program level, hence they require a `programId` to be associated with and an URL to register. You can register up to ten webhook URLs per event type for each program. You can use the program hooks endpoint for registering program webhooks. The events that can be registered are `card.linked `, `card.failed`, `card.data.sharing.started`,  `card.data.sharing.ended`, `card.verification.started`, `card.verification.time.exhausted`, `card.verification.failed`,  `transaction.auth`, `transaction.clearing`, and `transaction.refund`.

Here's an example of how to create a webhook on a program for the `transaction.auth` event, with `example.com` as the URL:

```sh
curl -X POST \
https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
-H 'content-type: application/json' \
-H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
-d '{
    "event": "transaction.auth",
    "url": "https://example.com"
}'
```

## Custom Headers

Fidel API allows you to define custom HTTP headers when you register a webhook URL. The custom headers get added to the HTTP POST request headers that are sent to your application.

Custom headers can be defined when creating a new webhook in the Dashboard, or by using the [Webhooks API](https://reference.fidel.uk/reference/create-webhook-program) and setting the optional `headers` object with a key-value pair.

Here's an example that creates a webhook with a custom header using the [Program Hooks](https://reference.fidel.uk/reference/create-webhook-program) endpoint:

```sh
curl -X POST \
https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
-H 'Content-Type: application/json' \
-H 'Fidel-Key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
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
-H 'content-type: application/json' \
-H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
-d '{
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "event": "transaction.auth",
    "url": "https://example.com",
    "headers": {}
}'
```

A maximum of five custom headers per webhook can be defined, and they need to follow strict character validation patterns. The key name must be between 1 and 64 characters and only accepts the Roman alphabet, numbers, dashes and underscores. The value must be between 1 and 1000 characters. Additionally, the HTTP reserved headers are blocklisted and cannot be used for the key name. The full list of blocklisted key names:

```json
["Accept", "Accept-Charset", "Accept-Encoding", "Accept-Language", "Accept-Datetime", "Access-Control-Request-Method", "Access-Control-Request-Headers", "Cache-Control", "Connection", "Content-Length", "Content-Type", "Cookie", "Date", "Expect", "Forwarded", "From", "Host", "If-Match", "If-Modified-Since", "If-None-Match", "If-Range", "If-Unmodified-Since", "Max-Forwards", "Origin", "Pragma", "Proxy-Authorization", "Range", "Referer", "Te", "Transfer-Encoding", "User-Agent", "Upgrade", "Via", "Warning", "Fidel-Account", "Fidel-Key", "Fidel-Live", "Fidel-Request-Id", "Fidel-User", "X-Fidel-Signature", "X-Fidel-Timestamp"]
```

## Events

### Card Events

There are four card-related events available on the `Webhooks` API: `card.linked`, `card.failed`, `card.data.sharing.started` and `card.data.sharing.ended`. They are useful for linking a card if you want to receive the response via your server instead of via the client when using the SDK callbacks.

#### Card Event Lifecycle

Cards will always follow the following lifecycle

1. A `card.linked` or `card.failed` event will be triggered to communicate the result of any initial linking attempt. This is when 16-digit PAN is communicated to Fidel API and tokenised within our system. _Note: Data sharing has not yet started. _
1. On receipt of `card.linked` the card will have `"verificationStatus": "unverified"`
2. A card will progress through a lifecycle of Card Verification Events, commencing with `card.verification.started` when a call to [verify card consent](https://transaction-stream.fidel.uk/reference/verify-card-consent) is made via API or within our provided SDKs.
1. If the verification fails, due to attempt or time limits, the event lifecycle will restart when the card is re-enrolled. 
3. A `card.data.sharing.started` event will be triggered upon successful verification and indicates that transaction data events will now be communicated for the card. 
4. A `card.data.sharing.ended` event will be triggered whenever a card consent is revoked from the Fidel API platform to communicate to your system that it should no longer expect transactions for the card. The only cause of consent revocation today is through the `Delete Card` endpoint. 

#### Card Event Definitions and example messages

A `card.linked` event is triggered when a card is successfully linked.

```json
{
    "accountId": "144547ef-5106-45b4-8ca4-123456789012",
    "countryCode": "USA",
    "created": "2021-07-27T20:14:32.004Z",
    "expDate": "2022-10-31T23:59:59.999Z",
    "expMonth": 10,
    "expYear": 2022,
    "firstNumbers": "123456",
    "id": "f88d6484-b5be-4402-84e6-123456789012",
    "lastNumbers": "1234",
    "live": true,
    "programId": "3312c23b-97c1-4fab-aa63-123456789012",
    "scheme": "visa",
    "type": "visa",
    "updated": "2021-07-28T12:52:46.840Z",
    "verificationStatus": "unverified"
}
```

A `card.failed` event is triggered when card linking fails. This payload includes the event name and error message.

```json
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
        "scheme": "visa",
        "type": "visa",
        "updated": "2018-10-29T17:34:56.380Z"

    },
    "event": "card.failed",
    "message": "Error linking card."
}
```

A `card.data.sharing.started` event is triggered when Fidel API can process the card's transactions. This can happen immediately after the card has been linked or, if verification is required, as soon as the card has been verified.

```json
{
    "id": "556f8179-bfdb-4274-852e-ba6e43559590",
    "accountId": "d346d574-d5c2-4a0e-8e02-ffd813fd1a8d",
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "scheme": "visa",
    "countryCode": "GBR",
    "expMonth": 10,
    "expYear": 2028,
    "firstNumbers": "402360",
    "lastNumbers": "6015",
    "live": true,
    "occurrenceDate": "2023-03-29T17:01:05.013Z",
}
```

A `card.data.sharing.ended` event is triggered when Fidel API can no longer process the card's transactions.

```json
{
    "id": "556f8179-bfdb-4274-852e-ba6e43559590",
    "accountId": "d346d574-d5c2-4a0e-8e02-ffd813fd1a8d",
    "programId": "06471dbe-a3c7-429e-8a18-16dc97e5cf35",
    "live": true,
    "occurrenceDate": "2023-03-30T17:01:05.013Z",
}
```

### Card Verification Events

Card-verification related events are useful if you want to be notified on your server about the verification status of any card.

A `card.verification.started` is triggered when card verification is initiated after a request for Card Consent is successfully submitted. The card must be verified in order for Fidel API to start processing its transactions.

```json
{
    "context": {
        "consent": {
            "consentId": "4b52ce31-b00d-44a3-85db-710e50ac5b30",
            "cardId": "2d332191-d1dc-48b3-8ad6-08e91b80f41a",
            "programId": "a470491d-48a5-4763-bcb3-4953c391a440",
        }
    },
    "remainingVerificationAttempts": 3,
    "live": false,
    "occurrenceDate": "2023-03-02T17:56:04.874Z"
}
```

A `card.verification.time.exhausted` event is triggered when a card reaches 48 hours of unverified status and, as a consequence, it gets deleted. At this point, the card will have to be re-enrolled for Fidel API to start processing its transactions.

```json
{
    "context": {
        "consent": {
            "consentId": "4b52ce31-b00d-44a3-85db-710e50ac5b30",
            "created": "2023-02-28T16:56:04.874Z",
            "cardId": "2d332191-d1dc-48b3-8ad6-08e91b80f41a",
            "programId": "a470491d-48a5-4763-bcb3-4953c391a440",
        }
    },
    "live": false,
    "occurrenceDate": "2023-03-02T17:56:04.874Z"
}
```

A `card.verification.failed` event is triggered when all card verification attempts are exhausted. There is a maximum of three attempts to confirm the charged amount, after which the card will be blocked from further attempts. Blocked cards become available to be re-enrolled after 72h.

```json
{
    "context": {
        "consent": {
            "consentId": "4b52ce31-b00d-44a3-85db-710e50ac5b30",
            "cardId": "2d332191-d1dc-48b3-8ad6-08e91b80f41a",
            "programId": "a470491d-48a5-4763-bcb3-4953c391a440"
        }
    },
    "live": false,
    "occurrenceDate": "2023-03-02T17:56:04.874Z"
}
```

### Transaction Events

A `transaction.auth` or **authorization transaction event** is triggered when a transaction is registered on a linked card. For example, when a customer makes a payment with a linked debit/credit card in an auth-enabled location either in store or online, the `transaction.auth` webhook is triggered and the transaction object sent to your specified URL in real time.

```json
{
    "accountId": "4ca457bc-9092-4865-8108-c4c32ef8df7b",
    "amount": 10,
    "auth": true,
    "authCode": "207222",
    "billingAmount": 8,
    "billingCurrency": "EUR",
    "brand": {
        "name": "Custom Brand"
    },
    "card": {
        "firstNumbers": "444400",
        "id": "293c7f8f-157b-4236-8e57-bd3538bd1b01",
        "lastNumbers": "4001",
        "scheme": "visa"
    },
    "cleared": false,
    "created": "2022-05-24T14:09:37.321Z",
    "currency": "USD",
    "datetime": "2022-05-24T14:09:37",
    "id": "2e824ed5-8991-4c5a-be4c-51bc33e41bb7",
    "identifiers": {
        "amexApprovalCode": null,
        "mastercardAuthCode": null,
        "mastercardRefNumber": null,
        "mastercardTransactionSequenceNumber": null,
        "visaAuthCode": "207222"
    },
    "location": {
        "city": "New York",
        "countryCode": "USA",
        "postcode": "1234-567",
        "state": null
    },
    "merchantCategoryCode": "5812",
    "merchantDescriptor": "Purchase",
    "programId": "76b6bd16-f1db-4eef-8636-bf94b2442e40",
    "programType": "transaction-stream",
    "reimbursementEligible": false,
    "updated": "2022-05-24T14:09:37.321Z",
    "wallet": null
}

```

A `transaction.clearing` or **clearing transaction event** is triggered when a transaction is settled and usually occurs 48 to 72 hours after a payment is made. Fidel API's processes for clearing transactions and triggering the `transaction.clearing` webhook events run multiple times per day for Visa. Only one transaction is sent per event.

```json
{
    "accountId": "asdf1234-1234-1234-1234-asdfgh123456",
    "amount": 10,
    "auth": true,
    "authCode": "589531",
    "billingAmount": 8,
    "billingCurrency": "EUR",
    "brand": {
        "name": "Custom Brand"
    },
    "card": {
        "firstNumbers": "444400",
        "id": "asdf1234-12ab-123a-12ab-123456asdfgh",
        "lastNumbers": "4111",
        "metadata": {
            "fromSDK": false,
            "live": 0
        },
        "scheme": "visa"
    },
    "cleared": true,
    "created": "2022-06-07T09:51:42.249Z",
    "currency": "USD",
    "datetime": "2022-06-07T09:51:42",
    "id": "42adf0ef-bdde-436e-a035-3ce142b4f167",
    "identifiers": {
        "amexApprovalCode": null,
        "mastercardAuthCode": null,
        "mastercardRefNumber": null,
        "mastercardTransactionSequenceNumber": null,
        "visaAuthCode": "589531"
    },
    "location": {
        "city": "New York",
        "countryCode": "USA",
        "postcode": "1234-567",
        "state": null
    },
    "merchantCategoryCode": "5812",
    "merchantDescriptor": "Purchase",
    "programId": "76b6bd16-f1db-4eef-8636-bf94b2442e40",
    "programType": "transaction-stream",
    "refundTransactionId": "ebb29156-d3e6-433e-826e-a03745f50a9f",
    "reimbursementEligible": true,
    "updated": "2022-06-24T14:13:46.757Z",
    "wallet": null
}
```

A `transaction.refund` or **refund transaction event** is triggered when a transaction is refunded. A refunded transaction also triggers a cleared event, with the `auth` property set to false. The amount on both events is negative. Fidel API tries to identify the initial transaction for which the refund was issued using `cardId`, `locationId`, `merchantId`, `amount` and `datetime`. If an associated initial transaction is identified, the webhook data contains the `originalTransactionId`. If no initial transaction is identified, the data comes in on both webhooks with a negative amount but no `originalTransactionId` property.

```json
{
    "accountId": "4ca457bc-9092-4865-8108-c4c32ef8df7b",
    "amount": -10,
    "auth": false,
    "authCode": "595677",
    "billingAmount": 8,
    "billingCurrency": "EUR",
    "brand": {
        "name": "Custom Brand"
    },
    "card": {
        "firstNumbers": "444400",
        "id": "7185bab5-06be-443b-89aa-a5da9a8dbc01",
        "lastNumbers": "4111",
        "metadata": {
            "fromSDK": false,
            "live": 0
        },
        "scheme": "visa"
    },
    "cleared": true,
    "created": "2022-06-07T09:56:24.052Z",
    "currency": "USD",
    "datetime": "2022-06-07T09:56:23",
    "id": "ebb29156-d3e6-433e-826e-a03745f50a9f",
    "identifiers": {
        "amexApprovalCode": null,
        "mastercardAuthCode": null,
        "mastercardRefNumber": null,
        "mastercardTransactionSequenceNumber": null,
        "visaAuthCode": "595677"
    },
    "location": {
        "city": "New York",
        "countryCode": "USA",
        "postcode": "1234-567",
        "state": null
    },
    "merchantCategoryCode": "5812",
    "merchantDescriptor": "Purchase",
    "originalTransactionId": "42adf0ef-bdde-436e-a035-3ce142b4f167",
    "programId": "76b6bd16-f1db-4eef-8636-bf94b2442e40",
    "programType": "transaction-stream",
    "reimbursementEligible": true,
    "updated": "2022-06-07T09:56:24.052Z",
    "wallet": null
}
```

## Signatures

If you want to confirm that incoming requests on your webhook URL are coming from Fidel API, we recommend verifying webhook signatures. Fidel API sends the `x-fidel-signature` and `x-fidel-timestamp` HTTP headers for each Fidel API request to a webhook URL. If you want an additional layer of security on your application, we recommend implementing signature validation for your webhooks.

Fidel API generates a unique secret key for each webhook you register. The key is returned in the response's `secretKey` property if you are using the Webhooks API. You can also copy the key from the Fidel API Dashboard's Webhooks page by clicking the **Show Key** button next to your webhook endpoint. To verify a webhook request, generate a signature using the same key that Fidel API uses and compare that to the value of the `x-fidel-signature` header.

Replay attacks are a common MITM attack vector where a valid payload and its signature are intercepted and re-transmitted. If you want to safeguard against them, you can use the `x-fidel-timestamp` header and confirm that the timestamp is not too old. We recommend you validate the requests within five minutes. In the case of retries, a new signature and timestamp are generated for each retry request.

> **1.** Create a string concatenating the body of the request, the webhook URL and the timestamp value from the `x-fidel-timestamp` header.  
> **2.** Double hash the resulting string using the webhook `secretKey` with HMAC-SHA256 and encode it in Base-64.  
> **3.** Compare the signature you generated with the signature provided in the `x-fidel-signature` header.

##### Example Javascript Implementation

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

    /** You can check how much time has passed since the request has been sent */
    /** timestamp - UTC Unix Timestamp (milliseconds) */
    const timestamp = fidelHeaders['x-fidel-timestamp'];
    const content = JSON.stringify(payload) + url + timestamp;

    const signature = base64Digest(base64Digest(content));
    return fidelHeaders['x-fidel-signature'] === signature;
}
```