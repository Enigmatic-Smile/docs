# React Native SDK v2 Releases

## 3.0.4

- Update Fidel and iOS SDK versions.

## 3.0.3

- Update Fidel Android SDK version.

## 3.0.2

- Fix referenced unused iOS adapter

## 3.0.1

- Fix syntax error on android Bridge Library Event

## 3.0.0
- Remove transaction stream related logic
- Update Fidel & Android SDK versions.

# 2.4.0
- Update Fidel & Android SDK versions.

# 2.3.2
- Update Fidel & Android SDK versions.

# 2.3.1
- Update Fidel & Android SDK versions.
- Fix language on consent terms on card linking screen

# 2.3.0
- Update Fidel & Android SDK versions.
- Improve consent terms on card linking screen

# 2.2.1
- Upgrade Android SDK version to 2.2.1

## 2.2.0
- Migrate to Enigmatic-Smile GitHub organization

## 2.1.1
- No relevant changes for this product

## 2.1.0
- Add card enrollment & verification metrics tracking features to help us improve our services.

## 2.0.0
- Renamed the `apiKey` property to `sdkKey`.
- Added the `programType` property to specify the program type that you'll use in your app. This property determines the card enrollment flow that the SDK presents.
- Removed the `Fidel.setOptions` function. All SDK properties are now being set via the `Fidel.setup` function. Please check our Reference documentation to see a complete list of the SDK properties.
- The `privacyUrl` property has been renamed to `privacyPolicyUrl`.
- The `termsConditionsUrl` has been renamed to `termsAndConditionsUrl`.
- The `Fidel.openForm` function has been renamed to `Fidel.start`. The callback parameter of this function has been moved. It is now the second parameter of the `Fidel.setup` function. The callback received results also have changed. Please check our reference documentation for more details.
- Removed the card scanning feature of the SDK as it was not working well. That's why we also removed the `autoScan` SDK property.
- Added support for cards issued in Norway, so you can add the `Fidel.Country.norway` to your `allowedCountries` array.
- For the Android library:
   1. It does not use `jCenter()` repository anymore, but `mavenCentral()`.
   2. Updated `targetSdkVersion` version to `32`.
- Added a lot of new features for our Transaction Stream product, which requires card verification. These features are not useful for Select Transactions/Loyalty use cases. Please check the Transaction Stream documentation for more details.

# React Native SDK v1 Releases

## 1.6.4
- Updated the consent text for United States and Canadian issued cards.

## 1.6.3
- Replace "Fidel" with "Fidel API" in the consent text.
- Allow expiration date editing on Android without switching to country selection.

## 1.6.2
- Update Fidel API logo

## 1.6.2
- Update Fidel API logo

## 1.6.1
- Always provide the `scheme` field in Android, after successful card enrollments.
- Provide support for `resConfigs` optimization parameter in Android.

## 1.6.0
- Added the `defaultSelectedCountry` property which sets the country that will be selected by default, when opening the card enrollment screen.

## 1.5.0
- Remove card scanning confirmation screen.

## 1.4.3
- Add United Arab Emirates option as a country of issuance.
- Country label shrinks its font size, to fit longer country names, on smaller devices.

## 1.4.2
- Update Fidel Android SDK version to allow French & Swedish translations be available in more countries.

## 1.4.1
- Update Fidel & Android SDK versions.
- Prepare for the newest React Native versions.
- Fix some unit tests.
- Made it easier to automate testing this React Native bridge library.

## 1.4.0

- Now the SDK allows you to select multiple allowed countries from which the user can pick. Please check the docs for information about the new `allowedCountries` option.
- Removed the `country` option. To set a default country and not allow the user to pick the country, set a single country in the new `allowedCountries` option. Check the example project or the README docs to see how to do that.
- If available, the linking result object now includes the `firstNumbers` field. So, if in the Fidel Dashboard, under your security settings, you allow showing the first numbers of the linked card numbers, the information will be available in the linking result object too. If you do not allow showing the first numbers in the linking result, the `firstNumbers` field will return `"******"` (just like the object which the Fidel API returns).

## 1.3.1

- Fixes an Android issue in the latest React Native versions, when specifying the banner image in release environment.
- Checking for `null` values, in mandatory SDK fields, to prevent unclear bugs.

## 1.3.0

- Localised the SDK for French and Swedish users.
- The terms & conditions text now adjusts to the card scheme name that the user inputs (Visa, MasterCard or Amex).
- If you set the default country of the SDK to USA or Canada (with `country` option) or, if you do not set a default country, the terms and conditions text will adjust depending on the country you have set. For USA & Canada, the following would be an example Terms & Conditions text, for Cashback Inc (an example company):

*By submitting your card information and checking this box, you authorize Visa to monitor and share transaction data with Fidel (our service provider) to participate in  program. You also acknowledge and agree that Fidel may share certain details of your qualifying transactions with Cashback Inc to enable your participation in  program and for other purposes in accordance with the Cashback Inc Terms and Conditions, Cashback Inc privacy policy and Fidel’s Privacy Policy. You may opt-out of transaction monitoring on the linked card at any time by contacting support.*

For the rest of the world:

*I authorise Visa to monitor my payment card to identify transactions that qualify for a reward and for Visa to share such information with Cashback Inc, to enable my card linked offers and target offers that may be of interest to me. For information about Cashback Inc privacy practices, please see the privacy policy. You may opt-out of transaction monitoring on the payment card you entered at any time by contacting support.*

- Added the `programName` and `termsConditionsUrl` options. They are used when building the new USA / Canada specific terms and conditions text that the user must agree with, before linking a card. If you set the `country` option to a USA or Canada, it's mandatory for you to provide your terms and conditions URL via `termsConditionsUrl`. If you do not provide it, you will receive an error when you try to open Fidel's linking screen.
- Fixed bug that returned an empty link result in Android, when linking is successful.
