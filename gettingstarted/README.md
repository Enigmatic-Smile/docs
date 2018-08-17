## Getting Started

**FIDEL API** helps you collect payment card details in minutes on iOS, Android and Web with the help of native SDKs. We developed customizable UI using an iFrame so you don't have to handle sensitive information on your servers.

Card numbers are instantly tokenised and we return to you a unique token that identifies each successfully linked card on your database. You should save the `id` of the card object that is returned inside your user's object. Every transaction we will send to you will have a `cardId` that you can use to identify the user that made that transaction.

<br/>

# Step 1 - Create Account
Before getting started with the integration you need to **create a FIDEL API account** that will give you access to the dashboard, card-linked program configuration, playground environment and API keys. You can create a new account by clicking [here](https://dashboard.fidel.uk/sign-up).

<br/>

# Step 2 - Get API Key
After you created an account, in your account page on the dashboard, you will find two SDK keys and two API keys. The SDK keys are used to link the web and mobile SDKs to your account and the API keys to access the API endpoints directly. [See API Reference.](https://reference.fidel.uk) To access the test environment data you must use the test keys and to access the live environment data use the live keys.

<div class="info-box">
    <small>Important note</small><br />
To use the **Create Card** endpoint you must use the test public key. This only applies to this endpoint. Using the **Create Card** endpoint on live environment requires your company to be PCI Compliant. If you want to use it instead of the SDKs, please contact us at developer@fidel.uk.
</div>


<h5>Your SDK and API keys are displayed in your account page</h5>

![API keys](https://docs.fidel.uk/assets/images/api-keys.png "API keys")

<br/>

When you're ready to go live with your integration and test with plastic credit/debit cards on VISA and Mastercard networks you can request access to the live keys by using the test-live switch on your dashboard.

<br/>

# Step 3 - Install SDKs
You can use our pre-built web UI to collect user's card number and link cards to your program without the need of additional security implementations on your server-side code.

We are working on two native SDKs for mobile, iOS and Android, that will be available soon.

<br/>

<div class="row">
<div class="column">
    <a href="/web" class="content">
        <img src="https://docs.fidel.uk/assets/images/icon-desktop.svg"/>
        <h2>Web</h2>
        <h3>Use our pre-built UI to link cards on your website</h3>
    </a>
</div>
    <div class="column">
        <a href="/gettingstarted" class="content">
            <img src="https://docs.fidel.uk/assets/images/icon-mobile.svg"/>
            <h2>Mobile</h2>
            <h3>Coming soon... We are working on native SDKs for iOS and Android</h3>
        </a>
    </div>
</div>

<br/>

Please follow the specific [Web SDK](/web) integration guide to learn how to configure and customize the pre-built UI to collect card details securely on your website. You should visit the [Programs](/programs), [Cards](/cards), [Transactions](/transactions) and [Webhooks](/webhooks) sections to get an overview of the FIDEL API object structure and description.
