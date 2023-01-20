# Mobile SDKs

The native mobile SDKs allow you to integrate card linking technology in your mobile apps in minutes. Card details are collected securely, removing any PCI compliance requirements from your side. See below code examples on how to integrate the SDKs in your apps.

For more detailed documentation about any class in the SDKs and available customization options, check the Github repositories for [iOS](https://github.com/FidelLimited/fidel-ios) and [Android](https://github.com/FidelLimited/fidel-android).

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

Set a default card country (if set, country picker will be hidden):
```swift
Fidel.country = .unitedKingdom
```

Present the SDK view:
```swift
Fidel.present(self, onCardLinkedCallback: { (card: LinkResult) in
  print(card.id)
}, onCardLinkFailedCallback: { (err: LinkError) in
  print(err.message)
})
```

If you run into any issues, [check issues on Github](https://github.com/FidelLimited/fidel-ios).

## Android

<img
  src="https://docs.fidel.uk/assets/images/androidsdk.png"
  srcset="https://docs.fidel.uk/assets/images/androidsdk.png, https://docs.fidel.uk/assets/images/androidsdk@2x.png 2x"
  alt="Preview of the Android SDK"
/>

### Install

Add [jitpack.io](https://www.jitpack.io) to your root `build.gradle` at the end of repositories:
```java
allprojects {
  repositories {
    ...
    maven { url 'https://jitpack.io' }
  }
}
```

Add Fidel SDK as a dependency:
```java
dependencies {
  implementation 'com.github.FidelLimited:android-sdk:1.3.1'
}
```

### Usage

Set your public SDK key (`pk_test` or `pk_live`) and the `programId` you want to link cards to:
```swift
Fidel.apiKey = "pk_test_6e94da6f-145a-47db-b56b-f1314e74aa2e"
Fidel.programId = "f8d6890e-145d-46ea-b66f-afacfd954580"
```

Set cardholder consent variables:
```swift
Fidel.companyName = "Your Company Name" // default: "Company Name".
Fidel.privacyURL = "https://yourcompany.com/privacyURL"
Fidel.deleteInstructions = "Your delete instructions" // Maximum 60 characters, default: "going to your account settings."
```

Set a default card country (if set, country picker will be hidden):
```swift
Fidel.country = Fidel.Country.UNITED_KINGDOM;
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

If you run into any issues, [check issues on Github](https://github.com/FidelLimited/fidel-android).

## React Native

### Getting started

`$ npm install fidel-react-native --save`

### (Almost) Automatic linking (for RN >= 0.60)

`$ react-native link fidel-react-native`

#### iOS

1. Go to the `ios` folder. Please check that in your `Podfile` you have the following line:  
`pod 'fidel-react-native', :path => '../node_modules/fidel-react-native'`
2. Make sure that the minimum platform is set to 9.1 in your Podfile: `platform :ios, '9.1'`.
3. Run `pod install`.

#### Android

1. Append Jitpack to `android/build.gradle`:
  ```java
  allprojects {
    repositories {
      ...
      maven { url "https://jitpack.io" }
    }
  }
  ```
2. Make sure that the `minSdkVersion` is the same or higher than the `minSdkVersion` of our native Android SDK:
  ```java
  buildscript {
    ext {
      ...
      minSdkVersion = 19
      ...
    }
  }
  ```

---

### Manual linking on iOS

#### Step 1: Add the Fidel React Native iOS project as a dependency

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `fidel-react-native` and add `Fidel.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libFidel.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Under your target's `Build Settings`, make sure to set `YES` for `Always Embed Swift Standard Libraries`. That's because, by default, your project might not need it. It needs to be `YES` because otherwise the project will not find the Swift libraries to be able to run our native iOS SDK.

#### Step 2: Add the Native iOS SDK as a dependency

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
    If you use an older Swift version, you should install a different `Fidel` pod version. If your project uses Swift `4.2.1`, for example, your Podfile should include `pod Fidel, '~>1.4`, *not* `pod 'Fidel'` (which installs the latest version of the latest Swift supported version). Please check our [iOS SDK docs](/mobile-sdks/ios). You’ll find a suitable version you should set for our Fidel iOS SDK.
2. Run `pod install` in your terminal.
3. Make sure to use the new `.xcworkspace` created by Cocoapods when you run your iOS app. React Native should use it by default.

In order to allow scanning cards with the camera, make sure to add the key `NSCameraUsageDescription` to your iOS app `Info.plist` and set the value to a string describing why your app needs to use the camera (e.g. "To scan credit cards."). This string will be displayed when the app initially requests permission to access the camera.

### Manual linking on Android

1. Append the following lines to `android/settings.gradle`:
    ```java
    include ':fidel-react-native'
    project(':fidel-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/fidel-react-native/android')
    ```
2. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
    ```java
    implementation project(':fidel-react-native')
    ```
3. Append Jitpack to `android/build.gradle`:
    ```java
    allprojects {
      repositories {
        ...
        maven { url "https://jitpack.io" }
      }
    }
    ```
4. Make sure that the `minSdkVersion` is the same or higher than the `minSdkVersion` of our native Android SDK:
    ```java
    buildscript {
      ext {
        ...
        minSdkVersion = 19
        ...
      }
    }
    ```
5. Only for projects initialized with **RN <= 0.59**:
    Open up `android/app/src/main/java/[...]/MainApplication.java`  
    Add `import com.fidelreactlibrary.FidelPackage;` to the imports at the top of the file  
    Add `new FidelPackage()` to the list returned by the `getPackages()` method:  
    ```java
    protected List <ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new MainReactPackage(),
        new FidelPackage(),
        // you might have other Packages here as well.
      );
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
});
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
  metaData: { 'id': 'yourCustomId', 'someOtherKey': 'someOtherValue' },
  companyName: 'My RN Company',
  deleteInstructions: 'My custom delete instructions',
  privacyUrl: 'https://example.com/privacy',
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

Error and result look like the following:

##### Result

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

##### Error

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
  bannerImage: resolvedImage,
});
```

#### country

Set a default country the SDK should use with:

```javascript
Fidel.setOptions({
  country: Fidel.Country.unitedKingdom,
});
```

When you set a default country, the card linking screen will not show the country picker UI. The other options, for now, are: `unitedStates`, `ireland`, `sweden`, `japan`, `canada`.

#### supportedCardSchemes

We currently support _Visa_, _Mastercard_ and _AmericanExpress_. You can choose to support only one, two or all three. By default the SDK is configured to support all three. (Geographical limitations exist, please [see available countries](https://fidel.uk/products) or [via email](mailto:devrel@fidel.uk) if you have questions).

If you set this option to an empty array or to `null`, of course, you will not be able to open the Fidel UI. You must support at least one of our supported card schemes.

```javascript
/* this is the default value for supported card schemes,
 * but you can remove the support for some of the card schemes if you want to */
const supportedCardSchemes = [
  Fidel.CardScheme.visa,
  Fidel.CardScheme.mastercard,
  Fidel.CardScheme.americanExpress,
];

Fidel.setOptions({ supportedCardSchemes });
```

#### autoScan

Set this property to `true`, if you want to open the card scanning UI immediately after executing `Fidel.openForm`. The default value is `false`.

```javascript
Fidel.setOptions({
  autoScan: true,
});
```

#### metaData

Use this option to pass any other data with the card data:

```javascript
Fidel.setOptions({
  metaData: {
    id: 'your-custom-id',
    'meta-data-key-2': 'value2',
  },
});
```

#### companyName

Set your company name as it will appear in our consent checkbox text. Please set it to a maximum of 60 characters.

```javascript
Fidel.setOptions({
  companyName: 'Your Company Name',
});
```

#### deleteInstructions

Write your custom opt-out instructions for your users. They will be displayed in the consent checkbox text as well.

```javascript
Fidel.setOptions({
  deleteInstructions: 'by contacting our support team at support@example.com',
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

Visa: `4444000000004***` (the last 3 numbers can be anything)

Mastercard: `5555000000005***` (the last 3 numbers can be anything)

Amex: `3400000000003**` or `3700000000003**` (the last 2 numbers can be anything)

### Feedback

The React Native SDK is in active development, we welcome your feedback!

[GitHub Issues](https://github.com/fidellimited/rn-sdk/issues) — For SDK issues and feedback.

## Banner Images

The banner image will take the device's width, but it has a fixed height of 100 pts. The image view has an "Aspect Fill" content mode, which means that the banner image you set will fill its entire predefined area. The banner image will be cropped from the top and bottom sides on wider devices. This is because of the "Aspect Fill" content mode that we set for the image view. Depending on what you want to display in the banner image, you might need to experiment a bit to ensure that nothing important from the image is hidden. The most important information should be displayed in the centre of the banner image.

We suggest using the aspect ratio of the smallest devices that you support.  If you support iPhone 5, the aspect ratio would be 320:100. If the smallest device you support is the iPhone 6, your banner image's aspect ratio would be 375:100.

The same logic works for the Android banner image. You can make the image as pixel-dense as you need to

On iOS, you would need to provide the image for all screen densities. We recommend 375 x 130 for non-retina, 750 x 260 @2x and 1050 x 390 @3x.
