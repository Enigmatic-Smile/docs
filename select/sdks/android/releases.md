# Android SDK v2 Releases

## 2.3.2

- Fix StringBuilder on consent terms

## 2.3.1

- Fix language on consent terms on card linking screen

## 2.3.0

- Improve consent terms on card linking screen

## 2.2.1

- Fix missing translation labels for fr and sv language on program name.

## 2.2.0

- Migrate to Enigmatic-Smile GitHub organization

## 2.1.2

- No relevant changes for this product

## 2.1.1

- Add a few more card verification metrics tracking features to help us improve our services.

## 2.1.0

- Add card enrollment & verification metrics tracking features to help us improve our services.

## 2.0.0

- Removed card scanning so `Fidel.autoScan` has been removed.
- Changes in the `Fidel` class:
  - Made the `companyName` property mandatory
  - `Fidel.apiKey` was renamed to `Fidel.sdkKey`.
  - `Fidel.termsConditionsURL` was renamed to `Fidel.termsAndConditionsUrl`.
  - `Fidel.privacyURL` was renamed to `privacyPolicyUrl`.
  - `getInstance()` has been removed
- `Fidel.present(...)` has been renamed to `Fidel.start(...)`.
- `Country`, `ProgramType` and `CardScheme` have been moved out of the `Fidel` class.
- `onActitivityResult` will no longer be used to get results.
- Instead of providing just one type of result, `LinkResult`, the SDK now provides a generic `Result` sealed class which incorporates different types of results: `EnrollementResult`, `VerificationSuccessful` (not relevant for Select Transaction API), `ErrorResult`. Each provides the appropriate objects for developers: `EnrollementResult`, `FidelError`.
- Changes in the `LinkResult` class:
  - Rename to `EnrollmentResult`
  - Rename `id` to `cardId`
  - The data type for `created` property is now `Date` not `String`. Renamed the property to `enrollmentDate`.
  - The data type for `scheme` property is now `CardScheme`, not `String`. Renamed property to `cardScheme`.
  - Renamed property `live` to `isLive`.
  - Renamed `firstNumbers` to `cardFirstNumbers`.
  - Renamed `lastNumbers` to `cardLastNumbers`.
  - Renamed `expYear` to `cardExpirationYear`.
  - Renamed `expMonth` to `cardExpirationMonth`.
  - The `expDate` string property has been removed.
  - The `countryCode` property was renamed to `cardIssuingCountry` and its data type is now `Country`.
  - The `error` property and the `getError` function have been removed.
- Changes to the `LinkResultError` class:
  - Renamed to `FidelError`.
  - The `date` property now has a `Date` data type, not `String`.
  - The (error) code property has been renamed to `type` and it is now a sealed class. It has 2 cases: `EnrollmentError` and `VerificationError` (not relevant for Select Transaction API)
  - Each error type now has a subtype.
- `Fidel.onMainActivityCreate(Activity)` needs to be called at the end of the setup.
- The new package name is `com.fidelapi` instead of `com.fidel.sdk` for all the SDK classes.

# Android SDK v1 Releases

## 1.7.5

- Updated the consent text for United States and Canadian issued cards.

## 1.7.4

- Allow expiration date editing without switching to country selection.

## 1.7.3

- Replace "Fidel" with "Fidel API" in the consent text.

## 1.7.2

- Update Fidel API logo

## 1.7.1

- Add support for the `resConfigs` optimization parameter. Now, if your app attempts to optimize resources and set the `resConfigs` parameter in gradle (which might remove some of our string resources), our SDK will display either strings in the default language (English) or in one of the languages supported by your app. This also depends on the device language and how the Android system resolves string resources.

## 1.7.0

- Added the `defaultSelectedCountry` property which sets the country that will be selected by default, when opening the card enrollment screen.

## 1.6.0

- Removed the card scanning confirmation screen. Users can confirm their card information by checking the information in the Fidel card enrollment screen.

## 1.5.6

- Add United Arab Emirates option as a country of issuance.
- Country text view shrinks its fonts size, to fit longer country names, on smaller devices.

## 1.5.5

- Improve French and Swedish language translations to cover more countries.

## 1.5.4

- Improvements to allow more automation for quality assurance and speed of new SDK version delivery.

## 1.5.3

- Improved compatibility with Kotlin projects
- Updated dependencies

## 1.5.2

- Improved the user experience by making the form static when switching the focus from one text input field to another. Previously the form was scrolling to show the focused text field on top of the screen. For this reason the banner image was hidden when display the card linking activity. Now the banner image is always visible and we do no scrolling animations when switching the focus from one text field to another.

## 1.5.1

- If available, the LinkResult object now includes the `firstNumbers` field. So, if in the Fidel Dashboard, under the your security settings, you allow showing the first numbers of the linked card numbers, the information will be available in the LinkResult object too. If you do not allow showing the first numbers in the linking result, the `firstNumbers` field will return `"******"` (just like the object which the Fidel API returns).

## 1.5.0

- Now the SDK allows you to select multiple allowed countries from which the user can pick. Please check the docs for the new `allowedCountries` property.
- Removed the `Fidel.country` property. To set a default country and not allow the user to pick the country, set a single country in the new `Fidel.allowedCountries` array.

## 1.4.0

- Localised the SDK for French and Swedish users.
- The terms & conditions text now adjusts to the card scheme name that the user inputs (Visa, MasterCard or Amex).
- If you set the default country of the SDK to USA or Canada (with `Fidel.country` property) or, if you do not set a default country, the terms and conditions text will adjust depending on the country you have set. For USA & Canada, the following would be an example Terms & Conditions text, for Cashback Inc (an example company):

*By submitting your card information and checking this box, you authorize Visa to monitor and share transaction data with Fidel (our service provider) to participate in  program. You also acknowledge and agree that Fidel may share certain details of your qualifying transactions with Cashback Inc to enable your participation in  program and for other purposes in accordance with the Cashback Inc Terms and Conditions, Cashback Inc privacy policy and Fidel’s Privacy Policy. You may opt-out of transaction monitoring on the linked card at any time by contacting support.*

For the rest of the world:

*I authorise Visa to monitor my payment card to identify transactions that qualify for a reward and for Visa to share such information with Cashback Inc, to enable my card linked offers and target offers that may be of interest to me. For information about Cashback Inc privacy practices, please see the privacy policy. You may opt-out of transaction monitoring on the payment card you entered at any time by contacting support.*

- Added the `programName` and `termsConditionsURL` properties. They are used when building the new USA / Canada specific terms and conditions text that the user must agree with, before linking a card. If you set the `Fidel.country` property to a USA or Canada, it's mandatory for you to provide your terms and conditions URL via `Fidel.termsConditionsURL`. If you do not provide it, you will receive an error when you try to open Fidel's Activity (via `onActivityResult` function).
- Updated play services auth dependency library to 18.1.0.
- Fixed some small bugs.

## 1.3.3

- Tapping the Android back button closes the Fidel UI with the same "user cancelled" error, just like tapping the top left X (close) button.

## 1.3.2

- Solved some UI issues.
- The close button clickable area has been increased.

## 1.3.1

- Updated gradle and some dependencies.

## 1.3.0

- Added new property that lets you define the card schemes that you suppport (`supportedCardSchemes`).

## 1.2.3

- Added the Canada country option.
- Remove `allowBackup` and `supportsRtl` flags from the SDK's `AndroidManifest.xml` file to allow you to choose to set them as you'd like.

## 1.2.2

- Added support for linking American Express cards

## 1.2.1

- Added the Japan country option.
- Hidden the PayPal logo in the card scanning UI.
- Disabled CardIO manual card details entry forms.
- Improved testing mode user experience and the overall UX.
- Allow the user to select the consent checkbox, even before filling in any information.
- Tapping anywhere on the screen dismisses the keyboard.
- If you don't set a banner image, we'll hide the top space reserved for it.

## 1.2.0

- Added the Sweden country option
- Returns an error, when the SDK encounters it, with the `LinkResult`. Get it with `linkResult.getError()` getter.
- Now you can customize the final consent text with the following API:

    `Fidel.companyName = "Your Company Name Inc.";` (Maximum 60 characters)

    `Fidel.privacyURL = "https://yourcompany.com/privacyURL";` (must be a valid URL)

    `Fidel.deleteInstructions = "Your delete instructions";` (Maximum 60 characters)

- If the data above is not valid, the UI will not be displayed and you will get an error with the `LinkResult` in the activity result.
- Set a default country the SDK should use with `Fidel.country = Fidel.Country.UNITED_KINGDOM`. When you set a default country, the card linking screen will not show the country picker UI.
- Add support for more test cards. Anything with the following format:

    Visa: _4444000000004***_

    Mastercard: _5555000000005***_


## 1.1.0

- Added the United States country option
- Updated gradle and build tools
- Removed the 'com.afollestad.material-dialogs:core:0.9.4.5' dependency
- Removed the 'com.koushikdutta.ion:ion:2.+' dependency
- Doesn't use the Gson library anymore for specifying meta data. Please use the `org.json.JSONObject` object instead of `com.google.gson.JsonObject`
- More protection against SSL exploits by using Google Play Services. When using the library on an emulator, make sure to use a emulator that has the Google Play Services.
- Updated `appcompat-v7` library to version `26.1.0`
- Support the use of all test card numbers. Previously some errors occured while using some of the test cards.
- Updated checkbox consent text
- Improved error message when the user tries to link a card that's already linked.
- Made only the color resources used by the SDK public. The rest of the resources are private now. This will prevent future resource conflicts.
- Added an error color resource, which you can adjust. It's called `colorError`.
