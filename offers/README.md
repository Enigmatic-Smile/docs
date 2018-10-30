## Offers

The Offer object holds the details about a card linked offer. Offers can be created by developers using the Offer API or by Brands/Merchants using the CLO dashboard at [clo.fidel.uk](https://clo.fidel.uk). Currently, this functionality is in private beta only available to a selected group of Brands. We expect the private beta testing to take 3 to 4 weeks before opening to the public.

A card linked offer specifies a set of parameters that will be used to qualify a card transaction made at a participating Brand’s online or offline store.

<br/>

# Offer object

```json
fileName:offer.json
{
  "id": "49b407ef-508a-432b-900c-a04e5e6ac86c",
  "additionalTerms": "none",
  "brandId": "e4928475-6a39-41e6-8484-951202d43de9",
  "brandName": "Dipp",
  "brandLogoURL": "https://dipp.com/logo.png",
  "countryCode": "USA",
  "created": "2018-10-19T12:12:00.000Z",
  "currency": "USD",
  "daysOfWeek": [0,1,2,3,4,5,6],
  "endDate": "2019-06-24T13:13:00.000Z",
  "feeSplit": 70,
  "locationsFile": "./all_locations.csv",
  "locationsTotal": 240,
  "maxRedemption": 1000,
  "maxRedemptionCounter": 730,
  "minTransactionAmount": 150,
  "name": "20% Off Everything",
  "performanceFee": 4,
  "priority": 1,
  "programId": "f446d575-d8c2-4a9e-8e12-ffd970d1a6t",
  "publisherId": "d346d574-d5c2-4a0e-8e02-ffd713fd1a9d",
  "returnPeriod": 30,
  "schemes": [
    "amex",
    "mastercard",
    "visa"
  ],
  "startDate": "2018-10-19T00:00:00.000Z",
  "status": "ACTIVE",
  "type": {
    "name": "discount",
    "value": 20
  },
  "updated": "2018-10-19T12:12:00.000Z",
  "userId": "82c5a9ed-5301-46ab-8599-6bcb0d017cc6"
}
```
<br/>
<strong>Parameters</strong>
<br/>

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
            <span><code>additionalTerms</code></span>
            <em>string</em>
        </dt>
        <dd>Additional terms related to the offer.</dd>
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
        <dd>Logo URL of the Brand. `null` if no URL is provided.</dd>
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
        <dd>Array of numbers between 0 and 6 representing the days of the week. Starting on Sunday. </dd>
    </div>
    <div>
        <dt>
            <span><code>endDate</code></span>
            <em>date</em>
        </dt>
        <dd>Date and time when the offer will complete.</dd>
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
            <span><code>locationsFile</code></span>
            <em>string</em>
        </dt>
        <dd>File in CSV format with the list of locations where offer is active.</dd>
    </div>
    <div>
        <dt>
            <span><code>locationsTotal</code></span>
            <em>number</em>
        </dt>
        <dd>Total number of locations in locations file.</dd>
    </div>
    <div>
        <dt>
            <span><code>maxRedemption</code></span>
            <em>number</em>
        </dt>
        <dd>Maximum number of qualified transactions available for the offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>maxRedemptionCounter</code></span>
            <em>number</em>
        </dt>
        <dd>Current number of qualified transactions for the offer.</dd>
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
            <span><code>performanceFee</code></span>
            <em>number</em>
        </dt>
        <dd>Percentage of the net transaction amount charged to Brand for qualifying transactions.</dd>
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
            <span><code>programId</code></span>
            <em>string</em>
        </dt>
        <dd>Id of the Pogram the offer will be activate on. Set when publisher accepts offer and selects Program. Participating locations will be added to this Program and synced with the schemes.</dd>
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
            <span><code>returnPeriod</code></span>
            <em>number</em>
        </dt>
        <dd>Id of the publisher. Refers to accountId.</dd>
    </div>
    <div>
        <dt>
            <span><code>schemes</code></span>
            <em>array: string</em>
        </dt>
        <dd>Schemes where the offer is valid. Possible values are  `visa`, `mastercard` and `amex`.</dd>
    </div>
    <div>
        <dt>
            <span><code>startDate</code></span>
            <em>date</em>
        </dt>
        <dd>Date and time when the offer gets activated and starts qualifying transactions.</dd>
    </div>
    <div>
        <dt>
            <span><code>status</code></span>
            <em>string</em>
        </dt>
        <dd>Current status of the offer such as `DRAFT`, `ACTIVE` or `EXPIRED`. See more detailed documentation below about offer status.</dd>
    </div>
    <div>
        <dt>
            <span><code>type</code></span>
            <em>object</em>
        </dt>
        <dd>An offer can be a fixed amount off or a percentage discount of the transactions amount. `type: {name: string, value: number}`. The `name` property can have two values: `amount` and `discount`. The `value` property saves the fixed amount of curency to be rewarded or the percentage value in case of a discount offer.</dd>
    </div>
    <div>
        <dt>
            <span><code>updated</code></span>
            <em>date</em>
        </dt>
        <dd>ISO 8601 date and time in UTC representing when the object was last updated.</dd>
    </div>
    <div>
        <dt>
            <span><code>userId</code></span>
            <em>string</em>
        </dt>
        <dd>Id of the User that created the offer.</dd>
    </div>
</dl>

<br/>

# Create Offer

To create an offer you should use the **Create Offer** API endpoint and specify the parameters for the card linked offer. 
Before creating an offer, the Brand User must complete the sign up process at [http://clo.fidel.uk](http://clo.fidel.uk) after they receive an invitation from a publisher:

1. Select password and complete sign up
2. Enter business details

See below an example on how to create an offer using the Create Offer endpoint:

```sh
curl -X POST \
  https://api.fidel.uk/v1/brands/4ed4b62b-aa4c-43a1-8064-da6d1368e17b/offers \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_live_demo' \
  -d '{
        "countryCode": "GBR",
        "name":"20% Off Everything",
        "publisherId":"4ed4b62b-aa4c-43a1-8064-nb7d1368e17a",
        "startDate":"2018-10-20T12:12:00.000Z",
        "type":{
            "name":"discount",
            "value":20
         }
     }'
```

You should pass the `brandId` of the Brand you wish to submit the offer to in the URL path and the offer parameters in the payload.

As required parameters, you need to set an offer `name`, your `accountId` as the `publisherId`, a `startDate` at least three weeks from today, the type of offer between `amount` or `discount`, an offer `value`, and the `countryCode` where the offer will be available. The `value` is a percentage if the offer `type` is `discount` or a fixed amount in the local currency of the `countryCode` specified.

Two more API calls are necessary to complete the offer object and submit to the publisher for aproval. These are the **Add locations** endpoint and after the **Submit offer** endpoint. After the offer has been submitted, the publisher can use the dashboard or the review endpoints to accept or reject the offer.

<br/>

# Offer Lifecycle

### Draft
First status of an offer after creation. The offer has not been submitted to a publisher. A locations file may have been uploaded for the offer, but no locations would be linked at this stage.

### Submitted
Offer has been submitted to a publisher for acceptance.

### Syncing
Offer has been accepted by the publisher and participating locations are now being onboarded with the schemes.  This stage takes between 1 and 10 business days.

### Active
Offer goes active on `startDate` and starts qualifying transactions at the participating locations.
  
### Rejected
If an offer is rejected by a publisher, you receive a notification email with the rejection reason and you can to edit the offer parameters and resubmit the offer. 

### Pending
Offer was created with no publisher. These offers are advertised to publishers on the Fidel CLO marketplace.

### Expired
Offer was not accepted or rejected by publisher in 14 days after it was submitted.

### Completed
Offer status will update to completed from active when the end date or the maximum number of qualified transactions is reached. The one that comes first.

<br/>

# Qualification

When an offer is `Active`, the transaction qualification will start. Every transaction made a by a linked card on a location where an offer is ative will be analysed considering the offer parameters and can qualify or not qualify for the offer.
In both cases, an offer object is appended to the original transaction object containing all the qualification offer data. In case the transaction qualifies, `cashback` and `performanceFee` amounts are automatically calculated and the `qualified` property is set to `true`. If the transactions does not qualify, `cashback` and `performanceFee` amounts will be `0` and `qualified` property `null`.


```json
fileName:qualified-transaction.json
{
  "id": "ce86c82c-fd7b-4ca4-84cf-88e070dcb40f",
  "accountId": "38599064-f4e9-4cba-88b0-bac11d02a8f5",
  "brandId": "ce6ca867-53ed-4744-8262-fb53c515be1b",
  "cardId": "2826d007-ce92-418c-8c99-3c503e764e81",
  "locationId": "f9a14399-8f6a-4bbc-8f3e-80547ba5534f",
  "merchantId": "TEST_MID_ccf00f5b-3ab8-4a2b-8a42-5dc03e399667",
  "midId": "0615387e-b5a3-4ff8-83e8-60bb289833fc",
  "programId": "daf5a825-1124-4212-bbb7-51b6a519c4ac",
  "address": "10 Downing Street",
  "amount": 100,
  "auth": 1,
  "city": "London",
  "cleared": true,
  "countryCode": "GBR",
  "created": "2018-10-19T12:12:00.000Z",
  "currency": "GBP",
  "date": "2018-10-17T10:10:00.000Z",
  "firstNumbers": "444400",
  "lastNumbers": "4010",
  "live": true,
  "postcode": "W1 90OL",
  "scheme": "mastercard",
  "time": "2018-10-17T10:10:00.000Z",
  "type": "mastercard",
  "updated": "2018-10-19T12:12:00.000Z",
  "wallet": "undefined",
  "offer": {
    "id": "7e55eeae-99d6-4daf-b8c4-ac9ca660e964",
    "cashback": 20,
    "message": [],
    "performanceFee": 3.2,
    "qualified": true,
    "qualificationDate": null
  }
}
```

<br/>

```json
fileName:non-qualified-transaction.json
{
  ...
  "updated": "2018-10-19T12:12:00.000Z",
  "wallet": "undefined",
  "offer": {
    "id": "7e55efae-99d6-4daf-b8c4-ac9ca660e864",
    "cashback": 0,
    "performanceFee": 0,
    "qualified": false,
    "qualificationDate": null,
    "message": [
        "Transaction amount of GBP100 is lower than offer's minimum transaction amount of GBP150"
    ]
  }
}
```

