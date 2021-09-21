# Reimbursement

Fidel Reimbursement is an add-on product to _Transaction_ tracking capability, that gives developers the ability to reimburse consumers faster and more easily than ever before, by pushing cash directly onto linked cards.

## Availability

Reimbursement is supported by both Visa and Mastercard in the US. Your account must be setup in the US to have access to the Reimbursement product.

## Activation

If your account is setup in the US and you are using an older [API version](https://reference.fidel.uk/docs/version-log) (reimbursement requires version `2021-09-28` or later) or haven't agreed to the _North America_ [Terms & Conditions](https://fidel.uk/legal) (_17 Jun 2021_), you will be prompted to go through the Reimbursement product activation.

![Reimbursement Activation](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/reimbursement-activation.gif "Reimbursement Activation")

## Credits

Before reimbursing cardholders you will need to buy Fidel Credits. To be able to make a reimbursement request you need to have enough credits in your balance to deduct the reimbursement amount. Credits currently support USD currency denomination and must be purchased by bank transfer to FIDEL LIMITED beneficiary account using your _unique reference code_ in the description. Find yours under the [Credits view in the dashboard](https://dashboard.fidel.uk/account/credits).

Fidel Credits are non refundable and non transferable. Take into consideration possible bank transfer delays.

![Credits view in the dashboard](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/credits-view.png "Credits view in the dashboard")

## Balance

Your credit balance is updated every time there is a new credits purchase or spent through reimbursement usage. When sending a reimbursement request, the amount is immediately deducted from the balance. If the reimbursement goes to `failed` status, the amount is added again to the balance. Read more information on the credits balance [API endpoint]().


### Credits Balance Example

Balances are denominated in multiple currencies to support other countries in the future. Currently we support `USD` and in this example `balances.USD` value is `$1000`.

```sh
curl -X GET \
  https://api.fidel.uk/v1/accounts/{accountId}/credits/balance \
  -H 'content-type: application/json' \
  -H 'fidel-key: {your secret key}'
```

```json
{
    "items": [
        {
          "accountId": "61741c3-3dc9-45f5-8e7c-db1dd649afab",
          "balances": {
            "AUD": 0,
            "CHF": 0,
            "JPY": 0,
            "EUR": 0,
            "GBP": 0,
            "CAD": 0,
            "USD": 1000,
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
    ],
    "execution": 135.265058,
    "resource": "/v1/accounts/{accountId}/credits/balance",
    "status": 200
  }
}
```

### Low Balance Notification

When a currency balance drops to 25% of the balance amount you had on your last purchase, `lowCreditsNotificationSent` is set to true and the following actions are then triggered:

- `credits.balance.low` webhook is sent;

- notification email is sent to account users emails;

- dashboard notification is shown.

### Low Balance Webhook

The credits.balance.low webhook notifies on the Low Balance Notification event that happens when the credits balance is running low. Use this webhook to automate credit purchases on your end without the risk of service disruption due to insufficient balance. See the example below with the webhook object triggered by the low USD balance:

### Low Balance Webhook

The `credits.balance.low` webhook notifies on the _Low Balance Notification_ event that happens when the credits balance is running low. Use this webhook to automate credit purchases on your end without the risk of service disruption due to insufficient balance. See the example below with the webhook object triggered by the low USD balance:

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
    "USD": 1000,
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

### History

In the Credits dashboard view you have access to your credits purchases in Purchased credits and spent usage in Reimbursements. Read more information on the credits history [API endpoint]().

![Credits history](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/credits-history.gif "Credits history")

## Reimbursement Request

### Eligibility

The reimbursement request is done towards a cardholder transaction with the path parameter `transactionId` and it needs to meet the eligibility criteria that can be checked with the transaction boolean property `reimbursementEligible`. You must be using the _API version_ `2021-09-28` or newer to have the property available in your transactions.

Eligibility has a `true` value when the transaction meets the following criteria:

- Transaction `currency` is `USD`;

- Transaction `reimbursement` is `undefined` or `reimbursement.status` is `failed`;

- Transaction `scheme` is `visa` or `mastercard`;

- Transaction status is `cleared`;

- Transaction `time` date is newer than 90 days.

### Sending Reimbursement

After selecting the `transactionId`, the reimbursement `amount` must be equal to or lower than the transaction `amount` and the `currency` is determined by the transaction. We currently support `USD` currency transactions. Optionally customise the `description` text that will show in the cardholder bank statement. Read more information on the reimbursement [API endpoint]().

![Creating a reimbursement](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/reimbursing.gif "Creating a reimbursement")

### Reimbursement Example

Example `amount` set to `2.55` and custom `description` to `Gift from Fidel`.

```sh
curl -X POST \
  https://api.fidel.uk/v1/transactions/{transactionId}/reimbursements \
  -H 'content-type: application/json' \
  -H 'fidel-key: {your secret key}' \
  -d '{
    "amount": 2.55,
    "description": "Gift from Fidel"
  }'
```

After the reimbursement request is received by the card scheme, the full transaction is returned with the newly created `transaction.reimbursement` object with `pending` status.

```json
{
    "items": [
        {
            // ... (other properties are hidden)
            "cleared": true, 
            "currency": "USD",
            "scheme": "visa",
            "time": "2021-08-20T11:11:11.000Z",
            "reimbursement": {
              "amount": 2.55,
              "created": "2021-09-30T11:11:11.000Z",
              "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
              "description": "Gift from Fidel",
              "status": "pending"
            }
        }
    ],
    "execution": 120.856835,
    "resource": "/v1/transactions/{transactionId}/reimbursements",
    "status": 200
}
```

### Lifecycle

When you request a reimbursement to an eligible cardholder transaction, we send the request to the card scheme in real time. For that reason, there is no way to stop or cancel your request after it has been sent.

Reimbursement `status` is set to `pending` while waiting for the card scheme to confirm the successful `issued` status, at which point funds normally hit the cardholder’ account. It takes between 48 to 72 hours for the `issued` status to be updated.

### Status

Find the reimbursement status in the transaction `reimbursement.status` property:

- `pending`: scheme is executing request;

- `issued`: scheme executed request successfully;

- `failed`: scheme request failed and `transaction.reimbursement.error` object is created. Retry is possible. [See error list for more information]().

### Webhook

When a reimbursement status is updated from `pending` to `issued` or `failed` the webhook named `transaction.reimbursement.status` is triggered. The webhook will send the full transaction object with the updated `reimbursement.status`. See the example below with the update to `issued` status:

```json
{
  // ... (other properties are hidden)
  "cleared": true, 
  "currency": "USD",
  "scheme": "visa",
  "time": "2021-08-20T11:11:11.000Z",
  "reimbursement": {
    "amount": 2.55,
    "created": "2021-09-30T11:11:11.000Z",
    "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
    "description": "Gift from Fidel",
    "status": "issued"
  }
}
```

## Errors

The reimbursement request and status update can fail for various reasons. Your application should handle these errors accordingly.

### API Errors

These errors might be returned in the sending reimbursement endpoint request.

| HTTP Status Code |                  Error Code                    |                                   Error Message                                    |
|:----------------:|------------------------------------------------|------------------------------------------------------------------------------------|
|        400       | `reimbursement-account-network-inactive`       | `Account cannot issue reimbursement for card network`                              |
|        400       | `reimbursement-account-not-enough-funds `      | `Account does not have enough funds for issuing reimbursement`                     |
|        404       | `reimbursement-account-not-found`              | `Account does not exist`                                                           |
|        400       | `reimbursement-already-created`                | `Transaction reimbursement already created`                                        |
|        400       | `reimbursement-amount-above-limit`             | `Reimbursement amount is above the allowed limit`                                  |
|        400       | `reimbursement-amount-greater-original-amount` | `Reimbursement amount is greater than original transaction amount`                 |
|        400       | `reimbursement-invalid-transaction`            | `Transaction is invalid to issue reimbursement`                                    |
|        500       | `reimbursement-network-internal-error `        | `Network responded with internal error`                                            |
|        404       | `reimbursement-transaction-not-found`          | `Transaction does not exist`                                                       |
|        400       | `reimbursement-unsupported-network`            | `Transaction network not supported for reimbursement`                              |
|        400       | `reimbursement-unsupported-currency`           | `Transaction currency not supported for reimbursement`                             |
|        401       | `reimbursement-not-activated`                  | `The reimbursement product is not activated for this account`                      |
|        400       | `reimbursement-time-limit-after-transaction`   | `Surpassed the time limit after the original transaction to issue a reimbursement` |
|        400       | `reimbursement-visa-invalid-community-code`    | `The transaction community code is invalid`                                        |
|        400       | `credits-account-not-enough-funds`             | `Account does not have enough credits`                                             |

### API Error Example

```json
{
    "error": {
        "code": "reimbursement-amount-greater-original-amount",
        "date": "2021-11-11T14:53:18.968Z",
        "message": "Reimbursement amount is greater than original transaction amount",
        "metadata": {}
    },
    "execution": 105.254359,
    "resource": "/v1/transactions/{transactionId}/reimbursements",
    "status": 400
}
```

### Status Errors

These errors might be returned in the reimbursement `status` update to `failed` from the card scheme and sent to the `transaction.reimbursement.status` webhook.

| Status Code |                   Error Code                    |                            Error Message                              |
|:-----------:|-------------------------------------------------|-----------------------------------------------------------------------|
|     400     | `reimbursement-issuing-mismatch`                | `Reimbursements issued by network do not match with original request` |
|     404     | `reimbursement-network-account-not-found`       | `Network was unable to find bank account`                             |
|     404     | `reimbursement-network-customer-not-found`      | `Network was unable to find customer account`                         |
|     500     | `reimbursement-network-invalid-account`         | `Network responded bank account is invalid`                           |
|     500     | `reimbursement-network-invalid-account-country` | `Network responded bank account country is invalid`                   |
|     500     | `reimbursement-network-invalid-currency`        | `Network responded the selected currency is invalid`                  |
|     400     | `reimbursement-network-multiple-accounts-found` | `Network responded multiple bank accounts were found `                |
|     500     | `reimbursement-network-others`                  | `Network responded with an irregular issue`                           |
|     500     | `reimbursement-request-failed `                 | `Failed to submit reimbursement request to network`                   |

### Status Error Example

```json
{
  // ... (other properties are hidden)
  "cleared": true, 
  "currency": "USD",
  "scheme": "visa",
  "time": "2021-08-20T11:11:11.000Z",
  "reimbursement": {
    "amount": 2.55,
    "created": "2021-09-30T11:11:11.000Z",
    "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
    "error": {
      "code": "reimbursement-network-account-not-found",
      "message": "Network was unable to find bank account",
      "status": 404 
    },
    "description": "Gift from Fidel",
    "status": "failed"
  }
}
```
