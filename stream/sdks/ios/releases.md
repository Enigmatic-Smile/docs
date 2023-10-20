# iOS SDK v2 Releases

## 2.1.0
- Add card enrollment & verification metrics tracking features to help us improve our services.

## 2.0.0

- Transaction Stream iOS SDK is now Stable!
  We are thrilled to announce a significant milestone in our journey – the Transaction Stream SDK has officially transitioned from its beta phase to a stable release!
  After months of rigorous testing, we are confident that Transaction Stream SDK is ready to empower your projects so you can create more valuable, engaging experiences for your customers.

## 2.0.0-beta20

- Removed card scanning feature, please make sure to delete the `Fidel.enableCardScanner` and `Fidel.shouldAutoScanCard` configuration.

## 2.0.0-beta19

- Improve the security of the SDK secrets.

## 2.0.0-beta18

- Make the Fidel API requests ephemeral.

## 2.0.0-beta17

- Updated the micro-charge descriptor text to the correct one.

## 2.0.0-beta16

- Fixed a bug that prevented corporate cardholders to delegate card verification to a third party, but later change their minds and verify the card (using the `Fidel.verifyCard` function), on the same device.

## 2.0.0-beta15

- Added Norway to the countries supported by the SDK.
  - If you specified the `allowedCountries` property, remember to add Norway, to have it in the `Country of issue` field.
  - If you didn't customize the `allowedCountries` property, Norway will automatically show up in the `Country of issue` field after you integrate the newest version of the SDK.

## 2.0.0-beta14

- Card scanning will now be disabled by default, use `Fidel.enableCardScanner = true` to enable card scanning.

## 2.0.0-beta13

- Useful features for corporate card enrollment, but not only:
  - The `Fidel.thirdPartyVerificationChoice` property to allow cardholders to choose between verifying the card on the spot (as previously available) or indicate that the cardholder does not have access to the card statement and needs to delegate card verification to a third-party entity.
  - The `Fidel.verifyCard` function to attempt card verification for a previously enrolled card. This function can be used for either the cardholder or a third-party entity (that cannot enroll cards, but can verify cards).
  - _(Experimental feature)_ The `Fidel.onCardVerificationChoiceSelected` callback that communicates the cardholder's choice and intention to either verify the card on the spot (because the cardholder has access to the card statement) or to express the intention to delegate card verification to a third-party entity.
- Adds the `Fidel.onCardVerificationStarted` callback which communicates that card verification has started for a card that was enrolled in a Fidel program. This callback provides the consent details that is created, in order to start card verification.
- Changes in the verification screen text that gives a better description on how the micro-charge will be displayed in the card statement
- Removes the currency symbol during the input of the verification token (the micro-charge amount).

## 2.0.0-beta12

- This version displays better text to cardholders explaining that card verification micro-charges will be refunded within 72 hours.

## 2.0.0-beta11

- Fixes error with Result not properly sent.

## 2.0.0-beta10

- This version provides a comprehensive error message in case the micro-charge fails. You don't need to make any changes to your code in order to use this version.

## 2.0.0-beta9

- Removed the `FidelResult.verificationSuccessful`. Replaced with `FidelResult.verificationResult`.
- We are now providing `cardID` as part of the `FidelResult.verificationResult`.

## 2.0.0-beta8

- We've now blocked the SDK for cardholders that use insecure devices. If a cardholder has an insecure device, you'll receive the `.deviceNotSecure` error via the `onResult` closure/parameter that you can set. Please treat this error in the most appropriate way for your app.
- We stopped allowing copying card details from our SDK's input fields. Only the "Paste" operation is allowed.

## 2.0.0-beta7

Support card verification in GBP for UK issued cards.

## 2.0.0-beta6

We updated the "Fidel" name to "Fidel API".

## 2.0.0-beta5

We updated the Fidel logo.

## 2.0.0-beta4

We merged a few changes that we did in our public SDK:

1. Added the `defaultSelectedCountry` property which sets the country that will be selected by default, when opening the card enrollment screen.
2. Removed the scanned card information confirmation screen. The card information is displayed immediately in the card enrollment screen after card scanning.
3. Fixed the bug that does not allow enrollment for cards that expire in the current month.

## 2.0.0-beta3

1. If the user did not complete the card verification step (but did succeed to enroll a card) the SDK opens the card verification step when the app is re-opened. We made a few changes related to this process:
   - Before opening the card verification step automatically, when the app is re-launched, we make sure that the Fidel SDK is configured correctly. If it is not configured correctly (it misses parameters or they are not valid) then you'll receive an appropriate error, if you're using the `Fidel.onResult` closure. In case the SDK was not configured correctly, the card verification step will not be opened automatically.
   - If the user did not complete the card verification step, closed the flow at this step, but does not quit your app and attempts to re-connect the card (by pressing on the button in your app), we'll take the user directly to the card verification step, to continue the process. Previously the user had to enroll the card again, which would have caused an error. By letting the user continuing the card verification process, we avoid errors in this scenario.

## 2.0.0-beta2

1. Renamed the `present` function (which opened the Fidel card connection) UI to `start`, in order to align with the web SDK.
2. We made the `companyName` property mandatory, as it is reflected in the consent text the user has to agree with before enrolling a card, so it made sense to make it mandatory. If the property is not set, you will not be able to open the Fidel card connection UI, but receive an error result immediately after the attempt to start the card connection process.
3. We wanted to improve the SDK APIs to comply with the [Swift API design guidelines](https://www.swift.org/documentation/api-design-guidelines/):
   1. Rename `programId` property to `programID`.
   2. Longer, more expressive names are fine, so we did the following renaming as well:
      1. Rename `autoScan` property to `shouldAutoScanCard`.
      2. Rename `privacyURL` property to `privacyPolicyURL`.
      3. Rename `termsConditionsURL` property to `termsAndConditionsURL`.
4. The `allowedCountries` property has been transformed from an `Array`, to a `Set`. If you allow multiple countries, they will now be sorted alphabetically.
5. `ProgramType` enum now implements the `Equatable` protocol (as all other enums made available by the Fidel SDK).
6. `FidelAppDelegate` class has been removed. There is no need to make the `Fidel.shared.applicationDidBecomeActive(_:)` function call in your `AppDelegate` anymore.
7. Removed **Objective-C support** from our SDK in order to make our responses enums and structs which are more appropriate for what most iOS developers use: Swift.
8. Improved the result entities and we are providing more types of results, during the card connection processes. In order to achieve this, we made a few changes to our SDK APIs:
   1. Removed the callback parameter from the `start` function (previously named `present`). The reason is that we introduced the flow to verify a card. If the `programType` that you set is `.transactionStream` and if the user enrolls a card, but does not finish verifying the card, then the card verification screen will be re-opened the next time your app is opened, to make sure the user finishes card verification. That means that it is possible to receive card verification results or error results by starting this process without the user’s action. That’s why handling the result has been separated from the `start` function.
   2. In order to handle a result, you have to set the `Fidel.onResult` property. It can be called multiple times during a card connection flow. In order to receive results during the entire card connection process, please set the `onResult` property in the `application(_:didFinishLaunchingWithOptions:)` function, like all other Fidel configuration properties.
   3. The possible results that you can handle are `FidelResult` enum cases. They are the following:
      1. `enrollmentResult(EnrollmentResult)`; `EnrollmentResult` was previously named `LinkResult`.
      2. `verificationSuccessful`.
      3. `error(FidelError)`.
   4. Renamed the `LinkResult` class to `EnrollmentResult`.
   5. Transformed the `EnrollmentResult` into a struct.
   6. `EnrollmentResult` implements `Equatable` protocol.
   7. Renamed a few properties of the `EnrollmentResult`, in order to comply with the [Swift API design guidelines](https://www.swift.org/documentation/api-design-guidelines/), and for some properties, we also changed their data types to be more suitable for these properties:
      1. `id` has been renamed to `cardID`.
      2. `programId` renamed to `programID`.
      3. `accountId` renamed to `accountID`.
      4. `created` has been renamed to `enrollmentDate`.
      5. Changed data type of `enrollmentDate` from `String` to a `Date`.
      6. Renamed the `live` property to `isLive`.
      7. Renamed `firstNumbers` to `cardFirstNumbers`.
      8. Renamed `lastNumbers` to `cardLastNumbers`.
      9. Renamed `expYear` to `cardExpirationYear`.
      10. Renamed `expMonth` to `cardExpirationMonth`.
      11. `expDate` of type `String` was removed, as we already provide `cardExpirationYear` and `cardExpirationMonth`.
      12. Renamed `scheme` to `cardScheme`.
      13. Changed the data type of `cardScheme` from `String` (which used internal scheme identifying strings as values) to `CardScheme`, our custom data type that is easier to work with in Swift and that it was already used for the `Fidel.supportedCardSchemes` property.
      14. `countryCode` has been renamed to `cardIssuingCountry`
      15. Changed the data type of `cardIssuingCountry` from `String` (which used identifiers of countries as values) to `Country`, our custom data type that is easier to work with in Swift and that it was already used for the `Fidel.allowedCountries` property.
   8. Changes in the `LinkResultError` class:
      1. Renamed it to `FidelError` as now it’s possible to receive errors during the card verification process, not just during the card enrollment process.
      2. Changed data type of the `date` property from `String` to a `Date`.
      3. Renamed the `code` property to `type`.
      4. Changed the data type of the `type` property from `String` to `FidelErrorType` , which is a new enum introduced to identify types of errors easily in Swift (please check this enum to see the types of errors that you can handle).
   9. `EnrollmentResult` and `FidelError` structs implement the `Equatable` and `Decodable` protocols.
   10. The only way possible to create an `EnrollmentResult` and `FidelError` instances is to decode them from our Fidel API responses (easier through our SDK).
   11. All properties of our result instances (of `FidelError` & `EnrollmentResult`) are now immutable (`let` constants).
   12. All properties of possible responses are correctly optional or non-optional
   13. It’s possible to handle errors with the type `.userCanceled` during any stage of the card verification flow, if the user cancels it.

## 2.0.0-beta1

- Renamed the `apiKey` property to `sdkKey`.
- Added the `programType` property and the `ProgramType` enum to specify the program type that you'll use in your app.
- If the `programType` property is set to `.transactionStream`, users will be able to start the card verification flow.
