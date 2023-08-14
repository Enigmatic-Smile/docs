# Guide to migrate to v2 of the iOS SDK for Loyalty applications

This is a guide to help you migrate from version 1.x.x of the Fidel API iOS SDK to version 2.0.0.

## Update your Fidel API iOS SDK integration

### If you used Cocoapods to integrate iOS SDK

Update to v2 of the iOS SDK by running the following Cocoapods command, from the root folder of your Xcode project.

```
pod update Fidel
```

You should see in the log the version to which the iOS SDK is updated to.

### If you manually integrated the iOS SDK

1. Download the latest `Fidel.xcframework` artefact from the [iOS SDK GitHub repo](https://github.com/FidelLimited/fidel-ios).
2. Replace your existing `Fidel.xcframework` with the one you downloaded.

## Check the following code as your migration guide

Use the following code as a guide related to the changes that to integrate with the Fidel API SDK:

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        configureFidel()
        return true
    }
    
    private func configureFidel() {
        // It is recommended to store the SDK key on your backend and retrieve it
        // before starting to use the SDK.
        Fidel.sdkKey = "Your Fidel SDK Key" //was apiKey
        Fidel.programID = "Your program ID" //was programId
        Fidel.bannerImage = UIImage(named: "fdl_test_banner")
        // Remove Fidel.autoScan
        Fidel.metaData = [
            "id": "this-is-the-metadata-id",
            "myUserId": 123,
            "myUserId2": 123.3,
            "myUserId3": [12, nil],
            "myUserId4": ["subkey": 12.2, "subkey2": true],
            "customKey1": "customValue1"
        ]
        Fidel.allowedCountries = [.canada, .unitedStates]
        Fidel.companyName = "Cashback Inc."
        Fidel.privacyPolicyURL = "https://www.fidel.uk/" // was privacyURL
        Fidel.termsAndConditionsURL = "https://fidel.uk/docs/" //was termsConditionsURL
        Fidel.deleteInstructions = "contacting our support team"
        Fidel.supportedCardSchemes = [.visa, .mastercard, .americanExpress]
        Fidel.onResult = self.onResult // result callback
    }
    
    func onResult(_ result: FidelResult) {
        switch result {
        case .enrollmentResult(let enrollmentResult):
            print(enrollmentResult.cardID)
        case .error(let fidelError):
            switch fidelError.type {
            case .enrollmentError(let enrollmentError):
                switch enrollmentError {
                case .cardAlreadyExists:
                    print("card was already enrolled")
                default:
                    print("other enrollment error")
                }
            case .sdkConfigurationError:
                print("the SDK should be provided with all the information")
            case .userCanceled:
                print("the user cancelled the flow")
        case .deviceNotSecure:
            print("the device you’re using is not secure")
            @unknown default:
                print("unknown error")
            }
        @unknown default:
            print("unknown result")
        }
    }
}
```

## Starting the card-linking flow

On any action that you want (on a tap of a button, for example), add the following code: 

```swift
Fidel.start(from: yourViewController)
```

The `Fidel.start` method replaces the previous: `Fidel.present` method. To get notified about card enrollment events, you ca use the `Fidel.onResult` callback. Please check our v2 Reference documentation for more details.