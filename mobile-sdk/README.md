## Mobile SDKs

The native mobile SDKs allow you to integrate card linkign technology in your mobile apps in minutes. Card details are collected securely, removing any PCI compliance requirements from your side. See below code examples on how to integrate the SDKs in your apps. 

For more detailed documentation about any class in the SDKs and available customisation options, check the Github repositories for [iOS](https://github.com/FidelLimited/fidel-ios) and [Android](https://github.com/FidelLimited/fidel-android).

# iOS

### Install
We recommend using CocoaPods to integrate Fidel iOS SDK with your project.
Add a `Podfile` (if you don't have one already), by running the following command: `pod init`.

Add Fidel pod (for Swift 4.2):

```sh
filename: Podfile
pod 'Fidel'
```

### Camera 
In order to allow scanning cards with the camera, make sure to add the key NSCameraUsageDescription to your app's Info.plist and set the value to a string describing why your app needs to use the camera (e.g. "To scan credit cards."). 

### Objective-C (skip for Swift projects)
If you have an Objective-C project and did not add any Swift code yet, please set the `Always Embed Swift Standard Libraries` flag in Build Settings to `YES`.

### Troubleshooting
In case Cocoapods doesn't find the Fidel specs or it finds older specs, try updating with `pod update`. After updating, run `pod install`.

<br/>

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
Fidel.privacyURL = "https://yourcompany.com/privacyURL" // Must be a valid URL.
Fidel.deleteInstructions = "Your delete instructions" // Maximum 60 characters, default: "going to your account settings."
```

Set a default card country:
```swift
Fidel.country = .unitedKingdom // If set, contry picker UI will not show
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

# Android
