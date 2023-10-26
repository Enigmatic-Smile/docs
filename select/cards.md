# Cards

Payment cards are the cornerstone of Fidel APIs. You can receive Fidel Transactions from a merchant location only from the registered (or linked) cards in your Program. That being said, we never store the full card number or CVV details in our system. We tokenise the card details and exchange only the card id with the card networks to retrieve the transactions made using the cards.

After the card is linked successfully, we will monitor any purchase made by this card at any of the program’s physical or online locations. The transaction object will be sent to a webhook URL specified by you.

Developers can use [our APIs](https://reference.fidel.uk/reference/create-card) or SDKs - we have [JavaScript](/select/sdks/web/v3), [Android](/select/sdks/android/guide-v2), [iOS](/select/sdks/ios/guide-v2) and [React Native](/select/sdks/react-native/guide-v2) SDKs - to register debit or credit cards on the Fidel platform, link them to a [Program](/select/programs) and start receiving transaction made with the card in the Program locations.

## Test Card Numbers

For security purposes, the Fidel test environment doesn't accept live card numbers. We've provided a range of test card numbers you can use while integrating or testing the Fidel APIs. The test card numbers work in the [Fidel Dashboard Playground](https://dashboard.fidel.uk/playground), with the Fidel SDKs and with the Fidel Cards APIs.

**Visa**: `4444000000004***`  
**Mastercard**: `5555000000005***`  
**Amex**: `3400000000003**` and `3700000000003**`

Where `*` can be any digit. For example, `4444000000004278`, `5555000000005093`, `340000000000301` `370000000000388` are all valid test card numbers.

## Adding Cards

There are multiple ways you can test card-linking on the Fidel platform before going live: in the Fidel Dashboard, with the Fidel SDKs or with the Fidel Cards API. For integrating card-linking into your application, we recommend using one of our PCI Compliant SDKs. We provide both Web(JavaScript) and Mobile(iOS, Android, ReactNative) SDKs. Fidel also provides a Cards API that can be used for linking cards to your Program, but we require you to be PCI Compliant before using it.

### Fidel Dashboard

You can create, view, delete or export Cards in any Program in the [Fidel Dashboard](https://dashboard.fidel.uk/cards).

![Fidel Dashboard Create Card](https://docs.fidel.uk/assets/images/dashboard-new-card.gif "Fidel API Dashboard Create Card")

### API Playground

To test creating Cards or deleting Cards in a Program you can use the [API Playground](https://dashboard.fidel.uk/playground), where we've got both options available.

When you choose `/create` from the `cards` endpoints, a dropdown appears where you can select a Program. Once you selected a Program, the `/programs/program_id/cards` POST request will be updated with the program id for the selected Program. The request body on the right is already pre-filled with a card number, expiration date, country code and terms of service. You can edit all the properties in the request body before running the request. You can use any of the available testing card numbers listed above, an expiry date in the future, and a three-letter `countryCode`. `termsOfUse` is set to `true` to simulate that the user agreed to the terms of use and opted-in. Once you've run the request, you'll be able to inspect the Card object in the response section if the card was successfully linked. If the card linking failed, you would be able to inspect the error object in the response section.

![API Playground Create Card](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/dashboard-create-card.gif "API Playground Create Card")

The `/delete` Card endpoint works similarly, with the difference being that you also get a dropdown to select the card you want to delete. The request and response objects are empty.

### SDKs

We recommend you use the secure and PCI compliant [SDKs](/select/sdks/web/v3) to create Cards in the Fidel environments. The SDKs work in both the live and test environment and use your SDK key. You can find your SDK keys (Live and Test) in your [Dashboard Account Settings](https://dashboard.fidel.uk/account/plan).

All our SDKs require a user to enter their card number and expiry date, along with the country of issue for the card. The SDKs won't require the CVV number, and will not make any active card checks against the cards. The SDKs will pre-populate the `countryCode` and the `programId` of the Program you want to link the card to. The card numbers are tokenised and transmitted directly from our secure pre-built SDKs to our API on submission. This way, your servers are never exposed to sensitive information, removing all PCI compliance requirements for you.

<div class="info-box">
  <small>Linking Multiple Cards</small><br/>
  To facilitate users linking multiple cards, add identifying key:value pairs from your system in the metadata field. You can read more in the <a href="/cards#Metadata">Metadata</a> section below.
</div>

### API

If you don't want to use our secure and PCI compliant SDKs, you must get PCI Compliant before using our Cards API.

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/f76ed1be-e434-480b-aa1d-ff48f548f62a/cards
  -H 'Content-Type: application/json'
  -H 'Fidel-Key: <KEY>'
  -d '{
  	"number": "4444000000004222",
    "expMonth": 10,
    "expYear": 2025,
    "countryCode": "GBR",
    "termsOfUse": true,
    "metadata": {
      "id": "my-brand-name-cards",
      "customKey1": "customValue1",
      "customKey2": "customValue2"
    }
}'
```

<div class="info-box">
    <small>Important note</small><br/>
    To use the <strong>Create Card</strong> endpoint, you must use the SDK key. Using the <strong>Create Card</strong> API endpoint with the Fidel live environment requires your company to be PCI Compliant.
</div>

## Card Object

The Fidel Card object holds the tokenised information about the physical card details submitted by your users. You can't access all the card details, just the tokenised data that allows you to uniquely identify a card. You will have access to the first six digits of the card number, last four digits of the card number, the expiry date, the issuing card scheme (Amex, Mastercard or Visa) and the country in which the card was issued. The CVV number is not needed to link a card, and you won't have access to it in the Fidel Card object.

```json
fileName:cardResponse.json
{
  "items": [
    {
      "accountId": "4f6cb653-5ceb-417a-8709-9cb2f8628691",
      "countryCode": "GBR",
      "created": "2021-02-05T18:15:56.202Z",
      "expDate": "2030-10-31T23:59:59.999Z",
      "expMonth": 10,
      "expYear": 2030,
      "firstNumbers": "444400",
      "id": "cce3e4d0-0a34-4f55-8725-e5f5acb5d0f9",
      "lastNumbers": "4002",
      "live": false,
      "metadata": {
        "id": "my-brand-name-cards",
        "customKey1": "customValue1",
        "customKey2": "customValue2"
      },
      "programId": "22610397-56a3-4770-9562-09405c4eedec",
      "scheme": "visa",
      "type": "visa",
      "updated": "2021-02-05T18:15:56.202Z"
    }
  ],
  "resource": "/v1/programs/22610397-56a3-4770-9562-09405c4eedec/cards",
  "status": 201,
  "execution": 47.506751
}
```

## Metadata

The Card object can also have associated metadata, i.e., auxiliary data to better describe or help tracking and working with cards. The metadata field is an object and it requires an `id` mandatory property, which works as a _non-unique index_ for retrieval purposes. It's recommended not to include unencoded confidential or sensitive data regarding the cardholder.

Via our API, when [creating a card](https://reference.fidel.uk/reference/create-card), you can set the `id` property of the metadata to a custom identifier, for example `my-brand-name-cards`. You can also [update a card's metadata](https://reference.fidel.uk/reference/update-card-metadata) at any time and additionally [retrieve a list of cards](https://reference.fidel.uk/v1/reference/list-cards-from-metadata-id) by using the metadata `id`.

## API Reference

If you're looking to find out more about our Cards API and how to use it with your application, please visit the [Fidel API Reference](https://reference.fidel.uk/reference/create-card).
