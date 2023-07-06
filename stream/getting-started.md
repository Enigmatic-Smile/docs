# Getting Started

The following image shows an overview of your journey with Fidel API to using real-time transaction data to provide your users a seamless experience:

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/customer-journey.png)

This page describes how to get started with the first step: testing the platform in the sandbox environment.

## Creating an Account

To get an understanding of the platform and its object models, [create a Fidel API account](https://dashboard.fidel.uk/sign-up). This account will give you access to the [Fidel API dashboard](https://dashboard.fidel.uk), where you can configure your card-linked program and test how transactions are received.

## Navigating on the Dashboard

The Fidel API dashboard is based on the following concepts:

- programs
- cards
- transactions
- webhooks

**Programs** are used to group cards and transactions. **Webhooks** allow you to react to transaction events of a certain program.

![Programs in the Fidel API dashboard](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/programs.png "Programs in the Fidel API dashboard")

Note that the pages of **Brands** and **Offers** don’t play any role in the Transaction Stream API, they apply only to the Select Transactions API.

In each program you can see the cards, transactions and webhooks that belong to that program.

![Cards, transactions and webhooks of a program](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/transactions-stream-highlight.png "Cards, transactions and webhooks of a program")

### Account Settings

You can manage your Fidel API account in the **Account settings** page. To navigate there, open the drop-down menu next to your account name.

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/account-settings-navigation.png)

Under **Account settings**, you can find the following information:

- API version, developer keys
- Billing information (billing period, billing history)
- Users (inviting users, granting privileges)
- Security settings (password, card number obfuscation settings, etc.)

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/account-settings.png)

## Configuring Your Account

This section walks you through creating a program and adding users to your account.

1. When you first log in, you have a Demo Program created for you. However, that program is based on the Select Transactions API.  
   To start exploring the Transaction Stream API instead, use the **New program** button in the top right corner to create a new program. Select `Transaction Stream` as the program type.
2. To add users, navigate to the **Account settings** page. You can find this option in the drop-down menu next to your account name. Then create the users:
   1. Select the **Users** tab and click on **Invite users**.
   2. Type the email address of the user, and select an account type (admin or developer).
   3. Click on the **Invite** button.  
      The user will receive an email with a link to sign up for a user account in your environment.

## Testing Cards and Transactions

In test environments, you cannot use real-life cards and transactions. This is why you need to test the platform with Fidel API’s test cards. You can do so in the Playground page of the Fidel API dashboard. Read the [Configuring Your Account](/stream/getting-started#configuring-your-account) section above to understand how.

Developers can also use the API endpoints and the Fidel API SDKs to link test cards to programs in the platform, and to simulate incoming transactions. Read the [For Developers](/stream/getting-started#for-developers) section to learn how.

The following sections walk you through linking a test card and creating a test transaction in the dashboard. It also shows how to clear test transactions.

### Linking a Test Card

See how card linking and verification works by linking a test card to your program:

1. Navigate to the **Cards** page, click the **New card** button.
2. Read through the information about the card verification, and select **Continue** to start linking a test card.
3. Fidel API provides a set of test cards that you can use to explore the product (only in the Fidel API platform, and only in test mode).  
   To link a test card of Fidel API, add the following card details. For other test card numbers, check the [Test Card Numbers](/stream/getting-started#test-card-numbers) section.
   - Card number: `4444000000004000`
   - Expiry date: any date in the future
   - Country of issue: United States

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/cards-create-stream-2-filled.png)

4. Mark the checkbox to accept the terms and conditions, and click **Next**.
5. To verify the card, type `0.67` as the amount of the microcharge, and click **Verify**.  
   (In a real-life scenario, users get this value by accessing their bank account and checking the value of the microcharge from “Card Verification”. The microcharge should appear on the user’s account within a few minutes.)

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/cards-create-stream-3-filled.png)

6. After the successful card verification, the card will be displayed on the Cards page:

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/cards-create-stream-4.png)

### Creating a Test Transaction

To see how transactions look like in the Fidel API platform, complete the following steps:

1. Navigate to the [Playground](https://dashboard.fidel.uk/playground).
2. In the drop-down box, select `Transaction Stream`.
3. Click on **Create Transaction**.
4. Select the **Program**.
5. Select the **Card** you created previously.
6. Click **Run** and a test transaction will be created.
7. Check the transactions you just created:
   1. Navigate to **Programs** and select your program.
   2. Click **Transactions**.
   3. Observe the list of transactions you created in the previous step.
   4. Click on one of the transactions and observe the transaction details.  
      You can read about each data field [here](/stream/#collected-data).
8. Close the **Transaction details** section to go back to the transaction list.  
   As you can see there, new test transactions are waiting for authorization. Simulate the clearing by selecting the **…** menu and then clicking **Clear**.

![](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/transactions-more.png)

## Test Card Numbers

For security purposes, the Fidel API test environment doesn't accept live card numbers. Fidel API has a range of test card numbers you can use while integrating or testing the API: `4444000000004***` where `*` can be any digit. For example, `4444000000004278` is a valid test card number. The test card numbers work in the [Playground](https://dashboard.fidel.uk/playground), with the Fidel API SDKs and with the Cards APIs.

When linking a Fidel API test card, type in `$0.67` for verification.

## For Developers

### Get API keys

On the [Account page](https://dashboard.fidel.uk/account) you will find two SDK and two API keys. The SDK keys are used to link the web and mobile SDKs to your account and the API keys enable you to access the API endpoints directly. Use the test keys to access the [test environment](/stream/test-mode-and-live-mode#test-mode) data, and live keys to access the [live environment](/stream/test-mode-and-live-mode#live-mode) data.

Note that due to security reasons, **the secret API key should not be used in a mobile application or a front-end web app**.

![API keys](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/api-keys.png "API keys")

When you're ready to [go live](/stream/test-mode-and-live-mode#the-go-live-process) with your integration and test with credit/debit cards, you can request access to the live keys by clicking the **Upgrade to go live** button at the bottom left of the dashboard.

### Create a Test Transaction with the API

If you have a program and a card linked to it, you can use the `/transactions/test `endpoint to create a test transaction.

```sh
curl -X POST \
  https://api.fidel.uk/v1/transactions/test \
  -H 'content-type: application/json' \
  -H 'fidel-key: sk_test_**************************' \
  -d '{
    "amount": 10,
    "cardId": "eb54205c-5827-47cf-8444-1e2f125837e4",
    "currency": "USD"
    }'
```

### Install SDKs

You can use the web and mobile SDKs to link your user's card to your program without having to implement additional security measures on your server-side code. The [tutorial](doc:tutorial) walks you through the necessary steps to start using the APIs and SDKs.

Learn how to use the [web](/stream/web-sdk/v3) and [mobile](/stream/mobile-sdks) SDKs in your applications, how to customize the UI and capture card details securely on your website or mobile applications.

Check the [Programs](/stream/programs), [Cards](/stream/cards), [Transactions](/stream/transactions) and [Webhooks](/stream/webhooks) sections to get an overview of the Transaction Stream API object structure.

## Learn More

- Read more about the objects of the platform: [Programs](/stream/programs), [Cards](/stream/cards), [Transactions](/stream/transactions) and [Webhooks](/stream/webhooks)
- Walk through our [tutorial](/stream/tutorial) to start using the APIs and SDKs
- Learn about the [web](/stream/web-sdk/v3) and [mobile](/stream/mobile-sdks) SDKs
- Check out the [API reference docs](https://transaction-stream.fidel.uk/reference/introduction-1)
