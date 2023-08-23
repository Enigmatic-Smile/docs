# React Native SDK v2 Releases

## 2.0.0

- We have released all the changes to the SDK from the beta track as a new major stable version.
  After months of rigorous testing, we are confident that Transaction Stream SDK is ready to empower your projects so you can create more valuable, engaging experiences for your customers.

## 2.0.0-beta14

- Card scanning is no longer supported. 
- The options `enableCardScanner` and `shouldAutoScanCard` were removed.

## 2.0.0-beta13

- Updated the micro-charge descriptor text to the correct one.

## 2.0.0-beta12

- Added the `Fidel.onCardVerificationChoiceSelected` callback _(Experimental feature)_ that communicates the cardholder's choice and intention to either verify the card on the spot (because the cardholder has access to the card statement) or to express the intention to delegate card verification to a third-party entity.

## 2.0.0-beta11

- Added the `thirdPartyVerificationChoice` property to allow cardholders to choose between verifying the card on the spot (as previously available) or indicate that the cardholder does not have access to the card statement and needs to delegate card verification to a third-party entity.
- Added the `onCardVerificationStarted` callback which communicates that card verification has started for a card that was enrolled in a Fidel program. This callback provides the _consent details_ that are created. Use these details to start the card verification.
- Added the `Fidel.verifyCard` function to attempt card verification for a previously enrolled card. This function can be used for either the cardholder or a third-party entity (that cannot enroll cards, but can verify cards).
- Changes in the verification screen text that gives a better description on how the micro-charge will be displayed in the card statement
- Removes the currency symbol during the input of the verification token (the micro-charge amount).
- Added Norway to the countries supported by the SDK.
  - If you specified the `allowedCountries` property, remember to add Norway, to have it in the `Country of issue` list.
  - If you didn't customize the `allowedCountries` property, Norway will automatically show up in the `Country of issue` field after you integrate the newest version of the SDK.

## 2.0.0-beta10

- Card scanning will now be disabled by default, use  the option `enableCardScanner: true` to enable card scanning.

## 2.0.0-beta9

- This version displays better text to cardholders explaining that card verification micro-charges will be refunded within 72 hours.

## 2.0.0-beta8

- This version provides a comprehensive error message in case the micro-charge fails. You don't need to make any changes to your code in order to use this version.

## 2.0.0-beta6

The library now includes all the changes in the Android 2.0.0-beta8 and all the changes in the iOS 2.0.0-beta8 releases.

## 2.0.0-beta5

The library now includes all the changes in the Android 2.0.0-beta6, Android 2.0.0-beta7 and all the changes in the iOS 2.0.0-beta6, iOS 2.0.0-beta7

## 2.0.0-beta4

The library now includes all the changes in the Android 2.0.0-beta5 and all the changes in the iOS 2.0.0-beta5

## 2.0.0-beta3

The library now includes all the changes in the Android 2.0.0-beta4 and all the changes in the iOS 2.0.0-beta4

## 2.0.0-beta2

1. The library now includes all the changes in the Android 2.0.0-beta3 and all the changes in the iOS 2.0.0-beta3.
2. For the Android library:
   1. It does not use `jCenter()` repository anymore, but `mavenCentral()`.
   2. Updated `targetSdkVersion` version to `31`.
3. For the iOS library we fixed the `companyName` parameter and the library compiles fine now.

## 2.0.0-beta1

- Renamed the `apiKey` property to `sdkKey`.
- Added the `programType` property to specify the program type that you'll use in your app.
- If the `programType` property is set to `.transactionStream`, users will be able to start the card verification flow.