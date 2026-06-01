# Programs

A **Program** is a set of locations that uniquely represent an offline or online store where offers are linked to. Objects such as Offers, Locations and Webhooks will always be linked to a Program. Brands are shared across Programs. They are unique and independent of each other allowing you to have several independent loyalty or rewards schemes inside your account.

## Create Program

To create a new program on the Fidel Dashboard:

1. Go to the [**Programs**](https://dashboard.fidel.uk/programs) page
2. Click the **New program** button
3. Enter the reward program name
4. Select **'Transaction Distro Only'** as the Program type
5. Click **Create**

You can also create a program using the [API](https://fidel-oaas.readme.io/reference/create-program):

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
