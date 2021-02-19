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

<p class="codepen" data-height="700" data-theme-id="dark" data-default-tab="result" data-user="fidelapi" data-slug-hash="BaQzjxp" data-preview="true" data-editable="true" style="height: 468px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Fidel Web SDK demo">
  <span>See the Pen <a href="https://codepen.io/fidelapi/pen/BaQzjxp">
  Fidel Web SDK demo</a> by Fidel API (<a href="https://codepen.io/fidelapi">@fidelapi</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>