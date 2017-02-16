<h2>Documentation</h2>
<div class="row">
    <div class="column">
        <div class="content">
            <img src="https://docs.fidel.uk/assets/images/get-started.svg"/>
            <h2>Get Started</h2>
            <h3>Integrate card-linked loyalty with our quickstart guide</h3>
        </div>
    </div>
    <div class="column">
        <div class="content">
            <img src="https://docs.fidel.uk/assets/images/sdk-box.svg"/>
            <h2>Mobile SDKs</h2>
            <h3>Coming soon... We are working on iOS and Android native SDKs</h3>
        </div>
    </div>
</div>
<div class="row">
    <div class="column">
        <div class="content">
            <img src="https://docs.fidel.uk/assets/images/playground.svg"/>
            <h2>Playground</h2>
            <h3>Test API requests in real-time and see how we format the returned data</h3>
        </div>
    </div>
    <div class="column">
        <div class="content">
            <img src="https://docs.fidel.uk/assets/images/api-reference.svg"/>
            <h2>Web SDK</h2>
            <h3>Easily link cards on your website using the Javascript SDK</h3>
        </div>
    </div>
</div>

<br/>

# Introduction
FIDEL is a card-linked API that lets developers create web and mobile applications for linking banks cards with reward services. FIDEL API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

**When a consumer makes a purchase at a specified store with a linked card, FIDEL API spots that transaction and sends it to you in real-time through webhooks.**

FIDEL API runs on top of the global payments network so it doesn't require changes on existing merchants infrastructure. No need for new software, POS integrations, staff training or new cards. All the PCI Compliance requirements are managed by us so you don't have to.

Currently the API is available for **VISA** and **Mastercard** networks in the **UK** and **Ireland**. We are working on adding support for other countries and networks. If you would like to deploy card-linked applications in other countries please contact us at [developer@fidel.uk](mailto:developer@fidel.uk).

Join our developers' Slack channel for API discussions, documentation, roadmap and new features. We’re always happy to help and answer any questions. Join by clicking the button below:

<button>
  <img src="https://docs.fidel.uk/assets/images/slack-icon.svg" />
  Slack
</button>

<br/>

# Demo
The iOS, Android and Javascript SDKs provide you with simple UI to collect your user’s card details securely on the web or in your mobile apps.

<h5>Preview of web and mobile FIDEL card linking iFrame.</h5>

![Intro demo](https://docs.fidel.uk/assets/images/intro-demo.png "Intro demo")

<h5>Web SDK integration code</h5>

```javascript
fileName:index.html
<script src="https://resources.fidel.uk/fidel.js"
    class="fidel-form"
    data-key="pk_test_b4325e25-d261-4dc6-a56f-8f22d881e295"
    data-title="Brand Name"
    data-sub-title="Earn 1 point for every £1 spent online or in-store."
    data-logo="https://brand-logo.png"
    data-lang="en"
    data-button-color="#ffffff"
    data-button-title="Link Card"
    data-button-text-color="#000000">
</script>
```

<br/>

___
### Help?
If you have any questions please contact us by [email](mailto:developer@fidel.uk) or talk with our developers in the [Slack channel](fidel.uk).

<br/>
