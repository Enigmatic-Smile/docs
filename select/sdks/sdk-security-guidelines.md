# A guide for achieving security with Fidel API SDKs

The Fidel API SDKs provide a secure way for you to add card-linking capabilities to your applications. They also take care of all PCI Compliance requirements, so you don't have to. In this document, we'll write a few guidelines that you can use to integrate our SDKs securely into your application.

## Secure your SDK key

Your SDK key allows you to enroll cards into your program. That's why it is important for your application to protect it by applying the best secret management practices.

### Best SDK key management practices

> Important note: Do NOT store the SDK key in your client-side application's codebase or commit it in your repository. Of course, the same applies for your API key. 

- We recommend adding an endpoint to your server (if you don't already have one that you can use) that securely retrieves the Fidel API SDK key for your application to use and configure the Fidel API SDK.

- Using obfuscation techniques with your SDK key is also preferable.

- It is more secure to NOT store your SDK key on the device (in a local storage/database, shared preferences, user defaults or Keychain etc.), but, if you have to, please use a secure on-device storage (like the Keychain or KeyStore).

> Note: Another security benefit for using your server-side endpoint is that you can easily rotate the key, when it's necessary to do so.

## Data that you can store safely

The Fidel API SDKs return non-sensitive card information during the enrollment process (including the card type, the last four digits of the card, the expiration date). This information isn’t subject to PCI compliance, so you can store any of these properties for your convenience. Additionally, you can store anything returned by our API. Please check our SDKs' reference pages and our API's reference page for more details about the returned data.
