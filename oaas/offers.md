# Offers

Fidel's Offers marketplace is available on the **Offers** tab, where you can see all available offers to your reward programs.

## Offer Lifecycle

Offers can be accessed via the [Fidel Dashboard](https://dashboard.fidel.uk/offers/pending). They are grouped into six categories: Marketplace, Requests, Upcoming, Live, Expiring soon and Expired.

### Marketplace

All Fidel marketplace offers available to your reward programs will be visible here. These have not yet been added to your program.

### Requests

The Offers which require approval from the content provider will be available here until the Content Provider has accepted or declined the offer request.

### Upcoming

The Offer has at least one Location linked, but the `startDate` is in the future. Offers in this category will automatically become "live" when the `startDate` is reached.

### Live

The current date is between the Offer `startDate` and `endDate`. The Offers in this category have been added to one of your reward programs and you are tracking live transactions against them.

If no `endDate` is provided when creating an Offer, it will have a default end date of 12 months from its `startDate`. The Offer can be extended at any time during its lifetime to another 12 months from the current date by the Content Provider and you will be notified with the relevant webhook. Please see [webhooks](/docs/oaas/webhooks) for more details. Your own sourced offers are exempt from this requirement.

### Expiring soon

The Offer is currently live on your reward program, but is approaching its `endDate`. Offers in this category will automatically move to "Expired" when the `endDate` is reached.

The Dashboard includes an "expires in" filter that allows you to view Offers expiring within a specific number of days. The filter provides preset values for quick filtering and also accepts custom day values. By default, it displays Offers expiring within 30 days.

### Expired

The current date is after the Offer `endDate`. The Offers in this category have expired and you should not be tracking transactions against them past the `endDate`.

## Self-sourced Offers

The following sections are for any self-sourced offers which you want to add to your reward program. Fidel will only do the reconciliation and billing for Fidel marketplace offers. Any self-sourced offers will need to be created, maintained, reconciled and billed by the Publisher.

## Get Marketplace Offers

Retrieves all available offers from the Fidel Marketplace. Publishers use this endpoint to browse and discover offers from content providers and adopt them into their programs.

Filter by brand, channel, country, or network to surface the most relevant offers. By default, only available offers are returned, with pagination and sorting supported for easy browsing.

```sh
curl --request GET \
     --url 'https://api.fidel.uk/v1/marketplace-offers?order=asc' \
     --header 'Content-Type: application/json' \
     --header 'accept: application/json'
```

## Creating a Self-sourced Offer

There are multiple options for creating a new self-sourced Offer. You can create an Offer via the [Fidel Dashboard](https://dashboard.fidel.uk/offers/pending), in the Offers section. Alternatively, Developers can use the [Create Offer endpoint](https://fidel-oaas.readme.io/reference/create-offer) from the Offers API to create an Offer.

### Create an Offer via Dashboard

![Create Offer in Fidel Dashboard](https://docs.fidel.uk/assets/images/gifs/create-offers.gif "Create Offer in Fidel API Dashboard")

Once you have created an Offer, it will enter the Offer Lifecycle in the Requests category.

### Create an Offer via Offers API

Here's a cURL example of using the endpoint, with only the minimum required parameters:

```sh
curl -X POST https://api.fidel.uk/v1/offers \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
        "countryCode": "GBR",
        "name":"20% Off Netflix Subscription",
        "publisherId":"3693ac7e-3e2b-432c-8c60-2b786453ca9b",
        "brandId":"f8bdb5e7-85c3-4acb-8a59-1b7e9218e412",
        "startDate":"2020-04-25T00:00:00",
        "type":{
          "name":"discount",
          "value":20
        }
      }'
```

### Required Parameters

These are the minimum required parameters to create a new Offer:

- **`brandId`**: Unique identifier for the Brand presenting the Offer.
- **`countryCode`**: Country where the Offer will be available.
- **`name`**: Name of your Offer.
- **`publisherId`**: Unique identifier for you, the same as your Fidel `accountId`.
- **`startDate`**: The start date for the Offer. The `startDate` time will be a local time relative to the Location where the Offer is active.
- **`type: name`**: Type of the Offer. Valid names are `"amount"` and `"discount"`.
- **`type: value`**: Numeric value of the discount associated with the Offer.

Offers with the type `amount` will use the indicated country's currency and apply the value as the amount of the discount, for example, £20 Off. The `discount` type applies the value as a percentage discount, for example, 20% Off.

### Optional Parameters

There are a range of optional parameters available, which influence how the Offer behaves on the Fidel platform. You can read more about the endpoint's full specification on our [API Reference](https://fidel-oaas.readme.io/reference/create-offer).

- **`activation`**: An object, showing if the Offer needs activation or not. Default is `{ enabled: false }`.
- **`activation: enabled`**: Boolean showing if the Offer needs to be activated on Cards or not. If it's `true`, the activation object also has a `qualifiedTransactionsLimit` property. Please read the section below on [Offer Activation](#offers-with-activation) before using this parameter.
- **`activation: qualifiedTransactionsLimit`**: Number of Transactions to qualify for each Offer activation. Default is `1`.
- **`daysOfWeek`**: Array of numbers, with possible values from `0` to `6`, to indicate the days of the week. `0` = Sunday, `1` = Monday, etc.
- **`endDate`**: The date to automatically end the Offer. Same as `startDate`, the time will be a local time relative to the Location where the Offer was active. A default 12 months lifeline is added to the `endDate` if no `endDate` is provided. This can be extended at any time to further 12 months.
- **`funded: id`**: Unique identifier for the account that funds the Offer. For self-funded Offers, this is not required. In the test environment, all Offers are self-funded, so this will always be the same as your `accountId`.
- **`funded: type`**: Type of Offer funding. Possible values are `"merchant"`, `"card-linking"` and `"affiliate"`. In the test environment, you can only create card-linked Offers, so the funding type will always be `"card-linking"`.
- **`maxTransactionAmount`**: Deprecated in favor of `type: maxRewardAmount`. For example, a 10% reward with a max transaction amount of £100 can't generate a reward larger than £10, even if the transaction amount is higher than £100. The new value for `maxRewardAmount` will be `10` since we want to limit the reward to £10.
- **`minTransactionAmount`**: Minimum transaction amount to qualify for the offer. For example, if your offer is to save 25% on purchases over £50, then the Offer should have a `minTransactionAmount` of `£50`.
- **`metadata`**: Object with your own metadata, will be returned on the Offer object.
- **`returnPeriod`**: Number of days between when a Transaction was created and when a Transaction qualifies for the Offer. The qualified Transaction will have the `offer.qualificationDate` set to the creation date plus the number of days in the return period.
- **`schemes`**: Array of schemes for which a Transaction qualifies for the Offer. Possible values are `"amex"`, `"mastercard"` and `"visa"`.
- **`type: maxRewardAmount`**: Numeric value of the maximum amount to be awarded for the Offer. This only applies to `discount` type offers. For example, a 10% reward with a max reward amount of 10 can't generate a reward larger than £10, even if the transaction amount is higher than £100.
- **`targeting.name`**: Type of customer targeting applied to the offer.
  - `new` → Customer has not transacted within the defined lookback window
  - `lapsed` → Customer previously transacted, but has been inactive for a defined period
  - `existing` → Customer has transacted recently within the defined lookback window
- **`targeting.from`**: Defines the start of the lookback window in days. This field is required when using `lapsed` or `existing` targeting types. Example: If `from = 180`, the targeting evaluation starts from 180 days ago.
- **`targeting.to`**: Defines the end of the lookback window in days. This field is required when using `lapsed` or `existing` targeting types. Maximum supported value: `365`. Example: If `to = 365`, the targeting evaluation includes customer activity up to 365 days ago.

## Offer Targeting Fields

Offer Targeting Fields introduce native customer segment targeting directly into the Fidel offer model. Content Providers can define eligibility rules for New, Lapsed, and Existing customers directly on an offer, enabling Publishers to reward transactions based on historical shopping behaviour and customer lifecycle status. You can read more about the endpoint's full specification on our [Create Offer API](https://fidel-oaas.readme.io/reference/create-offer).

### Key Features

- Optional feature — existing offers remain unaffected
- Targeting rules can be updated on live offers
- Maximum lookback window: 365 days
- Changes to targeting rules trigger webhook notifications to Publishers
- Fidel does not validate customer segment tags — the Publisher remains the source of truth
- Supports targeting for:
  - New customers
  - Lapsed customers
  - Existing customers

### Offer Configuration

When creating or updating an offer, a targeting configuration can optionally be added to define:

**Customer segment type:**
- `new`
- `lapsed`
- `existing`

**Lookback window:**
- Defined using a day range
- Maximum supported range: 365 days

### Transaction Processing

When submitting transactions against a targeted offer, the Publisher includes a customer segment tag in the transaction payload. Fidel compares the submitted segment tag against the targeting rules configured on the offer to determine eligibility.

Fidel does not independently validate segment tags against transaction history. Validation is performed upstream by the Publisher.

### Eligibility Logic

**Match**
The transaction segment tag matches the offer targeting rules.
Result: Transaction is eligible and the reward is applied.

**No Match**
The transaction segment tag does not match the offer targeting rules.
Result: Transaction is ineligible and no reward is applied.

**No Targeting Defined**
The offer does not contain a targeting configuration.
Result: All transactions remain eligible (default behaviour).

### Supported Audience Types

| Audience Type | Definition | Goal |
|---|---|---|
| **New** | No spend at the brand within the defined lookback window (e.g. 0–90 days) | Acquire first-time shoppers |
| **Lapsed** | Last transaction occurred within a defined inactive period (e.g. 180–365 days ago) | Re-engage inactive customers |
| **Existing** | Customer has transacted within the defined recent period (e.g. within the last 90 days) | Increase loyalty and reward active shoppers |

### Webhook Notifications

Any update to a live offer's targeting configuration triggers a webhook notification to the Publisher.

This includes changes to:
- Customer segment type
- Lookback day range
- Targeting configuration values

This ensures Publishers remain synchronised with the latest eligibility rules applied to active offers.

## Multiple Offers on the same Brand and Location

Each Transaction can be rewarded only once. If there is more than one Offer for the same Brand in the same Location for which a Transaction qualifies, the Fidel API platform has to select one of them which will provide the reward.

The platform uses the following rules to select the Offer that will reward the cardholder:

1. In case both Offers generate different cashback values, the platform selects and qualifies the most valuable offer.
2. In case both Offers generate the same cashback values, the platform selects and qualifies the most recent offer.

**Example:**
Let's suppose the following offers are on the same brand and a cardholder made a transaction of £100 at a brand's location.

| Offer 15% Off | Offer £50 Off |
|---|---|
| `name: "discount"` | `name: "amount"` |
| `value: 15` | `value: 50` |

Applying the stacking rules, the above transaction will qualify for "Offer £50 Off" according to the first rule.

## Linking Locations to Offers

Before an Offer goes live and starts qualifying transactions, you will need to link Locations to the Offer. Developers can use the [Link Location to Offer API endpoint](https://fidel-oaas.readme.io/reference/link-location-to-offer) to link any Location to an Offer. Similarly, the Fidel API has an endpoint to [unlink a Location from an Offer](https://fidel-oaas.readme.io/reference/unlink-location-from-offer).

Here's a cURL example of using the endpoint, with two path parameters, for the `offerId` and `locationId`:

```sh
curl -X POST \
  https://api.fidel.uk/v1/offers/feb9af3c-9b4e-49df-bb8f-13ae4ad8cd22/locations/1af3b7a0-4bfd-4b5e-a285-fab1c8a8421d \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>'
```

### Linking Offers in the Dashboard

When you create an Offer in the Fidel Dashboard, the second step of the creation dialogue allows you to link Locations to the newly created Offer.

![Link Locations in Offer Creation](https://docs.fidel.uk/assets/images/gifs/create-offer-location.gif "Link Locations in Offer Creation")

If you need to link more Locations after you've created an Offer, the Locations list in the Dashboard has a menu button next to each Location, which opens a contextual menu. Selecting 'Link to offer' in the context menu will open a drawer that lets you select a possible Offer to link.

![Link to offer in Fidel Dashboard](https://docs.fidel.uk/assets/images/dashboard-link-location.png "Link to offer in Fidel Dashboard")

Alternatively, you can edit an Offer in the Fidel Dashboard, which will allow you to link more Locations in the second step of the Offer drawer.

![Edit Offer Link Locations](https://docs.fidel.uk/assets/images/gifs/dashboard-edit-offer.gif "Edit Offer Link Locations")

### Test and Live environments

It is important to note that when testing Offers with activation in the test environment, all test transactions created will be visible for testing purposes. In the live environment only Transactions for activated Offers on Cards will be received and qualified.

### Offers with Activation in the Dashboard

You can create Offers with activation in the Fidel Dashboard as well. When creating an Offer, check the **"Enable offer activation"** checkbox. That will reveal a "1" transactions field, which you can use to change the number for the qualified transactions limit.

![Create Offer with Activation](https://docs.fidel.uk/assets/images/gifs/create-offers.gif "Create Offer with Activation")

To activate an Offer on a Card using the Fidel Dashboard, you'll want to go to the Cards list. Each Card has a menu button next to them, which opens a contextual menu. Selecting 'Activate offer' in the context menu will open a drawer that lets you select a possible Offer to activate on the Card.

![Activate offer on Card in Fidel Dashboard](https://docs.fidel.uk/assets/images/dashboard-activate-offer.png "Activate offer on Card in Fidel Dashboard")

Once an offer is live, when a consumer makes a payment with a linked card at a participating location, the transaction is evaluated against the offer parameters to determine qualification. If the offer includes a return period, the transaction will only be qualified after that period has passed.

## Offer Object

The Offers API's central piece of data is the Fidel Offer object, which holds all the details about a card-linked Offer. The card-linked Offer has a set of parameters used to qualify any Card Transaction made at a participating Brand's linked Location.

The Offer object looks similar to the following and will be returned by the API in a multitude of situations. Transaction objects will also have a smaller version of the object inside, making it easier to retrieve the full Offer object if necessary.

```json
{
  "id": "feb9af3c-9b4e-49df-bb8f-13ae4ad8cd22",
  "accepted": true,
  "activation": {
    "enabled": true,
    "qualifiedTransactionsLimit": 1
  },
  "additionalTerms": null,
  "brandId": "f8bdb5e7-85c3-4acb-8a59-1b7e9218e412",
  "brandName": "API Reference",
  "brandLogoURL": "https://example.com/logo.png",
  "countryCode": "GBR",
  "created": "2020-07-29T13:45:36.191Z",
  "currency": "GBP",
  "daysOfWeek": [0, 1, 2, 3, 4, 5, 6],
  "endDate": null,
  "funded": {
    "id": "3693ac7e-3e2b-432c-8c60-2b786453ca9b",
    "type": "card-linking"
  },
  "fees":
  {
    "publisherFee": 5,
    "fidelFee": 5
  },
  "live": true,
  "locationsTotal": -1,
  "maxTransactionAmount": 0,
  "minTransactionAmount": 0,
  "metadata": null,
  "name": "5% Off Netflix Every Month",
  "origin": {
    "id": "3693ac7e-3e2b-432c-8c60-2b786453ca9b",
    "type": "card-linking"
  },
  "priority": 1,
  "publisherId": "3693ac7e-3e2b-432c-8c60-2b786453ca9b",
  "returnPeriod": 15,
  "schemes": ["amex", "mastercard", "visa"],
  "startDate": "2020-06-30T00:00:00",
  "supplier": null,
  "type": {
    "name": "discount",
    "maxRewardAmount": 10,
    "value": 5
  },
  "transactionSource": "oaas",
  "status": "live",
  "updated": "2020-08-22T14:47:37.479Z"
}
```

### Parameters

- **`id`** string: Unique identifier for the object.
- **`accepted`** boolean: Whether the Offer was accepted by the Brand. To send the Offer to a Brand for funding, see the [Send Offer to Brand API endpoint](https://fidel-oaas.readme.io/reference/send-offer-to-brand).
- **`activation`** object: Has an `enabled` boolean property, showing if the Offer needs activation or not. If the `enabled` flag is set to `true`, the activation object also has a `qualifiedTransactionsLimit` property, specifying the number of transactions to qualify for each Offer activation.
- **`additionalTerms`** string: Support for additional Terms & Conditions related to the Offer. `null` by default.
- **`brandId`** string: Unique identifier of the associated Brand.
- **`brandName`** string: Name of the associated Brand.
- **`brandLogoURL`** string: Logo URL of the associated Brand. `null` by default.
- **`countryCode`** string: ISO 3166-1 alpha-3 country code for the Country where the Offer is active.
- **`created`** date: ISO 8601 date and time in UTC representing the creation date for the Offer object.
- **`currency`** string: ISO 4217 currency code based on the `countryCode` value.
- **`daysOfWeek`** array of numbers: Array of numbers between `0` and `6` representing the days of the week for which the Offer is active. Starting with Sunday.
- **`endDate`** date: Date and time, in the `YYYY-MM-DDThh:mm:ss` format, for when the Offer expires. Note: Time is local to the Location. Defaults to 12 months for Offers that do not have an end date passed at creation phase.
- **`funded`** object: Contains an `id` property, with the unique identifier for the account that is funding the Offer. Also contains a string `type` property, for the type of account that is funding the Offer. The type can have one of the values `"merchant"` | `"card-linking"` | `"affiliate"`.
- **`live`** boolean: Whether the Offer should be created in the live or test Fidel environment.
- **`locationsTotal`** number: Total number of Locations linked to the Offer. (always -1 as we won't have locations in OaaS)
- **`maxTransactionAmount`** number: Maximum transaction amount generating a proportional reward.
- **`minTransactionAmount`** number: Minimum amount needed for a Transaction to qualify for the Offer.
- **`name`** string: Name of the Offer.
- **`metadata`** object: Metadata to be associated with the Offer. Defaults to `null`.
- **`origin`** object: Contains an `id` property, with the unique identifier for the account that created the Offer. Also contains a string `type` property, for the type of account that created the Offer. The type can have one of the values `"merchant"` | `"card-linking"` | `"affiliate"`.
- **`priority`** number: Not in use. Its value is always `1`.
- **`publisherId`** string: Unique identifier of the Publisher. Refers to `accountId`.
- **`returnPeriod`** number: Number of days before a Transaction qualifies for the Offer.
- **`schemes`** array of strings: Card Schemes for which the Offer is valid. Possible values in the array are `visa`, `mastercard` and `amex`.
- **`startDate`** date: Date and time, in the `YYYY-MM-DDThh:mm:ss` format, for when the Offer is activated and starts qualifying transactions.
- **`status`** string: Offer status corresponding to the state in the Offer Lifecycle. Can be one of `requests`, `upcoming`, `live` or `expired`.
- **`supplier`** object: Contains an `id` property, with the unique identifier for the account that supplies the Offer. Also contains a string `type` property, for the type of account that supplies the Offer. The type can have one of the values `"merchant"` | `"card-linking"` | `"affiliate"`. Defaults to `null`.
- **`type`** object: Represents the type of Offer: a fixed amount or a percentage of the original transaction. The `name` property can have one of the following two values: `amount` and `discount`. The `value` property has either the fixed amount of currency to be rewarded or the percentage value, depending on the Offer type. The `maxRewardAmount` property has a maximum fixed amount of currency to be rewarded for percentage typed Offers. `maxRewardAmount` only applies to `discount` Offers, and will be `null` for `amount` Offers.
- **`updated`** date: ISO 8601 date and time in UTC representing the last time the Offer object was updated.

## Extending Self-sourced Offers

If you need to extend a self-sourced Offer beyond its original `endDate`, you can update the Offer to set a new end date. This is useful when you want to continue running a successful campaign or need to accommodate changes in your promotion schedule.

### Extending an Offer via API

You can extend an Offer by using the [Update Offer endpoint](https://fidel-oaas.readme.io/reference/update-offer) from the Offers API. Simply provide a new `endDate` value that is later than the current end date.

Here's a cURL example of extending an Offer:

```sh
curl -X PATCH \
  https://api.fidel.uk/v1/offers/feb9af3c-9b4e-49df-bb8f-13ae4ad8cd22 \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
        "endDate": "2020-12-31T23:59:59"
      }'
```

### Extending an Offer via Dashboard

To extend an Offer using the Fidel Dashboard, navigate to the Offers section, select the Offer you want to extend, and click the edit button. You can then update the end date field to your desired date.

![Extend Offer in Fidel Dashboard](https://docs.fidel.uk/assets/images/extending_offer.png "Extend Offer in Fidel Dashboard")

### Important Considerations

- The new `endDate` must be in the future from the current date.

## Deleting Offers

You can delete offers in the [Fidel API Dashboard](https://dashboard.fidel.uk/offers/pending), or using the API's [Delete Offer endpoint](https://fidel-oaas.readme.io/reference/delete-offer).

```sh
curl -X DELETE \
  https://api.fidel.uk/v1/offers/ec80c3a1-0899-4e10-abdf-2dfb699b509c \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63'
```

## Get Marketplace Offers

Retrieves all available offers from the Fidel Marketplace. Publishers use this endpoint to browse and discover offers from content providers and adopt them into their programs.

Filter by brand, channel, country, or network to surface the most relevant offers. By default, only available offers are returned, with pagination and sorting supported for easy browsing.

```sh
curl --request GET \
     --url 'https://api.fidel.uk/v1/marketplace-offers?order=asc' \
     --header 'Content-Type: application/json' \
     --header 'accept: application/json'
```

## API Reference

If you're looking to find out more about our Offers API and how to use it with your application, please visit the [Fidel API Reference](https://fidel-oaas.readme.io/reference).
