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

You can easily integrate the Web SDK in your website or mobile app with only a few lines of code.

##### Fidel Web SDK script

Here are the parameters that must be defined in your JavaScript. All of the parameters that begin with `data-` can be customised for your application, but `data-program-id`, `data-key`, `data-country-code` are the absolute minimum to have reports arrive back your Fidel Dashboard. The full parameter description is found below the code.  

> Note: The `data-key` in the Web SDK is your public SDK key.

```html
fileName:index.html
<script
  type="text/javascript"
  src="https://resources.fidel.uk/sdk/js/v2/fidel.js"
  class="fidel-form"
  data-company-name="Your company"
  data-key="pk_test_demo"
  data-program-id="bca59bd9-171b-4d1f-92af-4b2b7305268a"
  data-callback="callback"
  data-country-code="GBR"
  data-metadata="metadata"
  data-auto-open="false"
  data-auto-close="false"
  data-allowed-countries="GBR,IRL"
  data-background-color="#ffffff"
  data-button-color="#4dadea"
  data-button-title="Link Card"
  data-button-title-color="#ffffff"
  data-lang="en"
  data-logo="https://yourcompany.com/logo.png"
  data-subtitle="Earn 1 point for every £1 spent online or in-store"
  data-subtitle-color="#000000"
  data-privacy-url="https://yourcompany.com/privacy"
  data-delete-instructions="tapping remove in your settings page."
  data-terms-color="#000000"
  data-title="Link Card"
  data-title-color="#000000">
</script>
```
To integrate **Fidel Web SDK** in your website or mobile app, you need to add the script above in your website or mobile web view. For mobile apps, however, we recommend using the [mobile SDKs](/mobile-sdks).

> Hint: Adding user data in the metadata as key:value pairs can simplify reconciliation with your system. For example, adding 'myUserId':'123' will help you match the the added card to your user '123'.

> You can continue using v1, by setting the script `src` to `https://resources.fidel.uk/sdk/js/v1/fidel.js`.

##### Web SDK parameters

<dl>
    <div>
        <dt>
            <span><code>data-company-name</code></span>
            <strong>required</strong>
        </dt>
        <dd>the name of the company using card-linking. Max 35 chars</dd>
    </div>
    <div>
        <dt>
            <span><code>data-key</code></span>
            <strong>required</strong>
        </dt>
        <dd>a valid Public Key</dd>
    </div>
    <div>
        <dt>
            <span><code>data-program-id</code></span>
            <strong>required</strong>
        </dt>
        <dd>the id of the program to link the card to</dd>
    </div>
    <div>
        <dt>
            <span><code>data-callback</code></span>
        </dt>
        <dd>name of the global callback function</dd>
    </div>
    <div>
        <dt>
            <span><code>data-country-code</code></span>
            <em>values: GBR, IRL, USA, SWE</em>
        </dt>
        <dd>set card country of issue and remove the country select box</dd>
    </div>
    <div>
        <dt>
            <span><code>data-metadata</code></span>
        </dt>
        <dd>name of the global metadata object</dd>
    </div>
    <div>
        <dt>
            <span><code>data-auto-open</code></span>
            <em>default: false</em>
        </dt>
        <dd>whether the web form auto opens on page load</dd>
    </div>
    <div>
        <dt>
            <span><code>data-auto-close</code></span>
            <em>default: true</em>
        </dt>
        <dd>whether the web form auto closes on successful card link</dd>
    </div>
    <div>
        <dt>
            <span><code>data-allowed-countries</code></span>
            <em>default: all supported countries</em>
        </dt>
        <dd>comma separated list of countries that shown in the dropdown</dd>
    </div>
    <div>
        <dt>
            <span><code>data-background-color</code></span>
        </dt>
        <dd>CSS color code of the form background</dd>
    </div>
    <div>
        <dt>
            <span><code>data-button-color</code></span>
        </dt>
        <dd>CSS color code of the button background</dd>
    </div>
    <div>
        <dt>
            <span><code>data-button-title</code></span>
        </dt>
        <dd>the button title. Max 35 chars</dd>
    </div>
    <div>
        <dt>
            <span><code>data-button-title-color</code></span>
        </dt>
        <dd>CSS color code of the button title</dd>
    </div>
    <div>
        <dt>
            <span><code>data-close-events</code></span>
            <em>default: true</em>
        </dt>
        <dd>whether close button, overlay click and escape key events are added to the form</dd>
    </div>
    <div>
        <dt>
            <span><code>data-lang</code></span>
            <em>default: en</em>
        </dt>
        <dd>the localization language to be used. The default language is en (English using British English spellings). Additional languages: en-us (English with US spelling), sv (Swedish), fr (French)</dd>
    </div>
    <div>
        <dt>
            <span><code>data-logo</code></span>
        </dt>
        <dd>the logo URL of the company. Height 35px. Extensions jpg, jpeg, png</dd>
    </div>
    <div>
        <dt>
            <span><code>data-subtitle</code></span>
        </dt>
        <dd>the subtitle of the web form. Max 110 chars</dd>
    </div>
    <div>
        <dt>
            <span><code>data-subtitle-color</code></span>
        </dt>
        <dd>CSS color code of the subtitle</dd>
    </div>
    <div>
        <dt>
            <span><code>data-privacy-url</code></span>
        </dt>
        <dd>URL of your company's privacy policy</dd>
    </div>
    <div>
        <dt>
            <span><code>data-delete-instructions</code></span>
        </dt>
        <dd>user instructions to unlink card in your application</dd>
    </div>
    <div>
        <dt>
            <span><code>data-terms-color</code></span>
        </dt>
        <dd>CSS color code of the terms and conditions</dd>
    </div>
    <div>
        <dt>
            <span><code>data-title</code></span>
        </dt>
        <dd>the title of the web form. Max 25 chars</dd>
    </div>
    <div>
        <dt>
            <span><code>data-title-color</code></span>
        </dt>
        <dd>CSS color code of the title</dd>
    </div>
    <div>
        <dt>
            <span><code>data-scheme-*</code></span>
            <em>amex / visa / mastercard: <code>false</code></em>
        </dt>
        <dd>disables Amex, Visa or Mastercard. Multiple can be specified, e.g. <code>data-scheme-mastercard: false</code>, <code>data-scheme-amex: false</code></dd>
    </div>
</dl>

The `data-auto-open` property allows you to open the web form automatically on page load if set to `true`. If `data-auto-close` is set to `false` the form won't be automatically closed after linking a card. You can set `data-close-events` to `false` and the form won't add the default close events. After adding the Web SDK script on your website a global variable `Fidel` is created with two methods that you can use to open and close the web form manually, `Fidel.openForm()` and `Fidel.closeForm()`. See an example below:

##### Fidel.openForm() global function
To open the iframe form with a button, you could use this example:

```html
<button type="submit" onclick="Fidel.openForm()">Link Card</button>
```

To receive the callback after the form submission you must pass a Javascript global function name reference on the `data-callback` property that will return the response and error objects. Please see an example below:

##### Web SDK callback global function example

```html
fileName:index.html
<script>
  function callback(error, card) {
    console.log('Card Link Error', error);
    console.log('Card Linked Successfully', card)
  }
</script>
```

To store custom data related to the card you must pass a Javascript global object name reference on the `data-metadata` property. The metadata `id` property is a *non-unique index* to that card, so you can set it to a custom UID (unique identifier). Later you can retrieve the card(s) using the same `metadata.id`, reference [List Cards from Metadata ID](https://reference.fidel.uk/v1/reference#list-cards-from-metadata-id).

##### Web SDK metadata global object example

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

The Fidel Web SDK can be customised to better fit your website. You can provide a button title, title, subtitle and logo by using the properties `data-button-title`, `data-title`, `data-subtitle` and `data-logo`. You can customize CSS colors by using `data-background-color`, `data-button-color`, `data-button-title-color`, `data-subtitle-color`, `data-terms-color` and `data-title-color`.
