# Cards

## Card Linking

To be able to participate in your program, users must link their cards to it first. You can implement this in your application using Fidel API's [SDKs](/stream/sdks/web/v3) and APIs.

By using Fidel API's SDKs to link cards to a program, card details are sent directly to Fidel API through an encrypted connection without exposing your servers to sensitive information. The SDKs also take care of all relevant PCI compliance requirements.

Note that to use the APIs (without the SDK) to create cards, you must be PCI compliant. [Contact us](https://fidelapi.com/contact) to find out more.

### Tokenization

Fidel API never stores the full card number or CVV details. Fidel API tokenizes the card details and exchanges the card identifier only with the card networks to retrieve the transactions made using the cards.

To facilitate users linking multiple cards, developers can add identifying key-value pairs from your system in the card’s metadata. You can read more in the [Metadata section](#metadata) below.

### What Is Card Verification

When linking a card, cardholders are required to complete one of the two available verification processes to verify card ownership to ensure we protect them.

When linking a card, a property `verificationMethod` (included on the response of the create card endpoint) will identify what verification method that card is meant to follow - `3ds` or `microcharge`.

### 3DS (3D Secure)

3D Secure is a card verification method that is meant to enhance security measures for shoppers and vendors alike. This verification process consists of the following steps:

1. The cardholder links their card to a Fidel API program using the Verified Enrollment SDK or via the Create Card and the Create Consent endpoints.
2. When calling the Create Consent endpoint, you would need to provide more cardholder details (i.e cardholder's name and CVV/CVC)
3. After inputting that information:
    1. If the verification is required, it should proceed to the next steps
    2. If no extra verification is required, the card consent will be verified
4. Fingerprinting process will take place (this is part of the network's requirements)
5. When the fingerprinting process completes:
    1. If there is no need for further verification (frictionless), card consent will be verified
    2. If there is a need for challenge (consent `status` is `awaiting` ) a challenge prompt will be shown, for the cardholder to input a code from a known device (2FA)
6. After inputting the correct code, consent will be verified

If for some reason the verification process is interrupted, the cardholder will have to wait an hour for the consent to be automatically revoked (for security reasons).

### Microcharge

This verification process consists of the following steps:

1. The cardholder links their card to a Fidel API program using the Verified Enrollment SDK or via the Create Card and the Create Consent endpoints.
2. Fidel API will charge the card for a nominal amount (equivalent to USD$0.50-1.00), which will be refunded within 72 hours.
3. A person with access to the card statement will identify the charged amount from the statement.
4. Consent is verified by inputting the exact amount charged to the card in the SDK or in the Verify Consent endpoint. After this, the card will be fully linked to the program, and transactions will start coming through.

Additionally, below are the key points for consideration related to the card verification process.

- The field `verificationStatus` controls the verification process. This field has three possible values:
- `"unverified"` - if there was no successful verification yet
- `"verified" `- after the user has provided the correct verification amount
- `"error"` - in the case of an error, for example in the charge
- Transaction data will come through only after a card has been verified.
- The charge to the card could fail. If the charge fails, the Create Consent endpoint will return an error.
- The cardholder has a maximum of three attempts to confirm the charged amount, after which they will be blocked from further attempts.
- Cards that are not verified will be purged from the Fidel API system 48 hours after linking.
- If the cardholder has any issues with card linking and verification, the card can be deleted from the Dashboard to restart the linking from the beginning, or can be automatically purged after 48 hours.
- When linking a Fidel API test card, type in $0.67 for verification.

## Supported Cards and Transactions

The Transaction Stream API supports the following card types and transactions:

- Credit cards
- Debit cards
- Virtual cards \*
- Online transactions
- Transactions via tap, swipe, insert

\* Some virtual cards which use a child PAN may need to be enrolled with the parent PAN.

Not supported:

- Encryption verification transactions
- Transactions that don’t travel through the network (e.g. ON-US, some recurring payments, some subscriptions of digital services) \*\*
\*\* Specific scenarios are geographically dependent.

## Creating Test Cards on the Fidel API Dashboard

You can create and delete test cards on the [API Playground](https://dashboard.fidel.uk/playground).

To create a card associated to a program, follow these steps:

1. In the **Playground** area, select the name of the API (`Transaction Stream`) in the dropdown.
2. Under **CARDS**, select `Create card`.
3. Select the program to which you want to associate the new card.
4. Once you select a program, the `/programs/program_id/cards` POST request will be updated with the program identifier for the selected program.
5. The request body on the right is already pre-filled with the following information:
- the country code,
- expiration date,
- a card number,
- whether the card holder agrees with the terms of service.
You can edit all the properties in the request body before running the request. You can use any of the available testing card numbers listed above, an expiry date in the future, and a three-letter `countryCode`. `termsOfUse` is set to true to simulate that the user agreed to the terms of use and opted in.
6. Click `Run` to execute the request.
7. If the card was successfully linked, you can inspect the card object in the response section.
If the card linking failed, you can inspect the error object in the response section.

The **Delete Card** option works similarly, with the difference being that you also get a dropdown to select the card you want to delete. The request and response objects are empty.

## Test Card Numbers

For security purposes, the Fidel API test environment doesn't accept live card numbers. Fidel API has a range of test card numbers you can use while integrating or testing the API: `4444000000004***` where `*` can be any digit. For example, `4444000000004278` is a valid test card number. The test card numbers work in the [Playground](https://dashboard.fidel.uk/playground), with the Fidel API SDKs and with the Cards APIs.

When linking a Fidel API test card, type in `$0.67` for verification.

## For Developers

### Creating Test Cards Via SDK And API

There are multiple ways you can test card linking on the Fidel API platform before going live:

- Using the UI of the Fidel API dashboard (see in the previous sections)
- With the Fidel API SDKs
- With API calls using the Cards API

For integrating card linking into your application, we recommend using one of Fidel API’s PCI-compliant [SDKs](/stream/sdks/web/v3). You can choose from Web (JavaScript) and Mobile (iOS, Android, ReactNative) SDKs. Fidel API also provides a Cards API that can be used for linking cards to your program, but it requires you to be PCI compliant before using it. [Contact us](https://fidelapi.com/contact) for more information.

The following sections give you an overview of how to create a test card with the Fidel API SDKs and API endpoints. The possible test card numbers that you can use are listed in the [Test Card Numbers](#test-card-numbers) section above.

#### SDKs

We recommend you use the secure and PCI-compliant Web SDK or the Mobile SDKs to create cards in Fidel API environments. The [SDKs](/stream/sdks/web/v3) work in both the live and test environment and they use your SDK keys. You can find your SDK keys (live and test) in your [Dashboard Account Settings](https://dashboard.fidel.uk/account/plan).

All of Fidel API's SDKs require a user to enter their card number and expiration date, along with the country of issue for the card. The SDKs won't require the CVV number, and will not make any active card checks against the cards. The SDKs will pre-populate the countryCode and the programId of the Program you want to link the card to. The card numbers are tokenized and transmitted directly from our secure pre-built SDKs to the API on submission. This way, your servers are never exposed to sensitive information, removing all PCI compliance requirements for you.

##### Linking Multiple Cards

To facilitate users linking multiple cards, add identifying key:value pairs from your system in the metadata field. You can read more in the [Metadata](#metadata) section below.

#### API

If you don't want to use Fidel API's secure and PCI compliant SDKs, **you must become PCI Compliant** before using the Cards API. [Contact us](https://fidelapi.com/contact) for more information.

**Start the card enrollment:**

```sh
curl -X POST \
https://api.fidel.uk/v1/programs/f76ed1be-e434-480b-aa1d-ff48f548f62a/cards
-H 'Content-Type: application/json'
-H 'Fidel-Key: pk_test_62f02030-0409-4eb5-ab94-6eff05b3d888'
-d '{
    "number": "4444000000004222",
    "expMonth": 10,
    "expYear": 2025,
    "countryCode": "USA",
    "termsOfUse": true,
    "metadata": {
        "id": "my-brand-name-cards",
        "customKey1": "customValue1",
        "customKey2": "customValue2"
    }
}'
```

**Create a consent:**

```sh
curl -X POST \
https://api.fidel.uk/v1/cards/f76ed1be-e434-480b-aa1d-ff48f548f62a/consents \
-H 'Content-Type: application/json' \
-H 'Fidel-Key: pk_test_62f02030-0409-4eb5-ab94-6eff05b3d888' \
-d '{
    "number": "4444000000004222",
    "expMonth": 10,
    "expYear": 2025,
    "countryCode": "USA"
    "cvc": "123", // required if verificationMethod is 3ds
    "cardholderName": "John Doe" // required if verificationMethod is 3ds
}'
```

**Verify card consent:**

```sh
curl -X POST \
https://api.fidel.uk/v1/cards/f76ed1be-e434-480b-aa1d-ff48f548f62a/consents/612ff311-dc0c-448d-8199-5a371f07433d/actions/verify \
-H 'Content-Type: application/json' \
-H 'Fidel-Key: pk_test_62f02030-0409-4eb5-ab94-6eff05b3d888' \
-d '{
    "code": 0.67 // only if verificationMethod is microcharge
}'
```

**Important note:** To use the Create Card endpoint, you must use the SDK key.

Using the Create Card API endpoint in the live environment requires your company to be PCI Compliant. If you want to use the API instead of the SDKs, please [contact us](https://fidelapi.com/contact).

### Card Object

The card object holds the tokenized information about the physical card details submitted by your users. You can't access all the card details, just the tokenized data that allows you to uniquely identify a card. You will have access to the first six digits of the card number, last four digits of the card number, the expiration date, the issuing card network, and the country in which the card was issued. The CVV number is not needed to link a card, it’s not in the card object.

The card object includes the following data:

| Field name         | Description                                                                                | Type                            |
| ------------------ | ------------------------------------------------------------------------------------------ | ------------------------------- |
| accountId          | The unique identifier of the user account at Fidel API                                     | string                          |
| countryCode        | The country code of the issuing bank                                                       | string                          |
| created            | The timestamp in UTC format when the card object was created in the Fidel API database     | string, timestamp in UTC format |
| expDate            | The expiration date of the card, based on the the last day of the expiration month         | string, timestamp in UTC format |
| expMonth           | The expiration month of the card                                                           | number                          |
| expYear            | The expiration year of the card                                                            | number                          |
| firstNumbers       | The first six numbers of the card, used for helping users identify their cards             | string                          |
| id                 | The tokenized card identifier used by Fidel API                                            | string                          |
| lastNumbers        | The last four numbers of the card, used for helping users identify their cards             | string                          |
| live               | Indicates whether the card is a live card                                                  | boolean                         |
| metadata           | Allows you to add key-value pairs to the card, e.g. to be able to add your own identifiers |                                 |
| programId          | The identifier of the program to which the card is linked                                  | string                          |
| scheme             | The card network (scheme) to which the card belongs to                                     | string                          |
| type               | The card network (scheme) to which the card belongs to. Identical to the scheme field.     | string                          |
| updated            | The timestamp of the last modification of the card object                                  | string, timestamp in UTC format |
| verificationMethod | The verification method to be used ("3ds" or "microcharge")                      | string                          |
| verificationStatus | The status of the verification ("unverified", "verified", or "error")                      | string                          |

An example of the API response when you create a card object:

```json
{
    "items": [
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
        "metadata": {
            "id": "my-brand-name-cards",
            "customKey1": "customValue1",
            "customKey2": "customValue2"
        },
        "programId": "3312c23b-97c1-4fab-aa63-123456789012",
        "scheme": "visa",
        "type": "visa",
        "updated": "2021-07-28T12:52:46.840Z",
        "verificationMethod": "microcharge",
        "verificationStatus": "unverified"
    }
    ],
    "resource": "/v1/programs/22610397-56a3-4770-9562-09405c4eedec/cards",
    "status": 201,
    "execution": 47.506751
}
```

### Metadata

The card object can also have associated metadata, with an id property that is a non-unique index. When creating a card, you can set the id property of the metadata to a custom identifier, for example: `my-brand-name-cards`. Later you can use the Cards API to retrieve a list of cards by using the metadataId. Learn more in the API Reference for the [List Cards from Metadata ID endpoint](https://transaction-stream.fidel.uk/reference/list-cards-from-metadata-id).

### API Reference

If you're looking to find out more about our Cards API and how to use it with your application, please visit the [API Reference](https://transaction-stream.fidel.uk/reference/create-card).