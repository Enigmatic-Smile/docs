# Webhooks

Fidel API uses webhooks to notify your application when events happen in your account. For Offer Marketplace, available events include `marketplace.offer.live` and `marketplace.offer.updated`.

When an event occurs, Fidel API sends an HTTP POST request containing the event payload to each subscribed endpoint.

For example, once transactions are received via the SFTP server and successfully qualified, webhooks are triggered to notify the publisher and, where applicable, reports are generated and sent to affiliate networks.

## Managing webhook subscriptions

You can view, create, update and delete webhook subscriptions in the [Fidel API Dashboard](https://dashboard.fidel.uk/webhooks) or through the [Webhooks API](https://fidel-oaas.readme.io/reference/create-hook).

Webhook endpoints must use HTTPS and have a valid certificate. Webhooks created in test mode, or with a test API key, receive test events. Switch the Dashboard to live mode, or use a live API key, to receive live events.

Program webhooks require a `programId`. You can register up to 10 URLs per event type for each program.

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

## Receiving webhook deliveries

Return any `2xx` HTTP status code within 20 seconds to acknowledge a delivery. A timeout or any non-`2xx` response is treated as a failed attempt.

Fidel API makes up to three attempts for each delivery:

1. The first attempt is made immediately.
2. The second attempt is made one minute after the first attempt fails.
3. The third and final attempt is made two minutes after the second attempt fails.

Your endpoint might receive the same event more than once. Acknowledge the request before starting long-running work and process events asynchronously. Use the `fidel-message-id` header as an idempotency key so repeated attempts do not cause duplicate processing.

Each request includes these delivery headers:

| Header | Description |
|---|---|
| `fidel-message-id` | Identifies the logical event. It remains the same across automatic attempts and manual replays. |
| `fidel-attempt-number` | The attempt number for the delivery, starting at `1`. |
| `Fidel-Request-Id` | Identifies the individual HTTP request. |
| `x-fidel-signature` | Signature used to verify that Fidel API sent the request. |
| `x-fidel-timestamp` | Unix timestamp in milliseconds used to generate the signature. |

## Viewing delivery history

Open **Webhooks > Deliveries** in the Fidel API Dashboard to inspect webhook deliveries from the last 90 days. Test and live deliveries are separated by the Dashboard mode.

Delivery history is available for event types that have been migrated to Webhooks 2.0. Other event types will appear as the rollout progresses.

The delivery list shows the status, event, program, destination and creation time. You can:

- filter by status, event, program or time range;
- search for a specific `fidel-message-id`;
- group deliveries that share a `fidel-message-id`;
- sort deliveries by creation time.

Delivery statuses are `processing`, `succeeded` and `failed`.

<img src="https://docs.fidel.uk/assets/images/list_webhooks_deliveries.png" alt="Webhook Deliveries list with status, event, program and time-range filters" />

Select a delivery to inspect its payload and metadata. The details panel shows every delivery attempt, including its timing, status code, masked request headers and response body.

<img src="https://docs.fidel.uk/assets/images/webhooks_delivery_detail.png" alt="Webhook delivery details showing the payload and request and response attempt details" />

## Replaying a delivery

You can manually replay any completed delivery that is less than 90 days old, whether its original status is `succeeded` or `failed`.

1. Open **Webhooks > Deliveries** in the Dashboard.
2. Select the delivery.
3. Select **Replay delivery** and confirm the action.

A replay creates a new delivery using the stored payload and the webhook subscription's current URL and configuration. It retains the original `fidel-message-id`, starts again at attempt `1` and appears separately in delivery history. Replays are delivered through a separate queue so they do not delay live webhook traffic.

<img src="https://docs.fidel.uk/assets/images/webhooks_replay_prompt.png" alt="Webhook delivery replay confirmation" />

## Custom request headers

You can define up to five custom HTTP headers when creating or updating a webhook. Header names must contain 1 to 64 letters, numbers, dashes or underscores. Values must contain 1 to 1000 characters. Fidel API-managed and other reserved HTTP headers cannot be overridden.

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "event": "marketplace.offer.updated",
    "url": "https://example.com",
    "headers": {
      "Custom-Header": "my-custom-header"
    }
  }'
```

To remove all custom headers, use the [Update Hook endpoint](https://fidel-oaas.readme.io/reference/update-hook) and send an empty `headers` object.

## Verifying signatures

Fidel API generates a unique `secretKey` for each webhook subscription. The API returns it when the webhook is created, and you can reveal it from the Dashboard's Webhooks page.

To verify a request:

1. Concatenate the raw request body, webhook URL and `x-fidel-timestamp` value.
2. Hash the result twice with HMAC-SHA256 using the webhook's `secretKey`, then Base64-encode each digest.
3. Compare the result with `x-fidel-signature` using a constant-time comparison.

Reject timestamps outside your accepted tolerance to reduce the risk of replay attacks. A five-minute tolerance is recommended.

```js
function isSignatureValid(fidelHeaders, rawBody, secret, url) {
  function base64Digest(value) {
    return crypto.createHmac("sha256", secret).update(value).digest("base64");
  }

  const timestamp = fidelHeaders["x-fidel-timestamp"];
  const timestampAge = Math.abs(Date.now() - Number(timestamp));

  if (!Number.isFinite(timestampAge) || timestampAge > 5 * 60 * 1000) {
    return false;
  }

  const content = rawBody + url + timestamp;
  const signature = base64Digest(base64Digest(content));

  const received = Buffer.from(fidelHeaders["x-fidel-signature"]);
  const expected = Buffer.from(signature);

  return received.length === expected.length &&
    crypto.timingSafeEqual(received, expected);
}
```

Use the exact raw request body when calculating the signature. Parsing and re-serializing JSON can change the content and cause verification to fail.

## Deleting a webhook

Delete webhook subscriptions in the [Fidel API Dashboard](https://dashboard.fidel.uk/webhooks) or through the [Delete Webhook endpoint](https://fidel-oaas.readme.io/reference/delete-hook).

```sh
curl -X DELETE \
  https://api.fidel.uk/v1/hooks/b9ef3795-a38f-4ef2-8d8d-293dd7fbe1a7 \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>'
```

## API reference

See the [Fidel API Reference](https://fidel-oaas.readme.io/reference) for Webhooks API endpoints and schemas.
