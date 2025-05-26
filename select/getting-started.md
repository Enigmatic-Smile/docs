# Getting Started

Fidel API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

Card numbers are instantly tokenised and we return to you a unique token that identifies each successfully linked card on your database. You should save the `id` of the card object that is returned inside your user's object. Every transaction we will send to you will have a `cardId` that you can use to identify the user that made that transaction.

## 1. Create an account

First step is to [create a Fidel API developer account](https://dashboard.fidel.uk/sign-up) that will give you access to the developers' dashboard, card-linked program configuration, sandbox environment and test API keys.

## 2. Managing API Keys

After you created an account, in your account page on the dashboard, you will need to create an API or SDK key. **This will be a necessary step for integrating with Fidel**.

The SDK keys are used to link the web and mobile SDKs to your account and the API keys to access the API endpoints directly. [See API Reference.](https://reference.fidel.uk) To access the test environment data you must use the test keys and to access the live environment data use the live keys.

### Accessing API Key Settings

1. Log into your account.
2. Navigate to **Account Settings** > **Plan**.

You’ll now see a list of your available API keys, along with their **creation date** and **last used** date (you can hover on the last used to see the full timestamp).

To switch between **Live** and **Test** keys, use the **Live Mode toggle** located at the bottom left of the screen.

##### Plan tab on account settings

![API keys](https://docs.fidel.uk/assets/images/list-api-keys.png "Plan tab on account settings")

### Creating a new API Key

![Create API key](https://docs.fidel.uk/assets/images/create-api-key.png "Create API Key")

1. Click the **Create** button.
2. In the dialog, provide:
   - The **type** of key you intend to create (SDK or API)
3. Click **Create key**.
4. **Copy and store the key securely. You won’t be able to view it again (you can click on it in order to copy it to your clipboard).**

##### Generated API key
![Generated API key](https://docs.fidel.uk/assets/images/api-key-generated.png "Generated API key")

> ⚠️ **Important:** Treat your API keys like passwords. Do not share them publicly or commit them to version control.

### Revoking an API Key

1. Go to the **Plan** section under your account settings.
2. Find the key you want to revoke.
3. Click the **Revoke** button next to it.
4. Confirm the revocation in the dialog by entering your password (for security reasons).

<div style="border-left: 4px solid #f0ad4e; padding: 0.75em 1em; background-color: #fff8e1; margin: 1em 0;">
<strong>Note:</strong> You can have a maximum of <strong>two keys per key type</strong> (e.g., <strong>API</strong> and <strong>SDK</strong>).<br />
To <strong>revoke</strong> a key, you must have at least <strong>one other active key</strong> of the same type.
</div>

##### Revoke API key
![Revoke API key](https://docs.fidel.uk/assets/images/revoke-api-key.png "Revoke API key")

> ⛔ **Warning:** Revoked keys stop working immediately and cannot be restored.

### Best Practices

> 💡 Use the following best practices to secure your API/SDK usage:

- Use separate keys for different environments (e.g., dev, staging, production)
- Rotate keys regularly
- Immediately revoke unused or compromised keys

## 3. Install SDKs

You can use the web and mobile SDKs to capture your user’s card number and link cards to your program without the need of additional security implementations on your server-side code.

Click on the links below to see how to use the web and mobile SDKs in your applications.

<div class="row">
  <div class="column">
    <a href="/select/sdks/web/v3" class="content">
      <img src="https://docs.fidel.uk/assets/images/svgs/web_sdk.svg" />
      <h2 data-no-link>Web</h2>
      <h3>Customizable and secure iframe to link cards on your website</h3>
    </a>
  </div>
  <div class="column">
    <a href="/select/sdks/ios/guide-v2" class="content">
      <img src="https://docs.fidel.uk/assets/images/svgs/mobile_sdk.svg" />
      <h2  data-no-link>Mobile</h2>
      <h3>PCI compliant SDKs for iOS, Android and React Native</h3>
    </a>
  </div>
</div>

The [Card-Linking tutorial](/select/tutorials/card-linking) walks you through all the necessary steps to start using the Fidel APIs and SDKs.

Follow the links above to access more detailed documentation and integration instructions for the [SDKs](/select/sdks/web/v3), learn how to customize the UI and capture card details securely on your website or mobile applications. Check [Brands](/select/brands), [Programs](/select/programs), [Cards](/select/cards), [Transactions](/select/transactions) and [Webhooks](/select/webhooks) sections to get an overview of the Fidel API object structure.

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.

You can see an example implementation for integrating the Fidel APIs and Web SDK in our [sample application on GitHub](https://github.com/Enigmatic-Smile/fidel-api-sample-app).
