# Mobile SDKs

The native mobile SDKs allow you to integrate card linking technology in your mobile apps in minutes. Card details are collected securely, removing any PCI compliance requirements from your side. See below code examples on how to integrate the SDKs in your apps. 

For more detailed documentation about any class in the SDKs and available customisation options, check the Github repositories for [iOS](https://github.com/FidelLimited/fidel-ios) and [Android](https://github.com/FidelLimited/fidel-android).

## iOS

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

In order to allow scanning cards with the camera, make sure to add the key `NSCameraUsageDescription` to your app’s Info.plist and set the value to a string describing why your app needs to use the camera (e.g. “Used for scanning credit cards.”) 

### Objective-C (skip for Swift projects)

If you have an Objective-C project and did not add any Swift code yet, please set the `Always Embed Swift Standard Libraries` flag in Build Settings to `YES`.

### Troubleshooting

In case Cocoapods doesn't find the Fidel specs or it finds older specs, try updating with `pod update`. After updating, run `pod install`.

### Usage

Import the SDK:
```swift
import Fidel
```

Set your public SDK key (`pk_test` or `pk_live`) and the `programId` you want to link cards to:
```swift
Fidel.apiKey 	= "pk_test_6e94da6f-145a-47db-b56b-f1314e74aa2e"
Fidel.programId = "f8d6890e-145d-46ea-b66f-afacfd954580"
```

Set cardholder consent variables:
```swift
Fidel.companyName = "Your Company Name" // default: "Company Name".
Fidel.privacyURL = "https://yourcompany.com/privacyURL"
Fidel.deleteInstructions = "Your delete instructions" // Maximum 60 characters, default: "going to your account settings."
```

Set a default card country:
```swift
Fidel.country = .unitedKingdom // If set, country picker UI will not show
```

Present the SDK view:
```swift
Fidel.present(self, onCardLinkedCallback: { (card: LinkResult) in
  print(card.id)
}, onCardLinkFailedCallback: { (err: LinkError) in
  print(err.message)
})
```

For more detailed documentation about any class in the SDKs and available customisation options, check the [iOS repository on Github](https://github.com/FidelLimited/fidel-ios).

## Android

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

Add Fidel SDK as a dependency:
```sh
dependencies {
  implementation 'com.github.FidelLimited:android-sdk:1.3.1'
}
```

### Usage

Set your public SDK key (`pk_test` or `pk_live`) and the `programId` you want to link cards to:
```swift
Fidel.apiKey    = "pk_test_6e94da6f-145a-47db-b56b-f1314e74aa2e"
Fidel.programId = "f8d6890e-145d-46ea-b66f-afacfd954580"
```

Set cardholder consent variables:
```swift
Fidel.companyName = "Your Company Name" // default: "Company Name".
Fidel.privacyURL = "https://yourcompany.com/privacyURL"
Fidel.deleteInstructions = "Your delete instructions" // Maximum 60 characters, default: "going to your account settings."
```

Set a default card country:
```swift
Fidel.country = Fidel.Country.UNITED_KINGDOM; // If set, country picker UI will not show
```

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

## React Native

### Getting started

`$ npm install fidel-react-native --save`

### Project setup

#### iOS

##### Step 1: Add the Fidel React Native iOS project as a dependency

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`

2. Go to `node_modules` ➜ `fidel-react-native` and add `Fidel.xcodeproj`

3. In XCode, in the project navigator, select your project. Add `libFidel.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`

4. Under your target's `Build Settings`, make sure to set `YES` for `Always Embed Swift Standard Libraries`. That's because, by default, your project might not need it. It needs to be `YES` because otherwise the project will not find the Swift libraries to be able to run our native iOS SDK.

##### Step 2: Add the Native iOS SDK as a dependency

You can use Cocoapods or install the library as a dynamic library.

1. Add a `Podfile` in your `ios/` folder of your React Native project. It should include the following dependency:

```ruby
use_frameworks!
pod 'Fidel'
```

Here is the simplest `Podfile` you need to link with our native iOS SDK:

```ruby
platform :ios, '9.1'

workspace 'example'

target 'example' do
  use_frameworks!
  pod 'Fidel'
end
```

If you use an older Swift version, please check our [iOS SDK README](https://github.com/FidelLimited/fidel-ios#readme). You'll find a suitable version you should set for your Cocoapods dependency.

2. Run `pod install` in your terminal.

3. Make sure to use the new `.xcworkspace` created by Cocoapods when you run your iOS app. React Native should use it by default.

In order to allow scanning cards with the camera, make sure to add the key `NSCameraUsageDescription` to your iOS app `Info.plist` and set the value to a string describing why your app needs to use the camera (e.g. "To scan credit cards."). This string will be displayed when the app initially requests permission to access the camera.

### Android

1. Append the following lines to `android/settings.gradle`:

```java
include ':fidel-react-native'
project(':fidel-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/fidel-react-native/android')
```

2. Insert the following lines inside the dependencies block in `android/app/build.gradle`:

```java
implementation project(':fidel-react-native')
```

3. Open up `android/app/src/main/java/[...]/MainApplication.java`

- Add `import com.fidelreactlibrary.FidelPackage;` to the imports at the top of the file
- Add `new FidelPackage()` to the list returned by the `getPackages()` method:

```java
protected List <ReactPackage> getPackages() {
  return Arrays.<ReactPackage>asList(
    new MainReactPackage(),
    new FidelPackage(),
    // you might have other packages here as well.
  );
}
```

4. Append Jitpack to `android/build.gradle`:

```java
allprojects {
  repositories {
    ...
    maven { url "https://jitpack.io" }
  }
}
```

5. Make sure that the `minSdkVersion` is the same or higher than the `minSdkVersion` of our native Android SDK:

```java
buildscript {
  ext {
    ...
    minSdkVersion = 19
    ...
  }
}
```

6. Ensure that you have *Google Play Services* installed.

For a physical device you need to search on Google for *Google Play Services*. There will be a link that takes you to the Play Store and from there you will see a button to update it (*do not* search within the Play Store).

### Usage

Import Fidel in your RN project:

```javascript
import Fidel from 'fidel-react-native';
```

Set up Fidel with your API key and your `programId`.

```javascript
Fidel.setup({
  apiKey:'your api key',
  programId: 'your program id'
})
```

Set the options that create the experience of linking a card with Fidel:

```javascript
const myImage = require('./images/fdl_test_banner.png');
const resolveAssetSource = require('react-native/Libraries/Image/resolveAssetSource');
const resolvedImage = resolveAssetSource(myImage);

Fidel.setOptions ({
  bannerImage: resolvedImage,
  country: Fidel.Country.sweden,
  autoScan: false,
  metaData: {'meta-data-1': 'value1'}, //additional data to pass with the card
  companyName: 'My RN Company', //the company name displayed in our 
  deleteInstructions: 'My custom delete instructions!',
  privacyUrl: 'https://fidel.uk',
});
```

Open the card linking view by calling:

```javascript
Fidel.openForm((error, result) => {
  if (error) {
    console.error(error);
  } else {
    console.info(result);
  }
});
```

Both `result` and `error` are objects that look like in the following examples:

#### Result

```javascript
{
  accountId: "the-account-id",
  countryCode: "GBR",
  created: "2019-04-22T05:26:45.611Z",
  expDate: "2023-12-31T23:59:59.999Z",
  expMonth: 12,
  expYear: 2023,
  id: "card-id",
  lastNumbers: "4001",
  live: false,
  mapped: false,
  metaData: {key1: "value1"},
  programId: "your program ID, as specified for the Fidel SDK",
  scheme: "visa",
  type: "visa",
  updated: "2019-04-22T05:26:45.611Z"
}
```

#### Error

```javascript
{
  code: "item-save",
  date: "2019-04-22T05:34:04.621Z",
  message: "Item already exists"
}
```

### SDK options

#### bannerImage
Use this option to customize the topmost banner image with the Fidel UI. Your custom asset needs to be resolved in order to be passed to our native module:

```javascript
const myImage = require('./images/fdl_test_banner.png');
const resolveAssetSource = require('react-native/Libraries/Image/resolveAssetSource');
const resolvedImage = resolveAssetSource(myImage);
Fidel.setOptions({
  bannerImage: resolvedImage
})
```

#### country

Set a default country the SDK should use with:

```javascript
Fidel.setOptions({
  country: Fidel.Country.unitedKingdom
});
```

When you set a default country, the card linking screen will not show the country picker UI. The other options, for now, are: `.unitedStates`, `.ireland`, `.sweden`, `.japan`, `.canada`.

#### supportedCardSchemes

We currently support _Visa_, _Mastercard_ and _AmericanExpress_. You can choose to support only one, two or all three. By default the SDK is configured to support all three. (Geographical limitations exist, please [ask on Slack](https://fidel.uk/join-us-on-slack) or [via email](mailto:support@fidel.uk) if you have questions). 

If you set this option to an empty array or to `null`, of course, you will not be able to open the Fidel UI. You must support at least one of our supported card schemes.

Please check the example below:

```javascript
/* this is the default value for supported card schemes,
 * but you can remove the support for some of the card schemes if you want to */
const cardSchemes = new Set([
  Fidel.CardScheme.visa,
  Fidel.CardScheme.mastercard,
  Fidel.CardScheme.americanExpress
]);

Fidel.setOptions({
  supportedCardSchemes: Array.from(cardSchemes)
});
```

#### autoScan

Set this property to `true`, if you want to open the card scanning UI immediately after executing `Fidel.openForm`. The default value is `false`.

```javascript
Fidel.setOptions({
  autoScan: true
});
```

#### metaData

Use this option to pass any other data with the card data:

```javascript
Fidel.setOptions({
  metaData: {'meta-data-key-1': 'value1'}
});
```

#### companyName

Set your company name as it will appear in our consent checkbox text. Please set it to a maximum of 60 characters.

```javascript
Fidel.setOptions({
  companyName: 'Your Company Name'
});
```

#### deleteInstructions

Write your custom opt-out instructions for your users. They will be displayed in the consent checkbox text as well.

```javascript
Fidel.setOptions({
  deleteInstructions: 'by contacting our support team at support@example.com'
});
```

#### privacyUrl

This is the privacy policy URL that you can set for the consent checkbox text.

```javascript
Fidel.setOptions({
  privacyUrl: 'https://fidel.uk',
});
```

#### Test card numbers

In the test environment, you can use the following Visa, Mastercard or American Express test card numbers. You must use a test API Key for them to work.

Visa: _4444000000004***_ (the last 3 numbers can be anything)

Mastercard: _5555000000005***_ (the last 3 numbers can be anything)

American Express: _3400000000003**_ or _3700000000003**_ (the last 2 numbers can be anything)

### Feedback

The React Native SDK is in active development, we welcome your feedback!

[GitHub Issues](https://github.com/fidellimited/rn-sdk/issues) — For SDK issues and feedback  
[Developers Slack Channel](https://fidel-developers-slack-invites.herokuapp.com) — for personal & community support at any phase of integration.
