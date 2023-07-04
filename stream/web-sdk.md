# Web SDK

The Fidel API SDKs provide a secure way for you to add card-linking capabilities to your applications. The alternative involves using the [Cards API](https://transaction-stream.fidel.uk/reference/create-card), but you'll need to be PCI Compliant before you can use it. Using the Fidel API SDKs, card details are sent directly to Fidel API through a secure connection without exposing your servers to sensitive information. They take care of all PCI Compliance requirements, so you don't have to.

The Fidel API Web SDK is a JavaScript-based SDK that injects a secure HTML iframe into your web page. Your users will need to have JavaScript enabled in their browsers.

The SDK provides a pre-built UI that allows you to easily collect credit card details, tokenize and link credit/debit cards with rewards services from your website, e-commerce platform or mobile web apps. However, for mobile apps, we recommend using Fidel API's [Mobile SDKs](https://transaction-stream.fidel.uk/docs/mobile-sdk).

You can customize the UI of the SDK to match your use case. All modern desktop and mobile browsers are supported, including Chrome, Firefox, Safari, Microsoft IE11 and Edge. Please [contact us](https://fidelapi.com/contact/) if you experience any browser-related issues.

![](https://files.readme.io/cbd9f5d-0455159F-93D4-47E8-930A-F83BCF543353.jpeg)

## Card Verification

After the card has been linked to the Fidel system, a user with access to the account statement of the card must provide a verification token to verify consent. To implement this verification, use the Verified Enrollment SDK. In the verification process Fidel API will charge the card for a nominal amount (USD$0.50-1.00), which will be refunded within 72 hours. The cardholder checks their card statement to identify the charged amount and types the exact amount in the verification screen. 

Since the verification process is not instantaneous and may require the cardholder to exit the SDK, the SDK stores a token which represents their card object (not the card details themselves) on the device they used. This means that cardholders will need to use the same device or enter their card details again to complete the verification. This token is erased once the verification is completed.

The charges made to the enrolled card will be made in the currency of the country selected. If a cardholder selects “United States” as the country where the card was issued, the charge will be in USD. If the developer listed only one country in SDK options (e.g. Canada), the currency of that country will be used (e.g. CAD).

> 🚧 
> 
> If you are using a non-live environment, the microcharge will have a fixed amount of $0.67 which you can use to verify a test card.

![](https://files.readme.io/bb7c0a2-Screenshot_2023-02-09_at_17.05.14.png)

## Integrating the Web SDK

Integrating the Fidel API Web SDK into your web application starts with loading the JavaScript file we provide, <https://resources.fidel.uk/sdk/js/v3/fidel.js>, in a script tag. It registers a global Fidel variable and uses an options object to allow you to configure it when calling Fidel.openForm().

```html
<script
  type="text/javascript"
  src="https://resources.fidel.uk/sdk/js/v3/fidel.js"
  data-id="fidel-sdk">
</script>'
```

## Web SDK Configuration

You can configure the Fidel Web SDK by defining parameters when calling the desired method.

### Fidel.openForm()

This method is used for linking cards in `stream` and `select` programs. For `transaction-stream` programs, it can also trigger the verification process of the card. This method allows:

- Linking a card in a `select` program.
- Linking a card in a `stream`program, if the cardholder does not have access to the bank statement.
- Linking and verifying a card in a `stream`program, if the cardholder has access to the bank statement.

#### Configuration

| Parameter                                                       | Value    | Description                                                                                                                                                                                                                 |
| :-------------------------------------------------------------- | :------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| companyName\*                                                   | String   | The name of the company using card linking.                                                                                                                                                                                 |
| sdkKey\*                                                        | String   | A valid SDK Key. You can find yours on the Dashboard. It starts with "pk\_".                                                                                                                                                |
| programId\*                                                     | String   | The id of the Fidel API Program to link the card to.                                                                                                                                                                        |
| thirdPartyVerification                                          | Object   | This object contains the data related to third-party verification flow.                                                                                                                                                     |
| thirdPartyVerification.isChoiceAvailable\*                      | Boolean  | If true, provides the cardholder with the choice between verifying the card on the spot or delegating it to a third-party verified.                                                                                         |
| thirdPartyVerification.onCardVerificationChoiceSelectedCallback | Function | Function to be called when third-party enrollment flow is enabled and the user selects a verification method. Receives an object with a 'choice' payload that could have 'on-the-spot' or 'delegated-to-third-party' value. |

There are also [additional parameters](/stream/web-sdk#customization) that can improve your SDK experience.

#### Example

```javascript
Fidel.openForm({
    companyName: "Fidel",
    sdkKey: "pk_test_demo",
    programId: "bca59bd9-171b-4d1f-92af-4b2b7305268a",
    programType: "transaction-stream",
    onCardEnrolledCallback() {
      console.log("onCardEnrolledCallback called");
      console.log(arguments);
    },
    onCardVerificationStartedCallback() {
      console.log("onCardVerificationStartedCallback called");
      console.log(arguments);
    },
    onCardEnrollFailedCallback() {
      console.log("onCardEnrollFailedCallback called");
      console.log(arguments);
    },
    onCardVerifiedCallback() {
      console.log("onCardVerifiedCallback called");
      console.log(arguments);
    },
    onCardVerifyFailedCallback() {
      console.log("onCardVerifyFailedCallback called");
      console.log(arguments);
    },
    onUserCancelledCallback() {
      console.log("onUserCancelledCallback called")
    },
});
```

If `thirdPartyVerificationChoice.isChoiceAvailable`is set to true, this screen appears, allowing the cardholder to choose between verifying the card on the spot or delegating the verification.

![](https://files.readme.io/0d46614-Screenshot_2023-03-15_at_10.47.12.png)

### Fidel.verifyCard()

This method is used to resume the verification of a card that was previously linked but not verified.

#### Configuration

| Parameter                | Value  | Description                                                                                         |
| :----------------------- | :----- | :-------------------------------------------------------------------------------------------------- |
| sdkKey\*                 | String | A valid SDK Key. You can find yours on the Dashboard. It starts with "pk\_".                        |
| programId\*              | String | The id of the Fidel API Program to link the card to.                                                |
| cardToVerify\*           | Object | This object contains the required data for the card to be verified.                                 |
| cardToVerify.id\*        | String | The id of the card to be verified.                                                                  |
| cardToVerify.consentId\* | String | The consent id of the card to be verified.                                                          |
| cardToVerify.last4Digits | String | The last 4 digits of the card to be verified. This property is used only for presentation purposes. |

There are also [additional parameters](/stream/web-sdk#customization) that can improve your SDK experience.

#### Example

```javascript
Fidel.verifyCard({
 sdkKey: "pk_test_demo",
 programId: "bca59bd9-171b-4d1f-92af-4b2b7305268a",
 cardToVerify: {
 	id: "bf98f881-53fc-45ab-8788-5c63915b9cf2",
 	consentId:"a9e5c141-4f61-428f-aedd-2248e6767dd7",
 	last4Digits: "4011”
 }
}); 
```

### Customization

The Fidel Web SDK can be [customized](/stream/web-sdk#customizing-the-web-sdk) to better fit your website. The functionality is customizable with the language, schemes, countries and closeOnSuccess parameters. There is also a versatile custom property that accepts CSS styles and allows you to greatly customize the UI of the SDK.

```javascript
Fidel.openForm({
    companyName: "Fidel",
    sdkKey: "pk_test_demo",
    programId: "bca59bd9-171b-4d1f-92af-4b2b7305268a",
    programType: "transaction-stream",
    custom: {
        close: {
            location: "form"
        },
        overlay: {
            style: {
                background: "#111111"
            }
        },
        form: {
            style: {
                background: "#1C1C1C",
                border: "none"
            }
        },
        title: {
            style: {
                textAlign: "center",
                color: "white"
            }
        },
        cardNumber: {
            label,
            input
        },
        expiryDate: {
            label,
            input
        },
        country: {
            label,
            input
        },
        submit: {
            style: {
                background: "#44EDB0",
                color: "#111111",
                borderRadius: "4px"
            }
        }
    }
});

const input = {
    style: {
        color: "#737373",
        border: "none",
        background: "#282828"
    },
    focusStyle: {
        background: "#303030",
        borderWidth: "2px",
        borderColor: "black"
    },
    errorStyle: {
        borderWidth: "2px",
        borderColor: "red"
    }
};

const label = {
    hide: true
};
```

## Web SDK Global Object

After adding the Web SDK script to your website, a global variable **Fidel** is created with two methods that you can use to open and close the web form manually, **Fidel.openForm()**, **Fidel.verifyCard()** and **Fidel.closeForm()**.  
To open the iframe overlay form with a button, you could use this example:

```html
<button id="open-form" type="submit">
  Link Card
</button>

<script type="text/javascript" data-id="fidel-sdk" src="https://resources.fidel.uk/sdk/js/v3/fidel.js">
</script>

<script type="text/javascript">
document.getElementById("open-form").addEventListener("click", function() {
  Fidel.openForm({
    companyName: "Fidel",
    sdkKey: "pk_test_demo",
    programId: "bca59bd9-171b-4d1f-92af-4b2b7305268a",
    programType: "transaction-stream",
		onCardEnrolledCallback() {
      console.log("onCardEnrolledCallback called");
      console.log(arguments);
    },
    onCardVerificationStartedCallback() {
      console.log("onCardVerificationStartedCallback called");
      console.log(arguments);
    },
    onCardEnrollFailedCallback() {
      console.log("onCardEnrollFailedCallback called");
      console.log(arguments);
    },
    onCardVerifiedCallback() {
      console.log("onCardVerifiedCallback called");
      console.log(arguments);
    },
    onCardVerifyFailedCallback() {
      console.log("onCardVerifyFailedCallback called");
      console.log(arguments);
    },
    onUserCancelledCallback() {
      console.log("onUserCancelledCallback called")
    },
  });
});
</script>
```

## Web SDK Metadata

The metadata **id** property is a non-unique index, so you can set it to a custom UID (unique identifier). Later you can query the cards using the same **metadata.id**. You can use our [List Cards from Metadata ID API Endpoint](https://transaction-stream.fidel.uk/reference/list-cards-from-metadata-id).

> 📘 Hint
> 
> Adding user data in the metadata as key: value pairs can simplify reconciliation with your system. For example, adding 'myUserId':'123' will help you match the added card to your user with id 123.

```html
<button id="open-form" type="submit">
  Link Card
</button>

<script type="text/javascript" data-id="fidel-sdk" src="https://resources.fidel.uk/sdk/js/v3/fidel.js">
</script>

<script type="text/javascript">
document.getElementById("open-form").addEventListener("click", function() {
  Fidel.openForm({
    companyName: "Fidel",
    sdkKey: "pk_test_demo",
    programId: "bca59bd9-171b-4d1f-92af-4b2b7305268a",
    programType: "transaction-stream",
    callback(err, data) {
        console.log(err, err);
        console.log('data', data);        console.log(arguments);
    },
    metadata: {
        id: 'this-is-the-metadata-id',
        myUserId: '123',
        customKey: 'customValue'
    }
  });
});
</script>
```

## Customizing the Web SDK

### Additional properties

| **Parameter** | **Value** | **Description** |
|---|---|---|
|`onCardEnrolledCallback`|function|Function to be called when a Card is enrolled successfully. Receives the Card entity as argument.|
|`onCardEnrollFailedCallback`|function|Function to be called when a Card enrollment fails. Receives an error object as argument.|
|`onCardVerificationStartedCallback`|function|Function to be called when a Card verification starts. Receives an object with consentId as argument.|
|`onCardVerifiedCallback`|function|Function to be called when a Card is verified successfully. Receives an object with cardId as argument.|
|`onCardVerifyFailedCallback`|function|Function to be called when a Card verification fails. Receives an error object as argument including the cardId.|
|`onUserCancelledCallback`|function|Function to be called when the user cancels the card enrollment with one of the following actions:  \n- Closing the window with the \"x\" in the top right corner  \n- Clicking/tapping outside the SDK's frame  \n- Pressing the ESC key on the keyboard  \nNo arguments are provided.|
|`container`|HTML element|A wrapper element that you can wrap around the iframe.|
|`countries`|array of strings  \nPossible values: 'GBR', 'IRL', 'USA', 'SWE'|If the array has only one value, that will be set as the country of issue for all collected cards and the country select box is removed from the UI.  \nIf the array has multiple values, those will be the available options in the country select box.|

### Custom UI

The **custom** property allows for customizing most of the Fidel Web SDK UI. It supports the same CSS properties as the [HTML DOM Style Object](https://www.w3schools.com/jsref/dom_obj_style.asp). Here is a complete list of parameters, all of which are optional.

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Value",
    "h-2": "Description",
    "0-0": "logo.url",
    "0-1": "String",
    "0-2": "The URL of the logo on the iframe.",
    "1-0": "logo.style",
    "1-1": "CSS properties",
    "1-2": "The style of the logo on the iframe.",
    "2-0": "form.style",
    "2-1": "CSS properties",
    "2-2": "The style of the form element of the iframe.",
    "3-0": "overlay.style",
    "3-1": "CSS properties",
    "3-2": "The style of the overlay element of the iframe.",
    "4-0": "close.location",
    "4-1": "String  \nPossible values: form | overlay | none  \nDefault value: overlay",
    "4-2": "The location of the close icon.",
    "5-0": "close.color",
    "5-1": "String",
    "5-2": "The color of the close icon.",
    "6-0": "title.text",
    "6-1": "String  \nDefault value: \"Connect your card\"",
    "6-2": "The text of the title at the top of the page.",
    "7-0": "title.style",
    "7-1": "CSS properties",
    "7-2": "The style of the title at the top of the page.",
    "8-0": "cardNumber.label",
    "8-1": "LabelStyles (see definition below)",
    "8-2": "The style of the Card Number label.",
    "9-0": "cardNumber.input",
    "9-1": "InputStyle (see definition below)",
    "9-2": "The style of the Card Number input field.",
    "10-0": "expiryDate.label",
    "10-1": "LabelStyles (see definition below)",
    "10-2": "The style of the Expiry Date label.",
    "11-0": "expiryDate.input",
    "11-1": "InputStyle (see definition below)",
    "11-2": "The style of the Expiry Date input field.",
    "12-0": "country.label",
    "12-1": "LabelStyles (see definition below)",
    "12-2": "The style of the Country field label.",
    "13-0": "country.input",
    "13-1": "InputStyle (see definition below)",
    "13-2": "The style of the Country input field.",
    "14-0": "terms.text",
    "14-1": "String  \nDefault value: \"I authorize {{scheme}} to monitor my payment card to identify transactions that qualify for a reward and for {{scheme}} to share such information with {{companyName}}, to enable my card linked offers and target offers that may be of interest to me.\"",
    "14-2": "The Terms of Use text.",
    "15-0": "terms.privacyUrl",
    "15-1": "String",
    "15-2": "The Privacy Policy URL.",
    "16-0": "terms.termsUrl",
    "16-1": "String",
    "16-2": "The Terms and Conditions URL.",
    "17-0": "terms.deleteInstructions",
    "17-1": "String",
    "17-2": "Default value: \"going to your account settings\"  \nText to inform the user about how to opt out from the transaction monitoring. It is appended to the text \"You may opt out of transaction monitoring on the payment card you entered at any time by\".",
    "18-0": "terms.checkbox",
    "18-1": "**style**: CSS properties  \n**checkedStyle**: CSS properties",
    "18-2": "The styles of the terms checkbox.",
    "19-0": "terms.checkbox.checkColor",
    "19-1": "String",
    "19-2": "The border color of the terms checkbox.",
    "20-0": "submit",
    "20-1": "**style**: CSS properties  \n**hoverStyle**: CSS properties  \n**loadingStyle**: CSS properties  \n**successStyle**: CSS properties  \n**defaultText**: string  \nDefault value: \"Continue\" | \"Verify\", depending on the step  \n**loadingText**: string  \n**successText**: string",
    "20-2": "Parameters to customize the CTA button on the screens of the card verification."
  },
  "cols": 3,
  "rows": 21,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]

The **LabelStyles** type includes the following parameters:

| Parameter              | Value          | Description                          |
| :--------------------- | :------------- | :----------------------------------- |
| label.hide             | CSS properties | The default style of the input       |
| label.style            | CSS properties | The style of the input               |
| label.errorStyle       | CSS properties | The error style of the input         |
| label.focusStyle       | CSS properties | The focus style of the input         |
| label.placeholderColor | String         | The color of the input's placeholder |

The **InputStyles** type includes the following parameters:

| Parameter              | Value          | Description                          |
| :--------------------- | :------------- | :----------------------------------- |
| input.style            | input.style    | The default style of the input       |
| input.errorStyle       | CSS properties | The error style of the input         |
| input.focusStyle       | CSS properties | The focus style of the input         |
| input.placeholderColor | String         | The color of the input's placeholder |

The definition of the **custom** parameter:

```javascript
Fidel.openForm({
    ...
    custom: {
        logo: {
            url: string,
            style: CSSPropertiesObject,
        },
        form: {
            style: CSSPropertiesObject,
        },
        overlay: {
            style: CSSPropertiesObject,
        },
        close: {
            location: 'form' | 'overlay' | 'none',
            color: string | { form: string, overlay: string },
        },
        title: {
            text: string,
            style: CSSPropertiesObject,
        },
        cardNumber: {
            label: {
                style: CSSPropertiesObject,
                errorStyle: CSSPropertiesObject,
                focusStyle: CSSPropertiesObject,
                placeholderColor: string,
                hide: boolean
            },
            input: {
                style: CSSPropertiesObject,
                errorStyle: CSSPropertiesObject,
                focusStyle: CSSPropertiesObject,
                placeholderColor: string
            },
        },
        expiryDate: {
            label: {
                style: CSSPropertiesObject,
                errorStyle: CSSPropertiesObject,
                focusStyle: CSSPropertiesObject,
                placeholderColor: string,
                hide: boolean
            },
            input: {
                style: CSSPropertiesObject,
                errorStyle: CSSPropertiesObject,
                focusStyle: CSSPropertiesObject,
                placeholderColor: string
            },
        },
        country: {
            label: {
                style: CSSPropertiesObject,
                errorStyle: CSSPropertiesObject,
                focusStyle: CSSPropertiesObject,
                placeholderColor: string,
                hide: boolean
            },
            input: {
                style: CSSPropertiesObject,
                errorStyle: CSSPropertiesObject,
                focusStyle: CSSPropertiesObject,
                placeholderColor: string
            },
        },
        terms: {
            text: string,
            privacyUrl: string,
            termsUrl: string,
            deleteInstructions: string,
            checkbox: {
                style: CSSPropertiesObject,
                checkedStyle: CSSPropertiesObject,
                checkColor: string,
            },
        },
        submit: {
            style: CSSPropertiesObject,
            hoverStyle: CSSPropertiesObject,
            loadingStyle: CSSPropertiesObject,
            successStyle: CSSPropertiesObject,
            defaultText: string,
            loadingText: string,
            successText: string,
        },
    }
});
```

Here is an example of what is achievable using the customization:

![](https://files.readme.io/3956c98-69BEA0AF-DA33-4F53-81EC-6143BAF19AE3.jpeg)

### Card entity

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Value",
    "h-2": "Description",
    "0-0": "id",
    "0-1": "String",
    "0-2": "The ID of the enrolled card",
    "1-0": "accountId",
    "1-1": "String",
    "1-2": "The account ID of the program where the card was enrolled",
    "2-0": "programId",
    "2-1": "String",
    "2-2": "The ID of the program where the card was enrolled",
    "3-0": "scheme",
    "3-1": "String  \nPossible values: 'mastercard', 'visa', 'amex'",
    "3-2": "Card scheme",
    "4-0": "lastNumbers",
    "4-1": "Number",
    "4-2": "If made available, this property will be populated with the last 4 numbers of the enrolled card. If they're not available, you'll receive \"\\*\\*\\*\\*\" (4 stars, an obfuscated string value). To turn on or off receiving these numbers, please check your Fidel API Dashboard account's Security settings.",
    "5-0": "firstNumbers",
    "5-1": "Number",
    "5-2": "If made available, this property will be populated with the first 6 numbers of the enrolled card. If they're not available, you'll receive \"**\\*\\***\" (6 stars, an obfuscated string value). To turn on or off receiving these numbers, please check your Fidel API Dashboard account's Security settings.",
    "6-0": "expYear",
    "6-1": "Number",
    "6-2": "Expiration year of the card. The values are full year values (2023), not shortened year values (23)",
    "7-0": "expMonth",
    "7-1": "Number",
    "7-2": "Expiration month of the card",
    "8-0": "expDate",
    "8-1": "string  \nFormat: MM/YY",
    "8-2": "Expiration date (MM/YY format)",
    "9-0": "created",
    "9-1": "string  \nFormat: YYYY-MM-DDTHH:mm:ss.SSSZ",
    "9-2": "Card enrollment date",
    "10-0": "countryCode",
    "10-1": "String",
    "10-2": "This field will have the code of the card issuing country, as selected by the cardholder in the card enrollment step.",
    "11-0": "termsOfUse",
    "11-1": "Boolean",
    "11-2": "Accepted or declined terms of use",
    "12-0": "live",
    "12-1": "Boolean",
    "12-2": "Flag that checks if the card is from a live or a test environment",
    "13-0": "metadata",
    "13-1": "Object",
    "13-2": "This will contain the exact JS object that might have been set using the metadata parameter."
  },
  "cols": 3,
  "rows": 14,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]

### Error object

| Parameter | Value             | Description                                                                                                                                                                                                                   |
| :-------- | :---------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cardId    | String (optional) | The ID of the enrolled card. This field will only be present if the card is already enrolled. For example, if the error is thrown in the card enrollment phase, it will not have a cardId since the card is not enrolled yet. |
| date      | String            | The ate when the error happened                                                                                                                                                                                               |
| type      | String            | Returns the error [code](#error-codes)                                                                                                                                                                                        |
| Message   | String            | Returns the error message                                                                                                                                                                                                     |

### Error codes

| Value                                      | Description                                            |
| :----------------------------------------- | :----------------------------------------------------- |
| service-unavailable                        | Service unavailable at the moment                      |
| item-exists                                | Card already exists                                    |
| uuid                                       | Invalid program ID                                     |
| credential                                 | Invalid SDK key                                        |
| unauthorized                               | Unauthorized request                                   |
| card-consent-authentication-code-incorrect | Incorrect amount for microcharge verification          |
| card-consent-max-attempts-reached          | Max failed attempts reached on card verification       |
| verification-not-found                     | Verification not found for the current linking card    |
| card-already-verified                      | Verification already done for the current linking card |
| card-not-found                             | Card not found when creating a new card verification   |
| verification-error-generic                 | Unable to verify due to unknown reason                 |