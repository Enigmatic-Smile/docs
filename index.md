<div class="row">
  <div class="column">
    <a href="/getting-started" class="content" data-path="/getting-started">
      <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/get-started.svg" />
      <h2 data-no-link>Get Started</h2>
      <h3>Start building card-linked applications with our quickstart guide</h3>
    </a>
  </div>
  <div class="column">
    <a href="/tutorials/card-linking" class="content">
      <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/playground.svg" />
      <h2 data-no-link>Tutorials</h2>
      <h3>Learn how to build a card-linking feature into your applications.</h3>
    </a>
  </div>
</div>
<div class="row">
  <div class="column">
    <a href="/web-sdk/v3" data-path="/web-sdk/v3" class="content">
      <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/web_sdk.svg" />
      <h2 data-no-link>Web SDK</h2>
      <h3>Learn to integrate a secure Javascript SDK to link cards on your website</h3>
    </a>
  </div>
  <div class="column">
    <a href="/mobile-sdks" class="content" data-path="/mobile-sdks">
      <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/mobile_sdk.svg" />
      <h2 data-no-link>Mobile SDKs</h2>
      <h3>PCI Compliant native SDKs for iOS, Android and React Native</h3>
    </a>
  </div>
</div>

## Introduction

Fidel API provides a developer-friendly, secure and reliable API for businesses to link payment cards with mobile and web applications. Through a single API, developers can securely access data from the three major card networks and build their applications on top of the powerful payments infrastructure.

**When a consumer makes a purchase at a participating store with a linked card, Fidel API spots that transaction and sends it to your server in real-time through webhooks.** 💳⚡️

Fidel API runs on top of global payment networks, so it doesn’t require changes to existing merchant infrastructures. No need for new software, POS integrations, staff training or new cards. All the PCI compliance requirements are managed by us so you don’t have to.

Currently, the API is available in the **United States**, **UK**, **Ireland**, **Canada**, **Sweden** and **UAE**. **Japan** is currently in beta stage. Read the full list of available locations for the [Select Transactions API](https://fidelapi.com/products/select-transactions) and for the [Transaction Stream API](https://fidelapi.com/products/transaction-stream) on the product pages. We work continuously to add support for other countries and networks. If you would like to deploy card-linked applications in other countries please [contact us](https://fidelapi.com/contact).

You can see an example implementation for integrating the Fidel APIs and Web SDK in our [sample application on GitHub](https://github.com/FidelLimited/fidel-api-sample-app).

Check out the [API Reference](https://reference.fidel.uk) to see all available requests, code examples and response payloads.

Join our [Developer Community](https://community.fidel.uk/) for API discussions, documentation, frequently asked questions, roadmap and new features. We’re always happy to help and answer any questions.

## Card Linking
The [iOS](/mobile-sdks/#ios), [Android](/mobile-sdks/#android) and [Web](/web-sdk/v3) SDKs provide you a secure UI to collect your user’s card details securely on the web or mobile.

By using Fidel API SDKs, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements. [Our Card-Linking tutorial](/tutorials/card-linking) explains in detail all the steps required to build a card-linking feature into your application using the Fidel API SDKs.

Your apps will receive an `id` back that identifies the card in each transaction.

<div>
  <img
    src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/sdks_main.png"
    srcset="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/sdks_main.png, https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/sdks_main@2x.png 2x"
    alt="Preview of the web and mobile Fidel API card linking UI"
  />
</div>

<button id="link-card-button" class="with-icon" type="submit" onClick={() => window.location.href = "/docs/web-sdk/v3/#web-sdk-codepen"}>
  <img src="https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/eye.svg" />
  <span>View demo</span>
</button>

Use one of our [test card](/cards/#testing-card-numbers) numbers (for example `4444000000004004`) and enter an expiry date in the future.

##### Web SDK integration script

```html
fileName:index.html
<script
  type="text/javascript"
  src="https://resources.fidel.uk/sdk/js/v3/fidel.js"
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
  data-button-title="Link Card"
></script>
```

Check the [Web SDK documentation](/web-sdk/v3) section for more information about all available parameters, customization options, and the metadata nested object.
