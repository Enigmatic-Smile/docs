# Locations

To add a location, select a Program from the [Programs](https://dashboard.fidel.uk/programs) page. This will open the Locations page of the Fidel Dashboard, with the program selected. You can then click the **New location** button, and enter the requested info about the location. You can also use the [API](https://reference.fidel.uk/reference#create-location) to create and update locations.

##### You can add Locations after creating a Program and a Brand.
![Add locations](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/add-locations.png "Add locations")

##### You can do the same by using our API.

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/b58d8057-9159-4daf-a003-1397e28f6822/locations \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
  -d '{
    "address": "2 Soho Square",
    "brandId": "838d648e-5614-48a7-8e8b-6b3014638c66",
    "city": "London",
    "countryCode": "GBR",
    "postcode": "W1D 3PX"
}'
```

The information required for each location is the Brand (with approved consent), address, postcode, city and country. If your location has been assigned Merchant IDs (MIDs) from the card networks, you may add them now. Adding MIDs during Location creation will speed up the onboarding process.

<div class="info-box">
    <small>Payment Facilitators</small><br/>
    If you need to onboard payment facilitators (Square, iZettle, SumUp, etc.), they may be using shared merchant IDs that have an impact on how transactions are monitored. Reach out to Fidel if you work with merchants that use payment facilitators.
</div>

You can read more about [shared MIDs in this article](https://community.fidel.uk/t/what-is-a-shared-merchant-id-mid/41).
Once you have added your locations to Fidel, we can begin the process of Location Sync ⚡️. This step requires submitting all of the locations for onboarding with Visa, Mastercard and American Express, so that we may track credit/debit card transactions at each location. During this process, you can monitor and keep track of the status of each location.

## Location Sync

Location status is reported separately for each card scheme. Each location/scheme combination can have one of four statuses: **Inactive, Syncing, Active, or Failed**.

<div class="info-box">
In the test environment, every added Location skips the Location Sync process and is automatically set to Active.
</div>

### Location Status Webhook

The `location.status` webhook can be registered on the Fidel Dashboard or via the Fidel API. An event is triggered when there are updates from a card network for a Location in a Program. In the `test` environment, this webhook triggers three times for each location upon creating a Location, once for each card scheme, with a `location.active` event. In the `live` environment, this would trigger whenever a location has synced successfully for a card scheme, with a `location.active` event. Or whenever a location has failed to sync for a card scheme, with a `location.failed` event.

Here's an example on how to register the webhook on a Program, with `example.com` as the URL:

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/06471dbe-a3c7-429e-8a18-16dc97e5cf35/hooks \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_50ea90b6-2a3b-4a56-814d-1bc592ba4d63' \
  -d '{
    "event": "location.status",
    "url": "https://example.com"
  }'
```

### Location Sync Process
In the live environment, the location begins in an *Inactive* state. In order initiate a change in status of your location, you must start the syncing process for the entire program. You can start the process by pressing the "Sync locations" button on the Fidel Dashboard. The button is only visible when you have an inactive location in your Program. Location Sync can take 1-2 weeks. Only one sync per program can be run at a time, so ensure that you are ready to run this process.

![Sync button](https://raw.githubusercontent.com/FidelLimited/docs/new-dashboard-images/assets/images/programsync_button.png "Add locations")

You can also start the sync process programmatically by calling the [Update Program](https://reference.fidel.uk/reference#update-program) endpoint of our API. You'll need to add a body parameter of `{ "status" : "syncing" }` to start the process. Because syncing only works for live programs, you'll need to use the `programId` of a live program, and your live API key, when you call the API endpoint. Using a test API key will throw an error.

```sh
curl -X PATCH https://api.fidel.uk/v1/programs/1ed2d0a4-b778-4ea4-b991-b5ede2d4eeaa \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_live_b2e6a6ab-1a11-47ec-831c-81c2b23fd5b2' \
  -d '{
    "status": "syncing"
  }'
```

Alternatively, you can start the sync process for each location when you create it, by using the `"status": "syncing"` property. This only works in the live environment, so you'll need to use a live API key when making the API request.

```sh
curl -X POST \
  https://api.fidel.uk/v1/programs/1ed2d0a4-b778-4ea4-b991-b5ede2d4eeaa/locations \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_live_b2e6a6ab-1a11-47ec-831c-81c2b23fd5a0' \
  -d '{
    "address": "2 Soho Square",
    "brandId": "8c092cdb-e53d-4512-8168-c2a7f6b87286",
    "city": "London",
    "countryCode": "GBR",
    "postcode": "W1D 3PX",
    "status": "syncing"
}'
```

Once the sync process is initiated, the Location status is updated to *Syncing*. It then moves to *Active* when we receive confirmation from the card networks that the location has been successfully on-boarded to your program. If the card networks have an issue with a specific location, the status is set to *Failed* and a case is opened to resolve the issue. While your locations are syncing you have a progress bar to follow the current status and estimated finish time.

For Locations in the live environment, status can be tracked on the dashboard. A green check ✅ means we have received at least one transaction from this Location confirming the active onboarding status. A **RT** icon will show where we can track real-time authorisation transaction and the **RT** will turn green when we receive the first auth transaction from this Location.
