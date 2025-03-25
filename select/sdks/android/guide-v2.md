# A guide for card enrollment with the Android SDK <a style="color: #111;" class="improve-docs" href="/select/sdks/android/guide-v1">v1</a> <a style="margin-right: auto; border-bottom: 2px solid #0048ff;" class="improve-docs" href="/select/sdks/android/guide-v2">v2</a>

Please take the following steps to integrate and configure the SDK for your Loyalty use case application.

> Note: All code examples in this guide and other Android SDK pages will be written in Kotlin, but our SDK works well in Java projects as well.

> Note: If an example project helps with your SDK integration & configuration, please check our [GitHub repository](https://github.com/Enigmatic-Smile/fidel-android/).

### 1. Set up your Fidel API account & your Transaction Select program

To get started, you'll need a Fidel API account. [Sign up](https://dashboard.fidel.uk/sign-up), if you didn't create an account yet.

If you didn't create a program for your application yet, please create a Transaction Select program from your Fidel API dashboard (or via the API).

### 2. Integrate the Android SDK into your project

#### From the remote repository (JitPack)

- Add the JitPack repository to your Android project.
    - If your repositories are declared in your project's `build.gradle` file, add the JitPack repository:

    ```groovy
    fileName:build.gradle
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
    ```

    - If your project centrally declares repositories, add the JitPack repository in your `settings.gradle` file:

    ```groovy
    fileName:settings.gradle
    dependencyResolutionManagement {
        repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
    ```

- Check what's the latest version of the Fidel API Android SDK in our Releases page.

- Add the latest version of our SDK as a dependency in your app's module `build.gradle` file:

```groovy
fileName:app/build.gradle
implementation 'com.github.Enigmatic-Smile:android-sdk:2.2.1'
```

- In the future, to update to the latest Android SDK version, please update the version in your `app/build.gradle` file and `Sync` your Android project again.

### 3. Set your SDK Key

- Please [sign into](https://dashboard.fidel.uk/sign-in) your Fidel API dashboard account, if you didn't already.
- Click on your account name _(on the top-left hand side of the dashboard)_ -> then on `Account Settings`.
- Go to the `Plan` tab and copy your `Test` or `Live` SDK Key.
- Set your SDK Key in your app:

> Important note: For security reasons, please DO NOT store the SDK Key in your codebase. Follow our [SDK security guide](/select/sdks/security-guidelines) for detailed recommendations.

```kotlin
Fidel.sdkKey = "Your-SDK-Key"
```

### 4. Set your Program ID

- Please [sign into](https://dashboard.fidel.uk/sign-in) your Fidel API dashboard account, if you didn't already.
- Go to the `Programs` section of your Fidel API dashboard.
- Click on the `Program ID` of the program that you want to enroll cards into. The program ID will be copied to your pasteboard.
- Set your Program ID in your app:

```kotlin
Fidel.programId = "Your-Program-ID"
```

### 5. Configure the cardholder consent management feature

Asking for consent from the cardholder to enroll the card in your program is an important part of the SDK. The cardholder will need to read & agree with the conditions expressed using the consent language displayed during the card enrollment process. Making it clear for cardholders is essential.

#### Set your company name

```kotlin
Fidel.companyName = "Your Company Name"
```

#### To support US or Canada issued cards, please set your terms and conditions

You need to set your terms and conditions URL if you would like to:
a. support all the countries that Fidel API supports
b. set a specific `allowedCountries` set of countries AND include US or Canada in your set of allowed countries.

```kotlin
Fidel.termsAndConditionsUrl = "https://yourwebsite.com/terms"
```

#### Explain how the cardholder can opt out (optional, but recommended)

Please inform the cardholder about how to opt out of transaction monitoring in your program.

```kotlin
Fidel.deleteInstructions = "how can the cardholder opt out"
```

#### Set your privacy policy (optional, but recommended)

```kotlin
Fidel.privacyPolicyUrl = "https://yourwebsite.com/privacy-policy"
```

# Enrollment notifications

In order to be notified about different, useful events (a card was linked, card failed to link etc.) that happen during a enrollment process, we recommend using our webhooks.

If client side notifications are useful for your application, make sure to check our SDK callback reference documentation.

# Enroll a card

Call the `Fidel.start` function to open the UI and start a card enrollment process:

```kotlin
Fidel.start(yourContext)
```

You can test the card enrollment flow, by setting a test SDK Key and by using the Fidel API [test card numbers](/select/cards/#test-card-numbers).

If your Fidel API account is `live` then cardholders can also enroll real, live cards. Make sure that you set a live SDK Key, in order to allow live card enrollments.

# Optional: Set any of the other useful properties

Please check our SDK Reference for details about any other SDK properties that might be useful for your application.

# Frequently asked questions

### How can I upgrade the Android SDK to the latest version?

- Check what's the latest version of the Fidel API Android SDK in our Releases documentation page.

- Add the latest version of our SDK as a dependency in your app's module `build.gradle` file:

```groovy
fileName:app/build.gradle
implementation 'com.github.Enigmatic-Smile:android-sdk:{latestFidelSDKVersion}'
```

### Can I customize the UI of the Android SDK?

The Android SDK offers the `Fidel.bannerImage` property for you to set a custom, branded banner image that will be displayed during the card enrollment process. Please check our Reference documentation for more details.

### How do I configure the consent text correctly?

In order to properly set the consent text, please follow these steps:

1. **Set the company name**

The `Fidel.companyName` property is optional, but we recommended setting it. If you don't set a company name, we'll show the default value in the consent text: `Your Company Name`.

2. **Set the privacy policy URL**

The `Fidel.privacyPolicyUrl` property is optional. It is added as a hyperlink to the `privacy policy` text. Please check the full behavior below.

3. **Set the delete instructions**

The `Fidel.deleteInstructions` property is optional. The default value is `going to your account settings`. This default value is applied for both consent texts that the SDK forms.

4. **Set the card scheme name**

You can do so via the `Fidel.supportedCardSchemes` property. By default, we allow the user to input card numbers from either Visa, Mastercard or American Express, but you can control which card networks you accept. The consent text changes based on what you define or based on what the user inputs. Please check the full behavior below.

5. **Set the program name (applied to the consent text specific to the US and Canada)**

The `Fidel.programName` property is taken into account only for the consent text specific to USA and Canada. If you don't plan to support USA nor Canada, you can ignore this property. The default value for program name is `our`.

6. **Set the terms and conditions URL (applied to the consent text only for USA and Canada issued cards)**

The `Fidel.termsAndConditionsUrl` property is mandatory for the SDK if you plan to support USA and Canada issued cards. Once set, it will be applied as a hyperlink on the `Terms and Conditions` text.

### How is the SDK's consent text formed?

The SDK forms two distinct consent texts, depending on the country the cardholder selects as the card issuing country:
1. A specific consent text for US and Canada
2. Another consent text for the other countries supported by Fidel API.

#### Consent text for United States and Canada

You are allowing US and Canada issued cards when you:
1. set United States and/or Canada as `allowedCountries`
2. don't set a value for the `Fidel.allowedCountries` property, which means that all countries supported by Fidel API will be included in your SDK implementation (including US & Canada).

For USA & Canada, the following would be an example consent text for `Cashback Inc` (an example company) that uses `Awesome Bonus` as their program name:

_By submitting your card information and checking this box, you authorize `card_scheme_name` to monitor and share transaction data with Fidel API (our service provider) to participate in `Awesome Bonus` program. You also acknowledge and agree that Fidel API may share certain details of your qualifying transactions with `Cashback Inc` to enable your participation in `Awesome Bonus` program and for other purposes in accordance with the `Cashback Inc` Terms and Conditions, `Cashback Inc` privacy policy and Fidel’s Privacy Policy. You may opt-out of transaction monitoring on the linked card at any time by `deleteInstructions`._

#### Consent text for the other Fidel API supported countries

When you allow countries other than US & Canada and the user selects one of these countries from the list of card issuing countries, a consent text specific for these countries will be applied.

The following would be an example Terms & Conditions text for `Cashback Inc` (an example company):

_I authorise `card_scheme_name` to monitor my payment card to identify transactions that qualify for a reward and for `card_scheme_name` to share such information with `Cashback Inc`, to enable my card linked offers and target offers that may be of interest to me. For information about `Cashback Inc` privacy practices, please see the privacy policy. You may opt-out of transaction monitoring on the payment card you entered at any time by `deleteInstructions`._

### Is the SDK localized?

The SDK's default language is English, but it's also localized for French and Swedish languages. When the device has either `Français (Canada)` or `Svenska (Sverige)` as its language, the appropriate texts will be displayed.

Please make sure that your project also supports localization for the languages that you want to support.
