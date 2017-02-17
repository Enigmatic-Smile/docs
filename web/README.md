## Web SDK
**FIDEL Web SDK** is a secure HTML iFrame with customisable pre-built UI that allows you to easily collect credit card details, tokenise and link credit/debit cards with loyalty services from your website or e-commerce platform. By using FIDEL Web SDK, card details are sent directly to Fidel through a secure connection without exposing your servers to sensitive information taking care of all PCI compliance requirements for you.

<h5>Preview of FIDEL card-linking Web SDK</h5>

![Web form](https://docs.fidel.uk/assets/images/web-form.png "Web form")

<br/>

After successfully tokenising and linking the card on Visa or MasterCard networks, Fidel returns a unique token that you use to link each unique card and transaction to your user’s account. You can now create card-linked web applications with online-to-offline marketing capabilities in a matter of minutes.

All modern desktop and mobile browsers are supported, including Chrome, Firefox, Safari, Microsoft IE and Edge. Please contact us at [developer@fidel.uk](mailto:developer@fidel.uk) if you experience any browser issues.

<br/>

# Integrating Web SDK
You can easily integrate FIDEL Web SDK in your website with only a few lines (or one) of code.

<h5>FIDEL card-linking Web SDK integration code.</h5>

```html
fileName:index.html
<script type="text/javascript" src="https://please.waitfor.release/v1/fidel.js"
    class="fidel-form"
    data-auto-open="false"
    data-callback="response"
    data-key="pk_live_5be54ed6-0c10-48a8-9db0-ca34852fbbfd"
    data-program-id="d1e38621-6f5d-424f-be26-d6fe33d2663a"
    data-title="Link Card"
    data-sub-title="Earn 1 point for every £1 spent online or in-store"
    data-logo="https://brand-logo.png"
    data-lang="en"
    data-button-color="#589144"
    data-button-title="Link Card"
    data-button-title-color="#ffffff">
</script>
```
<br/>

To connect **FIDEL Web SDK** to your **FIDEL API** account and Program you will need to add your test API Key to the `data-api-key` property and add a **programId** to the `data-program-id` property. When you want to go live a link real cards on Visa and Mastercard networks you will need to replace the test key for your live key.

The FIDEL Web SDK can be customised to better fit your website. You can provide a title, subtitle and logo by using the properties `data-title`, `data-subtitle` and `data-logo`. Also, the action button can be customised by changing it's background color, title, and title color by using the properties `data-button-color`, `data-button-title` and `data-button-title-color`.

The `data-auto-open` property allows you to open the web form automatically on page load if set to _true_. To receive the callback after the form submission you must pass a Javascript global function name reference on the `data-callback` property that will return the response and error objects. Please see an example below:

<h5>Web form callback global function example.</h5>

```javascript
fileName:index.html
<script>
    function response(error, response) {
        console.log('Card linked successfully', response)
        console.log('Error linking card', error);
    }
</script>
```
