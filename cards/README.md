## Cards
The Card object holds information about the card details submitted by the user using the web form or the mobile SDKs. One user can link multiple cards, debit or credit under the same account.

The details required to link a card are the 16-digit card number, expiry date and the programId of the Program you want to link this card to. The card number is validated using the Luhn algorithm and then tokenised before being submitted to Visa and MasterCard networks. This way, your servers are never exposed to the credit card number, removing all PCI compliance requirements from your side.

FIDEL never stores the 16-digit PAN (Primary Account Number) entered by the user. It is tokenised instantly and submitted to the card schemes. After this point only the card token is exchanged between your servers, the card schemes and FIDEL.

After the card is linked successfully, we will monitor any purchase made by this card on any of the program’s locations and send the transaction object to a specified webhook.

<br/>

# Add Card

To add a new card, go to the **API Playground** on the dashboard, choose **Add Card** from the left menu. The method will be set to POST and the endpoint to _/cards/_. An editable sample JSON object like the following one will be used to create the card. You can add and delete sample cards in test mode by using the **API Playground**.

<h5>Create sample cards in test mode using the API Playground.</h5>

![Create card](/assets/images/create-card.png "Create card")

<br/>

If the card is successfully linked, the newly created card object is returned in JSON format with the properties shown below. You can also be notified in real-time by creating a webhook with the event `card.link.success`. If the card linking fails we use the event `card.link.error`.

When linking real Visa cards in live mode, if the card number and expiry date are successfully validated, the `card.link.success` event is triggered in real-time by FIDEL API despite the confirmation from Visa only being available in the next 24 hours. For this reason, we assume that all Visa cards that pass validation are successfully linked and we run a daily schedule at 11:00am UTC that triggers `card.link.error` events in case of any errors.

This process is not required for Mastercard cards since we get successful card linking confirmation in real-time.

<h5>JSON Card object</h5>

```json
fileName:card.json
{
  "items": [
    {
      "id": "6b9d11af-cd1e-4bc9-8000-78f6bfcd3db3",
      "accountId": "a3de60c3-849a-4faa-8447-bcd16efb148c",
      "programId": "60b4f64e-c6f2-4c0b-a00e-b60192632dfd",
      "cardId": "42b8eb31-3cb9-4171-8077-c6669d9aa7a2",
      "provider": "mastercard",
      "type": "master-card",
      "lastNumbers": "3183",
      "expMonth": 1,
      "expYear": 2018,
      "expDate": "2018-01-01T00:00:00.000Z",
      "countryCode": "GBR",
      "mapped": true,
      "live": true,
      "created": "2017-02-13T17:02:12.535Z",
      "updated": "2017-02-13T17:17:02.833Z",
    }
  ],
  "resource": "/v1d/cards",
  "status": 201,
  "execution": 2803.465225
}
```

<br/>

# Testing Card Numbers
Note that you can only add cards in test mode. In live mode new cards can only be created using the SDKs and be real issued card numbers. Use the following test card numbers to test card linking in test mode:

<h5>Visa testing card numbers</h5>

```json
visa: [
  '4444000000000001',
  '4444000000000002',
  '4444000000000003',
  '4444000000000004',
  '4444000000000005',
  '4444000000000006',
  '4444000000000007',
  '4444000000000008',
  '4444000000000009',
  '4444000000000010'
]
```
<h5>Mastercard testing card numbers</h5>

```json
mastercard: [
  '5555000000000001',
  '5555000000000002',
  '5555000000000003',
  '5555000000000004',
  '5555000000000005',
  '5555000000000006',
  '5555000000000007',
  '5555000000000008',
  '5555000000000009',
  '5555000000000010'
]
```

<br/>

___

### Help?
If you have any questions please contact us by [email](mailto:developer@fidel.uk) or talk with our developers in the [Slack channel](fidel.uk).

<br/>
