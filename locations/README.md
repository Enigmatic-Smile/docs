## Add Locations
Every Location is linked to a Brand, and the Brand is linked to the Program.

To add a location, select a Program from the **Programs** page, click the **+** icon and enter the requested info about the location. You can also use the [API to add and modify locations](https://reference.fidel.uk/reference#create-location).

The information required for each location is the Brand (with approved consent), address, postcode, city and country.  If your location has been assigned Merchant IDs (MIDs) from the card networks, you may add them now. Adding MIDs during Location creation will speed the onboarding process.

>Note: If you you use a Payment facilitator, you may be using a [shared MID](https://community.fidel.uk/t/what-is-a-shared-merchant-id-mid/41) that affects how transactions are monitored.  Reach out to Fidel if you use a paymant facilitator and you believe you might have a shared MID.

##### After creating a Program, you can add Locations.

![Add locations](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/add-locations.png "Add locations")

Once you have added your locations to Fidel, we can begin the process of Location Sync ⚡️. This step requires submitting all of the locations for onboarding with VISA, Mastercard and American Express, so that we may track credit/debit card transactions at each location. During this process, you can monitor and keep track of the status of each location. 

### Location Sync

Location status is reported separately for each card scheme.  Each location/scheme combination can have one of four statuses: **Inactive, Syncing, Active, or Failed**. 
> In the test environment, every added Location skips the Location Sync processand is automatically set to *Active*.

In the live environment, the location begins in an *Inactive* state. In order initiate a change in status of your location, you must start the syncing process by pressing the sync button on the dashboard. 

>Location Sync can take 1-2 weeks. Only one sync per program can be run at a time, so ensure that you are ready to run this process.

![Sync button](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/programsync_button.png "Add locations")

Once the sync process is initaited, the Location status is updated to *Syncing*. It then moves to *Active* when we receive confirmation from the card networks that the location has been successfully on-boarded to your program. If the card networks have an issue with a specific location, the status is set to *Failed* and a case is opened to resolve the issue.

For Locations in the live environment, status can be tracked on the dashboard. A green check ✅ means we have received at least one transaction from this Location confirming the active onboarding status. A **RT** icon will show where we can track real-time authorisation transaction and the **RT** will turn green when we receive the first auth transaction from this Location.

While your locations are syncing you have a progress bar to follow the current status and estimated finish time.

In test mode, a new Location is automatically synced and set to active status. In a live environment, new Locations added will require 1-2 weeks to be synced with the card schemes before becoming active. During that time they will have a pending status.
