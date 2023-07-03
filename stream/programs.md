# Programs

## What is a Program

A program represents a distinct group of linked cards. The program is the parent object of your card-linked structure. Other objects such as cards and transactions will link to a program. Programs are unique and independent of each other, allowing you to have several independent programs with different uses inside your account.

To be able to use the Transaction Stream API, you need to create a program first.

##### Hierarchy diagram of the Fidel API object structure.

![](https://files.readme.io/7e19538-object-diagram.png "object-diagram.png")

## Create a Program

You can create a program through the Fidel API dashboard or via API. In [test mode](/stream/test-mode-and-live-mode#test-mode), you can create programs that use either Select Transactions API or Transaction Stream API, and in [live mode](/stream/test-mode-and-live-mode#live-mode), you can create programs that use the API type that your account is approved for. Once a program is created, a program identifier will be generated and is used as a data property when using the SDKs and APIs to link cards to this program. Cards are linked to programs and consequently are able to track all purchases in the program.

To create a new program on the Fidel API dashboard, go to the **Programs** page and click the **New program** button. If in test mode, choose the API type and enter a name for the new program.

##### Go to the Programs page on the dashboard to create a new program.

![](https://files.readme.io/5180f23-createProgram.png "createProgram.png")

## For Developers

You can create programs using the [API](https://transaction-stream.fidel.uk/reference/create-program). Using curl, we can add 'Program X' as follows:

```sh
curl -X POST https://api.fidel.uk/v1/programs \
  -H 'content-type: application/json' \
  -H 'fidel-key: <secret key>' \
  -d '{
    "name": "Program X",
    "metadata": {
      "customKey1": "customValue1",
      "customKey2": "customValue2"
    },
    "type" : "transaction-stream"
  }'
```

## Learn More

- After setting up your program, you can go ahead and [create test cards](https://transaction-stream.fidel.uk/docs/cards) and [webhooks](https://transaction-stream.fidel.uk/docs/webhooks), and [create test transactions](https://transaction-stream.fidel.uk/docs/transactions) to see how Fidel API works.
- Walk through our [tutorial](https://transaction-stream.fidel.uk/docs/tutorial) to start using the APIs and SDKs.
- Learn about the [web](https://fidelapi.com/docs/web-sdk/v3) and [mobile](https://fidelapi.com/docs/mobile-sdks) SDKs.
- Check out the [API reference docs](https://transaction-stream.fidel.uk/reference/introduction-1).
