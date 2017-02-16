<h2>Documentation</h2>
<div class="row">
    <div class="column">
        <div class="content">
            ![Get started](https://docs.fidel.uk/assets/images/get-started.svg "Get started")
            <img src="assets/images/get-started.svg"/>
            <h2>Get Started</h2>
            <h3>Integrate card-linked loyalty with quickstart guide</h3>
        </div>
    </div>
    <div class="column">
        <div class="content">
            <img src="https://docs.fidel.uk/assets/images/sdk-box.svg"/>
            <h2>SDKs</h2>
            <h3>Learn how to install and use iOS, Android and Javascript SDKs</h3>
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
            <h2>API Reference</h2>
            <h3>See all available API requests and object attributes</h3>
        </div>
    </div>
</div>

# Introduction
FIDEL is a card-linked API that lets developers create web and mobile applications for linking banks cards with reward services. FIDEL API lets you connect your applications to credit and debit cards and monitor transactions in-store and online, chip & pin or contactless.

**When a consumer makes a purchase at a specified store with a linked card, FIDEL API spots that transaction and sends it to you in real-time through webhooks.**

FIDEL API runs on top of the global payments network so it doesn't require changes on existing merchants infrastructure. No need for new software, POS integrations, staff training or new cards. All the PCI Compliance requirements are managed by us so you don't have to.

Currently the API is available for **VISA** and **Mastercard** networks in the **UK** and **Ireland**. We are working on adding support for other countries and networks. If you would like to deploy card-linked applications in other countries please contact us at [developer@fidel.uk](mailto:developer@fidel.uk).

Our developers’ community in Slack is the place to get help with our API, discuss ideas, and show off what you build. Hit the button to join:

<button>
  <img src="https://docs.fidel.uk/assets/images/slack-icon.svg" />
  Slack
</button>

We’re always happy to help and answer to any questions. You can also check our [support](fidel.uk) page or send us an [email](mailto:developer@fidel.uk).

Check out the [API Reference](fidel.uk) to see all available API requests, and visit the [Playground](fidel.uk) to test requests in real-time with sample data.

# Demo
The iOS, Android and Javascript SDKs provide you with simple UI to collect your user’s card details securely on the web or in your mobile apps.

![Intro demo](https://docs.fidel.uk/assets/images/intro-demo.png "Intro demo")

**Swift** | Javascript  
📝 ViewController.swift
```swift
// Present link card view controller modally
Fidel.presentLinkCardViewController(programId: "a32c45c1") { (success) -> Void in

    if success {
        // Card linked successfully
    }
}
```
<br/>

___
### Help?
If you have any questions please contact us by [email](fidel.uk) or talk with our developers in the [Slack channel](fidel.uk).

<br/>
