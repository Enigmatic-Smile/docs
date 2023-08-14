# Guide to migrate to v2 of the Android SDK for Loyalty applications

## Update your Fidel API Android SDK integration

### If you used Jitpack to integrate the Android SDK

Replace the Fidel dependency inside your app/build.gradle by this one:

```groovy
implementation 'com.github.FidelLimited:android-sdk:2.0.0'
```

### If you manually integrated the Android SDK

1. Replace the `FidelSDK.aar` by the latest one. 
2. Remove the following dependency
```groovy
implementation 'io.card:android-sdk:5.5.1'
```
3. Add the following dependencies:
```groovy
implementation 'com.scottyab:rootbeer-lib:0.1.0'
implementation 'io.split.client:android-client:2.13.1'
implementation 'androidx.work:work-runtime-ktx:2.8.1'
```

## Update the minSdkVersion

Previously the minSdkVersion was `19`. You'll have to update it to `21`, to use the Fidel API Android SDK v2.

## Check the following code as your migration guide

Use the following code as a guide related to the changes that  to integrate with the Fidel API SDK:

```kotlin
...
import com.fidelapi.Fidel // was import com.fidel.sdk.Fidel
//remove all com.fidel.sdk imports
...

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        Fidel.programId = "Your Program ID"
        
        /*
         * It is recommended to store the SDK key on your backend and retrieve it
         * before starting to use the SDK.
         */
        Fidel.sdkKey = yourSdkKey // previously apiKey
        // remove Fidel.autoScan
        Fidel.allowedCountries = setOf(Country.CANADA, Country.UNITED_STATES, Country.UNITED_KINGDOM)
        Fidel.supportedCardSchemes = setOf(CardScheme.VISA, CardScheme.MASTERCARD, CardScheme.AMERICAN_EXPRESS)
        Fidel.companyName = "[Developer company]"
        Fidel.termsAndConditionsUrl = "https://fidel.uk" // termsConditionsURL
        Fidel.privacyPolicyUrl = "https://fidel.uk" // was privacyURL
        Fidel.deleteInstructions = "[Developer set delete instructions]"
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
        
        //Fidel.start() replaces Fidel.present()
        btn.setOnClickListener { Fidel.start(this@MainActivity) }
        
        // replaces Fidel.setCardLinkingObserver
        Fidel.onResult = OnResultObserver { result ->
            when (result) {
                is FidelResult.Enrollment -> {
                    Log.d(FIDEL_DEBUG_TAG, "The card ID = " + result.enrollmentResult.cardId)
                }
                is FidelResult.Error -> {
                    Log.e(FIDEL_DEBUG_TAG, "Error message = " + result.error.message)
                }
            }
        }
    }

    private companion object {
        private const val FIDEL_DEBUG_TAG = "fidel.debug"
    }
}
```
