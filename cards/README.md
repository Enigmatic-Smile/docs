## Cards
The Card object holds information about the card details submitted by the user using the web or mobile SDKs. One user can link multiple cards, debit or credit under the same account.

The details required to link a card to a program are the 16-digit card number, expiry date, country code and the `programId` of the Program you want to link this card to. The card number is tokenised and transmitted directly from our secure pre-built iFrame to the API. This way, your servers are never exposed to sensitive information, removing all PCI compliance requirements from your side.

We never store the 16-digit PAN (Primary Account Number). To identify the user in a transaction object you should the use the `cardId` property. After this point only the `cardId` is exchanged between your servers, the card schemes and FIDEL API.

After the card is linked successfully, we will monitor any purchase made by this card on any of the program’s physical or online locations and send the transaction object to a webhook URL specified by you.

<br/>

# Add Card

To add a new card, go to the **API Playground** on the dashboard, choose **Add Card** from the left menu endpoints. The method will be set to POST and the endpoint to **_/cards_**. An editable sample JSON object like the one displayed below will be used to create the card. In order to add a card from the Playground, you’ll need to use one of the available testing card numbers displayed below. Also, provide an expiry date in the future, `countryCode` and `programId` you want to link this card to. You must set `termsOfUse` to `true` to define that the user agreed to the terms of use and opt-in. 

You can use the dropdown menus to automatically set the ids in the request body or copy them to use in your application code without leaving the playground.

<h5>Create sample cards in test mode using the API Playground.</h5>

![Create card](https://docs.fidel.uk/assets/images/create-card.png "Create card")

<br/>

If the card is successfully linked, the newly created card object is returned in JSON and displayed in the response body box. You can also use the [Web SDK](/web-sdk) to create cards in test environment using your SDK test key. If an error occurs on card creation, you receive the error message in the HTTP response body.

<h5>JSON Card object</h5>

```json
fileName:card.json
{
  "id": "e3fff00f-ab85-4a0a-b572-08b464ebf67a",
  "accountId": "a30e933f-cde1-4ac1-9a5e-acd329497a48",
  "programId": "e3fff00f-ab85-4a0a-b572-08b464ebf67a",
  "metadata": {
    "id": "this-is-the-metadata-id",
    "customKey1": "customValue1",
    "customKey2": "customValue2"
  },
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
  "updated": "2017-02-13T17:17:02.833Z"
}
```

<br/>

# Testing Card Numbers
Note that on the Playground you can only create cards in test mode. In live mode new cards can only be linked to programs using the SDKs or the API if you are PCI compliant.

Use the following test card number ranges to test card linking in test mode, either in the Playground or using the SDKs with your test SDK key.

**Visa**: `4444000000004***`  
**Mastercard**: `5555000000005***`

Where the last three digits `***` can be any number. E.g. `4444000000004278` and `5555000000005093` are both valid test card numbers.
