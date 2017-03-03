## Web SDK
**FIDEL Web SDK** is a secure HTML iFrame with customisable pre-built UI that allows you to easily collect credit card details, tokenise and link credit/debit cards with rewards services from your website, e-commerce platform or mobile apps. By using FIDEL Web SDK, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements for you.

<h5>Preview of FIDEL card-linking Web SDK</h5>

![Web form](https://docs.fidel.uk/assets/images/web-form.png "Web form")

<br/>

After successfully tokenising and linking the card on Visa or MasterCard networks, FIDEL API returns the created card object with a unique `id` that you must use to link each unique card and transaction to your user’s account. The `id` of each linked card is present on the transaction object as `cardId`. You can now create card-linked web and mobile applications with online and offline transactional data visibility in a matter of minutes.

All modern desktop and mobile browsers are supported, including Chrome, Firefox, Safari, Microsoft IE and Edge. Please contact us at [developer@fidel.uk](mailto:developer@fidel.uk) if you experience any browser issues.

<br/>

# Integrating Web SDK
You can easily integrate FIDEL Web SDK in your website or mobile app with only a few lines (or one) of code.

<h5>FIDEL Web SDK script.</h5>

```html
fileName:index.html
<script type="text/javascript" src="https://resources.fidel.uk/sdk/js/v1/fidel.js"
    class="fidel-form"
    data-auto-open="false"
    data-callback="callback"
    data-key="pk_test_demo"
    data-program-id="bca59bd9-171b-4d1f-92af-4b2b7305268a"
    data-company-name="Fidel"
    data-title="Link Card"
    data-subtitle="Earn 1 point for every £1 spent online or in-store"
    data-logo="https://company.com/logo.png"
    data-lang="en"
    data-button-color="#4dadea"
    data-button-title="Link Card"
    data-button-title-color="#ffffff">
</script>
```

<br/>

To integrate **FIDEL Web SDK** in your website or mobile app, you need to add the script above in your website or mobile web view. We are working on native mobile SDKs for iOS and Android that will simplify the mobile integration.

Most of the data properties in the script are self explanatory but you can check the description of each property below.

<br/>

- **data-auto-open**: whether the web form auto opens on page load.

- **data-callback**: name of the global callback function.

- **data-key**: a valid API key.

- **data-program-id**: the id of the program to link the card to.

- **data-company-name**: the name of the company using card-linked services.

- **data-title**: the title of the web form. _Max 25 chars._

- **data-subtitle**: the subtitle of the web form. _Max 110 chars._

- **data-logo**: the logo URL of the company using card-linked services. _Recommended height 35px. Extensions jpg, jpeg, png._

- **data-lang**: the localization language to be used.

- **data-button-color**: the hex color code of the button background.

- **data-button-title**: the button title. _Max 35 chars._

- **data-button-title-color**: the hex color code of the button title.

<br/>

The `data-auto-open` property allows you to open the web form automatically on page load if set to `true`. After adding the Web SDK script on your website a global variable `Fidel` is created with two methods that you can use to open and close the web form manually, `Fidel.openForm()` and `Fidel.closeForm()`. See an example below:

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
    function callback(error, response) {
        console.log('Card Link Error', error);
        console.log('Card Linked Successfully', response)
    }
</script>
```

<br/>

The FIDEL Web SDK can be customised to better fit your website. You can provide a title, subtitle and logo by using the properties `data-title`, `data-subtitle` and `data-logo`. Also, the action button can be customised by changing it's background color, title, and title color by using the properties `data-button-color`, `data-button-title` and `data-button-title-color`.
