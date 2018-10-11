## Getting Started

**FIDEL API** helps you collect payment card details in minutes on iOS, Android and Web with the help of native SDKs. We developed customizable UI using an iFrame so you don't have to handle sensitive information on your servers.

Card numbers are instantly tokenised and we return to you a unique token that identifies each successfully linked card on your database. You should save the `id` of the card object that is returned inside your user's object. Every transaction we will send to you will have a `cardId` that you can use to identify the user that made that transaction.

# 1. Create an account 👨🏻‍💻
First step is to **create a Fidel API developer account** that will give you access to the developers' dashboard, card-linked program configuration, sandbox environment and test API keys. You can create a new account [here](https://dashboard.fidel.uk/sign-up).

<br/>

# 2. Get API keys 🔑
After you created an account, in your account page on the dashboard, you will find two SDK keys and two API keys. The SDK keys are used to link the web and mobile SDKs to your account and the API keys to access the API endpoints directly. [See API Reference.](https://reference.fidel.uk) To access the test environment data you must use the test keys and to access the live environment data use the live keys.

<div class="info-box">
    <small>Important note</small><br />
    To use the <strong>Create Card</strong> endpoint you must use the test public key. This only applies to this endpoint. Using the <strong>Create Card</strong> endpoint on live environment requires your company to be PCI Compliant. If you want to use it instead of the SDKs, please contact us at developer@fidel.uk.
</div>


Your SDK and API keys are displayed in your account page

![API keys](https://docs.fidel.uk/assets/images/api-keys.png "API keys")

<br/>

When you're ready to go live with your integration and test with plastic credit/debit cards on VISA, Mastercard and American Express networks you can request access to the live keys by using the test-live switch on your dashboard.

<br/>

# 4. Install SDKs 📱
You can use the web and mobile SDKs to capture your user's card number and link cards to your program without the need of additional security implementations on your server-side code.

Click on the links below to  on how to use the web and mobile SDKs in your applications.

<br/>

<div class="row">
<div class="column">
    <a href="/web-sdk" class="content">
        <img src="https://docs.fidel.uk/assets/images/web_sdk.svg"/>
        <h2>Web</h2>
        <h3>Customisable and secure iFrame to link cards on your website</h3>
    </a>
</div>
    <div class="column">
        <a href="/getting-started" class="content">
            <img src="https://docs.fidel.uk/assets/images/mobile_sdk.svg"/>
            <h2>Mobile</h2>
            <h3>PCI Compliant native SDKs for iOS and Android</h3>
        </a>
    </div>
</div>

<br/>

Follow the links above to access more detailed documentation and integration instructions for the [Web SDK](/web-sdk) and [mobile SDKs](/mobile-sdk), learn how to customise the UI and capture card details securely on your website or mobile applications. Check [Brands](/brands), [Programs](/programs), [Cards](/cards), [Transactions](/transactions) and [Webhooks](/webhooks) sections to get an overview of the FIDEL API object structure.

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.
