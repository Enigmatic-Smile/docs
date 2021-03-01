# Offers

Fidel Offers help you create and manage card-linked offers with various retailers – all in one place. The Fidel Offers API serves as a Transaction qualification engine for Fidel Offers. Developers can create Offers via the [Fidel Offers API](https://reference.fidel.uk/v1/reference#create-offer), which allows your application to create and update Offers, link and unlink Locations to the created Offers, activate and deactivate Offers on specific Cards and send the Offers for approval to a Brand. Brands and Merchants can interact with the Fidel Offers by using the [Fidel CLO Dashboard](https://clo.fidel.uk).

## Offer Lifecycle

Offers can be accessed via the [Fidel Dashboard](https://dashboard.fidel.uk/offers/pending). They are grouped into four categories: Requests, Upcoming, Live and Expired.

![Fidel Dashboard with Offers](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/dashboard-offers.png "Fidel Dashboard with Offers")

### Requests

The Offer `startDate` is set in the future, and/or there are no linked Locations.

### Upcoming

The Offer has at least one Location linked, but the `startDate` is in the future. Offers in this category will automatically become "live" when the `startDate` is reached.

### Live

The current date is between the Offer `startDate` and `endDate`. The Offers in this category are qualifying Transactions at the locations linked to them.

### Expired

The current date is after the Offer `endDate`. The Offers in this category have stopped qualifying Transactions.

## Create Offer

There are multiple options for creating a new Offer. Developers can use the [Create Offer endpoint](https://reference.fidel.uk/v1/reference#create-offer) from the Offers API to create an Offer.

Here's a cURL example of using the endpoint, with only the minimum required parameters:

```sh
curl -X POST https://api.fidel.uk/v1/offers \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
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

* `brandId`: Unique identifier for the Brand presenting the Offer.
* `countryCode`: Country where the Offer will be available.
* `name`: Name of your Offer.
* `publisherId`: Unique identifier for you, the same as your Fidel `accountId`.
* `startDate`: The start date for the Offer. The `startDate` time will be a local time relative to the Location where the Offer is active.
* `type: name`: Type of the Offer. Valid names are `"amount"` and `"discount"`.  
* `type: value`: Numeric value of the discount associated with the Offer.

Offers with the type `amount` will use the indicated country's currency and apply the value as the amount of the discount, for example, `£20 Off`. The `discount` type applies the value as a percentage discount, for example, `20% Off`.

### Optional Parameters

There are a range of optional parameters available, which influence how the Offer behaves on the Fidel platform. You can read more about the endpoint's full specification on our [API Reference](https://reference.fidel.uk/reference#create-offer).

* `activation`: An object, showing if the Offer needs activation or not. Default is `{ enabled: false }`.
* `activation: enabled`: Boolean showing if the Offer needs to be activated on Cards or not. If it's `true`, the `activation` object also has a `qualifiedTransactionsLimit` property. **Please read the section below on Offer Activation before using this parameter.**
* `activation: qualifiedTransactionsLimit`: Number of Transactions to qualify for each Offer activation. Default is 1.
* `daysOfWeek`: Array of numbers, with possible values from 0 to 6, to indicate the days of the week. 0 = Sunday, 1 = Monday, etc.
* `endDate`: The date to automatically end the Offer. Same as `startDate`, the time will be a local time relative to the Location where the Offer was active.
* `funded: id`: Unique identifier for the account that funds the Offer. For self-funded Offers, this is not required. In the test environment, all Offers are self-funded, so this will always be the same as your `accountId`.
* `funded: type`: Type of Offer funding. Possible values are `"merchant"`, `"card-linking"` and `"affiliate"`. In the test environment, you can only create card-linked Offers, so the funding type will always be `"card-linking"`.
* `maxTransactionAmount`: Maximum amount for a Transaction to qualify for the Offer. For example, suppose your promotion sounded something similar to "Save 25% on purchases over £50, save 40% on purchases over £100". In that case, the first Offer should have a `maxTransactionAmout` of £100.
* `minTransactionAmount`: Maximum amount for a Transaction to qualify for the Offer. For example, suppose your promotion sounded something similar to "Save 25% on purchases over £50". In that case, the Offer should have a `minTransactionAmount` of £50.
* `metadata`: Object with your own metadata, will be returned on the Offer object.
* `returnPeriod`: Number of days between when a Transaction was created and when a Transaction qualifies for the Offer. The qualified Transaction will have the `offer.qualificationDate` set to the creation date plus the number of days in the return period.
* `schemes`: Array of schemes for which a Transaction qualifies for the Offer. Possible values are `"amex"`, `"mastercard"` and `"visa"`.

### Create an Offer in the Dashboards

Alternatively, you can create an Offer via the [Fidel Dashboard](https://dashboard.fidel.uk/offers/pending), in the Offers section. If you have an account in the [Offers Dashboard](https://clo.fidel.uk), you can create an Offer there as well.

![Create Offer in Fidel Dashboard](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/create-offers.gif "Create Offer in Fidel Dashboard")

Once you have created an Offer, it will enter the Offer Lifecycle in the Requests category.

## Linking Locations to Offers

Before an Offer goes live and starts qualifying transactions, you will need to link Locations to the Offer. Developers can use the [Link Location to Offer](https://reference.fidel.uk/reference#add-location-to-offer) API endpoint to link any Location to an Offer. Similarly, the Fidel API has an endpoint to [unlink a Location from an Offer](https://reference.fidel.uk/reference#unlink-location-to-offer).

Here's a cURL example of using the endpoint, with two path parameters, for the `offerId` and `locationId`:

```sh
curl -X POST \
  https://api.fidel.uk/v1/offers/feb9af3c-9b4e-49df-bb8f-13ae4ad8cd22/locations/1af3b7a0-4bfd-4b5e-a285-fab1c8a8421d \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63'
```

### Linking All Brand Locations to an Offer

Developers can use the [Link All Brand Locations to Offer](https://reference.fidel.uk/reference#link-all-program-locations-to-offer) API endpoint to link all Location from a Brand to an Offer.

Here's a cURL example of using the endpoint, with the two required path parameters, for the `offerId` and `programId`:

```sh
curl -X POST \
  https://api.fidel.uk/v1/offers/feb9af3c-9b4e-49df-bb8f-13ae4ad8cd22/programs/0228b979-6f7c-4238-a309-40f9d6efd3ea/locations \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63'
```

### Linking Offers in the Dashboard

When you create an Offer in the Fidel Dashboard, the second step of the creation dialogue allows you to link Locations to the newly created Offer.

![Link Locations in Offer Creation](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/create-offer-location.gif "Link Locations in Offer Creation")

If you need to link more Locations after you've created an Offer, the [Locations list in the Dashboard](https://dashboard.fidel.uk/locations) has a menu button next to each Location, which opens a contextual menu. Selecting 'Link to offer' in the context menu will open a drawer that lets you select a possible Offer to link.

![Link to offer in Fidel Dashboard](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/dashboard-link-location.png "Link to Offer in Fidel Dashboard")

Alternatively, you can edit an Offer in the Fidel Dashboard, which will allow you to link more Locations in the second step of the Offer drawer.

![Edit Offer Link Locations](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/dashboard-edit-offer.gif "Edit Offer Link Locations")
## Offers with Activation

Offers with activation require an Offer to be activated on a Card before they can go through the qualification process. Developers can use the Offers API to specify an Offer requires activation. When [creating an Offer](https://reference.fidel.uk/v1/reference#create-offer), the `activation` object should have the `enabled: true` property and a `qualifiedTransactionsLimit` property higher or equal to 1. The `qualifiedTransactionsLimit` property specifies how many Transactions will be qualified for each Offer activation. Here's a cURL example:

```sh
curl -X POST https://api.fidel.uk/v1/offers \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
  -d '{
        "countryCode": "GBR",
        "name":"20% Off Netflix Subscription",
        "publisherId":"3693ac7e-3e2b-432c-8c60-2b786453ca9b",
        "brandId":"f8bdb5e7-85c3-4acb-8a59-1b7e9218e412",
        "startDate":"2020-04-25T00:00:00",
        "type":{
          "name":"discount",
          "value":20
        },
        "activation":{
          "enabled":true,
          "qualifiedTransactionsLimit":1
        }
      }'
```

Offers need to be activated on Cards before the purchase to receive and qualify Transactions for Offers with activation. Developers can do that by using the [Activate Offer on Card](https://reference.fidel.uk/reference#activate-offer-on-card) API endpoint as follows:

```sh
curl -X POST \
  https://api.fidel.uk/v1/offers/:offerId/cards/:cardId \
  -H 'content-type: application/json' \
  -H 'fidel-key: your-secret-key'
```

After an Offer is activated on a Card, it will qualify the number of Transactions specified by the `qualifiedTransactionsLimit` value. After the limit of Transactions is qualified, the Offer is automatically deactivated from the Card. If developers need to deactivate an Offer from the Card for any reason before that event, they can use the [Deactivate Offer on Card](https://reference.fidel.uk/reference#unlink-card-from-offer) API endpoint.

Note that when you link Locations to an Offer with activation, you will only receive Transactions from Cards where the Offer has been activated on. To receive all Transactions from all Cards, you will need to disable the Offer activation with `activation: { enabled: false, qualifiedTransactionsLimit: 1 }` or unlink the Locations from the Offer.

<div class="info-box">
    <small>Test and Live environments</small><br/>
    It is important to note that when testing Offers with activation in the test environmentm all test transactions created will be visible for testing purposes. In the live environment only Transactions for activated Offers on Cards will be received and qualified.
</div>

### Offers with Activation in the Dashboard

You can create Offers with activation in the [Fidel Dashboard](https://dashboard.fidel.uk/offers/pending) as well. When creating an Offer, check the "Enable offer activation" checkbox. That will reveal a "1" transactions field, which you can use to change the number for the qualified transactions limit.

![Create Offer with Activation](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/create-offers.gif "Create Offer with Activation")

To activate an Offer on a Card using the Fidel Dashboard, you'll want to go to the [Cards list](https://dashboard.fidel.uk/cards). Each Card has a menu button next to them, which opens a contextual menu. Selecting 'Activate offer' in the context menu will open a drawer that lets you select a possible Offer to activate on the Card.

![Activate offer on Card in Fidel Dashboard](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/dashboard-activate-offer.png "Activate offer on Card in Fidel Dashboard")

## Transaction Qualification

When an Offer is Live, each transaction made by an enrolled Card will be analysed against the Offer parameters and will be qualified against the Offer. If the Offer has a return period, transactions will only qualify for the Offer after the return period has passed.

In both cases, an `offer` object is appended to the original Transaction object containing the qualification result data. In case the Transaction qualifies, `cashback` and `performanceFee` properties are calculated and the `qualified` property is set to `true`. If the qualified Transaction was for an Offer with a return period, the `offer.qualificationDate` is set to the Transaction creation date plus the number of days in the Offer return period. If the Transaction does not qualify, `cashback` and `performanceFee` properties will have a `0` value. The `qualified` property will be set to `false` and a message explaining why the Offer did not qualify will be set in the `message` property. If a Transaction does not qualify for multiple Offers, the rejection `message` is for the most recent Offer. You can see examples for qualified and non-qualified Transactions below.

```json
fileName:qualified-transaction.json
{
  "id": "7fdfd5d8-9589-402f-8477-4a727ad239a2",
  "accountId": "4ed4b62b-aa4c-43a1-8064-da6d1368e17a",
  "programId": "6e38aa0c-b7ef-46bd-b1bd-c07c648d9cba",
  "datetime": "2020-04-21T19:12:01",
  "created": "2020-04-21T19:12:01.744Z",
  "updated": "2020-04-21T19:12:01.744Z",
  "auth": true,
  "cleared": false,
  "amount": 5,
  "currency": "USD",
  "wallet": null,
  "card": {
    "id": "bc538b71-31c5-4699-840a-6d4a08693314",
    "firstNumbers": "555500",
    "lastNumbers": "5001",
    "scheme": "visa",
    "metadata": {
      "customKey1": "customValue1",
      "customKey2": "customValue2"
    }
  },
  "brand": {
    "id": "9d136f2e-df99-4a08-a0a5-3bc1534b7db9",
    "name": "Starbucks",
    "logoUrl": null
  },
  "location": {
    "id": "7a916fbd-70a0-462f-8dbc-bd7dbfbea160",
    "address": "5th Avenue",
    "city": "New York",
    "postcode": "120000",
    "countryCode": "USA",
    "timezone": "America/New_York",
    "geolocation": {
      "latitude": 51.5152346,
      "longitude": -0.1310718
    },
    "metadata": {
      "customKey1": "customValue1",
      "customKey2": "customValue2"
    }
  },
  "offer": {
    "id": "7e55eeae-99d6-4daf-b8c4-ac9ca660e964",
    "cashback": 20,
    "message": [],
    "performanceFee": 3.2,
    "qualified": true,
    "qualificationDate": null
  },
  "identifiers": {
    "MID": "123456789",
    "mastercardTransactionSequenceNumber": "0000000000000",
    "mastercardRefNumber": "AABBCCDDE",
    "amexApprovalCode": "AA00BB",
    "visaAuthCode": "000000",
    "mastercardAuthCode": null
  }
}
```

```json
fileName:non-qualified-transaction.json
{
  "...": "...",
  "updated": "2020-04-19T12:12:00.000Z",
  "offer": {
    "id": "7e55efae-99d6-4daf-b8c4-ac9ca660e864",
    "cashback": 0,
    "performanceFee": 0,
    "qualified": false,
    "qualificationDate": null,
    "message": [
      "Transaction amount of 5 USD is lower than the offer's minimum transaction amount of 10 USD"
    ]
  }
}
```

## Offer Object

The Offers API's central piece of data is the Fidel Offer object, which holds all the details about a card-linked Offer. The card-linked Offer has a set of parameters used to qualify any [Card Transaction](https://fidel.uk/docs/transactions) made at a participating Brands' linked Location.

The Offer object looks similar to the following and will be returned by the API in a multitude of situations. Transaction objects will also have a smaller version of the object inside, making it easier to retrieve the full Offer object if necessary.

```json
fileName:offer.json
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
  "feeSplit": 70,
  "funded": {
    "id": "3693ac7e-3e2b-432c-8c60-2b786453ca9b",
  	"type": "card-linking"
  },
  "live": true,
  "locationsTotal": 240,
  "maxTransactionAmount": 0,
  "minTransactionAmount": 0,
  "metadata": null,
  "name": "£5 Off Netflix Every Month",
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
  	"name": "amount",
  	"value": 5
  },
  "updated": "2020-08-22T14:47:37.479Z"
}
```

### Parameters

<dl>
  <div>
    <dt>
      <span><code>id</code></span>
      <em>string</em>
    </dt>
    <dd>Unique identifier for the object.</dd>
  </div>
  <div>
    <dt>
      <span><code>accepted</code></span>
      <em>boolean</em>
    </dt>
    <dd>Whether the Offer was accepted by the Brand. To send the Offer to a Brand for funding, see the <a href="https://reference.fidel.uk/reference#send-offer-to-brand">Send Offer to Brand</a> API endpoint.</dd>
  </div>
  <div>
    <dt>
      <span><code>activation</code></span>
      <em>object</em>
    </dt>
    <dd>Has an <code>enabled</code> <em>boolean</em> property, showing if the Offer needs activation or not. If the <code>enabled</code> flag is set to true, the <code>activation</code> object also has a <code>qualifiedTransactionsLimit</code> property, specifying the number of transactions to qualify for each Offer activation. You can read more about Offers with Activation below.</dd>
  </div>
  <div>
    <dt>
      <span><code>additionalTerms</code></span>
      <em>string</em>
    </dt>
    <dd>Support for additional Terms & Conditions related to the Offer. <code>null</code> by default. Accepts Markdown link syntax. E.g. [Additional Terms](https://fidel.uk/terms).</dd>
  </div>
  <div>
    <dt>
      <span><code>brandId</code></span>
      <em>string</em>
    </dt>
    <dd>Unique identifier of the associated Brand.</dd>
  </div>
  <div>
    <dt>
      <span><code>brandName</code></span>
      <em>string</em>
    </dt>
    <dd>Name of the associated Brand.</dd>
  </div>
  <div>
    <dt>
      <span><code>brandLogoURL</code></span>
      <em>string</em>
    </dt>
    <dd>Logo URL of the associated Brand. <code>null</code> by default.</dd>
  </div>
  <div>
    <dt>
      <span><code>countryCode</code></span>
      <em>string</em>
    </dt>
    <dd><a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3">ISO 3166-1 alpha-3</a> country code for the Country where the Offer is active.</dd>
  </div>
  <div>
    <dt>
      <span><code>created</code></span>
      <em>date</em>
    </dt>
    <dd><a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> date and time in UTC representing the creation date for the Offer object.</dd>
  </div>
  <div>
    <dt>
      <span><code>currency</code></span>
      <em>string</em>
    </dt>
    <dd><a href="https://en.wikipedia.org/wiki/ISO_4217">ISO 4217</a> currency code based on the <code>countryCode</code> value.</dd>
  </div>
  <div>
    <dt>
      <span><code>daysOfWeek</code></span>
      <em>array: number</em>
    </dt>
    <dd>Array of numbers between 0 and 6 representing the days of the week for which the Offer is active. Starting with Sunday.</dd>
  </div>
  <div>
    <dt>
      <span><code>endDate</code></span>
      <em>date</em>
    </dt>
    <dd>Date and time, in the <code>YYYY-MM-DDThh:mm:ss</code> format, for when the Offer expires. Note: Time is local to the Location. Defaluts to <code>null</code> for Offers that do not expire.</dd>
  </div>
  <div>
    <dt>
      <span><code>feeSplit</code></span>
      <em>number</em>
    </dt>
    <dd>Percentage of the performance fee to be charged by Fidel to the participating Brand.</dd>
  </div>
  <div>
    <dt>
      <span><code>funded</code></span>
      <em>object</em>
    </dt>
    <dd>Contains an <code>id</code> property, with the unique identifier for the account that is funding the Offer. Also contains a <em>string</em> <code>type</code> property, for the type of account that is funding the Offer. The type can have one of the values <code>"merchant" | "card-linking" | "affiliate"</code>.</dd>
  </div>
  <div>
    <dt>
      <span><code>live</code></span>
      <em>boolean</em>
    </dt>
    <dd>Whether the Offer should be created in the live or test Fidel environment.</dd>
  </div>
  <div>
    <dt>
      <span><code>locationsTotal</code></span>
      <em>number</em>
    </dt>
    <dd>Total number of Locations linked to the Offer.</dd>
  </div>
  <div>
    <dt>
      <span><code>maxTransactionAmount</code></span>
      <em>number</em>
    </dt>
    <dd>Maximum amount needed for a Transaction to qualify for the Offer.</dd>
  </div>
  <div>
    <dt>
      <span><code>minTransactionAmount</code></span>
      <em>number</em>
    </dt>
    <dd>Minimum amount needed for a Transaction to qualify for the Offer.</dd>
  </div>
  <div>
    <dt>
      <span><code>name</code></span>
      <em>string</em>
    </dt>
    <dd>Name of the Offer.</dd>
  </div>
  <div>
    <dt>
      <span><code>metadata</code></span>
      <em>object</em>
    </dt>
    <dd>Metadata to be associated with the Offer. Defaults to <code>null</code>.</dd>
  </div>
  <div>
    <dt>
      <span><code>origin</code></span>
      <em>object</em>
    </dt>
    <dd>Contains an <code>id</code> property, with the unique identifier for the account that created the Offer. Also contains a <em>string</em> <code>type</code> property, for the type of account that created the Offer. The type can have one of the values <code>"merchant" | "card-linking" | "affiliate"</code>.</dd>
  </div>
  <div>
    <dt>
      <span><code>priority</code></span>
      <em>number</em>
    </dt>
    <dd>Number, starting with 1, representing the stacking priority for the publisher. By default, the publisher that invites the Brand gets the top priority, 1.</dd>
  </div>
  <div>
    <dt>
      <span><code>publisherId</code></span>
      <em>string</em>
    </dt>
    <dd>Unique identifier of the Publisher. Refers to <code>accountId</code>.</dd>
  </div>
  <div>
    <dt>
      <span><code>returnPeriod</code></span>
      <em>number</em>
    </dt>
    <dd>Number of days before a Transaction qualifies for the Offer.</dd>
  </div>
  <div>
    <dt>
      <span><code>schemes</code></span>
      <em>array: string</em>
    </dt>
    <dd>Card Schemes for which the Offer is valid. Possible values in the array are <code>visa</code>, <code>mastercard</code> and <code>amex</code>.</dd>
  </div>
  <div>
    <dt>
      <span><code>startDate</code></span>
      <em>date</em>
    </dt>
    <dd>Date and time, in the <code>YYYY-MM-DDThh:mm:ss</code> format, for when the Offer is activated and starts qualifying transactions.</dd>
  </div>
  <div>
    <dt>
      <span><code>supplier</code></span>
      <em>object</em>
    </dt>
    <dd>Contains an <code>id</code> property, with the unique identifier for the account that supplies the Offer. Also contains a <em>string</em> <code>type</code> property, for the type of account that supplies the Offer. The type can have one of the values <code>"merchant" | "card-linking" | "affiliate"</code>. Defaluts to <code>null</code>.</dd>
  </div>
  <div>
    <dt>
      <span><code>type</code></span>
      <em>object</em>
    </dt>
    <dd>Represents the type of Offer: a fixed amount or a percentage of the original transaction The <code>name</code> property can have one of the following two values: <code>amount</code> and
      <code>discount</code>. The <code>value</code> property has either the fixed amount of currency to be rewarded or the percentage value, depending on the Offer type.</dd>
  </div>
  <div>
    <dt>
      <span><code>updated</code></span>
      <em>date</em>
    </dt>
    <dd><a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> date and time in UTC representing the last time the Offer object was updated.</dd>
  </div>
</dl>

## API Reference

If you're looking to find out more about our Offers API and how to use it with your application, please visit the [Fidel API Reference](https://reference.fidel.uk/reference#create-offer).
