## Programs

A **Program** is a set of locations that uniquely represent an offline or online store where transactions from linked cards will be monitored. The Program is the parent object of your card linked structure. Other objects such as Cards, Locations, Webhooks and Transactions will always be linked to a Program. Programs are unique and independent of each other allowing you to have several independent loyalty or rewards schemes inside your account.

<h5>Hierarchy diagram of the FIDEL API object structure.</h5>

![Hierarchy diagram](https://docs.fidel.uk/assets/images/hierarchy-diagram.png "Hierarchy diagram")

# Create Program
To create a new program, go to the **Programs** page on the dashboard, click the **+** icon and enter a name. The program will be created with the name provided without any initial locations.

<h5>Go to the Programs page on the dashboard to create a new program.</h5>

![Create program](https://docs.fidel.uk/assets/images/create-program.png "Create program")

<br/>

A `programId` will be generated and must be passed as a data property when using the SDKs to link cards to this program. Cards are linked to Programs and consequently to a set of Locations.

<br/>

# Add Locations
To add locations to a program, first you will need to create Brands. Every Location is linked to a Brand.

Once you have a Brand, select a program from the **Programs** page, click the **+** icon and enter the requested info. To add a location you need to select the Brand, enter address, postcode, city and one or more merchantIds (MID).

<h5>After creating a program, you can add locations.</h5>

![Add locations](https://docs.fidel.uk/assets/images/add-locations.png "Add locations")

<br/>

The `merchantId` (MID) is an identifier for a retail location that can be found on the receipt printed by any card machine in that location. It is usually referred as MID on the receipt. One retail location can have several MIDs. On an online store the `merchantId` can be requested from the payment service provider used by the e-commerce platform.

In test mode, a new location is automatically approved and set to active status. On live environment, new locations added will require 1-2 weeks to be synced with the card schemes before becoming active. During that time they will have a pending status.
