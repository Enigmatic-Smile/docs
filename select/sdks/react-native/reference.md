# React Native SDK reference

1. [Fidel class](#class-fidel)
2. [Properties](#properties)
3. [Methods](#methods)
4. [Callbacks](#callbacks)

## class Fidel

This class is designed as a facade, used to configure the card enrollment process, via many of its [static properties](#properties), and present [the card linking flow](#methods). It's also the class that provides [callbacks](#callbacks) that might be useful for your application.

## Properties

### Mandatory properties

These are properties that must be set correctly. In the case where one of these properties are not set or they are set incorrectly, the SDK will return an error immediately via its callback.

```javascript
Fidel.setup ({
    apiKey: 'Your-SDK-Key',
    programId: 'Your-program-ID',
});
```

#### apiKey: String

This key is used to authenticate your Fidel API account. Get it from your Fidel API dashboard -> Account Settings -> SDK Keys section.

> Important note: Make sure that you store this key securely (not on the client side).

> Note: If you use a **test SDK Key**, your users can only enroll [test card numbers](/docs/select/cards#test-card-numbers).

#### programId: String

The program ID indicates the Fidel API program in which the cards will be enrolled. Get the program ID by navigating to the Fidel API dashboard -> Programs section -> Click on the ID of the program you want to use. Clicking on it will copy the ID in your pasteboard.

### Optional properties (but we recommend setting them)

The following properties are technically not mandatory to be set. However, in order to make your Loyalty use case work with your Transaction Select program, please consider setting them correctly. The following is an example:

```javascript
const myImage = require("./images/fdl_test_banner.png");
const resolveAssetSource = require("react-native/Libraries/Image/resolveAssetSource");
const resolvedImage = resolveAssetSource(myImage);

//this is the default value for supported card schemes,
//but you can remove the support for some of the card schemes if you want to
const cardSchemes = [
  Fidel.CardScheme.visa,
  Fidel.CardScheme.mastercard,
  Fidel.CardScheme.americanExpress,
];

const countries = [Fidel.Country.ireland, Fidel.Country.unitedStates];

Fidel.setOptions({
  bannerImage: resolvedImage,
  allowedCountries: countries,
  defaultSelectedCountry: Fidel.Country.unitedStates,
  supportedCardSchemes: cardSchemes,
  metaData: { id: "your-metadata-id", customKey: "customValue" },
  companyName: "My RN Company",
  deleteInstructions: "Your custom delete instructions!",
  privacyUrl: "https://fidel.uk",
  termsConditionsUrl: "https://fidel.uk/privacy",
  programName: "My program name",
});
```

#### termsConditionsUrl

Expected type: `string`.

You need to set your terms and conditions URL if you would like to:

1. support all the countries that Fidel API supports

2. set a specific `allowedCountries` set of countries AND include US or Canada in your set of allowed countries.

By setting this property we add a link to your Terms & Conditions in the consent text. The cardholder needs to read and agree with your terms, before enrolling a card.

#### companyName

Expected type: `string`.

By setting this property we customize the consent text, that the cardholder needs to read and agree with, before enrolling a card.

The maximum number of characters allowed for this property is `60`.

#### deleteInstructions

Expected type: `string`.

Default value: `"going to your account settings"`

This text informs the cardholder how to opt out of transaction monitoring in your program. It is appended at the end of the consent text. The maximum number of characters allowed for this property is `60`.

#### privacyUrl

Expected type: `string`.

If you provide a value for this parameter, the card enrollment consent text will include a phrase that will provide the user with more privacy related information at the URL that you provide.

When the value of this parameter remains `null` no such phrase will be displayed in the card enrolling consent text.

If you provide an invalid URL string, you will not be able to start the card enrollment flow. Instead you will receive an error via any of the callback methods that you used, immediately after attempting to start the card enrollment flow.

### Optional properties

#### supportedCardSchemes

Type: `array`

Default value: `[Fidel.CardScheme.visa, Fidel.CardScheme.mastercard, Fidel.CardScheme.americanExpress]`

Sets a list of supported card schemes. If a card scheme is supported, cardholders will be able to enroll their card. If a card scheme is not in the list, then the cardholders will see an error message while typing or pasting the unsupported card number.

If you set a `null` value, you will not be able to start the Fidel SDK enrollment flow. In this case, immediately after attempting to start the flow, you will receive an error in callback.

#### allowedCountries

Type: `array`

Default value: `[Fidel.Country.canada, Fidel.Country.ireland, Fidel.Country.japan, Fidel.Country.sweden, Fidel.Country.unitedArabEmirates, Fidel.Country.unitedKingdom, Fidel.Country.unitedStates]`

Sets the list of countries that cardholders can pick to be the card issuing country. When two or more countries are set, cardholders will be able to select the card issuing country with our country selection UI.

If you set a value with only one country, the country selection UI will not be displayed in the card enrollment screen. The country that you set will be considered the card issuing country for all cards enrolled in your Fidel API program using the SDK.

If you set an empty value, you will not be able to start the enrollment flow. Instead you will receive an error immediately after the attempt to start the enrollment flow.

#### defaultSelectedCountry

Default value: `Fidel.Country.unitedKingdom`

Sets the country that will be selected by default when the user opens the card enrollment screen. If the `defaultSelectedCountry` is not part of the `allowedCountries` list, then the first country in the `allowedCountries` list will be selected.

#### metaData

Expected type: object.

Default value: `null`.

This is an object that you can use to associate custom data to an enrolled card.

We advise setting an `id` value for this object. Later, it might be useful for you to use our [List Cards from Metadata ID](https://transaction-stream.fidel.uk/reference/list-cards-from-metadata-id) API Endpoint to query for cards using this ID.

Example of meta data that you can set:

```javascript
metaData: {
    id: "this-is-the-metadata-id",
    userId: "123",
    customKey: 456
}
```

You would receive an object equal to this one, after successfully enrolling a card, in the callback.

#### bannerImage

Will display the banner image that you set in this parameter at the top of the card details screen. Your custom asset needs to be resolved in order to be passed to our native module:

```javascript
const myImage = require('./images/your_banner.png');
const resolveAssetSource = require('react-native/Libraries/Image/resolveAssetSource');
const resolvedImage = resolveAssetSource(myImage);
```

The banner image will take the device's width, but it has a fixed height of 100 pts.
The image view has an `Aspect Fill` content mode, which means that the banner image that you set will fill its entire predefined area, while keeping the aspect ratio.

For the banner image that you can set, we suggest to use the aspect ratio of the smallest devices that you support. On wider devices, the banner image will be cropped from top and bottom sides. This is because of the `Aspect Fill` content mode that we set for the image view.

If a device that opens the SDK has 320dp in width, the aspect ratio of the image view would be *320 : 100*.
If a device that opens the SDK has 475dp in width, the aspect ratio of your banner image would be *475 : 100*.

You need to provide the image for all screen densities (x1, x2 and x3).

Depending on what you want to display in the banner image, you might need to experiment a bit to make sure that nothing important from the image is hidden. The most important information should be displayed in the centre of the banner image.

#### programName

Expected type: `string`

Default value: `"our"`

This value is used in the consent text when enrolling a card issued in United States or Canada. Please set it to a maximum of `60` characters.

## Methods

### openForm(callback)

Opens the card enrollment form.

#### Parameters

- `callback`: A callback that will be called when card enrollment succeeds or fails, with appropriate arguments.

## Callbacks

You can set a `callback` parameter via the `Fidel.openForm` function.

Arguments (in order):
1. `error`: An object that includes information about a card enrollment error. Please check details of this object below.
2. `result`: An object that includes information about a successful card enrollment. Please check details of this object below.

#### Successful card enrollment result

Properties:
- `id`: The identifier of the card enrolled with your Fidel API program.
- `accountId`: The Fidel API account identifier.
- `programId`: The identifier of the program that the card was enrolled into.
- `created`: The date when the card was linked. It is formatted as a string. The following is an example: `"2021-05-19T12:37:55.278Z"`.
- `updated`: The date when the card was updated. It is formatted as a string. The following is an example: `"2021-05-19T12:37:55.278Z"`. _Deprecated_: Please use `created` instead.
- `scheme`: The enrolled card's scheme. Possible values are: `"visa"`, `"mastercard"`, `"amex"`. 
- `type`: The type of card that was linked. Possible values are: `"visa"`, `"mastercard"`, `"amex"`. _Deprecated_: Please use `scheme` instead.
- `mapped`: _Deprecated_
- `live`: This property will be `true` when your Fidel API account is live and the card was enrolled in your `live` Fidel API program. If the program that you enrolled the card into is not a `live` one, then this property will be `false`.
- `firstNumbers`: If available, this property will be populated with the first 6 numbers of the enrolled card. To turn on or off receiving these numbers, please check your Fidel API account's settings.
- `lastNumbers`: If available, this property will be populated with the last 4 numbers of the enrolled card. To turn on or off receiving these numbers, please check your Fidel API account's settings.
- `expYear`: The expiration year of the enrolled card. The values are four digit year values (ex: 2031), **not** shortened, two digit values (ex: 31).
- `expMonth`: The expiration month of the enrolled card. The values start with `1` (January) and end with `12` (December).
- `expDate`: The expiration date of the linked card, formatted as a string. Example values: `2024-01-31T23:59:59.999Z`, `2023-12-31T23:59:59.999Z`
- `countryCode`: The country where the enrolled card was issued. Possible values: `"GBR"` (Great Britain), `"IRL"` (Ireland), `"USA"` (United States of America), `"SWE"` (Sweden), `"JPN"` (Japan), `"CAN"` (Canada).
- `metaData`: Custom data object assigned to the enrolled card via the `metaData` SDK property.

#### Card enrollment error object

Properties:

- `message`: An error message explaining more details about the error. It is not localized.
- `date`: The date and time when the error occurred, expressed as a `String` value. Example value: `2021-05-19T16:34:20.497Z`.
- `code`.
