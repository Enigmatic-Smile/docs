## Mobile SDKs

The native mobile SDKs allow you to integrate card linking technology in your mobile apps in minutes. Card details are collected securely, removing any PCI compliance requirements from your side. See below code examples on how to integrate the SDKs in your apps. 

For more detailed documentation about any class in the SDKs and available customisation options, check the Github repositories for [iOS](https://github.com/FidelLimited/fidel-ios) and [Android](https://github.com/FidelLimited/fidel-android).

# iOS

<img
  src="https://docs.fidel.uk/assets/images/iossdk.png"
  srcset="https://docs.fidel.uk/assets/images/iossdk.png, https://docs.fidel.uk/assets/images/iossdk@2x.png 2x"
  alt="Preview of the iOS SDK"
/>

### Install

We recommend using CocoaPods to integrate Fidel iOS SDK with your project.
Add a `Podfile` (if you don't have one already), by running the following command: `pod init`.

Add Fidel pod:
```sh
pod 'Fidel'
```

### Camera 
In order to allow scanning cards with the camera, make sure to add the key `NSCameraUsageDescription` to your app’s Info.plist and set the value to a string describing why your app needs to use the camera (e.g. “Used for scanning credit cards”). 
<br/>

### Objective-C (skip for Swift projects)
If you have an Objective-C project and did not add any Swift code yet, please set the `Always Embed Swift Standard Libraries` flag in Build Settings to `YES`.
<br/>

### Troubleshooting
In case Cocoapods doesn't find the Fidel specs or it finds older specs, try updating with `pod update`. After updating, run `pod install`.
<br/>

### Usage
Import the SDK:
```swift
import Fidel
```
<br/>

Set your public SDK key (`pk_test` or `pk_live`) and the `programId` you want to link cards to:
```swift
Fidel.apiKey 	= "pk_test_6e94da6f-145a-47db-b56b-f1314e74aa2e"
Fidel.programId = "f8d6890e-145d-46ea-b66f-afacfd954580"
```
<br/>

Set cardholder consent variables:
```swift
Fidel.companyName = "Your Company Name" // default: "Company Name".
Fidel.privacyURL = "https://yourcompany.com/privacyURL"
Fidel.deleteInstructions = "Your delete instructions" // Maximum 60 characters, default: "going to your account settings."
```
<br/>

Set a default card country:
```swift
Fidel.country = .unitedKingdom // If set, country picker UI will not show
```
<br/>

Present the SDK view:
```swift
Fidel.present(self, onCardLinkedCallback: { (card: LinkResult) in
  print(card.id)
}, onCardLinkFailedCallback: { (err: LinkError) in
  print(err.message)
})
```
<br/>

For more detailed documentation about any class in the SDKs and available customisation options, check the [iOS repository on Github](https://github.com/FidelLimited/fidel-ios).
<br/>


# Android

<img
  src="https://docs.fidel.uk/assets/images/androidsdk.png"
  srcset="https://docs.fidel.uk/assets/images/androidsdk.png, https://docs.fidel.uk/assets/images/androidsdk@2x.png 2x"
  alt="Preview of the Android SDK"
/>

### Install

Add [jitpack.io](https://www.jitpack.io) to your root `build.gradle` at the end of repositories:
```sh
allprojects {
  repositories {
    ...
    maven { url 'https://jitpack.io' }
  }
}
```
<br/>

Add Fidel SDK as a dependency:
```sh
dependencies {
  implementation 'com.github.FidelLimited:android-sdk:1.3.1'
}
```
<br/>

### Usage

Set your public SDK key (`pk_test` or `pk_live`) and the `programId` you want to link cards to:
```swift
Fidel.apiKey    = "pk_test_6e94da6f-145a-47db-b56b-f1314e74aa2e"
Fidel.programId = "f8d6890e-145d-46ea-b66f-afacfd954580"
```
<br/>

Set cardholder consent variables:
```swift
Fidel.companyName = "Your Company Name" // default: "Company Name".
Fidel.privacyURL = "https://yourcompany.com/privacyURL"
Fidel.deleteInstructions = "Your delete instructions" // Maximum 60 characters, default: "going to your account settings."
```
<br/>

Set a default card country:
```swift
Fidel.country = Fidel.Country.UNITED_KINGDOM; // If set, country picker UI will not show
```
<br/>

You can pass additional data with [org.json.JSONObject](https://stleary.github.io/JSON-java/org/json/JSONObject.html):

```java
JSONObject jsonMeta = new JSONObject();
try {
    jsonMeta.put("id", "this-is-the-metadata-id");
    jsonMeta.put("customKey1", "customValue1");
    jsonMeta.put("customKey2", "customValue2");
}
    catch(JSONException e) {
    Log.e(Fidel.FIDEL_DEBUG_TAG, e.getLocalizedMessage());
}

Fidel.metaData = jsonMeta;
```

Present the activity:
```swift
Fidel.present(YourActivityClass.this);
```
<br/>

Get the resulting card object:
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
  super.onActivityResult(requestCode, resultCode, data);

  if(requestCode == Fidel.FIDEL_LINK_CARD_REQUEST_CODE) {
    if(data != null && data.hasExtra(Fidel.FIDEL_LINK_CARD_RESULT_CARD)) {
      LinkResult card = (LinkResult)data.getParcelableExtra(Fidel.FIDEL_LINK_CARD_RESULT_CARD);
      Log.d("d", "CARD ID = " + card.id);
    }
  }
}
```

For more detailed documentation about any class in the SDKs and available customisation options, check the [Android repository on Github](https://github.com/FidelLimited/fidel-android).
<br/>
