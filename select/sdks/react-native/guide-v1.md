# A guide for card enrollment with the React Native SDK <a style="border-bottom: 2px solid #0048ff;" class="improve-docs" href="/select/sdks/react-native/guide-v1">v1</a> <a style="margin-right: auto; color: #111;" class="improve-docs" href="/select/sdks/react-native/guide-v2">v2</a>

Please take the following steps to integrate and configure the SDK for your Loyalty use case application.

> Note: If an example project helps with your SDK integration & configuration, please check our [GitHub repository](https://github.com/FidelLimited/rn-sdk).

### 1. Set up your Fidel API account & your Transaction Select program

To get started, you'll need a Fidel API account. [Sign up](https://dashboard.fidel.uk/sign-up), if you didn't create an account yet.

If you didn't create a program for your application yet, please create a Transaction Select program from your Fidel API dashboard (or via the API).

### 2. Integrate the React Native SDK into your project

- Add the Fidel API React Native library:
    - using `npm`: 
    ```
    npm install fidel-react-native@1.6.4 --save
    ```
    - using `yarn`:
    ```
    yarn add fidel-react-native@1.6.4
    ```

- In your `ios` folder, run the following command to install the Fidel dependency:

```
pod install
```

### 3. Import the SDK module

In your project files where you want to use the SDK, please import `Fidel`:

```js
import Fidel from 'fidel-react-native';
```

### 4. Start setting up the SDK by setting the SDK Key

- Please [sign into](https://dashboard.fidel.uk/sign-in) your Fidel API dashboard account, if you didn't already.
- Click on your account name _(on the top-left hand side of the dashboard)_ -> then on `Account Settings`.
- Go to the `Plan` tab and copy your `Test` or `Live` SDK Key.
- Set your SDK Key in your app:

> Important note: For security reasons, please DO NOT store the SDK Key in your codebase. Follow our [SDK security guide](/select/sdks/security-guidelines) for detailed recommendations.

```javascript
Fidel.setup({
    apiKey: yourSdkKey,
});
```

### 5. Add your Program ID

- Please [sign into](https://dashboard.fidel.uk/sign-in) your Fidel API dashboard account, if you didn't already.
- Go to the `Programs` section of your Fidel API dashboard.
- Click on the `Program ID` of the program that you want to enroll cards into. The program ID will be copied to your pasteboard.
- Set your Program ID in your app:

```javascript
Fidel.setup ({
    apiKey: yourSdkKey,
    programId: 'Your-program-ID',
});
```

### 6. Configure the cardholder consent management feature

Asking for consent from the cardholder to enroll the card in your program is an important part of the SDK. The cardholder will need to read & agree with the conditions expressed using the consent language displayed during the card enrollment process. Making it clear for cardholders is essential.

You can use the `setOptions` function to set the following properties:

#### Set your company name

```javascript
Fidel.setOptions ({
    companyName: 'Your Company Name',
});
```

#### To support US or Canada issued cards, please set your terms and conditions

You need to set your terms and conditions URL if you would like to:

a. support all the countries that Fidel API supports

b. set a specific `allowedCountries` set of countries AND include US or Canada in your set of allowed countries.

```javascript
Fidel.setOptions ({
    companyName: 'Your Company Name',
    termsConditionsUrl: 'https://yourwebsite.com/terms',
});
```

#### Explain how the cardholder can opt out (optional, but recommended)

Please inform the cardholder about how to opt out of transaction monitoring in your program.

```javascript
Fidel.setOptions ({
    companyName: 'Your Company Name',
    termsConditionsUrl: 'https://yourwebsite.com/terms',
    deleteInstructions: 'how can the cardholder opt out',
});
```

#### Set your privacy policy (optional, but recommended)

```javascript
Fidel.setOptions ({
    companyName: 'Your Company Name',
    termsConditionsUrl: 'https://yourwebsite.com/terms',
    deleteInstructions: 'how can the cardholder opt out',
    privacyUrl: 'https://yourwebsite.com/privacy-policy',
});
```

### 7. Check your setup

You should have the SDK set up similar to what you see in the example below:

```javascript
Fidel.setup ({
    apiKey: yourSdkKey,
    programId: 'Your-program-ID',
});

Fidel.setOptions ({
    companyName: 'Your Company Name',
    termsConditionsUrl: 'https://yourwebsite.com/terms',
    deleteInstructions: 'how can the cardholder opt out',
    privacyUrl: 'https://yourwebsite.com/privacy-policy',
});
```

# Enrollment notifications

In order to be notified about different, useful events (a card was linked, card failed to link etc.) that happen during a enrollment process, we recommend using our webhooks.

If client side notifications are useful for your application, make sure to check our SDK callback reference documentation.

# Enroll a card

Call the following function to open the UI and start a card enrollment process:

```javascript
Fidel.openForm((error, result) => {
  if (error) {
    console.error(error);
  } else {
    console.info(result);
  }
});
```
You can test the card enrollment flow, by setting a test SDK Key and by using the Fidel API [test card numbers](/select/cards/#test-card-numbers).

If your Fidel API account is `live` then cardholders can also enroll real, live cards. Make sure that you set a live SDK Key, in order to allow live card enrollments.

# Optional: Set any of the other useful properties

Please check our SDK reference page for details about any other SDK properties that might be useful for your application.

# Frequently asked questions

### How can I upgrade the React Native SDK to the latest version?

- At the root of your project, run 

`yarn upgrade fidel-react-native` or 

`npm update -g fidel-react-native`.

- In your `ios` folder, run the following command to update the Fidel dependency:
```
pod update Fidel
```

### Can I customize the UI of the SDK?

The React Native SDK offers the `bannerImage` property for you to set a custom, branded banner image that will be displayed during the card enrollment process. Please check our Reference documentation for more details.
