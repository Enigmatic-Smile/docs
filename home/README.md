<h2>Documentation</h2>
<div class="row">
    <div class="column">
        <a href="/getting-started" class="content">
            <img src="https://docs.fidel.uk/assets/images/get_started.svg"/>
            <h2>Get Started</h2>
            <h3>Start building card-linked applications with our quickstart guide</h3>
        </a>
    </div>
    <div class="column">
        <a href="https://intercom.help/fidelapi" class="content">
            <img src="https://docs.fidel.uk/assets/images/help_center.svg"/>
            <h2>Help Centre</h2>
            <h3>Find information and answers to frequently asked questions</h3>
        </a>
    </div>
</div>
<div class="row">
    <div class="column">
      <a href="/web-sdk" class="content">
            <img src="https://docs.fidel.uk/assets/images/web_sdk.svg"/>
            <h2>Web SDK</h2>
            <h3>Learn to integrate a secure Javascript SDK to link cards on your website</h3>
      </a>
    </div>
    <div class="column">
        <a href="/mobile-sdk" class="content">
            <img src="https://docs.fidel.uk/assets/images/mobile_sdk.svg"/>
            <h2>Mobile SDKs</h2>
            <h3>PCI Compliant native SDKs for iOS and Android</h3>
        </a>
    </div>
</div>

<br/>

# Introduction
Fidel is a card-linked API that lets developers create web and mobile applications for linking bank cards with reward services. Fidel API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

**When a consumer makes a purchase at a participating store with a linked card, Fidel API spots that transaction and sends it to your server in real-time through webhooks.** 💳⚡️

Fidel API runs on top of global payment networks, so it doesn’t require changes to existing merchant infrastructures. No need for new software, POS integrations, staff training or new cards. All the PCI compliance requirements are managed by us so you don’t have to.

Currently, the API is available in the **UK**, **Ireland**, **Sweden**, **Canada** and the **United States**, with **Norway**, **Denmark**, **Finland**, **Japan**, **Australia** and **South Africa** going live soon. Please refer to the up to date list of countries and schemes on [fidel.uk](https://fidel.uk/). We work continuously to add support for other countries and networks. If you would like to deploy card-linked applications in other countries please contact us at [developer@fidel.uk](mailto:developer@fidel.uk).

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.

Join our developers’ Slack channel for API discussions, documentation, roadmap and new features. We’re always happy to help and answer any questions. Join by clicking the button below:

<a class="button with-icon" href="https://fidel.uk/join-us-on-slack" target="blank">
  <img src="https://docs.fidel.uk/assets/images/slack-icon.svg" />
  <span>Slack</span>
</a>

<br/>
<br/>

# Card Linking
The iOS, Android and Web SDKs provide you a secure UI to collect your user’s card details securely on the web or mobile.

By using Fidel SDKs, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements.

Your apps will receive an `id` back that identifies the card in each transaction.

<br/>


<img
  src="https://docs.fidel.uk/assets/images/sdks_main.png"
  srcset="https://docs.fidel.uk/assets/images/sdks_main.png, https://docs.fidel.uk/assets/images/sdks_main@2x.png 2x"
  alt="Preview of the web and mobile Fidel card linking UI"
/>

<button id="link-card-button" class="with-icon" type="submit" onclick="Fidel.openForm()">
  <img src="https://docs.fidel.uk/assets/images/eye.svg" />
  <span>View demo</span>
</button>

Use one of the test cards, such as `4444000000004004` and enter an expiry date in the future.

<h5>Web SDK integration script</h5>

```html
fileName:index.html
<script
  type="text/javascript"
  src="https://resources.fidel.uk/sdk/js/v1/fidel.js"
  class="fidel-form"
  data-company-name="Your Company"
  data-key="pk_test_demo"
  data-program-id="bca59bd9-171b-4d1f-92af-4b2b7305268a"
  data-callback="callback"
  data-country-code="GBR"
  data-title="Link Card"
  data-subtitle="Earn 1 point for every £1 spent online or in-store"
  data-privacy-url="https://yourcompany.com/privacy"
  data-delete-instructions="taping remove in your settings page."
  data-button-title="Link Card">
</script>
```
Check the [Web SDK documentation](/web-sdk) section for more information about all available parameters, customisation options, and the metadata nested object.

