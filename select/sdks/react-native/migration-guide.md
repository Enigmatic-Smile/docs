# Guide to migrate to v2 of the React Native SDK for Loyalty applications

This is a guide to help you migrate from version 1.x.x of the Fidel API React Native SDK to version 2.0.0.

## Update your Fidel API React Native SDK

### If you used Cocoapods to integrate iOS SDK

- At the root of your project, run 

`yarn upgrade fidel-react-native` or 

`npm update -g fidel-react-native`.

- In your `ios` folder, run the following command to update the Fidel dependency:
```
pod update Fidel
```

## Check the following code as your migration guide

Use the following code as a guide related to the changes needed to integrate with the Fidel API SDK:

```javascript
import Fidel, { ENROLLMENT_RESULT, ERROR } from 'fidel-react-native';
//...

export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.configureFidel();
  }

  configureFidel() {
    const myImage = require('./demo_images/your_banner.png');
    const resolveAssetSource = require('react-native/Libraries/Image/resolveAssetSource');
    const resolvedImage = resolveAssetSource(myImage);

    const yourSupportedCountries = [
        Fidel.Country.canada,
        Fidel.Country.ireland,
        Fidel.Country.japan,
        Fidel.Country.sweden,
        Fidel.Country.unitedArabEmirates,
        Fidel.Country.unitedKingdom, 
        Fidel.Country.unitedStates,
        Fidel.Country.norway, // new addition
    ];

    Fidel.setup ({
      sdkKey: yourSdkKey, // previously named `apiKey`; make sure to store it securely
      programId: 'Your program ID',
      programType: Fidel.ProgramType.transactionSelect, // new property
      options: {
        bannerImage: resolvedImage, // moved from the properties of the previous `Fidel.setOptions` function
        allowedCountries: yourSupportedCountries, // moved from the properties of the previous `Fidel.setOptions` function
        defaultSelectedCountry: Fidel.Country.unitedStates, // moved from the properties of the previous `Fidel.setOptions` function
        supportedCardSchemes: [Fidel.CardScheme.visa], // moved from the properties of the previous `Fidel.setOptions` function
        metaData: { id: 'your-metadata-id', userId: 1234 }, // moved from the properties of the previous `Fidel.setOptions` function
      },
      consentText: {
        termsAndConditionsUrl: 'https://yourwebsite.com/terms', // previously named `termsConditionsUrl`; moved from the properties of the previous `Fidel.setOptions` function
        companyName: 'Your Company Name', // moved from the properties of the previous `Fidel.setOptions` function
        privacyPolicyUrl: 'https://yourwebsite.com/privacy-policy', // previously named `privacyUrl`; moved from the properties of the previous `Fidel.setOptions` function
        deleteInstructions: "following our delete instructions", // moved from the properties of the previous `Fidel.setOptions` function
        programName: 'Your program name', // moved from the properties of the previous `Fidel.setOptions` function
      },
     // moved the call back from the previously named `Fidel.openForm` function; please check our reference for more details on the `result` object.
     }, (result) => {
      switch (result.type) {
        case ENROLLMENT_RESULT:
          console.log("card was enrolled: " + result.enrollmentResult.cardId);
          break;
        case ERROR:
          this.handleError(result.error);
          break;
      }
    });
  }

  onButtonPress = () => {
    Fidel.start(); // Previously named `Fidel.openForm`; This function also had a callback parameter, which now has moved to be the second parameter of the `Fidel.setup` function.
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

## Starting the card-linking flow

On any action that you want (on a tap of a button, for example), add the following code: 

```swift
Fidel.start()
```

The `Fidel.start` function replaces the previous: `Fidel.openForm` function. To get notified about card enrollment events, you can use the second paramter of the `Fidel.setup` function, which is a callback that can send back a `result`. Please check our v2 Reference documentation for more details.