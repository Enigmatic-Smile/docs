## Web SDK
**FIDEL Web SDK** is a secure HTML iFrame with customisable pre-built UI that allows you to easily collect credit card details, tokenise and link credit/debit cards with rewards services from your website, e-commerce platform or mobile apps. By using FIDEL Web SDK, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements for you.

<h5>Preview of FIDEL card-linking Web SDK</h5>

![Web form](https://docs.fidel.uk/assets/images/web-form.png "Web form")

<br/>

After successfully tokenising and linking the card on Visa or MasterCard networks, FIDEL API returns the created card object with a unique `id` that you must use to link each unique card and transaction to your user’s account. The `id` of each linked card is present on the transaction object as `cardId`. You can now create card-linked web and mobile applications with online and offline transactional data visibility in a matter of minutes.

All modern desktop and mobile browsers are supported, including Chrome, Firefox, Safari, Microsoft IE and Edge. Please contact us at [developer@fidel.uk](mailto:developer@fidel.uk) if you experience any browser realted issues.

<br/>

# Integrating Web SDK
You can easily integrate FIDEL Web SDK in your website or mobile app with only a few lines (or one) of code.

<h5>FIDEL Web SDK script.</h5>

```html
fileName:index.html
<script type="text/javascript" src="https://resources.fidel.uk/sdk/js/v1/fidel.js"
    class="fidel-form"
    data-company-name="Fidel"
    data-key="pk_test_demo"
    data-program-id="bca59bd9-171b-4d1f-92af-4b2b7305268a"
    data-callback="callback"
    data-metadata="metadata"
    data-auto-open="false"
    data-overlay-close="false"
    data-background-color="#ffffff"
    data-button-color="#4dadea"
    data-button-title="Link Card"
    data-button-title-color="#ffffff"
    data-lang="en"
    data-logo="https://company.com/logo.png"
    data-subtitle="Earn 1 point for every £1 spent online or in-store"
    data-subtitle-color="#000000"
    data-terms-color="#000000"
    data-title="Link Card"
    data-title-color="#000000">
</script>
```

<br/>

To integrate **FIDEL Web SDK** in your website or mobile app, you need to add the script above in your website or mobile web view. We are working on native mobile SDKs for iOS and Android that will simplify the mobile integration.

Most of the data properties in the script are self explanatory but you can check the description of each property below.

<br/>

- **data-company-name**: (required) the name of the company using card-linking. _Max 35 chars._

- **data-key**: (required) a valid Public Key.

- **data-program-id**: (required) the id of the program to link the card to.

- **data-callback**: name of the global callback function.

- **data-metadata**: name of the global metadata object.

- **data-auto-open**: (default: false) whether the web form auto opens on page load.

- **data-auto-close**: (default: true) whether the web form auto closes on successful card link.

- **data-background-color**: CSS color code of the form background.

- **data-button-color**: CSS color code of the button background.

- **data-button-title**: the button title. _Max 35 chars._

- **data-button-title-color**: CSS color code of the button title.

- **data-close-events**: (default: true) whether close button, overlay click and escape key events are added to the form.

- **data-lang**: the localization language to be used.

- **data-logo**: the logo URL of the company. _Height 35px. Extensions jpg, jpeg, png._

- **data-subtitle**: the subtitle of the web form. _Max 110 chars._

- **data-subtitle-color**: CSS color code of the subtitle.

- **data-terms-color**: CSS color code of the terms and conditions.

- **data-title**: the title of the web form. _Max 25 chars._

- **data-title-color**: CSS color code of the title.

<br/>

The `data-auto-open` property allows you to open the web form automatically on page load if set to `true`. If `data-auto-close` is set to `false` the form won't be automatically closed after linking a card. You can set `data-close-events` to `false` and the form won't add the default close events. After adding the Web SDK script on your website a global variable `Fidel` is created with two methods that you can use to open and close the web form manually, `Fidel.openForm()` and `Fidel.closeForm()`. See an example below:

<h5>Fidel.openForm() global function.</h5>

```html
<button type="submit" onclick="Fidel.openForm()">Link Card</button>
```

<br/>

To receive the callback after the form submission you must pass a Javascript global function name reference on the `data-callback` property that will return the response and error objects. Please see an example below:

<h5>Web SDK callback global function example.</h5>

```html
fileName:index.html
<script>
    function callback(error, card) {
        console.log('Card Link Error', error);
        console.log('Card Linked Successfully', card)
    }
</script>
```
<br/>

To store custom data related to the card you must pass a Javascript global object name reference on the `data-metadata` property. The metadata `id` property is a *non-unique index* to that card, so you can set it to a custom UID (unique identifier). Later you can retrieve the card(s) using the same `metadata.id`, reference [List Cards from Metadata ID](https://reference.fidel.uk/v1/reference#list-cards-from-metadata-id).

<h5>Web SDK metadata global object example.</h5>

```html
fileName:index.html
<script>
    var metadata = {
        id: 'this-is-the-metadata-id',
        customKey1: 'customValue1',
        customKey2: 'customValue2'
    };
</script>
```

<br/>

The FIDEL Web SDK can be customised to better fit your website. You can provide a button title, title, subtitle and logo by using the properties `data-button-title`, `data-title`, `data-subtitle` and `data-logo`. You can customize CSS colors by using `data-background-color`, `data-button-color`, `data-button-title-color`, `data-subtitle-color`, `data-terms-color` and `data-title-color`.
