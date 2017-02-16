## Transactions

The transaction object is the central piece of data of your card-linked application. When a consumer makes a purchase with an linked card at a program's location, FIDEL API spots the transaction and sends it to your server using the `transaction.auth` webhook event.

Application logic can be implemented in real-time, when the user pays in-store (only available on MasterCard. Please email [developer@fidel.uk](fidel.uk) for VISA availability) or when the transaction is cleared usually 24-48 hours after the purchase.

Transaction objects are sent to specified webhooks in JSON format. If you’re using MasterCard real-time transaction notifications, you will receive them as the customer pays in-store. All Visa and Mastercard settled transactions are processed and sent daily at 12:00pm UTC to your servers through `transaction.clearing.success` webhook events.

Please see below the properties of the transaction JSON object are self explanatory.

<br/>

# Create Transaction

For testing purposes, you can use the **API Playground** to create transactions and test your application logic.

To create a transaction you will need a test program, at least one location where the transaction can be originated from and a webhook where the transaction object will be sent to at creation time. Also a test card will need to be created and linked to the test program.

On the dashboard, go to **API Playground** and click on **Create transaction** on the left menu. The method will be set to POST and the endpoint to _/transactions/test_. An editable sample JSON object like the following one will be use to create the transaction.

To create a test transaction you only need to submit three properties, the `cardId`, `locationId` and the `amount` of the transaction you want.

<br/>

<h5>Create sample transactions in test mode using the API Playground.</h5>

![Create transaction](https://docs.fidel.uk/assets/images/create-transaction.png "Create transaction")

<br/>

Click **Send** and a test transaction will be created and sent to the webhook URL created for this program with the event property set to `transaction.auth`. An example of the returned transaction object is represented below.

<br/>

<h5>JSON transaction object</h5>

```json
fileName:transaction.json
{
  "items": [
    {
      "id": "6b9d11af-cd1e-4bc9-8000-78f6bfcd3db3",
      "accountId": "a3de60c3-849a-4faa-8447-bcd16efb148c",
      "programId": "60b4f64e-c6f2-4c0b-a00e-b60192632dfd",
      "cardId": "42b8eb31-3cb9-4171-8077-c6669d9aa7a2",
      "locationId": "7398177f-e2c1-4055-b27e-ba106483c08f",
      "amount": 133,
      "currency": "GBP",
      "countryCode": "GB",
      "card": "mastercard",
      "created": "2017-02-13T17:02:12.535Z",
      "updated": "2017-02-13T17:17:02.833Z",
      "metadata": {},
    }
  ],
  "resource": "/v1d/transactions/test",
  "status": 201,
  "execution": 2803.465225
}
```
