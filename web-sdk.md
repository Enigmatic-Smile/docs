# Web SDK

**The Fidel Web SDK** is a secure HTML iframe with customisable pre-built UI that allows you to easily collect credit card details, tokenise and link credit/debit cards with rewards services from your website, e-commerce platform or mobile apps. By using the Fidel Web SDK, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements for you.

<img
  src="https://docs.fidel.uk/assets/images/sdk_web.png"
  srcset="https://docs.fidel.uk/assets/images/sdk_web.png, https://docs.fidel.uk/assets/images/sdk_web@2x.png 2x"
  alt="Preview of the web SDK"
/>

After successfully tokenising and linking the card on Visa, Mastercard or American Express networks, Fidel API returns the created card object with a unique `id` that you must use to link each unique card and transaction to your user’s account. The `id` of each linked card is present on the transaction object as `cardId`. You can now create card-linked web and mobile applications with online and offline transactional data visibility in a matter of minutes.

All modern desktop and mobile browsers are supported, including Chrome, Firefox, Safari, Microsoft IE and Edge. Please contact us at [devrel@fidel.uk](mailto:devrel@fidel.uk) if you experience any browser related issues.

## Integrating Web SDK

<Codepen slug="BaQzjxp" title="Fidel Web SDK v3" />

## Customization

<MultipleCodepens
  codepens={[
    { buttonText: 'Theme 1', slug: 'XWNezea', title: 'Fidel Web SDK v3' },
    { buttonText: 'Theme 2', slug: 'OJbxZzW', title: 'Fidel Web SDK v3' },
    { buttonText: 'Theme 3', slug: 'eYBGrrQ', title: 'Fidel Web SDK v3' },
  ]}
/>