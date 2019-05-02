## Programs

A **Program** is a set of locations that uniquely represent an offline or online store where transactions from linked cards will be monitored. The Program is the parent object of your card linked structure. Other objects such as Cards, Locations, Webhooks and Transactions will always be linked to a Program. Brands are shared across Programs. They are unique and independent of each other allowing you to have several independent loyalty or rewards schemes inside your account.

<h5>Hierarchy diagram of the Fidel API object structure.</h5>

![Programs structure diagram](https://docs.fidel.uk/assets/images/programs_diagram.png "Programs structure diagram")

# Create Program
To create a new program, go to the **Programs** page on the dashboard, click the **+** icon and enter a name. The program will be created with the name provided without any initial locations.

<h5>Go to the Programs page on the dashboard to create a new program.</h5>

![Create program](https://docs.fidel.uk/assets/images/create-program.png "Create program")

<br/>

A `programId` will be generated and must be passed as a data property when using the SDKs to link cards to this program. Cards are linked to Programs and consequently to Locations.

<br/>

# Add Locations
To add locations to a program, first you will need to create a Brand. Every Location is linked to a Brand.

Once you have a Brand, select a program from the **Programs** page, click the **+** icon and enter the requested info. To add a location you need to select the Brand with approved consent, enter address, postcode, city and select the country for this location.

<h5>After creating a program, you can add locations.</h5>

![Add locations](https://docs.fidel.uk/assets/images/add-locations.png "Add locations")

<br/>

To start tracking credit/debit card transactions on specific locations, we need to have the address of each location, submit them for onboarding with VISA, Mastercard and American Express, and keep track of the status of each location. 
We call this process Program Sync ⚡️.

We do not require you to send us merchant IDs, we only need the addresses of the merchant locations you want to track and we do the rest.

Location status is saved by scheme. You can also access more information about each location, such as whether the location is real-time enabled and whether a transaction has ever been recorded at the location. Each location can have one of four status: **Inactive, Syncing, Active, or Failed** for each scheme. Prior to syncing the location is ‘Inactive’. The location status is updated to ‘Syncing’ after we submit it to the schemes. It then moves to ‘Active’ when we receive confirmation from the schemes that the location has been sucessfully on-boarded to your program. If the schemes have an issue with a specific location, the status is set to ‘Failed’ and a case is opened to resolve issue.

You can track your location status on the dashboard. A green check ✅ means we have received at least one transaction from this location confirming the active onboarding status. On Mastercard and Amex an **RT** icon will show where we can track real-time authorisation transaction and the **RT** will turn green when we receive the first auth transaction from this location.

While your Program is syncing you have a progress bar to follow the current status and estimated finish time.

In test mode, a new location is automatically synced and set to active status. On live environment, new locations added will require 1-2 weeks to be synced with the card schemes before becoming active. During that time they will have a pending status.
