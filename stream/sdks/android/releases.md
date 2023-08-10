# Android SDK v2 Releases

## 2.0.0

- Stable release, stream is no longer in beta

## 2.0.0-beta18

- Removed card scanning feature, please make sure to delete the `Fidel.enableCardScanner` and `Fidel.shouldAutoScanCard` configuration.

## 2.0.0-beta17

- Updated the micro-charge descriptor text to the correct one.
- Fixed a security issue where the screenshots were taken in the Task Switcher.

## 2.0.0-beta16

- Added Norway to the countries supported by the SDK.
  - If you specified the `allowedCountries` property, remember to add Norway, to have it in the `Country of issue` field.
  - If you didn't customize the `allowedCountries` property, Norway will automatically show up in the `Country of issue` field after you integrate the newest version of the SDK.

## 2.0.0-beta15

- Card scanning will now be disabled by default, use `Fidel.enableCardScanner = true` to enable card scanning.

## 2.0.0-beta14

- Useful features for corporate card enrollment, but not only:
  - The `Fidel.thirdPartyVerificationChoice` property to allow cardholders to choose between verifying the card on the spot (as previously available) or indicate that the cardholder does not have access to the card statement and needs to delegate card verification to a third-party entity.
  - The `Fidel.verifyCard` function to attempt card verification for a previously enrolled card. This function can be used for either the cardholder or a third-party entity (that cannot enroll cards, but can verify cards).
  - _(Experimental feature)_ The `Fidel.onCardVerificationChoiceSelected` callback that communicates the cardholder's choice and intention to either verify the card on the spot (because the cardholder has access to the card statement) or to express the intention to delegate card verification to a third-party entity.
- Adds the `Fidel.onCardVerificationStarted` callback which communicates that card verification has started for a card that was enrolled in a Fidel program. This callback provides the consent details that are created, in order to start the card verification.
- Changes in the verification screen text that gives a better description on how the micro-charge will be displayed in the card statement
- Removes the currency symbol during the input of the verification token (the micro-charge amount).

## 2.0.0-beta13

- Fix the error that caused the user to be stuck on the "Link card" page after choosing to reconnect a card.

## 2.0.0-beta12

- This version displays better text to cardholders explaining that card verification micro-charges will be refunded within 72 hours.

## 2.0.0-beta11

- Fixes some result errors.

## 2.0.0-beta10

- This version provides a comprehensive error message in case the micro-charge fails. You don't need to make any changes to your code in order to use this version.

## 2.0.0-beta9

- Removed the `FidelResult.VerificationSuccessful`. Replaced with `FidelResult.Verification`.
- We are now providing `cardId` as part of the `FidelResult.Verification`

## 2.0.0-beta8

- We've now blocked the SDK for cardholders that use insecure devices. If a cardholder has an insecure device, you'll receive an error of type `DeviceNotSecure` via the `onResult` callback that you can set. Please treat this error in the most appropriate way for your app.
- We stopped allowing copying card details from our SDK's input fields. Only the "Paste" operation is allowed.
- Allow expiration date editing without switching to country selection.

## 2.0.0-beta7

Support card verification in GBP for UK issued cards.

## 2.0.0-beta6

We updated the "Fidel" name to "Fidel API".

## 2.0.0-beta5

We updated the Fidel logo.

## 2.0.0-beta4

We merged a few changes that we did in our public SDK:

1. Add support for the `resConfigs` optimization parameter. Now, if your app attempts to optimize resources and set the `resConfigs` parameter in gradle (which might remove some of our string resources), our SDK will display either strings in the default language (English) or in one of the languages supported by your app. This also depends on the device language and how the Android system resolves string resources.
2. Added the `defaultSelectedCountry` property which sets the country that will be selected by default, when opening the card enrollment screen.
3. Removed the card scanning confirmation screen. Users can confirm their card information by checking the information in the Fidel card enrollment screen.

## 2.0.0-beta3

1. The Card Verification steps/screen now supports locales that use ","(comma) instead of "."(dot) as amount digits separator. The view will force the display of the amount with a "."(dot) digit separator.
2. If the user did not complete the card verification step (but did succeed to enroll a card) the SDK opens the card verification step when the app is re-opened. We made a few changes related to this process:
   - Before opening the card verification step automatically, when the app is re-launched, we make sure that the Fidel SDK is configured correctly. If it is not configured correctly (it misses parameters or they are not valid) then you'll receive an appropriate error, if you're using the `Fidel.onResult` closure. In case the SDK was not configured correctly, the card verification step will not be opened automatically.
   - If the user did not complete the card verification step, closed the flow at this step, but does not quit your app and attempts to re-connect the card (by pressing on the button in your app), we'll take the user directly to the card verification step, to continue the process. Previously the user had to enroll the card again, which would have caused an error. By letting the user continuing the card verification process, we avoid errors in this scenario.
3. The `targetSdkVersion` and `compileSdkVersion` have been updated to version `31`.
4. Updated the following dependencies:
   1. `androidx.core:core-ktx:1.8.0-alpha02`
   2. `androidx.appcompat:appcompat:1.4.1`
   3. `androidx.constraintlayout:constraintlayout:2.1.3`
   4. `com.google.android.gms:play-services-auth:20.0.1`

## 2.0.0-beta2

1. Renamed the package of the SDK to `com.fidelapi`.
2. Renamed the `present` function (which opened the Fidel card connection UI) to `start`, in order to align with the web SDK.
3. Rename `privacyURL` property to `privacyPolicyUrl`.
4. Rename `termsConditionsURL` property to `termsAndConditionsUrl`.
5. Rename `autoScan` property to `shouldAutoScanCard`.
6. `CardScheme`, `Country`, `ProgramType` data types are not be subtypes of the `Fidel` class anymore. They are separate enum classes now.
7. We made the `companyName` property mandatory, as it is reflected in the consent text the user has to agree with before enrolling a card, so it made sense to make it mandatory. If the property is not set, you will not be able to open the Fidel card connection UI, but receive an error result immediately after the attempt to start the card connection process.
8. The `allowedCountries` property has been transformed from an `List` to a `Set`. If you allow multiple countries, they will now be sorted alphabetically.
9. Updated dependencies:
   1. `androidx.constraintlayout:constraintlayout` to version `2.1.2`.
   2. `androidx.annotation:annotation` to version `1.3.0`.
10. Remove the `getInstance()` function of the `Fidel` facade class for consistency purposes. We’ll keep using static properties and functions.
11. `onMainActivityCreate` function is now a static function, so it will be called like in the following line: `Fidel.onMainActivityCreate(this)`.
12. Improved the result objects and we are providing **more** types of results, during the card connection processes. In order to achieve this, we made a few changes to our SDK results related APIs:
    1. Removed the ability to receive results in the `onActivityResult` function, because it allows receiving only one result during the card connection process and only when the process is finished. However, users can receive multiple errors during the card connection process, but also a successful `EnrollmentResult` and a `VerificationSuccessful` result.
    2. In order to handle a result, you have to set the `Fidel.onResult` property. We removed the `fidelCardLinkingObserver` and the `FidelCardLinkingObserver` interface. The `Fidel.onResult` callback can be called multiple times during a card connection flow. In order to receive results during the entire card connection process, please set the `onResult` property in your Main Activity’s `onCreate` function, like all other Fidel configuration properties.
    3. The possible results that you can handle are subclasses of `FidelResult`(sealed Kotlin class). They are the following:
       1. `data class Enrollment(val enrollmentResult: EnrollmentResult)`; `EnrollmentResult` was previously named `LinkResult`.
       2. `object VerificationSuccessful`.
       3. `data class Error(val error: FidelError)`; `FidelError` was previously named `LinkResultError`.
    4. Renamed the `LinkResult` class to `EnrollmentResult`.
    5. Transformed the `EnrollmentResult` into a Kotlin data class.
    6. Renamed a few properties of the `EnrollmentResult` class and for some properties, we also changed their data types which are more suitable:
       1. `id` has been renamed to `cardId`.
       2. `created` has been renamed to `enrollmentDate`.
       3. Changed data type of `enrollmentDate` from `String` to a `Long`, that you can use with your preferred Android date & time APIs.
       4. Renamed the `live` property to `isLive`.
       5. Renamed `firstNumbers` to `cardFirstNumbers`.
       6. Renamed `lastNumbers` to `cardLastNumbers`.
       7. Renamed `expYear` to `cardExpirationYear`.
       8. Renamed `expMonth` to `cardExpirationMonth`.
       9. `expDate` of type String was removed, as we already provide `cardExpirationYear` and `cardExpirationMonth`.
       10. Renamed `scheme` to `cardScheme`.
       11. Changed the data type of `cardSheme` from `String` (which used internal scheme identifying strings as values) to `CardScheme`, our custom data type to provide type safety and which was already used for the `Fidel.supportedCardSchemes` property.
       12. `countryCode` has been renamed to `cardIssuingCountry`
       13. Changed the data type of `cardIssuingCountry` from `String` (which used identifiers of countries as values) to `Country`, our custom data type that is easier to work with in Swift and that it was already used for the `Fidel.allowedCountries` property.
       14. Removed the `getError` function. 
    7. Changes in the `LinkResultError` class:
       1. Renamed it to `FidelError` as now it’s possible to receive errors during the card verification process, not just during the card enrollment process.
       2. Changed data type of the date property from `String` to a `Long`, that you can use with your preferred Android date & time APIs.
       3. Renamed the `code` property to `type`.
       4. Changed the data type of the `type` property from `String` to `FidelErrorType` , which is a sealed class introduced to identify types of errors easily in Kotlin (please check this class to see the types of errors that you can handle).
    8. No results are Parcelable anymore, as that is no longer necessary.
    9. All properties of our result instances (of `FidelError` & `EnrollmentResult`) are now immutable (`val` constants)
    10. All properties of possible responses are correctly optional or non-optional.
    11. It’s possible to handle errors with the type `UserCanceled` during any stage of the card verification flow, if the user cancels it.

## 2.0.0-beta1

- Renamed the `apiKey` property to `sdkKey`.
- Added the `programType` property and the `ProgramType` enum to specify the program type that you'll use in your app.
- If the `programType` property is set to `.transactionStream`, users will be able to start the card verification flow.
