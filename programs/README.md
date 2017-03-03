## Programs

After creating a FIDEL API account and have access to the dashboard, the next step is to create a test program so you can add locations, cards and webhooks in order to generate and receive sample transaction data.

<h5>Hierarchy diagram of the FIDEL API object structure.</h5>

![Hierarchy diagram](https://docs.fidel.uk/assets/images/hierarchy-diagram.png "Hierarchy diagram")

# Create Program
A **Program** represents a set of locations or merchantsIds that uniquely identify an offline or online store where transactions from linked cards will be monitored. A new program should be created for each independent loyalty or rewards scheme.

To create a new program, go to the **Programs** page on the dashboard, click **Create Program** and enter a name. The program will be created with the name provided without any initial locations.

<h5>Go to the Programs page on the dashboard to create a new program.</h5>

![Create program](https://docs.fidel.uk/assets/images/create-program.png "Create program")

<br/>

A `programId` will be generated and must be passed as a data property when using the SDKs to link cards to this program.  

<br/>

# Add Locations
To add locations to a program, select a program from the **Programs** page, click **Add Locations** and enter the requested info. To add a location you need to enter a name, address, postcode, city and merchantId.

<h5>After creating a program, you can add locations.</h5>

![Add locations](https://docs.fidel.uk/assets/images/add-locations.png "Add locations")

<br/>

The `merchantId` is an unique identifier for a retail location that can be found on the receipt printed by any card machine in that location. It is usually referred as MID on the receipt. On an online store the `merchantId` can be requested from the payment service provider used by the e-commerce platform.

In test mode, a new location is automatically approved and set to active status. On live environment, new locations added will require 1-2 weeks processing before becoming active. During that time they will be with pending status.
