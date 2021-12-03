# Reimbursement

Fidel Reimbursement is an add-on product to _Transaction_ tracking capability, that gives developers the ability to reimburse customers faster and more easily than ever before, by pushing cash directly onto linked cards.

## Availability

Reimbursement supports both debit and credits cards by Visa and Mastercard in the US. Your account must be setup in the US to have access to the Reimbursement product.

## Activation

If your account is setup in the US and you are using an older [API version](https://reference.fidel.uk/docs/version-log) (reimbursement requires version `2021-09-28` or later) or haven't agreed to the _North America_ [Terms & Conditions](https://fidel.uk/legal) (_17 Jun 2021_), you will be prompted to go through the Reimbursement product activation.

![Reimbursement Activation](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/reimbursement-activation.gif "Reimbursement Activation")

## Credits

Before reimbursing cardholders you will need to buy Fidel Credits. To be able to make a reimbursement request you need to have enough credits in your balance to deduct the reimbursement amount. Credits currently support USD currency denominations only and must be purchased by bank transfer to FIDEL LIMITED’s beneficiary account using your unique reference code in the description. Find yours under the [Credits view in the dashboard](https://dashboard.fidel.uk/account/credits).

Fidel Credits are non refundable and non transferable. Take into consideration possible bank transfer delays that can take between 2-3 business days.

![Credits view in the dashboard](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/credits-view.png "Credits view in the dashboard")

### Balance

Your credit balance is updated every time you purchase credits or spend by using Reimbursement. When sending a reimbursement request, the amount is immediately deducted from the balance. If the reimbursement goes to `failed` status, the amount is added again to the balance. Read more information on the [credits balance endpoint](https://reference.fidel.uk/reference#get-account-balance).


### Credits Balance Example

Balances are denominated in multiple currencies to support other countries in the future. Currently we support `USD` and in this example `balances.USD` value is `$1000`.

```sh
curl -X GET \
  https://api.fidel.uk/v1/accounts/{accountId}/credits/balance \
  -H 'content-type: application/json' \
  -H 'fidel-key: {your secret key}'
```

```json
fileName:credits-balance.json
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

<h3 id="low-balance-notification">Low Balance Notification</h3>

When a currency balance drops to _25%_ of the balance amount you had on your last purchase, `lowCreditsNotificationSent` is set to true and the following actions are then triggered:

- notification email is sent to account users emails;

- dashboard notification is shown;

- `credits.balance.low` webhook is sent if you are listening on your end. Read below on how to set it up.

### Low Balance Webhook

The `credits.balance.low` webhook notifies on the _[Low Balance Notification](https://fidel.uk/docs/reimbursement/#low-balance-notification)_ event that happens when the credits balance is running low. Use this webhook to automate credit purchases on your end without the risk of service disruption due to insufficient balance. See the example below with the webhook object triggered by the low USD balance:

```json
fileName:credits-balance.json
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

In the Credits dashboard view you have access to your credit purchases in _Purchased credits_ and credit spent in _Reimbursements_. Read more information on the [credits history endpoint](https://reference.fidel.uk/reference#get-credits-history).

![Credits history](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/credits-history.gif "Credits history")

## Eligibility

The reimbursement request is done towards a cardholder transaction with the path parameter `transactionId` and it needs to meet the eligibility criteria that can be checked with the transaction boolean property `reimbursementEligible`. You must be using the API version `2021-09-28` or newer to have the property available in your transactions.

Eligibility has a `true` value when the transaction meets the following criteria:

- Transaction `currency` is `USD`;

- Transaction `reimbursement` is `undefined` or `reimbursement.status` is `failed`;

- Transaction `scheme` is `visa` or `mastercard`;

- Transaction status is `cleared`;

- Transaction `time` date is newer than 90 days.

### Finding Eligible Transactions by Card

Find eligible card transactions with `cardId` for a specific `amount` and `currency`. Optionally pass `brandId` to find transactions in a specific brand.

Transactions are sorted automatically in descending order by the best available, following the eligibility criteria, giving more emphasis on the transaction time and then amount. Read more information on the [find eligible reimbursement transactions endpoint](https://reference.fidel.uk/reference/get-transactions-for-reimbursement-by-card).

![Reimbursement by Card](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/reimburse-card.gif "Reimbursement by Card")

### Eligible Transactions Example

Example `amount` set to `5`, `currency` to `USD` and `brandId` to `Star`. 

```sh
curl -X GET \
  GET https://api.fidel.uk/v1/cards/{cardId}/transactions/reimbursement?amount={amount}&currency={currency}&brandId={brandId} \
  -H 'content-type: application/json' \
  -H 'fidel-key: {your secret key}'
```

```json
fileName:transaction.json
{
  "count": 2,
  "items": [
    {
      // For the purpose of this example, only selected properties are shown
      "amount": 10,
      "cleared": true, 
      "currency": "USD",
      "card": {
        // For the purpose of this example, only selected properties are shown
        "scheme": "visa",  
      },
      "datetime": "2021-08-20T11:11:11",
      "reimbursementEligible": true
    },
    {
      // For the purpose of this example, only selected properties are shown
      "amount": 20,
      "cleared": true, 
      "currency": "USD",
      "card": {
        // For the purpose of this example, only selected properties are shown
        "scheme": "visa",  
      },
      "datetime": "2021-10-20T11:11:11",
      "reimbursementEligible": true
    }
  ],
  "resource": "/v1/cards/{cardId}/transactions/reimbursement",
  "status": 200,
  "execution": 26.392104
}
```

## Request

### Creating a request

After choosing the `transactionId`, the reimbursement `amount` must be equal to or lower than the transaction `amount` and the `currency` is determined by the transaction. Visa cards have a maximum reimbursement amount limit of _USD $250_. We currently support `USD` currency transactions. Optionally customise the `description` text that will show in the cardholder bank statement. Read more information on the [create reimbursement endpoint](https://reference.fidel.uk/reference#create-reimbursement).

![Creating a reimbursement](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/reimbursing.gif "Creating a reimbursement")

### Reimbursement Example

Example `amount` set to `2.55` and custom `description` to `Earned Stars`.

```sh
curl -X POST \
  https://api.fidel.uk/v1/transactions/{transactionId}/reimbursement \
  -H 'content-type: application/json' \
  -H 'fidel-key: {your secret key}' \
  -d '{
    "amount": 2.55,
    "description": "Earned Stars"
  }'
```

After the reimbursement request is received by the card scheme, the full transaction is returned with the newly created `transaction.reimbursement` object with `pending` status.

```json
fileName:transaction.json
{
    "items": [
        {
            // For the purpose of this example, only selected properties are shown
            "amount": 10,
            "cleared": true, 
            "currency": "USD",
            "card": {
              // For the purpose of this example, only selected properties are shown
              "scheme": "visa",  
            },
            "datetime": "2021-08-20T11:11:11",
            "reimbursementEligible": false,
            "reimbursement": {
              "amount": 2.55,
              "created": "2021-09-30T11:11:11.000Z",
              "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
              "description": "Earned Stars",
              "status": "pending"
            }
        }
    ],
    "execution": 120.856835,
    "resource": "/v1/transactions/{transactionId}/reimbursement",
    "status": 200
}
```

### Lifecycle

When you request a reimbursement to an eligible cardholder transaction, we send the request to the card scheme in real time. For that reason, there is no way to stop or cancel your request after it has been sent.

Reimbursement `status` is set to `pending` while waiting for the card scheme to confirm the successful `issued` status, at which point funds normally hit the cardholder’ account. It takes between 48 to 72 hours for the `issued` status to be updated.

![Reimbursement status](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/reimbursement-transactions.png "Reimbursement status")

### Status

Find the reimbursement status in the transaction `reimbursement.status` property:

- `pending`: scheme is executing request;

- `issued`: scheme executed request successfully;

- `failed`: scheme request failed and `transaction.reimbursement.error` object is created. Retry is possible. [See error list for more information](https://fidel.uk/docs/reimbursement/#errors).

## Automated Reimbursements

Automate reimbursement requests when creating an Offer by toggling the “enable automatic reimbursements” checkbox. This toggle will be available if the reimbursement product is active and the Offer’s country is the `USA`.

![Automatic reimbursement with Offers](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/offers-automatic-reimbursement.gif "Automatic reimbursement with Offers")

Reimbursements will be automatically requested whenever a transaction **is both cleared and qualified** for an Offer with `offer.automatedReimbursement.enabled` set to `true`.

Read more information on the Offers product [in the documentation page](https://fidel.uk/docs/offers).

### Tracking before issuing

Track if a transaction will get a reimbursement request by checking the transaction’s `offer` object:

```json
fileName:transaction-with-offer.json
{
  // For the purpose of this example, only selected properties are shown
  "amount": 10,
  "amount": 10,
  "cleared": true, 
  "currency": "USD",
  "datetime": "2021-08-20T11:11:11",
  "reimbursementEligible": true,
  "offer": {
    "automatedReimbursement": true,
    "id": "7e55eeae-99d6-4daf-b8c4-ac9ca660e964",
    "cashback": 20,
    "message": [],
    "performanceFee": 3.2,
    "qualified": false,
    "qualificationDate": null
  },
}
```

The `offer.automatedReimbursement` states whether a reimbursement request will be attempted for that transaction or not. The reimbursement will have the same amount as the `offer.cashback` property and the same currency as `currency`. Transactions that already have a `reimbursement` object will not have any reimbursements requested automatically.

Transactions with expected automated reimbursement attempts are shown in the dashboard with a grey label on the reimbursements column.

![Transactions with auto-reimbursements](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/transactions-auto-reimbursements.png "Transactions with auto-reimbursements")

### Tracking after issuing

Automated reimbursement attempts have the same payloads as regular reimbursements requests and also include three new properties to provide the user with more information on how and why they were requested - `automated`, `issuingBrandId`, `issuingOfferId`.

The `reimbursement.automated` property states whether that reimbursement request was issued automatically. Automated reimbursements are those that were not triggered directly by the user (via API or dashboard).

The `reimbursement.issuingBrandId` and `reimbursement.issuingOfferId` properties are informative properties which only exist for automated reimbursements and inform the user of what context the reimbursement was requested in.

```json
fileName:transaction-with-offer.json
{
  // For the purpose of this example, only selected properties are shown
  "amount": 10,
  "cleared": true, 
  "currency": "USD",
  "datetime": "2021-08-20T11:11:11",
  "reimbursementEligible": true,
  "reimbursement": {
    "amount": 2.55,
    "automated": true,
    "created": "2021-09-30T11:11:11.000Z",
    "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
    "description": "Earned Stars",
    "issuingBrandId": "459170aa-7490-467d-bb4d-19b35139e325",
    "issuingOfferId": "7e55eeae-99d6-4daf-b8c4-ac9ca660e964",
    "status": "pending"
  },
  "offer": {
    "automatedReimbursement": true,
    "id": "7e55eeae-99d6-4daf-b8c4-ac9ca660e964",
    "cashback": 20,
    "message": [],
    "performanceFee": 3.2,
    "qualified": false,
    "qualificationDate": null
  },
}
```

## Webhook

When a reimbursement status is updated from `pending` to `issued` or `failed` the webhook named `transaction.reimbursement.status` is triggered. The webhook will send the full transaction object with the updated `reimbursement.status`. See the example below with the update to `issued` status:

```json
fileName:transaction.json
{
  // For the purpose of this example, only selected properties are shown
  "amount": 10,
  "cleared": true, 
  "currency": "USD",
  "card": {
    // For the purpose of this example, only selected properties are shown
    "scheme": "visa",
  },
  "datetime": "2021-08-20T11:11:11",
  "reimbursementEligible": false,
  "reimbursement": {
    "amount": 2.55,
    "created": "2021-09-30T11:11:11.000Z",
    "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
    "description": "Earned Stars",
    "status": "issued"
  }
}
```

## Errors

The reimbursement request and status update can fail for various reasons. Your application should handle these errors accordingly.

### API Errors

These errors might be returned in the [request](https://fidel.uk/docs/reimbursement/#request) endpoint.

<ReimbursementTable 
  headings={['HTTP Status Code', 'Error Code', 'Error Message']} 
  rows={[
    ['400', 'reimbursement-account-network-inactive', 'Account cannot issue reimbursement for card network'],
    ['400', 'reimbursement-account-not-enough-funds', 'Account does not have enough funds for issuing reimbursement'],
    ['404', 'reimbursement-account-not-found', 'Account does not exist'],
    ['400', 'reimbursement-already-created', 'Transaction reimbursement already created'],
    ['400', 'reimbursement-amount-above-limit', 'Reimbursement amount is above the allowed limit'],
    ['400', 'reimbursement-amount-greater-original-amount', 'Reimbursement amount is greater than original transaction amount'],
    ['400', 'reimbursement-invalid-transaction', 'Transaction is invalid to issue reimbursement'],
    ['500', 'reimbursement-network-internal-error', 'Network responded with internal error'],
    ['404', 'reimbursement-transaction-not-found', 'Transaction does not exist'],
    ['400', 'reimbursement-unsupported-network', 'Transaction network not supported for reimbursement'],
    ['400', 'reimbursement-unsupported-currency', 'Transaction currency not supported for reimbursement'],
    ['401', 'reimbursement-not-activated', 'The reimbursement product is not activated for this account'],
    ['400', 'reimbursement-time-limit-after-transaction', 'Surpassed the time limit after the original transaction to issue a reimbursement'],
    ['400', 'reimbursement-visa-invalid-community-code', 'The transaction community code is invalid'],
    ['400', 'credits-account-not-enough-funds', 'Account does not have enough credits'],
  ]}
/>

### API Error Example

```json
fileName:reimbursement-request.json
{
    "error": {
        "code": "reimbursement-amount-greater-original-amount",
        "date": "2021-11-11T14:53:18.968Z",
        "message": "Reimbursement amount is greater than original transaction amount",
        "metadata": {}
    },
    "execution": 105.254359,
    "resource": "/v1/transactions/{transactionId}/reimbursement",
    "status": 400
}
```

### Status Errors

These errors might be returned in the reimbursement `status` update to `failed` from the card scheme and sent to the `transaction.reimbursement.status` webhook.

<ReimbursementTable 
  headings={['Status Code', 'Error Code', 'Error Message']} 
  rows={[
    ['400', 'reimbursement-issuing-mismatch', 'Reimbursements issued by network do not match with original request'],
    ['404', 'reimbursement-network-account-not-found', 'Network was unable to find bank account'],
    ['404', 'reimbursement-network-customer-not-foun', 'Network was unable to find customer account'],
    ['500', 'reimbursement-network-invalid-account', 'Network responded bank account is invalid'],
    ['500', 'reimbursement-network-invalid-account-country', 'Network responded bank account country is invalid'],
    ['500', 'reimbursement-network-invalid-currency', 'Network responded the selected currency is invalid'],
    ['400', 'reimbursement-network-multiple-accounts-found', 'Network responded multiple bank accounts were found'],
    ['500', 'reimbursement-network-others', 'Network responded with an irregular issue'],
    ['500', 'reimbursement-request-failed', 'Failed to submit reimbursement request to network'],
  ]}
/>

### Status Error Example

```json
fileName:transaction.json
{
  // For the purpose of this example, only selected properties are shown
  "cleared": true, 
  "currency": "USD",
  "card": {
    // For the purpose of this example, only selected properties are shown
    "scheme": "visa",
  },
  "datetime": "2021-08-20T11:11:11",
  "reimbursementEligible": true,
  "reimbursement": {
    "amount": 2.55,
    "created": "2021-09-30T11:11:11.000Z",
    "creditsTransactionId": "1250ab5a-0661-4a06-a40c-8514093a9241",
    "error": {
      "code": "reimbursement-network-account-not-found",
      "message": "Network was unable to find bank account",
      "status": 404 
    },
    "description": "Earned Stars",
    "status": "failed"
  }
}
```

## API Reference

If you're looking to find out more about our Reimbursement API and how to use it with your application, please visit the [Fidel API Reference](https://reference.fidel.uk/reference).
