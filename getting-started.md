# Getting Started

Fidel API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

Card numbers are instantly tokenised and we return to you a unique token that identifies each successfully linked card on your database. You should save the `id` of the card object that is returned inside your user's object. Every transaction we will send to you will have a `cardId` that you can use to identify the user that made that transaction.

## 1. Create an account
First step is to [create a Fidel API developer account](https://dashboard.fidel.uk/sign-up) that will give you access to the developers' dashboard, card-linked program configuration, sandbox environment and test API keys.

## 2. Get API keys
After you created an account, in your account page on the dashboard, you will find two SDK keys and two API keys. The SDK keys are used to link the web and mobile SDKs to your account and the API keys to access the API endpoints directly. [See API Reference.](https://reference.fidel.uk) To access the test environment data you must use the test keys and to access the live environment data use the live keys.

##### Your SDK and API keys are displayed in your account page

![API keys](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/api-keys.png "API keys")

When you're ready to go live with your integration and test with plastic credit/debit cards on VISA, Mastercard and American Express networks you can request access to the live keys by using the test-live switch on your dashboard.

## 3. Install SDKs
You can use the web and mobile SDKs to capture your user’s card number and link cards to your program without the need of additional security implementations on your server-side code.

Click on the links below to see how to use the web and mobile SDKs in your applications.

<div class="row">
  <div class="column">
    <a href="/web-sdk/v2" class="content">
      <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/web_sdk.svg" />
      <h2 data-no-link>Web</h2>
      <h3>Customisable and secure iframe to link cards on your website</h3>
    </a>
  </div>
  <div class="column">
    <a href="/mobile-sdks" class="content">
      <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/mobile_sdk.svg" />
      <h2  data-no-link>Mobile</h2>
      <h3>PCI compliant SDKs for iOS, Android and React Native</h3>
    </a>
  </div>
</div>

Follow the links above to access more detailed documentation and integration instructions for the [Web SDK](/web-sdk/v2) and [mobile SDKs](/mobile-sdks), learn how to customise the UI and capture card details securely on your website or mobile applications. Check [Brands](/brands), [Programs](/programs), [Cards](/cards), [Transactions](/transactions) and [Webhooks](/webhooks) sections to get an overview of the Fidel API object structure.

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.
