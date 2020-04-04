# Programs

A **Program** is a set of locations that uniquely represent an offline or online store where transactions from linked cards will be monitored. The Program is the parent object of your card linked structure. Other objects such as Cards, Locations, Webhooks and Transactions will always be linked to a Program. Brands are shared across Programs. They are unique and independent of each other allowing you to have several independent loyalty or rewards schemes inside your account.

##### Hierarchy diagram of the Fidel API object structure.

![Programs structure diagram](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/programs_diagram_2020.png "Programs structure diagram")

## Create Program
To create a new program, go to the **Programs** page on the dashboard, click the **+** icon and enter a name. The Program will be created with the name provided without any initial locations.

##### Go to the Programs page on the dashboard to create a new program.

![Create program](https://docs.fidel.uk/assets/images/create-program.png "Create program")

A `programId` will be generated and is used as a data property when using the SDKs to link Cards to this Program. Cards are linked to Programs and consequently to Locations.

## Add Locations
Every Location is linked to a Brand, and the Brand is linked to the Program.

Once you have a Brand, select a Program from the **Programs** page, click the **+** icon and enter the requested info. To add a location, you need to select the Brand with approved consent, enter address, postcode, city and select the country for this location.  If your location has been assigned Merchant IDs (MIDs) from the card processors, you may add them now. Creating locations with MIDs means we can identify merchant locations more accurately and onboard them faster.

##### After creating a Program, you can add Locations.

![Add locations](https://docs.fidel.uk/assets/images/add-locations.png "Add locations")

To start tracking credit/debit card transactions at specific Locations, we need the address of each Location so we can submit them for onboarding with VISA, Mastercard and American Express, and keep track of the status of each location. 
We call this process Program Sync ⚡️.

Location status is reported separately for each card scheme.  Each location/scheme combination can have one of four statuses: **Inactive, Syncing, Active, or Failed**. 
> In the test environment, every added Location skips the Program Sync processand is automatically set to *Active*.

In the live environment, the location begins in an *Inactive* state. The Location status is updated to *Syncing* after it is  submitted to the card schemes. It then moves to *Active* when we receive confirmation from the schemes that the location has been successfully on-boarded to your program. If the schemes have an issue with a specific location, the status is set to *Failed* and a case is opened to resolve the issue.

For Locations in the live environment, status can be tracked on the dashboard. A green check ✅ means we have received at least one transaction from this Location confirming the active onboarding status. A **RT** icon will show where we can track real-time authorisation transaction and the **RT** will turn green when we receive the first auth transaction from this Location.

While your Program is syncing you have a progress bar to follow the current status and estimated finish time.

In test mode, a new Location is automatically synced and set to active status. In a live environment, new Locations added will require 1-2 weeks to be synced with the card schemes before becoming active. During that time they will have a pending status.
