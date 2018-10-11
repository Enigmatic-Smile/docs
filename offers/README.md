## Offers

The Offer object holds the details about a card linked offer. Offers can be created by developers using the Offer API or by Brands/Merchants using the CLO dashboard at [clo.fidel.uk](https://clo.fidel.uk). Currently, this functionality is in private beta only available to a selected group of Brands. We expect the private beta testing to take 3 to 4 weeks before opening to the public.

A card linked offer specifies a set of parameters that will be used to qualify a card transaction made at a participating Brand’s online or offline store.

To create an offer you should use the **Create Offer** API endpoint and specify the parameters for the card liked offer. 

See below an example on how to create an offer using the Create Offer endpoint:

```
curl --request POST \
  --url 'https://api.fidel.uk/v1d/brands/{{brand.id}}/offers' \
  --header 'Content-Type: application/json' \
  --header 'Fidel-Key: sk_live_demo' \
  --data '{
   "name":"Integration test offer",
   "publisherId":"{{account.id}}",
   "startDate":"{{offer.start}}",
   "type":{
      "name":"amount",
      "value":10
   },
   "countryCode": "GBR"
}'
```

You should pass the `brandId` of the Brand you wish to submit the offer to in the URL path and the offer parameters in the payload.

As required parameters, you need to set an offer `name`, your `accountId` as the `publisherId`, a `startDate` at least three weeks from today, the type of offer between `amount` or `discount`, an offer `value`, and the `countryCode` where the offer will be available. The `value` is a percentage id the offer `type` is `discount` or a fixed amount in the local currency of the `countryCode` specified.

Check the **API Reference** for a more detailed description of all parameters, requests and response payloads of the Offer API.

### Offer Status

* Draft
* Submitted
* Syncing
* Active
* Rejected

* Offer status & lifecycle
* Add locations
* Submit