<h2>Documentation</h2>
<div class="row">
    <div class="column">
        <a href="/gettingstarted" class="content">
            <img src="https://docs.fidel.uk/assets/images/get-started.svg"/>
            <h2>Get Started</h2>
            <h3>Start building card-linked applications with our quickstart guide</h3>
        </a>
    </div>
    <div class="column">
      <a href="/web" class="content">
            <img src="https://docs.fidel.uk/assets/images/icon-desktop.svg"/>
            <h2>Web SDK</h2>
            <h3>Use our pre-built UI to link cards on your website</h3>
        </a>
    </div>
</div>
<div class="row">
    <div class="column">
        <a href="https://dashboard.fidel.uk/playground" class="content">
            <img src="https://docs.fidel.uk/assets/images/playground.svg"/>
            <h2>Playground</h2>
            <h3>Test API requests in real-time and see how we format the returned data</h3>
        </a>
    </div>
    <div class="column">
        <div class="content">
            <img src="https://docs.fidel.uk/assets/images/icon-mobile.svg"/>
            <h2>Mobile SDKs</h2>
            <h3>Coming soon... We are working on native SDKs for iOS and Android</h3>
        </div>
    </div>
</div>

<br/>

# Introduction
FIDEL is a card-linked API that lets developers create web and mobile applications for linking banks cards with reward services. FIDEL API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

**When a consumer makes a purchase at a specified store with a linked card, FIDEL API spots that transaction and sends it to you in real-time through webhooks.**

FIDEL API runs on top of the global payments network so it doesn't require changes on existing merchants infrastructure. No need for new software, POS integrations, staff training or new cards. All the PCI Compliance requirements are managed by us so you don't have to.

Currently the API is available for **VISA** and **Mastercard** networks in the **UK** and **Ireland**. We are working on adding support for other countries and networks. If you would like to deploy card-linked applications in other countries please contact us at [developer@fidel.uk](mailto:developer@fidel.uk).

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.

Join our developers' Slack channel for API discussions, documentation, roadmap and new features. We’re always happy to help and answer any questions. Join by clicking the button below:

<a href="https://fidel-developers-slack-invites.herokuapp.com/" target="blank">
  <button>
    <img src="https://docs.fidel.uk/assets/images/slack-icon.svg" />
    Slack
  </button>
</a>

<br/>

# Demo
The iOS, Android and Web SDKs provide you with pre-built UI to collect your user’s card details securely on the web or in your mobile apps.

<h5>Preview of web and mobile FIDEL card linking UI</h5>

![Intro demo](https://docs.fidel.uk/assets/images/intro-demo.png "Intro demo")

To see the Web SDK working, please click the the 'Link Card' button below:

<button style="background: #4dadea; border: none; border-radius: 3px; color: #ffffff;" type="submit" onclick="Fidel.openForm()">Link Card</button>

Use one of the test cards, such as `4444000000004004` and enter an expiry date in the future.

<h5>Web SDK integration script</h5>

```html
fileName:index.html
<script type="text/javascript" src="https://resources.fidel.uk/sdk/js/v1/fidel.js"
    class="fidel-form"
    data-company-name="Fidel"
    data-key="pk_test_demo"
    data-program-id="bca59bd9-171b-4d1f-92af-4b2b7305268a"
    data-callback="callback"
    data-metadata="metadata"
    data-button-title="Link Card"
    data-lang="en"
    data-subtitle="Earn 1 point for every £1 spent online or in-store"
    data-title="Link Card">
</script>
```
