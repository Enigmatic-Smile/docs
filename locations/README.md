# Locations

To add a location, select a Program from the **Programs** page, click the **+** icon and enter the requested info about the location. You can also use the [API](https://reference.fidel.uk/reference#create-location) to create and update locations.

##### You can add Locations after creating a Program and a Brand.
![Add locations](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/add-locations.png "Add locations")

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

The `location.status` webhook can be registered on the Fidel Dashboard or via the Fidel API. An event is triggered when there are updates from a card network for a Location in a Program. In the `test` environment, this webhook triggers three times for each location upon creating a Location, once for each card scheme, with a `location.active` event. In the `live` environment, this would trigger whenever a location has synced successfully for a card scheme, with a `location.active` event. Or whenever a location has failed syncing for a card scheme, with a `location.failed` event.

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
In the live environment, the location begins in an *Inactive* state. In order initiate a change in status of your location, you must start the syncing process by pressing the sync button on the dashboard. Location Sync can take 1-2 weeks. Only one sync per program can be run at a time, so ensure that you are ready to run this process.

![Sync button](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/programsync_button.png "Add locations")

Once the sync process is initiated, the Location status is updated to *Syncing*. It then moves to *Active* when we receive confirmation from the card networks that the location has been successfully on-boarded to your program. If the card networks have an issue with a specific location, the status is set to *Failed* and a case is opened to resolve the issue. While your locations are syncing you have a progress bar to follow the current status and estimated finish time.

For Locations in the live environment, status can be tracked on the dashboard. A green check ✅ means we have received at least one transaction from this Location confirming the active onboarding status. A **RT** icon will show where we can track real-time authorisation transaction and the **RT** will turn green when we receive the first auth transaction from this Location.
