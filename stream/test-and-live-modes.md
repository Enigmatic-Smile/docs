# Test Mode and Live Mode

## Test Mode

When you create your Fidel API account and start exploring the platform, you are doing so in test mode, which means in a test environment. In this mode you can only create and use test cards and mock transactions, as this environment is not connected to any live card networks.

The test environment provides a simulation of the live environment, meaning you can carry out most basic actions (create, retrieve, etc.) on test data. However, as the test environment is not connected to the card networks, it does not account for all behaviours of the payment ecosystem, such as the type of terminals, the POS provider, card types, etc. It also does not simulate the merchant onboarding process (e.g., the lead time for onboarding and the troubleshooting process if a location cannot be identified).

## Live Mode

After signing the contract with Fidel API and receiving the approval of your use case from the card networks, you can **go live**. Going live with a program means that **live mode** becomes available in your Fidel API account, in addition to the test mode. This live environment is connected to the card networks that have approved your use case, so you can start linking cards to your program and receiving actual, real-time transactions via Fidel API.

## The Go-Live Process

Click on the **Upgrade to go live** button in the bottom left corner to access the live environment. The launch process will request business details, use case description and a card for billing purposes. The go-live request will be sent to Fidel API for review and approval.

![](https://docs.fidel.uk/assets/images/upgrade-to-go-live-button.png)

## Switching Between Test and Live Mode

After the approval of your request, you can switch between the test and live environments by using the **Live mode** toggle in the bottom left corner on the Fidel API dashboard.

![](https://docs.fidel.uk/assets/images/live-mode-toggle.png)

In your code, using the live environment means using the live API and SDK keys instead of the test keys. You get access to your live API and SDK keys when going live. You can find your keys on the **Account settings** page.
