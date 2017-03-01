## Getting Started

**FIDEL API** helps you collect payment card details in minutes on iOS, Android and Web with the help of native SDKs. We developed customizable UI using an iFrame so you don't have to handle sensitive information on your servers.

Card numbers are instantly tokenised and we return to you a unique token that identifies each successfully linked card on your database. You should save the `id` of the card object that is returned inside your user's object. Every transaction we will send to you will have a `cardId` that you can use to identify the user that made that transaction.

<br/>

# Step 1: Create Account
Before getting started with the integration you need to **create a FIDEL API account** that will give you access to the dashboard, card-linked program configuration, playground environment and API keys. You can create a new account by clicking [here](sign-up).

<br/>

# Step 2: Get API Key
Your test API Key is available on account page of your dashboard. You should start by copying the test key that will allow you to test your integration on a sandbox environment and create test transactions using the **API Playground**.

<h5>Your API keys are displayed in your account page</h5>

![API keys](https://docs.fidel.uk/assets/images/api-keys@2x.png "API keys")

<br/>

When you're ready to go live with your integration and test with plastic credit/debit cards on VISA and Mastercard networks you can request access to the live key by using the test-live switch on your dashboard.

<br/>

# Step 3: Install SDKs
You can use our pre-built web UI to collect user's card number and link cards to your program without the need of additional security implementations on your server-side code.

We are working on two native SDKs for mobile, iOS and Android, that will be available in the next few weeks.

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

Please follow the specific [Web SDK](/web) integration guide to learn how to configure and customize the pre-built UI to collect card details securely on your website.
