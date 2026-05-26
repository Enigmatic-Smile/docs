# Webhooks

Fidel API uses webhooks to notify your application when relevant events happen in your account across multiple resources, namely with event types such as `brand.consent`, `marketplace.offer.live`, `marketplace.offer.updated`, `mid-request.succeeded`, `mid-request.failed`, `program.status`.

Fidel API will notify your registered webhook URLs as the event happens, via a HTTP POST request with a signature header for verification, which needs to be received and acknowledged in a timely manner. The HTTP request contains the event object as payload.

For example, once transactions are received via the SFTP server and successfully qualified, webhooks are triggered to notify the publisher and, where applicable, reports are generated and sent to affiliate networks.

## Management and behavior

There are two ways you can manage your webhooks—view, create, update, delete—with the Fidel API. You can create them in the [Fidel Dashboard](https://dashboard.fidel.uk/webhooks), under the **"Webhooks"** page, or make HTTP requests using the [Webhooks API](https://fidel-oaas.readme.io/reference/create-hook).

As requirements for creation, Fidel API only accepts **HTTPS URLs** for webhook endpoints, thus your server must support HTTPS and have a valid certificate.

Fidel API sends the data via HTTP POST in JSON format. It will send test events if your Dashboard is in test mode or if you are using test API keys when registering the webhook URLs. To receive live events, flip your switch on the Dashboard to go live, or create the webhooks using a live API key.

Fidel API has two types of webhooks (brand and program-related), both of which work similarly, but have slightly different requirements to be registered. For customization, we also allow additional HTTP headers to be added to help integrating with your systems.

## Brand-related webhooks

The `brand.consent` webhook only requires a URL to register. You can use the [Hooks endpoint](https://fidel-oaas.readme.io/reference/create-hook) for registering brand webhooks.

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

## Program-related webhooks

Program webhooks require a `programId` to be associated with and a URL to register. You can register up to 10 webhook URLs per event type for each program. You can use the [Program Hooks endpoint](https://fidel-oaas.readme.io/reference/create-program-hook) for registering program webhooks. The events that can be registered for a given program are `program.status`, `marketplace.offer.live`, `marketplace.offer.updated`.

Here's an example on how to create a webhook on a Program for the `marketplace.offer.live` event, with `example.com` as the URL:

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "event": "marketplace.offer.live",
    "url": "https://example.com"
  }'
```

## Acknowledging reception

To confirm receipt of a webhook event, your server endpoint should return a `200 OK` HTTP status code. Any other response, or not providing any response within 20 seconds will be treated as a failure and our system will retry sending the request twice (i.e. three tries in total), with one-minute wait on the second request and two-minute wait on the third (last) attempt.

To avoid timeouts, it is recommended to not run complex and time-consuming logic upon reception of the webhook in order to provide a response back and thus avoid unnecessary retries and potentially duplicate processing of the events.

## Custom request headers

Fidel API allows you to define custom HTTP headers when you register a webhook URL. The custom headers are included in the HTTP POST request headers that are sent to your application.

Custom headers can be defined when creating a new webhook in the Dashboard or by using the Webhooks API and setting the optional `headers` object with a key-value pair (see example below).

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

To delete custom headers from a registered webhook, use the [Update Hooks endpoint](https://fidel-oaas.readme.io/reference/update-hook) and send an empty `headers` object.

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

A maximum of **5 custom headers** per webhook can be defined, and they need to follow strict character validation patterns. The key name must be between 1 and 64 characters and only accepts the Roman alphabet, numbers, dashes and underscores. The value must be between 1 and 1000 characters. Additionally, the HTTP reserved headers are blocklisted and cannot be used for the key name. The full list of blocklisted key names:

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

## Program Status Event

A `program.status` event is triggered in the live environment when there are updates for each Location in a Program. In the test environment, this webhook doesn't trigger because there is no concept of Location syncing there.

```json
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

## Signatures

If you want to confirm that incoming requests on your webhook URL are coming from the Fidel API, we recommend verifying webhook signatures. We send the `x-fidel-signature` and `x-fidel-timestamp` HTTP headers for each request we make to a webhook URL.

Fidel API generates a unique secret key for each webhook you register. The key is returned in the response's `secretKey` property if you are using the Webhooks API. You can also copy the key from the Fidel Dashboard's Webhooks page by clicking the **Show Key** button next to your webhook endpoint. To verify a webhook request, generate a signature using the same key that the Fidel API uses and compare that to the value of the `x-fidel-signature` header.

Replay attacks are a common MITM attack vector where a valid payload and its signature is intercepted and re-transmitted. If you want to safeguard against them, you can use the `x-fidel-timestamp` header and confirm that the timestamp is not too old. We recommend you validate the requests in a 5-minute gap. In the case of retries, a new signature and timestamp are generated for each retry request.

The validation/verification can be conducted as follows:

1. Create a string concatenating the body of the request, the webhook URL and the timestamp value from the `x-fidel-timestamp` header.
2. Double hash the resulting string using the webhook `secretKey` with HMAC-SHA256 and encode it in Base-64.
3. Compare the signature you generated with the signature provided in the `x-fidel-signature` header.

**Example JavaScript implementation:**

```js
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

You can delete webhooks in the [Fidel API Dashboard](https://dashboard.fidel.uk/webhooks), or using the API's [Delete Webhook endpoint](https://fidel-oaas.readme.io/reference/delete-hook).

```sh
curl -X DELETE \
  https://api.fidel.uk/v1/hooks/b9ef3795-a38f-4ef2-8d8d-293dd7fbe1a7 \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63'
```

## API Reference

If you're looking to find out more about our Webhooks API and how to use it with your application, please visit the [Fidel API Reference](https://fidel-oaas.readme.io/reference).
