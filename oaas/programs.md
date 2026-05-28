# Programs

A **Program** is a set of locations that uniquely represent an offline or online store where transactions from linked cards will be monitored. The Program is the parent object of your card linked structure. Other objects such as Cards, Locations, Webhooks and Transactions will always be linked to a Program. Brands are shared across Programs. They are unique and independent of each other allowing you to have several independent loyalty or rewards schemes inside your account.

##### Hierarchy diagram of the Fidel API object structure.

![Programs structure diagram](https://docs.fidel.uk/assets/images/programs_diagram_2020.png "Programs structure diagram")

## Create Program

To create a new program on the Fidel Dashboard, go to the [**Programs**](https://dashboard.fidel.uk/programs) page, click the **New program** button, add the Reward program name and select **'Transaction Distro Only'** as the Program type, then click **Create**. The Program will be created with the name provided.

The same can be done with the [API](https://fidel-oaas.readme.io/reference/create-program). Using curl:

```sh
curl -X POST https://api.fidel.uk/v1/programs \
  -H 'Content-Type: application/json' \
  -H 'Fidel-Key: <KEY>' \
  -d '{
    "name": "Program X",
    "metadata": {
      "customKey1": "customValue1",
      "customKey2": "customValue2"
    }
  }'
```

A `programId` will be generated and can be used to add marketplace offers to your program via the endpoint:

```
https://api.fidel.uk/v1/marketplace-offers/{marketplaceOfferId}/offers
```
