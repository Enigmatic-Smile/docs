# Mobile SDKs

The Transaction Stream API is not supported by the Cordova SDK.

This document covers the basics of integrating, configuring, and using the mobile SDKs. 

## Using the iOS SDK

### Integrating the iOS SDK into your project

You’ll need to use [CocoaPods](https://cocoapods.org/) to integrate the SDK in your project. If you don’t already use CocoaPods for your project, check the basics of [CocoaPods](https://cocoapods.org/) and run `pod init` to create a Podfile.

To integrate the iOS mobile SDK in your project, execute the following steps:

1. Add the SDK from the `em` branch, by using the following statements:

```ruby
target 'YouriOSTarget' do
use_frameworks!
pod 'Fidel', :git => 'https://github.com/FidelLimited/fidel-ios', :branch => 'em'
# ... other pods, if you're using Cocoapods already
End
```

2. Run `pod install`.

### Updating the iOS SDK

In order to update just run the following command from your project’s folder: `pod update Fidel`

### Configuring the iOS SDK

**Important note: Handling incomplete card verifications**  
If the user submits their card number but does not complete the verification journey in the same session (closes the app), your app will have to relaunch the card verification flow when the application becomes active again. To make sure the user completes card verification you must configure the SDK as soon as the app launches, i.e. in the `application(_:didFinishLaunchingWithOptions:)` function of the `AppDelegate` object.

To configure the iOS mobile SDK in your project, please take the following steps:

1. Set the mandatory properties:
- `sdkKey`
- `programID`
- `termsAndConditionsURL`
- `companyName`
2. Set the `programType` property to `.transactionStream`:  
`Fidel.programType = .transactionStream`
3. Set the following properties to customize the card linking experience. For more information on these properties, see the detailed [SDK documentation](https://github.com/FidelLimited/fidel-ios).

- `allowedCountries`
- `privacyPolicyURL`. This property is not mandatory, but it is a great way to gain the user’s trust.
- `supportedCardSchemes`
- `deleteInstructions`. Text to inform the user about how to opt out of the transaction monitoring. It is appended to the consent text, after the phrase "You may opt out of transaction monitoring on the payment card you entered at any time by". The default value is: “going to your account settings.”
- `bannerImage`
- `metaData`

4. If there is the possibility for a cardholder to not have access to the card statement, make sure to customize the experience by using the `Fidel.thirdPartyVerificationChoice` property. The default value is `false`. If set to `true`, it will provide the choice for the cardholder to either verify the card on the spot (as available, when this property remains set to `false`) or indicate that the cardholder does not have access to the card statement and needs to delegate card verification to a third-party entity.
5. If necessary, as an alternative to the most recommended `card.verification.started` wehbook, use the `Fidel.onCardVerificationStarted` callback to get the details of the consent created in order to start the card verification process. Can be useful to attempt a card verification, as indicated in the **Verifying a card in iOS** section of this documentation.
6. _(Experimental feature)_ If you are setting the `Fidel.thirdPartyVerificationChoice` property to `true` and you would like to know the verification method choice that the cardholder makes, listen for the choice using the `Fidel.onCardVerificationChoiceSelected` callback.
7. `enableCardScanner` set to true if you want to enable the card scanning feature.

#### Example iOS SDK Configuration

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
        Fidel.sdkKey = "Your Fidel SDK Key"
        Fidel.programID = "Your program ID"
        Fidel.programType = .transactionStream
        Fidel.bannerImage = UIImage(named: "fdl_test_banner")
        Fidel.shouldAutoScanCard = false
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
        Fidel.privacyPolicyURL = "https://www.fidel.uk/"
        Fidel.termsAndConditionsURL = "https://fidel.uk/docs/"
        Fidel.deleteInstructions = "contacting our support team"
        Fidel.supportedCardSchemes = [.visa, .mastercard, .americanExpress]
        Fidel.thirdPartyVerificationChoice = true
        Fidel.onResult = self.onResult
        Fidel.onCardVerificationStarted = {
            print("Card verification started: \($0.consentID)")
        }

        // !Experimental feature!
        Fidel.onCardVerificationChoiceSelected = {
            switch ($0) {
                case .onTheSpot:
                print("Cardholder chose to verify the card on the spot")
                case .delegatedToThirdParty:
                print("Cardholder expressed the choice to delegate card verification to a third-party entity")
                @unknown default:
                print("Cardholder expressed an unknown choice")
            }
        }
    }

    func onResult(_ result: FidelResult) {
        switch result {
            case .enrollmentResult(let enrollmentResult):
            print(enrollmentResult.cardID)
            case .verificationResult(let verificationResult):
            print(verificationResult.cardID)
            case .error(let fidelError):
            switch fidelError.type {
                case .enrollmentError(let enrollmentError):
                switch enrollmentError {
                    case .cardAlreadyExists:
                    print("card was already enrolled")
                    default:
                    print("other enrollment error")
                }
                case .verificationError(let verificationError):
                switch verificationError {
                    case .incorrectAmount:
                    print("network connection error")
                    default:
                    print("other verification error")
                }
                case .sdkConfigurationError:
                print("the SDK should be provided with all the information")
                case .userCanceled:
                print("the user cancelled the flow")
                case .deviceNotSecure:
                print(“the device you’re using is not secure”)
                @unknown default:
                print("unknown error")
            }
            @unknown default:
            print("unknown result")
        }
    }
}
```

### Starting the card-linking flow

On any action that you want (on tap/click of a button, for example), add the following code:

```swift
Fidel.start(from: yourViewController)
```

### Verifying a card in iOS

Verifying a card can happen on the spot, when using the `Fidel.start` function as indicated above. Another possibility is to verify a card later (you could also involve a third-party entity) by using the following function:

```swift
let cardVerificationConfiguration = CardVerificationConfiguration(
cardID: "the id of the card you want to attempt to verify",
consentID: "the consent ID created when card verification started",
last4Digits: "(optional) the last 4 digits of the card to be verified"
)
Fidel.verifyCard(from: self, cardVerificationConfiguration: cardVerificationConfiguration)
```

The `from` parameter is the view controller that the card verification screen will be presented from.

The `CardVerificationConfiguration` needs to have the following details:

1. the `cardID`, which is the ID of a card that was previously enrolled in a Fidel API program. It can be obtained by using either:
1. (Recommended)  The `card.linked` webhook, as indicated [on our Webhooks page](https://transaction-stream.fidel.uk/docs/webhooks).
2. The `Fidel.onResult` callback, under an `.enrollmentResult`
2. the `consentID`, which is the ID of the consent created by the SDK, in order to start card verification. It can be obtained by using either:
1. (Recommended) The `card.verification.started` webhook, as indicated [on our Webhooks page](https://transaction-stream.fidel.uk/docs/webhooks).
2. The `Fidel.onCardVerificationStarted` callback, as documented above.
3. the `last4Digits` of the card to be verified. This property is _optional_. It has a pure aesthetic and user experience improvement role. If set, the verification screen will display them to visually help the the card verifier identify which card is being verified. If not set, the verification screen will be more generic.

## Using the Android SDK

### Integrating the Android SDK into your project

You’ll need to use JitPack in order to integrate the SDK into your Android project.

Execute the following steps:

1. Add the JitPack repository to your build file:

```groovy
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

2. Add the Fidel SDK dependency:

```groovy
dependencies {
    implementation 'com.github.FidelLimited:android-sdk:2.0.0-beta16'
}
```

### Configuring the Android SDK

**Important note: Handling incomplete card verifications**  
If the user submits their card number, but does not complete the verification journey in the same session (closes the app), your app will have to relaunch the card verification flow when the application becomes active again. To make sure the user completes card verification you must configure the SDK  in the main activity’s `onCreate(Bundle savedInstanceState)` function.

Follow these steps in the main activity’s `onCreate(Bundle savedInstanceState)` function to configure the SDK:

1. Set the mandatory properties:
- `sdkKey`
- `programID`
- `termsAndConditionsURL`
- `companyName`
2. Set the `programType` property to `Fidel.ProgramType.TRANSACTION_STREAM`:  
`Fidel.programType = Fidel.ProgramType.TRANSACTION_STREAM`
3. Set the following properties to customize the card linking experience. For more information on these properties, see the detailed [SDK documentation](https://github.com/FidelLimited/fidel-android):
- `allowedCountries`
- `privacyPolicyURL`. This property is not mandatory, but it is a great way to gain the user’s trust.
- `supportedCardSchemes`
- `deleteInstructions`. Text to inform the user about how to opt out of the transaction monitoring. It is appended to the consent text, after the phrase "You may opt out of transaction monitoring on the payment card you entered at any time by". The default value is: “going to your account settings.”
- `bannerImage`
- `metaData`
- `enableCardScanner` set to true if you want to enable the card scanning feature.
4. Important note: To handle incomplete card verifications, add the following line in the `onCreate` function, after you finished all the configuration steps:  
`Fidel.getInstance().onMainActivityCreate(this)`
5. If there is the possibility for a cardholder to not have access to the card statement, make sure to customize the experience by using the `Fidel.thirdPartyVerificationChoice` property. The default value is `false`. If set to `true`, it will provide the choice for the cardholder to either verify the card on the spot (as available, when this property remains set to `false`) or indicate that the cardholder does not have access to the card statement and needs to delegate card verification to a third-party entity.
6. If necessary, as an alternative to the most recommended `card.verification.started` wehbook, use the `Fidel.onCardVerificationStarted` callback to get the details of the consent created in order to start the card verification process. Can be useful to attempt a card verification, as indicated in the **Verifying a card in Android** section of this documentation.
7. _(Experimental feature)_ If you are setting the `Fidel.thirdPartyVerificationChoice` property to `true` and you would like to know the verification method choice that the cardholder makes, listen for the choice using the `Fidel.onCardVerificationChoiceSelected` callback.

#### Example Android SDK configuration

The following example is in Kotlin, but Java works as well:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        Fidel.programId = "Your Program ID"
        Fidel.sdkKey = "Your SDK key"
        Fidel.shouldAutoScanCard = false
        Fidel.supportedCardSchemes = setOf(CardScheme.VISA, CardScheme.MASTERCARD, CardScheme.AMERICAN_EXPRESS)
        Fidel.companyName = "[Developer company]"
        Fidel.termsAndConditionsUrl = "https://fidel.uk"
        Fidel.privacyPolicyUrl = "https://fidel.uk"
        Fidel.deleteInstructions = "[Developer set delete instructions]"
        Fidel.thirdPartyVerificationChoice = true
        val jsonMeta = JSONObject()
        try {
            jsonMeta.put("id", "this-is-the-metadata-id")
            jsonMeta.put("customKey1", "customValue1")
            jsonMeta.put("customKey2", "customValue2")
        } catch (e: JSONException) {
            val exceptionMessage = e.localizedMessage
            if (exceptionMessage != null) {
                Log.e(FIDEL_DEBUG_TAG, exceptionMessage)
            }
        }

        Fidel.metaData = jsonMeta
        Fidel.bannerImage = ContextCompat.getDrawable(this, R.drawable.fidel_test_banner)?.toBitmap()

        val btn = findViewById<View>(R.id.btn_show) as Button
        btn.setOnClickListener { Fidel.start(this@MainActivity) }
        Fidel.onResult = OnResultObserver { result ->
            when (result) {
                is FidelResult.Enrollment -> {
                    Log.d(FIDEL_DEBUG_TAG, "The card ID = " + result.enrollmentResult.cardId)
                }
                is FidelResult.Verification -> {
                    Log.d(FIDEL_DEBUG_TAG, "The card ID = " + result.verificationResult.cardId)
                }
                is FidelResult.Error -> {
                    Log.e(FIDEL_DEBUG_TAG, "Error message = " + result.error.message)
                }
            }
        }
        Fidel.onCardVerificationStarted = OnCardVerificationStartedObserver { consentDetails ->
            Log.d(FIDEL_DEBUG_TAG, "The consent ID = " + consentDetails.consentId)
        }
        Fidel.onCardVerificationChoiceSelected = OnCardVerificationChoiceSelectedObserver { choice ->
            Log.d(FIDEL_DEBUG_TAG, "The choice expressed by the cardholder: $choice")
        }

        Fidel.onMainActivityCreate(this)
    }

    private companion object {
        private const val FIDEL_DEBUG_TAG = "fidel.debug"
    }
}
```

### Starting the card-linking flow

On any action that you want (on tap/click of a button, for example), add the following code:  
`Fidel.start(this@YourActivity)`

To handle results during the card connection process, add an observer:

```kotlin
Fidel.onResult = OnResultObserver { result ->
    when (result) {
        is FidelResult.Enrollment -> {
            Log.d(FIDEL_DEBUG_TAG, "The card ID = " + result.enrollmentResult.cardId)
        }
        is FidelResult.Verification -> {
            Log.d(FIDEL_DEBUG_TAG, "The card ID = " + result.verificationResult.cardId)
        }
        is FidelResult.Error -> {
            Log.e(FIDEL_DEBUG_TAG, "Error message = " + result.error.message)
        }
    }
}
```

### Verifying a card in Android

Verifying a card can happen on the spot, when using the `Fidel.start` function as indicated above. Another possibility is to verify a card later (you could also involve a third-party entity) by using the following function:

```kotlin
val cardVerificationConfiguration = CardVerificationConfiguration(
id = "the id of the card you want to attempt to verify",
consentId = "the consent ID created when card verification started",
last4Digits = "(optional) the last 4 digits of the card to be verified"
)
Fidel.verifyCard(context, cardVerificationConfiguration)
```

The `context` parameter is the context used to present the card verification screen.

The `CardVerificationConfiguration` needs to have the following details:

1. the `id`, which is the ID of the card that was previously enrolled in a Fidel API program. It can be obtained by using either:
1. (Recommended)  The `card.linked` webhook, as indicated [on our Webhooks page](https://transaction-stream.fidel.uk/docs/webhooks).
2. The `Fidel.onResult` callback, under an `.enrollmentResult`
2. the `consentId`, which is the ID of the consent created by the SDK, in order to start card verification. It can be obtained by using either:
1. (Recommended) The `card.verification.started` webhook, as indicated [on our Webhooks page](https://transaction-stream.fidel.uk/docs/webhooks).
2. The `Fidel.onCardVerificationStarted` callback, as documented above.
3. the `last4Digits` of the card to be verified. This property is _optional_. It has a pure aesthetic and user experience improvement role. If set, the verification screen will display them to visually help the the card verifier identify which card is being verified. If not set, the verification screen will be more generic.

## Integrating the SDK - React Native

### Installation

1. Add the Fidel React native library from the `em` branch:  
`npm install https://github.com/FidelLimited/rn-sdk#em --save`  
or, using yarn:  
`yarn add https://github.com/FidelLimited/rn-sdk#em`.
2. In your iOS project’s Podfile, add the following line before the post_install part of the script:

```ruby
pod 'Fidel', :git => 'https://github.com/FidelLimited/fidel-ios', :branch => 'em'
```

3. In your `ios` folder, run the following command to install the `Fidel` dependency:  
`pod install`.

### Configuration

**Important note: Handling incomplete card verifications**  
If the user submits their card number but does not complete the verification journey in the same session (closes the app), your app will have to relaunch the card verification flow when the application becomes active again. To make sure the user completes card verification, the configuration must be executed as soon as the app launches, i.e. in the App.js.

To configure the SDK in your project, execute the following steps in the App.js:

1. Set the mandatory properties:
- `sdkKey`
- `programID`
- `consentText.termsAndConditionsUrl`
- `consentText.companyName`
2. Set the `programType` property to `Fidel.ProgramType.transactionStream`.
3. Set the following properties to customize the card linking experience. For more information on these properties, see the detailed [SDK documentation](https://github.com/FidelLimited/fidel-android):
- `allowedCountries`
- `privacyPolicyURL`. This property is not mandatory, but it is a great way to gain the user’s trust.
- `supportedCardSchemes`
- `deleteInstructions`. Text to inform the user about how to opt out of the transaction monitoring. It is appended to the consent text, after the phrase "You may opt out of transaction monitoring on the payment card you entered at any time by". The default value is: “going to your account settings.”
- `bannerImage`
- `metaData`
- `enableCardScanner`. Set to `true` if you want to enable the card scanning feature.
4. If there is the possibility for a cardholder to not have access to the card statement, make sure to customize the experience by using the `options.thirdPartyVerificationChoice` property, when setting up the SDK using the `Fidel.setup` function. The default value is `false`. If set to `true`, it will provide the choice for the cardholder to either verify the card on the spot (as available, when this property remains set to `false`) or indicate that the cardholder does not have access to the card statement and needs to delegate card verification to a third-party entity.
5. If necessary, as an alternative to the most recommended `card.verification.started` wehbook, set the `onCardVerificationStarted` callback property, of the object given as a parameter to the `Fidel.setup` function. It will give you the details of the consent created in order to start the card verification process. Can be useful to attempt a card verification, as indicated in the **Verifying a card in React Native** section of this documentation.
6. (Experimental feature) If you are setting the `options.Fidel.thirdPartyVerificationChoice` property to true and you would like to know the verification method choice that the cardholder makes, listen for the choice using the `options.onCardVerificationChoiceSelected` callback.

#### Example React Native SDK configuration

```javascript
export default class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isShown: true,
        };
        this.configureFidel();
    }

    configureFidel() {
        const myImage = require('./demo_images/fdl_test_banner.png');
        const resolveAssetSource = require('react-native/Libraries/Image/resolveAssetSource');
        const resolvedImage = resolveAssetSource(myImage);

        //this is the default value for supported card schemes,
        //but you can remove the support for some of the card schemes if you need to
        const cardSchemes = [
        Fidel.CardScheme.visa,
        Fidel.CardScheme.mastercard,
        Fidel.CardScheme.americanExpress,
        ];

        const countries = [
        Fidel.Country.unitedArabEmirates,
        Fidel.Country.unitedKingdom,
        Fidel.Country.unitedStates,
        Fidel.Country.japan,
        Fidel.Country.sweden,
        Fidel.Country.ireland,
        Fidel.Country.canada,
        ];

        Fidel.setup ({
            sdkKey: 'Your SDK Key', // mandatory
            programId: 'Your program ID', // mandatory
            programType: Fidel.ProgramType.transactionStream,
            options: {
                bannerImage: resolvedImage,
                allowedCountries: countries,
                supportedCardSchemes: cardSchemes,
                shouldAutoScanCard: false,
                enableCardScanner: false,
                metaData: { userId: 1234 },
                thirdPartyVerificationChoice: false //set to true if you need to enable third party verification
            },
            consentText: {
                termsAndConditionsUrl: 'https://fidel.uk', // mandatory
                companyName: 'Your Company Name', // mandatory
                privacyPolicyUrl: 'https://fidel.uk',
                programName: 'Your program name',
                deleteInstructions: "following our delete instructions",
            },
            onCardVerificationStarted: consentDetails => {
                console.log('card verification started: ' + JSON.stringify(consentDetails));
            },
            onCardVerificationChoiceSelected: verificationChoice => {
                switch (verificationChoice.CardVerificationChoice) {
                    case Fidel.CardVerificationChoice.onTheSpot:
                    console.log('card verification choice: on the spot');
                    break;
                    case Fidel.CardVerificationChoice.delegatedToThirdParty:
                    console.log('card verification choice: delegated to third party');
                    break;
                }
            },
        }, (result) => {
            switch (result.type) {
                case ENROLLMENT_RESULT:
                console.log("card was enrolled: " + result.enrollmentResult.cardId);
                break;
                case ERROR:
                this.handleError(result.error);
                break;
                case VERIFICATION_RESULT:
                console.log('card verification was successful 🎉: ' + result.verificationResult.cardId);
                break;
            }
        });
    }

    onButtonPress = () => {
        Fidel.start();
    }

    handleError = (error) => {
        console.log("Error message: " + error.message);
        switch (error.type) {
            case Fidel.ErrorType.userCanceled:
            console.log("User canceled the process");
            break;
            case Fidel.ErrorType.sdkConfigurationError:
            console.log("Please configure the Fidel SDK correctly");
            break;
            case Fidel.ErrorType.enrollmentError:
            this.handleEnrollmentError(error);
            break;
            case Fidel.ErrorType.verificationError:
            this.handleVerificationError(error);
            break;
        }
    }

    handleEnrollmentError = (enrollmentError) => {
        switch (enrollmentError.subtype) {
            case Fidel.EnrollmentErrorType.cardAlreadyExists:
            console.log("This card was already enrolled.");
            break;
            case Fidel.EnrollmentErrorType.invalidProgramId:
            console.log("Please configure Fidel with a valid program ID.");
            break;
            case Fidel.EnrollmentErrorType.invalidSdkKey:
            console.log("Please configure Fidel with a valid SDK Key.");
            break;
            case Fidel.EnrollmentErrorType.inexistentProgram:
            console.log("Please configure Fidel with a valid program ID.");
            break;
            case Fidel.EnrollmentErrorType.unexpected:
            console.log("Unexpected enrollment error occurred.");
            break;
        }
    }

    handleVerificationError = (verificationError) => {
        switch (verificationError.subtype) {
            case Fidel.VerificationErrorType.unauthorized:
            console.log("You are not authorized to do card verification.");
            break;
            case Fidel.VerificationErrorType.incorrectAmount:
            console.log("The card verification amount entered is not correct.");
            break;
            case Fidel.VerificationErrorType.maximumAttemptsReached:
            console.log("You have reached the maximum attempts allowed to verify this card.");
            break;
            case Fidel.VerificationErrorType.cardAlreadyVerified:
            console.log("This card was already verified.");
            break;
            case Fidel.VerificationErrorType.cardNotFound:
            console.log("This card is not found.");
            break;
            case Fidel.VerificationErrorType.verificationNotFound:
            console.log("Verification not found.");
            break;
            case Fidel.VerificationErrorType.genericError:
            console.log("Generic error.");
            break;
            case Fidel.VerificationErrorType.unexpected:
            console.log("Unexpected card verification error occurred.");
            break;
        }
    }

    render() {
        return (
        <View style={styles.container}>
            <Text style={styles.welcome}>Fidel React Native SDK example</Text>
            <Text style={styles.instructions}>To get started, tap the button below.</Text>
            <Button
            onPress={this.onButtonPress}
            title="Link a card"
            color="#3846ce"
            />
        </View>
        );
    }
};
```

### Starting the card verification flow

```javascript
Fidel.start();
```

### Verifying a card in React Native

Verifying a card can happen on the spot, when using the `Fidel.start` function as indicated above. Another possibility is to verify a card later (you could also involve a third-party entity) by using the following function:

```javascript Card Verification in React Native
Fidel.verifyCard({
    id: 'Your card ID to verify',
    consentId: 'Your consent ID to verify',
    last4Digits: '1234', // (Optional) The last 4 digits of the card to verify (used only for display purposes)
});
```

The card verification configuration object given as a parameter needs to have the following details:

1. the `id`, which is the ID of the card that was previously enrolled in a Fidel API program. It can be obtained by using either:
1. (Recommended)  The `card.linked` webhook, as indicated [on our Webhooks page](https://transaction-stream.fidel.uk/docs/webhooks).
2. The last `Fidel.setup` function callback, under an `ENROLLMENT_RESULT`.
2. the `consentId`, which is the ID of the consent created by the SDK, in order to start card verification. It can be obtained by using either:
1. (Recommended) The `card.verification.started` webhook, as indicated [on our Webhooks page](https://transaction-stream.fidel.uk/docs/webhooks).
2. The `onCardVerificationStarted` callback, as documented above.
3. the `last4Digits` of the card to be verified. This property is _optional_. It has a pure aesthetic and user experience improvement role. If set, the verification screen will display them to visually help the the card verifier identify which card is being verified. If not set, the verification screen will be more generic.

### Handling results

Add a callback, as demonstrated in the example code above, as a parameter of the setup function.

### Upgrading to the latest version

At the root of your project, run `yarn upgrade fidel-react-native` or ` npm update -g fidel-react-native`.

In your `ios` folder, run the following command to update the `Fidel` dependency:  
`pod update Fidel`.