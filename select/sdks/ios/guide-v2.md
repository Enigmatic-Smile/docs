# A guide for card enrollment with the iOS SDK <a style="color: #111;" class="improve-docs" href="/select/sdks/ios/guide-v1">v1</a> <a style="margin-right: auto; border-bottom: 2px solid #0048ff;" class="improve-docs" href="/select/sdks/ios/guide-v2">v2</a>

Please take the following steps to integrate and configure the SDK for your Loyalty use case application.

> Note: If an example project helps with your SDK integration & configuration, please check our [GitHub repository](https://github.com/FidelLimited/fidel-ios/).

### 1. Set up your Fidel API account & your Transaction Select program

To get started, you'll need a Fidel API account. [Sign up](https://dashboard.fidel.uk/sign-up), if you didn't create an account yet.

If you didn't create a program for your application yet, please create a Transaction Select program from your Fidel API dashboard (or via the API).

### 2. Integrate the iOS SDK into your project

#### Using Cocoapods
> Info: If you prefer *not* to use Cocoapods, please check our instructions about integrating the SDK manually below.

- Install [CocoaPods](https://cocoapods.org/), if you haven't already.

- If your iOS project does not use Cocoapods (it does not contain a `Podfile` file), run the following command to initialize it:

```
pod init
```

- Add the following line in your `Podfile`, representing the Fidel API iOS SDK dependency that you're adding to your project:

```ruby
pod 'Fidel'
```

Your `Podfile` should have a structure similar to the following:
```ruby
fileName:Podfile
platform :ios, '9.1'

target 'YouriOSTarget' do
    use_frameworks!

    pod 'Fidel'
    # ... other pods, if you're using Cocoapods already
end
```

- Run the following command to install the Fidel API iOS SDK dependency:

```
pod install
```

- To start working with the Fidel API iOS SDK in your project, from now on, please don't forget to open your project's `.xcworkspace` file, not the `.xcodeproj` file. The `.xcworkspace` file is created by Cocoapods, when running the previous command.

- In the future, to update to the latest iOS SDK version, please run:

```
pod update Fidel
```

> Note: For information about all the Fidel API iOS SDK versions that are used by loyalty use case applications, please check our Releases page.

#### Manual framework integration

> Info: If you prefer to use Cocoapods, please check our instructions about [integrating the SDK using Cocoapods](#using-cocoapods).

- Download the latest version of the `Fidel.xcframework` artefact from the `master` branch of our [GitHub repo](https://github.com/FidelLimited/fidel-ios).

- Open your iOS app project in Xcode.

- Right click on your project and then on the `Add Files to "YourProjectName"...` button.

- Select the `Fidel.xcframework` artefact. Make sure to select the `Copy items if needed` option.

- In the future, to update to the latest version of our SDK, repeat the previous steps.

### 3. Import the SDK module

In the Swift class that you want to use the SDK, please import our SDK module:

```swift
import Fidel
```

### 4. Set your SDK Key

- Please [sign into](https://dashboard.fidel.uk/sign-in) your Fidel API dashboard account, if you didn't already.
- Click on your account name _(on the top-left hand side of the dashboard)_ -> then on `Account Settings`.
- Go to the `Plan` tab and copy your `Test` or `Live` SDK Key.
- Set your SDK Key in your app:

```swift
Fidel.sdkKey = "Your-SDK-Key"
```

### 5. Set your Program ID

- Please [sign into](https://dashboard.fidel.uk/sign-in) your Fidel API dashboard account, if you didn't already.
- Go to the `Programs` section of your Fidel API dashboard.
- Click on the `Program ID` of the program that you want to enroll cards into. The program ID will be copied to your pasteboard.
- Set your Program ID in your app:

```swift
Fidel.programID = "Your-Program-ID"
```

### 6. Configure the cardholder consent management feature

Asking for consent from the cardholder to enroll the card in your program is an important part of the SDK. The cardholder will need to read & agree with the conditions expressed using the consent language displayed during the card enrollment process. Making it clear for cardholders is essential.

#### Set your company name

```swift
Fidel.companyName = "Your Company Name"
```

#### To support US or Canada issued cards, please set your terms and conditions

You need to set your terms and conditions URL if you would like to:
a. support all the countries that Fidel API supports
b. set a specific `allowedCountries` set of countries AND include US or Canada in it.

```swift
Fidel.termsAndConditionsURL = "https://yourwebsite.com/terms"
```

#### Explain how the cardholder can opt out (optional, but recommended)

Please inform the cardholder about how to opt out of transaction monitoring in your program.

```swift
Fidel.deleteInstructions = "how can the cardholder opt out"
```

#### Set your privacy policy URL (optional, but recommended)

```swift
Fidel.privacyPolicyURL = "https://yourwebsite.com/privacy-policy"
```

# Enrollment notifications

In order to be notified about different, useful events (a card was linked, card failed to link etc.) that happen during a enrollment process, we recommend using our webhooks.

If client side notifications are useful for your application, make sure to check our SDK callback reference documentation.

# Enroll a card

Call the `Fidel.start` function to open the UI and start a card enrollment process:

```swift
Fidel.start(from: yourViewController)
```
You can test the card enrollment flow, by setting a test SDK Key and by using the Fidel API [test card numbers](/docs/select/cards#test-card-numbers).

If your Fidel API account is `live` then cardholders can also enroll real, live cards. Make sure that you set a live SDK Key, in order to allow live card enrollments.

# Optional: Set any of the other useful properties

Please check our SDK Reference for details about any other SDK properties that might be useful for your application.

# Frequently asked questions

### How can I upgrade the iOS SDK to the latest version?

If you integrated the Fidel API iOS SDK into your project using Cocoapods, please run:

`pod update Fidel`

from the folder where your `Podfile` is stored.

If you integrated the SDK manually, please repeat all the steps from the manual framework integration section.

### Can I customize the UI of the iOS SDK?

The iOS SDK offers the `Fidel.bannerImage` property for you to set a custom, branded banner image that will be displayed during the card enrollment process. Please check our Reference documentation for more details.

### How do I configure the consent text correctly?

In order to properly set the consent text, please follow these steps:

1. **Set the company name**

The `Fidel.companyName` property is optional, but we recommended setting it. If you don't set a company name, we'll show the default value in the consent text: `Your Company Name`.

2. **Set the privacy policy URL**

The `Fidel.privacyPolicyURL` property is optional. It is added as a hyperlink to the `privacy policy` text. Please check the full behavior below.

3. **Set the delete instructions**

The `Fidel.deleteInstructions` property is optional. The default value is `going to your account settings`. This default value is applied for both consent texts that the SDK forms.

4. **Set the card scheme name**

You can do so via the `Fidel.supportedCardSchemes` property. By default, we allow the user to input card numbers from either Visa, Mastercard or American Express, but you can control which card networks you accept. The consent text changes based on what you define or based on what the user inputs. Please check the full behavior below.

5. **Set the program name (applied to the consent text specific to the US and Canada)**

The `Fidel.programName` property is taken into account only for the consent text specific to USA and Canada. If you don't plan to support USA nor Canada, you can ignore this property. The default value for program name is `our`.

6. **Set the terms and conditions URL (applied to the consent text only for USA and Canada)**

The `Fidel.termsAndConditionsURL` property is mandatory for the SDK if you plan to support USA and Canada issued cards. Once set, it will be applied as a hyperlink on the `Terms and Conditions` text.

### How is the SDK's consent text formed?

The SDK forms two distinct consent texts, depending on the country the cardholder selects as the card issuing country:
1. A specific consent text for US and Canada
2. Another consent text for the other countries supported by Fidel API.

#### Consent text for United States and Canada

You are allowing US and Canada issued cards when you:
1. set United States and/or Canada as allowed countries, via the `Fidel.allowedCountries` property.
2. don't set a value for the `Fidel.allowedCountries` property, which means that all countries supported by Fidel API will be included in your SDK implementation (including US & Canada).

For USA & Canada, the following would be an example consent text for `Cashback Inc` (an example company) that uses `Awesome Bonus` as their program name:

_By submitting your card information and checking this box, you authorize `card_scheme_name` to monitor and share transaction data with Fidel API (our service provider) to participate in `Awesome Bonus` program. You also acknowledge and agree that Fidel API may share certain details of your qualifying transactions with `Cashback Inc` to enable your participation in `Awesome Bonus` program and for other purposes in accordance with the `Cashback Inc` Terms and Conditions, `Cashback Inc` privacy policy and Fidel’s Privacy Policy. You may opt-out of transaction monitoring on the linked card at any time by `deleteInstructions`._

#### Consent text for the other Fidel API supported countries

When you allow countries other than US & Canada and the user selects one of these countries from the list of card issuing countries, a consent text specific for these countries will be applied.

The following would be an example Terms & Conditions text for `Cashback Inc` (an example company):

_I authorise `card_scheme_name` to monitor my payment card to identify transactions that qualify for a reward and for `card_scheme_name` to share such information with `Cashback Inc`, to enable my card linked offers and target offers that may be of interest to me. For information about `Cashback Inc` privacy practices, please see the privacy policy. You may opt-out of transaction monitoring on the payment card you entered at any time by `deleteInstructions`._

### Is the SDK localized?

The SDK's default language is English, but it's also localized for French and Swedish languages. When the device has either `Français (Canada)` or `Svenska (Sverige)` as its language, the appropriate texts will be displayed.

Please make sure that your project also supports localization for the languages that you want to support.
