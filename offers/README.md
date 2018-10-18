## Offers

The Offer object holds the details about a card linked offer. Offers can be created by developers using the Offer API or by Brands/Merchants using the CLO dashboard at [clo.fidel.uk](https://clo.fidel.uk). Currently, this functionality is in private beta only available to a selected group of Brands. We expect the private beta testing to take 3 to 4 weeks before opening to the public.

A card linked offer specifies a set of parameters that will be used to qualify a card transaction made at a participating Brand’s online or offline store.

To create an offer you should use the **Create Offer** API endpoint and specify the parameters for the card liked offer. 

See below an example on how to create an offer using the Create Offer endpoint:

```
curl --request POST \
     --url 'https://api.fidel.uk/v1/brands/4ed4b62b-aa4c-43a1-8064-da6d1368e17b/offers' \
     --header 'Content-Type: application/json' \
     --header 'Fidel-Key: sk_live_demo' \
     --data '{
         "countryCode": "GBR",
         "name":"20% Off Everything",
         "publisherId":"4ed4b62b-aa4c-43a1-8064-nb7d1368e17a",
         "startDate":"2018-10-20T12:12:00.000Z",
         "type":{
            "name":"amount",
            "value":10
         }
     }'
```

You should pass the `brandId` of the Brand you wish to submit the offer to in the URL path and the offer parameters in the payload.

As required parameters, you need to set an offer `name`, your `accountId` as the `publisherId`, a `startDate` at least three weeks from today, the type of offer between `amount` or `discount`, an offer `value`, and the `countryCode` where the offer will be available. The `value` is a percentage id the offer `type` is `discount` or a fixed amount in the local currency of the `countryCode` specified.

Check the **API Reference** for a more detailed description of all parameters, requests and response payloads of the Offer API.

<br/>

<h5>Offer object</h5>

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
  "minTransactionAmount": 150,
  "name": "10% Off Everything",
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
  "status": "20",
  "type": {
    "name": "discount",
    "value": 10
  },
  "updated": "2018-10-19T12:12:00.000Z",
  "userId": "82c5a9ed-5301-46ab-8599-6bcb0d017cc6"
}
```

See below the description of each parameter of the offer.

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
</dl>

```
  /** Optional - Additional offer terms */
  additionalTerms: string,

  /** Id of the Brand */
  brandId: string,

  /** Name of the Brand */
  brandName: string,

  /** Logo URL of the Brand */
  brandLogoURL: string | null,

  /** Country code where the offer is active */
  countryCode: string,

  /** Currency of the selected country */
  currency: Currency,

  /** Days of the week when transactions qualify for offer */
  /** Using Days of the week according to JavaScript  */
  /** https://www.w3schools.com/jsref/jsref_getday.asp */
  /* 0 = Sunday, 1 = Monday, ect ect */
  /* Optional: empty array means no limitation */
  daysOfWeek: Array<number>,

  /** Optional - End date of the offer */
  /** If null, offer runs continuously */
  endDate: string | null,

  /** Fee split for the publisher payout.
   *  If non present, it should be taken from the account object
   *  at the time in which the offer is accepted by publisher
   */
  feeSplit: ?number,

  /** File with the list of locations where offer is active */
  locationsFile: string | null,

  /** Number of locations in locations file */
  locationsTotal: number,

  /** Optional - counter of redeemable offers */
  maxRedemptionCounter: number | null,

  /** Optional - total number of redeemable offers */
  maxRedemptionTotal: number | null,

  /** Optional - Minimum amount for a transaction to be valid for the offer */
  minTransactionAmount: number,

  /** Name of the offer */
  name: string,

  /** Fee charged to brand. Between 0 and 100. Inherits from brand */
  performanceFee: number,

  /** Id of the Program where the offer is running */
  programId: string | null,

  /** Optional - number of days before a transaction qualifies for the offer */
  returnPeriod: number | null,

  /** Stacking priority */
  /** By default, publisher that invites Brand gets top priority */
  priority: number,

  /** Id of the publisher (Refers to accountId) */
  publisherId: string | null,

  /** Schemes where the offer is valid */
  schemes: Array<SchemeName>,

  /** Date when the offer will be activated */
  startDate: string,

  /** Status of the offer (See OfferStatusName type) */
  status: OfferStatusName,

  /** Type of the offer (can be of type 'AMOUNT', 'DISCOUNT') */
  type: IOfferType,

  /** Composite string used as sort key in Offer table indexes */
  statusUpdated: string,

  /** Id of the user that creates the offer */
  userId: string,
```



### Offer Status

* Draft
* Submitted
* Syncing
* Active
* Rejected
--
* Add locations
* Submit
* Webhooks