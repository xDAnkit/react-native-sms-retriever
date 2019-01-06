[![Platform][platform-badge]][platform-url]
[![License][license-badge]][license-url]
[![Gitter][gitter-badge]][gitter-url]

<p align="center">
  <img src="https://firebasestorage.googleapis.com/v0/b/furtado-a45bf.appspot.com/o/GitHub%2Freact-native-sms-retriever%2Fcover.png?alt=media&token=c1d91ddc-0100-46d3-ba6a-36666e1495d5" alt="Cover" title="React Native SMS Retriever" width="800">
</p>

With the [SMS Retriever API](https://developers.google.com/identity/sms-retriever/overview), you can perform SMS-based user verification in your Android app automatically, without requiring the user to manually type verification codes, and without requiring any extra app permissions.

<h1></h1>

<br/>
<p align="center">
  <img src="https://firebasestorage.googleapis.com/v0/b/furtado-a45bf.appspot.com/o/GitHub%2Freact-native-sms-retriever%2Frequest-phone-number.gif?alt=media&token=711086af-e728-4234-815b-49f2f738437f" alt="Read Phone Number" title="React Native SMS Retriever" height="400">

  <img src="https://firebasestorage.googleapis.com/v0/b/furtado-a45bf.appspot.com/o/GitHub%2Freact-native-sms-retriever%2Fsend-sms-with-emulator.gif?alt=media&token=3ccb1268-6d5b-420d-9090-13f6a6946ca3" alt="Read SMS" title="React Native SMS Retriever" height="400">
</p>
<br/>

## Installation

```bash
npm install --save react-native-sms-retriever
react-native link react-native-sms-retriever
```

#### Manual (if you don't like to use react-native link)

1.  Add the following lines to `android/settings.gradle`:

```gradle
include ':react-native-sms-retriever'
project(':react-native-sms-retriever').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-sms-retriever/android')
```

2.  Add the compile line to the dependencies in `android/app/build.gradle`:

```gradle
dependencies {
  // ...
  compile project(':react-native-sms-retriever')
}
```

3.  Add the import and link the package in `MainApplication.java`:

```java
import me.furtado.smsretriever.RNSmsRetrieverPackage; // <-- Add the import

public class MainApplication extends Application implements ReactApplication {

  // ...

  @Override
  protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
      new MainReactPackage(),
      // ...
      new RNSmsRetrieverPackage() // <-- Add it to the packages list
    );
  }

  // ...
}
```

## Basic Usage

```javascript
import SmsRetriever from 'react-native-sms-retriever';

// Get the phone number (first gif)
 _onPhoneNumberPressed = async () => {
  try {
    const phoneNumber = await SmsRetriever.requestPhoneNumber();
  } catch (error) {
    console.log(JSON.stringify(error));
  }
 };

// Get the SMS message (second gif)
_onSmsListenerPressed = async () => {
  try {
    const registered = await SmsRetriever.startSmsRetriever();
    if (registered) {
      SmsRetriever.addSmsListener(event => {
        console.log(event.message);
        SmsRetriever.removeSmsListener();
      }); 
    }
  } catch (error) {
    console.log(JSON.stringify(error));
  }
};
```

#### About Phone

To get the phone number, use a real device. The [SMS Retriever API](https://developers.google.com/identity/sms-retriever/overview) doesn't work very well with emulators.

#### About SMS 

The [SMS Retriever API](https://developers.google.com/identity/sms-retriever/overview) has some rules for message. A valid verification message might look like the following:

```txt
<#> Your Example app code is: 123ABC78
/SBnkJ8069Q
```

The `<#>` is required. The `/SBnkJ8069Q` is a hash string that identifies your app (in the case it's the hash of the [example](https://github.com/Bruno-Furtado/react-native-sms-retriever/blob/master/example) app). Too see how to generate the hash access [this link](https://developers.google.com/identity/sms-retriever/verify#1_construct_a_verification_message). Alternatively, you can get your app's hash string with the [AppSignatureHelper](https://github.com/googlesamples/android-credentials/blob/master/sms-verification/android/app/src/main/java/com/google/samples/smartlock/sms_verify/AppSignatureHelper.java) class.


## Methods and Errors

#### `requestPhoneNumber(): Promise<String>`

Obtain the user's phone number (using the hint picket).

| Error Type                        | Error Message                                                |
| --------------------------------- | ------------------------------------------------------------ |
| ACTIVITY_NULL_ERROR_TYPE          | Activity is null.                                            |
| ACTIVITY_RESULT_NOOK_ERROR_TYPE   | There was an error trying to get the phone number.           |
| CONNECTION_SUSPENENDED_ERROR_TYPE | Client is temporarily in a disconnected state.               |
| CONNECTION_FAILED_ERROR_TYPE      | There was an error connecting the client to the service.     |
| SEND_INTENT_ERROR_TYPE            | There was an error trying to send intent.                    |
| UNAVAILABLE_ERROR_TYPE            | Google Play Services is not available.                       |
| UNSUPORTED_VERSION_ERROR_TYPE     | The device version of Google Play Services is not supported. |

#### `startSmsRetriever(): Promise<Boolean>`

Start to listen for SMS messages.

| Error Type                        | Error Message                                                |
| --------------------------------- | ------------------------------------------------------------ |
| TASK_FAILURE_ERROR_TYPE           | Task failed.                                                 |
| UNAVAILABLE_ERROR_TYPE            | Google Play Services is not available.                       |
| UNSUPORTED_VERSION_ERROR_TYPE     | The device version of Google Play Services is not supported. |

#### `addSmsListener(event: Function): Promise<Boolean>`

Get the SMS content. Get the SMS content with: `event.message`.

| Error Param |  Type  | Description                                                  |
| ----------- | ------ | ------------------------------------------------------------ |
| extras      | String | Sent only when there was a problem getting the SMS content.  |
| status      | String | Sent only when status is not `OK` or `TIMEOUT`.              |
| timeout     | String | Sent only when status is equals to `TIMEOUT`.                |

#### `removeSmsListener(): Void`

Stop to listen for SMS messages.


## Change-log

A brief summary of each [React Native SMS Retriever](https://github.com/Bruno-Furtado/react-native-sms-retriever) release can be found on the [releases](https://github.com/Bruno-Furtado/react-native-sms-retriever/releases).


## License

This code is distributed under the terms and conditions of the [MIT License](https://github.com/Bruno-Furtado/react-native-sms-retriever/blob/master/LICENSE).


[platform-badge]: https://img.shields.io/badge/platform-Android-green.svg?style=flat
[platform-url]: https://developer.android.com/
[license-badge]: https://img.shields.io/badge/license-MIT-orange.svg?style=flat
[license-url]: https://github.com/Bruno-Furtado/react-native-sms-retriever/blob/master/LICENSE
[gitter-badge]: https://badges.gitter.im/react-native-sms-retriever/community.svg
[gitter-url]: https://gitter.im/react-native-sms-retriever/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
