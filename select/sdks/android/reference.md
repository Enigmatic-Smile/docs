# Android SDK reference

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

2. set a specific `allowedCountries` set of countries AND include US or Canada in your set of allowed countries.

By setting this property we add a link to your Terms & Conditions in the consent text. The cardholder needs to read and agree with your terms, before enrolling a card.

### Optional properties (but we recommend to set them)

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

Default value: `setOf(CardScheme.VISA, CardScheme.MASTERCARD, CardScheme.AMERICAN_EXPRESS)`

Sets a list of supported card schemes. If a card scheme is supported, cardholders will be able to enroll their card. If a card scheme is not in the list, then the cardholders will see an error message while typing or pasting the unsupported card number.

If you set a `null` value, you will not be able to start the Fidel SDK enrollment flow. In this case, immediately after attempting to start the flow, you will receive an error.

#### enum class CardScheme

Cases:

- `VISA`, `MASTERCARD`, `AMERICAN_EXPRESS`

#### allowedCountries

Type: `Set<Country>`

Default value: `setOf(Country.CANADA, Country.IRELAND, Country.JAPAN, Country.SWEDEN, Country.UNITED_ARAB_EMIRATES, Country.UNITED_KINGDOM, Country.UNITED_STATES)`

Sets the list of countries that cardholders can pick to be the card issuing country. When two or more countries are set, cardholders will be able to select the card issuing country with our country selection UI.

If you set a value with only one country (`size` of this set == 1), the country selection UI will not be displayed in the card enrollment screen. The country that you set will be considered the card issuing country for all cards enrolled in your Fidel API program using the SDK.

If you set an empty value, you will not be able to start the enrollment flow. Instead you will receive an error immediately after the attempt to start the enrollment flow.

##### enum class Country

Cases:

- `CANADA, IRELAND, JAPAN, SWEDEN, UNITED_ARAB_EMIRATES, UNITED_KINGDOM, UNITED_STATES`

#### defaultSelectedCountry: Country

Default value: `UNITED_KINGDOM`

Sets the `Country` that will be selected by default when the user opens the card enrollment screen. If the `defaultSelectedCountry` is not part of the `allowedCountries` list, then the first country in the `allowedCountries` list will be selected.

#### metaData: org.json.JSONObject?

This is a JSONObject that you can use to associate custom data with an enrolled card.

We advise setting an `"id"` key -> value pair in this JSONObject. Later, it might be useful for you to use our [List Cards from Metadata ID](https://reference.fidel.uk/reference/list-cards-from-metadata-id) API Endpoint to query for cards using this ID.

Example of meta data that you can set:

```kotlin
val jsonMetaData = JSONObject()
jsonMetaData.put("id", "this-is-the-metadata-id")
jsonMetaData.put("myUserId", "123")
jsonMetaData.put("customKey", "customValue")
Fidel.metaData = jsonMetaData
```

You would receive a JSONObject that is equal to this one, after successfully enrolling a card using the SDK callback.

#### bannerImage: Bitmap?

Default value: `null`.

Will display the banner image that you set in this parameter at the top of the card details screen.

The banner image will take the device's width, but it has a fixed height of 100 dp.
The image view has the `android:scaleType="centerCrop"` attribute, which means that the banner image that you set will fill its entire predefined area.

For the banner image that you can set, we suggest to use the aspect ratio of the smallest devices that you support. On wider devices, the banner image will be cropped from top and bottom sides. This is because of the `android:scaleType="centerCrop"` attribute that we set for the image view.

If a device that opens the SDK has 320dp in width, the aspect ratio of the image view would be *320 : 100*.
If a device that opens the SDK has 475dp in width, the aspect ratio of your banner image would be *475 : 100*.

You need to provide the image for all screen densities.

Depending on what you want to display in the banner image, you might need to experiment a bit to make sure that nothing important from the image is hidden. The most important information should be displayed in the centre of the banner image.

#### programName: String

Default value: `"our"`

This value is used in the consent text when enrolling a card issued in United States or Canada. Please set it to a maximum of `60` characters.

## Methods

### present(context: Context)

Presents the card linking activity. We start our activity for obtaining a result. So you can also override the `onActivityResult` function to get a result from the SDK's card linking Activity.

#### Parameters

- `startingActivity: Activity`: the `Activity` that will start the card enrollment flow. In this activity you can override the `onActivityResult` function and expect a result there. This is a *mandatory* parameter.

### setCardLinkingObserver(cardLinkingObserver: FidelCardLinkingObserver)

Parameter:

- `cardLinkingObserver: FidelCardLinkingObserver`: the `FidelCardLinkingObserver` that will be notified when card enrollment succeeds or fails. This is a *mandatory* parameter.

## Callbacks

The SDK provides callbacks capabilities via the following methods:

### Setting a FidelCardLinkingObserver

You can set an observer via the `Fidel.setCardLinkingObserver(cardLinkingObserver: FidelCardLinkingObserver)` function.

The `FidelCardLinkingObserver` interface has two "callback" functions:

```kotlin
fun onCardLinkingSucceeded(linkResult: LinkResult)
fun onCardLinkingFailed(linkResultError: LinkResultError)
```

Please check below the details of `LinkResult` and `LinkResultError`.

### Overriding the onActivityResult function in your starting Activity

You start the card enrollment experience by calling the `Fidel.present(yourActivity)` function. In that activity you can override the `onActivityResult` function and get Fidel card enrollment results, as exemplified below:

```kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    super.onActivityResult(requestCode, resultCode, data)

    // Use this request code to identifier the result you're trying to handle
    if (requestCode == Fidel.FIDEL_LINK_CARD_REQUEST_CODE) {
        if (data != null && data.hasExtra(Fidel.FIDEL_LINK_CARD_RESULT_CARD)) {
            val linkResult = data.getParcelableExtra(Fidel.FIDEL_LINK_CARD_RESULT_CARD) as? LinkResult
            val linkError = linkResult?.error
            if (linkError != null) {
                Log.e(Fidel.FIDEL_DEBUG_TAG, "Error message = " + linkError.message)
            } else if (linkResult != null) {
                Log.d(Fidel.FIDEL_DEBUG_TAG, "The link ID = " + linkResult.expMonth)
            }
        }
    }
}
```

Please check below the details of `LinkResult` and `LinkResultError`.

#### class LinkResult

This class represents a successful card enrollment/linking result.

Properties:
- `id: String!`: The identifier of the card enrolled with your Fidel API program.
- `accountId: String!`: The Fidel API account identifier.
- `programId: String!`: The identifier of the program that the card was enrolled into.
- `created: String!`: The date when the card was linked. It is formatted as a string. The following is an example: `"2021-05-19T12:37:55.278Z"`.
- `updated: String!`: The date when the card was updated. It is formatted as a string. The following is an example: `"2021-05-19T12:37:55.278Z"`. _Deprecated_: Please use `created` instead.
- `scheme: String!`: The enrolled card's scheme. Possible values are: `"visa"`, `"mastercard"`, `"amex"`. 
- `type: String!`: The type of card that was linked. Possible values are: `"visa"`, `"mastercard"`, `"amex"`. _Deprecated_: Please use `scheme` instead.
- `mapped: Boolean!`: _Deprecated_
- `live: Boolean!`: This property will be `true` when your Fidel API account is live and the card was enrolled in your `live` Fidel API program. If the program that you enrolled the card into is not a `live` one, then this property will be `false`.
- `firstNumbers: String!`: If available, this property will be populated with the first 6 numbers of the enrolled card. To turn on or off receiving these numbers, please check your Fidel API account's settings.
- `lastNumbers: String!`: If available, this property will be populated with the last 4 numbers of the enrolled card. To turn on or off receiving these numbers, please check your Fidel API account's settings.
- `expYear: Int`: The expiration year of the enrolled card. The values are four digit year values (ex: 2031), **not** shortened, two digit values (ex: 31).
- `expMonth: Int`: The expiration month of the enrolled card. The values start with `1` (January) and end with `12` (December).
- `countryCode: String!`: The country where the enrolled card was issued. Possible values: `"GBR"` (Great Britain), `"IRL"` (Ireland), `"USA"` (United States of America), `"SWE"` (Sweden), `"JPN"` (Japan), `"CAN"` (Canada).
- `metaData: JSONObject?`: Custom data assigned to the enrolled card via the `metaData` SDK property.
- `error: LinkResultError?`: It contains an error, when the card enrollment response is not successful.

#### class LinkResultError

Represents an error during the card enrollment process. Properties:

- `message: String`: An error message explaining more details about the error. It is not localized.
- `date: String`: The date and time when the error occurred, expressed as a `String` value. Example value: `2021-05-19T16:34:20.497Z`.
- `errorCode: LinkResultErrorCode`: Please check more details about `LinkResultErrorCode` below.

#### enum LinkResultErrorCode

- `ERROR_LINKING_CARD`: You'll receive an error with this code when receiving an error while attempting to link a card.
- `USER_CANCELED`: The user canceled the card enrollment flow at any stage.
- `INVALID_URL`: You'll receive an error with this code when you configured the `termsConditionsURL` or `privacyURL` with an invalid URL.
- `STRING_OVER_THE_LIMIT`: You'll receive an error with this code when you configured a Fidel SDK parameter with too many characters. Some of the string parameters that we accept have limited number of characters. Please check that all parameters that you set are valid.
- `MISSING_MANDATORY_INFO`: You'll receive an error with this code when you did not provide a mandatory parameter. Please check that all mandatory parameters are set.
- `NULL_STRING_TO_VALIDATE`: You'll receive an error with this code when you configured the Fidel SDK with a null URL when you actually needed to provide one.
