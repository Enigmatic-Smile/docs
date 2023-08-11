# iOS SDK reference <a style="border-bottom: 2px solid #0048ff;" class="improve-docs" href="/select/sdks/ios/reference-v1">v1</a> <a style="margin-right: auto; color: #111;" class="improve-docs" href="/select/sdks/ios/reference-v2">v2</a>

1. [Fidel class](#class-fidel)
2. [Properties](#properties)
3. [Methods](#methods)
4. [Callbacks](#callbacks)

## class Fidel

This class is designed as a facade, used to configure the card enrollment process, via many of its [static properties](#properties), and present [the card linking flow](#methods). It's also the class that provides [callbacks](#callbacks) that might be useful for your application.

## Properties

### Mandatory properties

These are properties that must be set correctly. In the case where one of these properties are not set or they are set incorrectly, the SDK will return an error immediately via its callback.

#### apiKey: String

This key is used to authenticate your Fidel API account. Get it from your Fidel API dashboard -> Account Settings -> SDK Keys section.

> Important note: Make sure that you store this key securely (not on the client side).

> Note: If you use a **test SDK Key**, your users can only enroll [test card numbers](/docs/select/cards#test-card-numbers).

#### programId: String

The program ID indicates the Fidel API program in which the cards will be enrolled. Get the program ID by navigating to the Fidel API dashboard -> Programs section -> Click on the ID of the program you want to use. Clicking on it will copy the ID in your pasteboard.

#### termsConditionsURL: String

You need to set your terms and conditions URL if you would like to:

1. support all the countries that Fidel API supports

2. set a specific `allowedCountries` set of countries AND include US or Canada in it.

By setting this property we add a link to your Terms & Conditions in the consent text. The cardholder needs to read and agree with your terms, before enrolling a card.

### Optional properties (but we recommend setting them)

The following properties are technically not mandatory to be set. However, in order to make your Loyalty use case work with your Transaction Select program, please consider setting them correctly.

#### companyName: String

By setting this property we customize the consent text, that the cardholder needs to read and agree with, before enrolling a card.

The maximum number of characters allowed for this property is `60`.

#### deleteInstructions: String

Default value: `"going to your account settings"`

This text informs the cardholder how to opt out of transaction monitoring in your program. It is appended at the end of the consent text. The maximum number of characters allowed for this property is `60`.

#### privacyURL: String?

Default value: `null`.

If you provide a value for this parameter, the card enrollment consent text will include a phrase that will provide the user with more privacy related information at the URL that you provide.

When the value of this parameter remains `null` no such phrase will be displayed in the card enrolling consent text.

If you provide an invalid URL string, you will not be able to start the card enrollment flow. Instead you will receive an error via any of the callback methods that you used, immediately after attempting to start the card enrollment flow.

### Optional properties

#### supportedCardSchemes

Type: `Set<CardScheme>`

Default value: `[.visa, .mastercard, .americanExpress]`

Sets a list of supported card schemes. If a card scheme is supported, cardholders will be able to enroll their card. If a card scheme is not in the list, then the cardholders will see an error message while typing or pasting the unsupported card number.

If you set a `null` value, you will not be able to start the Fidel SDK enrollment flow. In this case, immediately after attempting to start the flow, you will receive an error in the `onCardLinkFailedCallback`.

#### enum CardScheme

Cases:

- `visa`, `mastercard`, `americanExpress`

#### allowedCountries

Type: `Array<Country>`

Default value: `[.canada, .ireland, .japan, .sweden, .unitedArabEmirates, .unitedKingdom, .unitedStates]`

Sets the list of countries that cardholders can pick to be the card issuing country. When two or more countries are set, cardholders will be able to select the card issuing country with our country selection UI.

If you set a value with only one country, the country selection UI will not be displayed in the card enrollment screen. The country that you set will be considered the card issuing country for all cards enrolled in your Fidel API program using the SDK.

If you set an empty value, you will not be able to start the enrollment flow. Instead you will receive an error immediately after the attempt to start the enrollment flow.

##### enum class Country

Cases:

- `canada, ireland, japan, sweden, unitedArabEmirates, unitedKingdom, unitedStates`

#### defaultSelectedCountry: Country

Default value: `.unitedKingdom`

Sets the `Country` that will be selected by default when the user opens the card enrollment screen. If the `defaultSelectedCountry` is not part of the `allowedCountries` list, then the first country in the `allowedCountries` list will be selected.

#### metaData: [String: Any]?

This is a JSONObject that you can use to associate custom data with an enrolled card.

We advise setting an `"id"` key -> value pair in this JSONObject. Later, it might be useful for you to use our [List Cards from Metadata ID](https://reference.fidel.uk/reference/list-cards-from-metadata-id) API Endpoint to query for cards using this ID.

Example of meta data that you can set:

```swift
Fidel.metaData = [
    "id": "this-is-the-metadata-id",
    "myUserId": "123",
    "customKey1": "customValue1"
]
```

You would receive a dictionary that is equal to this one, after successfully enrolling a card using the SDK callback.

#### bannerImage: UIImage?

Default value: `null`.

Will display the banner image that you set in this parameter at the top of the card details screen.

The banner image will take the device's width, but it has a fixed height of 100 pts.
The image view has an `Aspect Fill` content mode, which means that the banner image that you set will fill its entire predefined area, while keeping the aspect ratio.

For the banner image that you can set, we suggest to use the aspect ratio of the smallest devices that you support. On wider devices, the banner image will be cropped from top and bottom sides. This is because of the `Aspect Fill` content mode that we set for the image view.

If you support 4" iPhones (iPhone 5s, 5c etc.), the aspect ratio would be *320 : 100*.
If the smallest device that you support is 4.7" iPhones (iPhone 6, 7, 8 etc.),
the aspect ratio of your banner image would be *375 : 100*.

You need to provide the image for all screen densities (x1, x2 and x3).

Depending on what you want to display in the banner image, you might need to experiment a bit to make sure that nothing important from the image is hidden. The most important information should be displayed in the centre of the banner image.


#### programName: String

Default value: `"our"`

This value is used in the consent text when enrolling a card issued in United States or Canada. Please set it to a maximum of `60` characters.

#### autoScan: Bool

Default value: `false`

When set to `true`, automatically starts the card camera scanning UI. After card scanning is finalized, the user will go to the expected card enrollment screen, with the card details prefilled.

## Methods

### present(_: UIViewController,onCardLinkedCallback:,onCardLinkFailedCallback:)

Presents the card linking activity.

#### Parameters

- `presentingViewController: UIViewController`: the `UIViewController` that will present the Fidel SDK card linking experience. This is a *mandatory* parameter.
- `onCardLinkedCallback: (_ cardResult: LinkResult) -> Void`: Will be called when card linking succeeds. It will send a `LinkResult` as a parameter. This is an *optional* parameter. Default value of this parameter is `nil`.
- `onCardLinkFailedCallback: (_ error: LinkError) -> Void`: Will be called when card linking fails. It will send a `LinkError` as a parameter. This is an *optional* parameter. Default value of this parameter is `nil`.

## Callbacks

The SDK provides callbacks capabilities by setting the `onCardLinkedCallback` & `onCardLinkFailedCallback` via the `Fidel.present` function.

### onCardLinkedCallback

You can set the callback via the `Fidel.present` function. You will receive a `LinkResult` as an argument. Please check the details of this class below.

### onCardLinkFailedCallback

You can set the callback via the `Fidel.present` function. You will receive a `LinkError` as an argument. Please check the details of this class below.

#### class LinkResult

This class represents a successful card enrollment/linking result.

Properties:
- `id: String`: The identifier of the card enrolled with your Fidel API program.
- `accountId: String?`: The Fidel API account identifier.
- `programId: String?`: The identifier of the program that the card was enrolled into.
- `created: String?`: The date when the card was linked. It is formatted as a string. The following is an example: `"2021-05-19T12:37:55.278Z"`.
- `updated: String?`: The date when the card was updated. It is formatted as a string. The following is an example: `"2021-05-19T12:37:55.278Z"`. _Deprecated_: Please use `created` instead.
- `scheme: String?`: The enrolled card's scheme. Possible values are: `"visa"`, `"mastercard"`, `"amex"`. 
- `type: String?`: The type of card that was linked. Possible values are: `"visa"`, `"mastercard"`, `"amex"`. _Deprecated_: Please use `scheme` instead.
- `mapped: Bool`: _Deprecated_
- `live: Bool`: This property will be `true` when your Fidel API account is live and the card was enrolled in your `live` Fidel API program. If the program that you enrolled the card into is not a `live` one, then this property will be `false`.
- `firstNumbers: String?`: If available, this property will be populated with the first 6 numbers of the enrolled card. To turn on or off receiving these numbers, please check your Fidel API account's settings.
- `lastNumbers: String?`: If available, this property will be populated with the last 4 numbers of the enrolled card. To turn on or off receiving these numbers, please check your Fidel API account's settings.
- `expYear: Int`: The expiration year of the enrolled card. The values are four digit year values (ex: 2031), **not** shortened, two digit values (ex: 31).
- `expMonth: Int`: The expiration month of the enrolled card. The values start with `1` (January) and end with `12` (December).
- `expDate: String?`: The expiration date of the linked card, formatted as a string. Example values: `2024-01-31T23:59:59.999Z`, `2023-12-31T23:59:59.999Z`
- `countryCode: String?`: The country where the enrolled card was issued. Possible values: `"GBR"` (Great Britain), `"IRL"` (Ireland), `"USA"` (United States of America), `"SWE"` (Sweden), `"JPN"` (Japan), `"CAN"` (Canada).
- `metaData: [String: Any]?`: Custom data assigned to the enrolled card via the `metaData` SDK property.

#### class LinkError

Represents an error during the card enrollment process. Properties:

- `message: String`: An error message explaining more details about the error. It is not localized.
- `date: String?`: The date and time when the error occurred, expressed as a `String` value. Example value: `2021-05-19T16:34:20.497Z`.
- `code: String?`: This property is filled in directly from the card linking API response. Another possible value is `"user-canceled"` which is sent when the user cancels the card enrollment flow at any stage.
