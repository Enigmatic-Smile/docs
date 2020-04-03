# Documentation

<div class="row">
  <div class="column">
    <a href="/getting-started" class="content" data-path="/getting-started">
      <img src="https://docs.fidel.uk/assets/images/get_started.svg" />
      <h2>Get Started</h2>
      <h3>Start building card-linked applications with our quickstart guide</h3>
    </a>
  </div>
  <div class="column">
    <a href="https://community.fidel.uk/c/Frequently-Asked-Questions" class="content">
      <img src="https://docs.fidel.uk/assets/images/help_center.svg" />
      <h2>Help Centre</h2>
      <h3>Find information and answers to frequently asked questions in our Community FAQ.</h3>
    </a>
  </div>
</div>
<div class="row">
  <div class="column">
    <a href="/web-sdk" data-path="/web-sdk" class="content">
      <img src="https://docs.fidel.uk/assets/images/web_sdk.svg" />
      <h2>Web SDK</h2>
      <h3>Learn to integrate a secure Javascript SDK to link cards on your website</h3>
    </a>
  </div>
  <div class="column">
    <a href="/mobile-sdks" class="content" data-path="/mobile-sdks">
      <img src="https://docs.fidel.uk/assets/images/mobile_sdk.svg" />
      <h2>Mobile SDKs</h2>
      <h3>PCI Compliant native SDKs for iOS, Android and React Native</h3>
    </a>
  </div>
</div>

## Introduction
Fidel is a card-linked API that lets developers create web and mobile applications for linking bank cards with reward services. Fidel API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

**When a consumer makes a purchase at a participating store with a linked card, Fidel API spots that transaction and sends it to your server in real-time through webhooks.** 💳⚡️

Fidel API runs on top of global payment networks, so it doesn’t require changes to existing merchant infrastructures. No need for new software, POS integrations, staff training or new cards. All the PCI compliance requirements are managed by us so you don’t have to.

Currently, the API is available in the **United States**, **UK**, **Ireland**, **Canada**, **Sweden**, **Norway** and **Finland**, with **Australia**, **Japan** and **South Africa** going live soon. Please refer to the up-to-date [list of countries and schemes](https://fidel.uk/products). We work continuously to add support for other countries and networks. If you would like to deploy card-linked applications in other countries please contact us at [developer@fidel.uk](mailto:developer@fidel.uk).

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.

Join our [Developer Community](https://community.fidel.uk/) for API discussions, documentation, frequently asked questions, roadmap and new features. We’re always happy to help and answer any questions. 

## Card Linking
The iOS, Android and Web SDKs provide you a secure UI to collect your user’s card details securely on the web or mobile.

By using Fidel SDKs, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements.

Your apps will receive an `id` back that identifies the card in each transaction.

<div>
  <img
    src="https://docs.fidel.uk/assets/images/sdks_main.png"
    srcset="https://docs.fidel.uk/assets/images/sdks_main.png, https://docs.fidel.uk/assets/images/sdks_main@2x.png 2x"
    alt="Preview of the web and mobile Fidel card linking UI"
  />
</div>

<button id="link-card-button" class="with-icon" type="submit" onclick="Fidel.openForm()">
  <img src="https://docs.fidel.uk/assets/images/eye.svg" />
  <span>View demo</span>
</button>

Use one of our [test card](https://docs.fidel.uk/cards/testing-card-numbers) numbers (for example `4444000000004004`) and enter an expiry date in the future.

##### Web SDK integration script

```html
fileName:index.html
<script
  type="text/javascript"
  src="https://resources.fidel.uk/sdk/js/v2/fidel.js"
  class="fidel-form"
  data-company-name="Your Company"
  data-key="pk_test_demo"
  data-program-id="bca59bd9-171b-4d1f-92af-4b2b7305268a"
  data-callback="callback"
  data-country-code="GBR"
  data-title="Link Card"
  data-subtitle="Earn 1 point for every £1 spent online or in-store"
  data-privacy-url="https://yourcompany.com/privacy"
  data-delete-instructions="tapping remove in your settings page."
  data-button-title="Link Card">
</script>
```

Check the [Web SDK documentation](/web-sdk) section for more information about all available parameters, customisation options, and the metadata nested object.
