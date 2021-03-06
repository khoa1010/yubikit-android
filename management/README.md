# MGMT Module for YubiKit Android
**MGMT** is module of Android YubiKit library provided Yubico.
It provides advanced features that YubiKey provides:
1) management functionality of YubiKey which allows to enable or disable applets/transport.
2) access to features that allows personalization of YubiKey - programming OTP slots
3) API to use HMAC-SHA1 challenge-response feature of YubiKey.

**MGMT** requires at minimum  Java 7 or Android 4.4, future versions may require a later baseline. Anything lower than Android 8.0 may receive less testing by Yubico.

## Integration Steps <a name="integration_steps"></a>
###Download
####Gradle:

```gradle
dependencies {  
  // core library, connection detection, and raw commands communication with YubiKey
  implementation 'com.yubico.yubikit:yubikit:$yubikitVersion'
  // mgmt
  implementation 'com.yubico.yubikit:mgmt:$yubikitVersion'
}
```
And in `gradle.properties` set latest version. Example:
```gradle
yubikitVersion=1.0.0-beta04
```
#### Maven:
```xml
<dependency>
  <groupId>com.yubico.yubikit</groupId>
  <artifactId>yubikit</artifactId>
  <version>1.0.0-beta04</version>
</dependency>

<dependency>
  <groupId>com.yubico.yubikit</groupId>
  <artifactId>mgmt</artifactId>
  <version>1.0.0-beta04</version>
</dependency>
```
###Using Library <a name="using_lib"></a>

This module requires usage of yubikit core library to detect `YubikeySession` (see [Using YubiKit](../yubikit/README.md))  
And use it to create `ManagementApplication` to select MGMT applet on YubiKey to have access to management functionality or `YubiKeyConfigurationApplication` to use customization/personalization of OTP slots or for challenge-response.

```java

    ManagementApplication application = new ManagementApplication(session);
    // run provided command/operation (readConfiguration/writeConfiguration) to enable
    
    
    // HMAC-SHA1 challenge-response
    YubiKeyConfigurationApplication application = new YubiKeyConfigurationApplication(session);
    byte[] response = application.calculateHmacSha1(challenge, Slot.TWO);
    
```

### Using the Demo Application <a name="using_demo"></a>
This module provides multiple demos. 
1. To see management functionality select "YubiKey Settings" pivot in navigation drawer. It allows to turn off/on some of yubikey service (supports mostly YubiKey 5+)
This demo also allows you to check how your application would behave if any of services is disabled on customers YubiKey (or for example, during usage of other demos).

2. Programming OTP slots is implemented in "Configure OTP" pivot. Currently it allows 4 type of customization: YubiOTP, static Password, secret for HMAC-SHA1 challenge-response or HOTP secret. 
Things that needs to be considered before running this demo. 
- By default the slot ONE is programmed with YubiOTP secret. Overriding first slot means loosing this secret.
- YubiOTP codes that generated by key can be read using "Yubico OTP demo" pivot. For integration with it on Android (see [OTP](../otp/README.md)) 
- For HOTP secrets it's suggested to use [OATH](../oath/README.md) and Authenticator application, it allows to store multiple of HOTP secrets and use Authenticator app to calculate them whenever you need.

3. HMAC-SHA1 challenge-response demo can be found in "Challenge-response" pivot. 
