# Offers

The Offer object holds the details about a card linked offer. Offers can be created by developers using the [Offer API](https://reference.fidel.uk/v1/reference#create-offer) or by Brands/Merchants using the CLO dashboard at [clo.fidel.uk](https://clo.fidel.uk).
A card linked offer specifies a set of parameters that will be used to qualify a card transaction made at a participating Brand’s online or offline store.

## Offer object

Once you have created an offer, you can request the contents of the object using the [Get Offer](https://reference.fidel.uk/reference#get-offer) API call. The response will appear similar to the following:

```json
fileName:offer.json
{
  "id": "49b407ef-508a-432b-900c-a04e5e6ac86c",
  "activation": {
    "enabled": true,
    "qualifiedTransactionsLimit": 1
  },
  "additionalTerms": null,
  "brandId": "e4928475-6a39-41e6-8484-951202d43de9",
  "brandName": "Starbucks",
  "brandLogoURL": "https://starbucks.com/logo.png",
  "countryCode": "USA",
  "created": "2020-04-18T12:12:00.000Z",
  "currency": "USD",
  "daysOfWeek": [0,1,2,3,4,5,6],
  "endDate": "2020-06-20T13:13:00",
  "feeSplit": 70,
  "funded": {
    "id": "d346d574-d5c2-4a0e-8e02-ffd713fd1a9d",
    "type": "card-linking"
  },
  "live": true,
  "locationsTotal": 240,
  "minTransactionAmount": 10,
  "maxTransactionAmount": 100,
  "name": "20% Off Everything",
  "priority": 1,
  "publisherId": "d346d574-d5c2-4a0e-8e02-ffd713fd1a9d",
  "returnPeriod": 15,
  "schemes": [
    "amex",
    "mastercard",
    "visa"
  ],
  "startDate": "2020-04-20T00:00:00",
  "type": {
    "name": "discount",
    "value": 20
  },
  "updated": "2020-04-18T12:12:00.000Z"
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
            <span><code>activation</code></span>
            <em>object</em>
        </dt>
        <dd>If <code>enabled: true</code> offers need to be activated on cards before the purchase so the transaction is tracked. The number of transactions to qualify for each offer activation can be specified by setting <code>qualifiedTransactionsLimit</code> to a specific number.</dd>
    </div>
    <div>
        <dt>
            <span><code>additionalTerms</code></span>
            <em>string</em>
        </dt>
        <dd>Additional terms related to the offer. Supports markdown link syntax. E.g. [My docs link](https://fidel.uk/docs)</dd>
    </div>
    <div>
        <dt>
            <span><code>brandId</code></span>
            <em>string</em>
        </dt>
        <dd>Id of the Brand.</dd>
    </div>
    <div>
        <dt>
            <span><code>brandName</code></span>
            <em>string</em>
        </dt>
        <dd>Name of the Brand.</dd>
    </div>
    <div>
        <dt>
            <span><code>brandLogoURL</code></span>
            <em>string</em>
        </dt>
        <dd>Logo URL of the Brand. <code>null</code> if no URL is provided.</dd>
    </div>
    <div>
        <dt>
            <span><code>countryCode</code></span>
            <em>string</em>
        </dt>
        <dd>ISO 3166-1 alpha-3 country code where the offer is active.</dd>
    </div>
    <div>
        <dt>
            <span><code>created</code></span>
            <em>date</em>
        </dt>
        <dd>ISO 8601 date and time in UTC representing when the object was created.</dd>
    </div>
    <div>
        <dt>
            <span><code>currency</code></span>
            <em>string</em>
        </dt>
        <dd>ISO 4217 currency code based on country code value.</dd>
    </div>
    <div>
        <dt>
            <span><code>daysOfWeek</code></span>
            <em>array: number</em>
        </dt>
        <dd>Array of numbers between 0 and 6 representing the days of the week. Starting on Sunday.</dd>
    </div>
    <div>
        <dt>
            <span><code>endDate</code></span>
            <em>date</em>
        </dt>
        <dd>Date and time, in format <code>YYYY-MM-DDThh:mm:ss</code>, when the offer will complete. Note: Time is local to the location.</dd>
    </div>
    <div>
        <dt>
            <span><code>feeSplit</code></span>
            <em>number</em>
        </dt>
        <dd>Percentage of the performance fee to be charged by Fidel to the Brand.</dd>
    </div>
    <div>
        <dt>
            <span><code>funded</code></span>
            <em>object</em>
        </dt>
        <dd><code>Id</code> of the account that is funding the offer. <code>type</code> is the type of account that is funding the offer and can have one of the following values: <code>{merchant, card-linking, affiliate}</code>
    </div>
    <div>
        <dt>
            <span><code>live</code></span>
            <em>boolean</em>
        </dt>
        <dd>Whether the offer should be created in live or test environment.</dd>
    </div>
    <div>
        <dt>
            <span><code>locationsTotal</code></span>
            <em>number</em>
        </dt>
        <dd>Total number of locations linked to the offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>minTransactionAmount</code></span>
            <em>number</em>
        </dt>
        <dd>Minimum transaction amount to qualify for offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>name</code></span>
            <em>string</em>
        </dt>
        <dd>Name of the offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>priority</code></span>
            <em>number</em>
        </dt>
        <dd>Number starting in 1, representing stacking priority for publisher. By default, publisher that invites Brand gets top priority = 1.</dd>
    </div>
    <div>
        <dt>
            <span><code>publisherId</code></span>
            <em>string</em>
        </dt>
        <dd>Id of the publisher. Refers to accountId.</dd>
    </div>
    <div>
        <dt>
            <span><code>returnPeriod</code></span>
            <em>number</em>
        </dt>
        <dd>Number of days before a transaction qualifies for the offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>schemes</code></span>
            <em>array: string</em>
        </dt>
        <dd>Schemes where the offer is valid. Possible values are <code>visa</code>, <code>mastercard</code> and <code>amex</code>.</dd>
    </div>
    <div>
        <dt>
            <span><code>startDate</code></span>
            <em>date</em>
        </dt>
        <dd>Date and time, in format <code>YYYY-MM-DDThh:mm:ss</code>, when the offer gets activated and starts qualifying transactions.</dd>
    </div>
    <div>
        <dt>
            <span><code>type</code></span>
            <em>object</em>
        </dt>
        <dd>An offer can be a fixed amount off or a percentage discount of the transactions amount. <code>type: {name: string, value: number}</code>. The <code>name</code> property can have one of the following two values: <code>amount</code> and <code>discount</code>. The <code>value</code> property saves the fixed amount of currency to be rewarded or the percentage value in case of a discount offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>updated</code></span>
            <em>date</em>
        </dt>
        <dd>ISO 8601 date and time in UTC representing when the object was last updated.</dd>
    </div>
</dl>

## Create Offer

To create an offer you can use the [Create Offer](https://reference.fidel.uk/v1/reference#create-offer) API endpoint or the dashboard and specify your offer parameters. If you have an account in the [Offers dashboard](https://clo.fidel.uk), you may create an offer there as well.

See below an example on how to create an offer using the Create Offer endpoint:

```sh
curl -X POST \
  https://api.fidel.uk/v1/offers \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_live_152c2c3c-3bc1-49af-84e2-82646f303c13' \
  -d '{
        "countryCode": "USA",
        "name":"20% Off Everything",
        "brandId":"585dca42-2c77-4007-8429-9496782fd16a",
        "publisherId":"4ed4b62b-aa4c-43a1-8064-nb7d1368e17a",
        "startDate":"2020-04-20T12:13:13",
        "type":{
          "name":"discount",
          "value":20
         }
       }'
```

### Required Parameters

There are a number of required parameters for the Offer to be created:
* `countryCode`: where the offer will be available.
* `name`: the name of your offer.
* `brandId`: of the brand presenting the offer.
* `publisherId`: this is your Fidel `accountId`.
* `startDate`: of the offer. Any time added to the startDate will be local to the location.
* `type: name`: valid names are `amount` or `discount`.  
* `type: value`: numeric value of the discount.

An offer with the type `amount` will use the currency of the indicated country, and apply the value as the amount of savings (for example: £25 off). The type `discount` applies the value as a percentage savings (for example: 25% off).

### Optional Parameters

* `activation: enabled`: if set to `true`, the offer requires to be activated on cards. **Please read the section below on Offer Activation before using this parameter.**
* `activation: qualifiedTransactionsLimit`: number of transactions to qualify after each offer activation. Default is 1.
* `daysOfWeek`: is an array of numbers 0-6 to indicate the days of the week. 0 = Sunday, 1 = Monday, etc.
* `endDate`: to automatically end the offer. Time is set to local time.
* `maxTransactionAmount`: the maximum transaction spend for an offer. Example: "Save 25% on purchases over £50, save 40% on purchases over £100" the first offer would have a maxTransactionAmout of £100.
* `minTransactionAmount`: The minimum transaction amount (example: "Save 25% on purchases over £50")
* `returnPeriod`: number of days before a transaction qualifies for the offer.


Check out the [Offer API Reference](https://reference.fidel.uk/v1/reference#create-offer) for more detailed documentation about available Offer API endpoints.

Once you have created the offer, it will enter the Offer lifecycle as a pending offer.

<hr>

## Offer Lifecycle

On the dashboard offers are grouped into four categories: pending, all-set, live and expired.

### Pending
Offer `startDate` is set in the future, and/or no locations are set.

### All-set
Offer has at least one location set, but `startDate` is in the future. Offers in this category will automatically become "live" when the `startDate` is reached.

### Live
Current day is between `startDate` and `endDate` and offer is qualifying transactions at the locations linked to it.

### Expired
Current day is after the `endDate`. The offer has stopped qualifying transactions.

---

## Linking Locations

Before an Offer goes live and starts qualifying transactions, you will need to link locations to the Offer. You can do that by using the dashboard and clicking on the options menu for a location and then clicking on 'Link to Offer'.

Alternatively, you can use the [Link Location to Offer](https://reference.fidel.uk/reference#add-location-to-offer) API endpoint to link any location to an Offer.


## Qualification

When an offer is live, each transaction made by a linked card on a location linked to the offer will be analysed against the offer parameters and can qualify or not qualify for the offer.

In both cases, an offer object is appended to the original transaction object containing the qualification result data. In case the transaction qualifies, `cashback` and `performanceFee` amounts are calculated and the `qualified` property is set to `true`. If the transactions does not qualify, `cashback` and `performanceFee` amounts will be `0`, the `qualified` property set to `false` and a message explaining why the offer did not qualify is set in the `message` property. You can see examples for qualified and non-qualified transactions below.

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

## Activation

Offer activation allows developers to require that offers are activated on cards in order to qualify for a reward. This feature is part of the Offers API and can be used by creating offers that require activation with `activation: { enabled: true, qualifiedTransactionsLimit: 1 }`. The `qualifiedTransactionsLimit` property specifies how many transactions will be qualified for each offer activation.
This can be done using the [Create Offer API](https://reference.fidel.uk/v1/reference#create-offer) endpoint or using the [dashboard](https://dashboard.fidel.uk).

To receive and qualify transactions for offers with activation, offers need to be activated on cards before the purchase. That is done by using the [Activate Offer on Card](https://reference.fidel.uk/reference#activate-offer-on-card) API endpoint as follows:

```sh
curl -X POST \
  https://api.fidel.uk/v1/offers/:offerId/cards/:cardId \
  -H 'content-type: application/json' \
  -H 'fidel-key: your-secret-key'
```

You can also activate an offer on a card using the dashboard by clicking on a card’s options menu, click ‘Activate offer on card’ and select the offer to activate.

When an offer is activated on a card, it will qualify the number of transactions specified by the `qualifiedTransactionsLimit` value. After one or more transactions are qualified the offer is automatically deactivated from the card.

Note that when you link locations to an offer with activation you will only receive transactions from cards where the offer has been activated on. To receive all transactions from all cards you disable the offer activation with `activation: { enabled: false, qualifiedTransactionsLimit: 1 }` or unlink the locations from the offer.

<div class="info-box">
    <small>Test and live environments</small><br/>
    It is important to note that when testing offers with activation in test environment all test transactions created will be visible for testing purposes as opposed to live environment where only transactions for activated offers on cards will be received and qualified.
</div>
