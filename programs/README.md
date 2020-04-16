# Programs

A **Program** is a set of locations that uniquely represent an offline or online store where transactions from linked cards will be monitored. The Program is the parent object of your card linked structure. Other objects such as Cards, Locations, Webhooks and Transactions will always be linked to a Program. Brands are shared across Programs. They are unique and independent of each other allowing you to have several independent loyalty or rewards schemes inside your account.

##### Hierarchy diagram of the Fidel API object structure.

![Programs structure diagram](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/programs_diagram_2020.png "Programs structure diagram")

## Create Program
To create a new program, go to the **Programs** page on the dashboard, click the **+** icon and enter a name. The Program will be created with the name provided without any initial locations.

##### Go to the Programs page on the dashboard to create a new program.

![Create program](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/create-program.png "Create program")

A `programId` will be generated and is used as a data property when using the SDKs to link Cards to this Program. Cards are linked to Programs and consequently to Locations.


