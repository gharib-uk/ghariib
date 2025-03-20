---
title: "Implementing Secure Biometric Authentication on Mobile Applications"
date: 2020-07-30T01:20:00.000-07:00
draft: false
type: posts
categories: 
- Android,Authentication,iOS,MAPT,Mobile
---
# Implementing Secure Biometric Authentication on Mobile Applications

<br/>

<br/>
###   

Nowadays, almost every mobile device has a biometric sensor that allows developers to implement local authentication and also store sensitive data securely through dedicated APIs.

Biometric authentication is generally more secure than classic username/password approach. Anyway it must be considered that a wrong implementation could allow an attacker to easily bypass authentication mechanisms by using hooking techniques which can be performed with tools like Frida, Objection, and other similar utilities.

  

In this article we are going to expose some common mistakes that developers can make while implementing  biometric authentication and how to implement it in the correct way.

  

  

### Biometric Authentication in Android

The Android platform introduced the biometric authentication in Android 6.0 (API level 23) with the class _FingerprintManager_ which supported only fingerprint authentication.

In Android 9 (API level 28), the _FingerprintManager_ was deprecated due to the release of _android.hardware.biometrics.BiometricPrompt_.

Lastly, In Android 10 (API level 29) the biometric authentication is managed through _android.hardware.biometrics.BiometricManager_.

  

It is worth considering that the Android platform introduces also the classes _androidx.biometric.BiometricManager_ and _androidx.biometric.BiometricPrompt_ that could be used instead of the previous ones. These classes will automatically query the _BiometricManager_ on devices running Android 10 (API 29) and _FingerprintManagerCompat_ on Android 9.0 (API 28) and prior versions.

  

The Android platform, unlike iOS, does not allow to save arbitrary data within Keystore. However, it allows to create encryption keys, which are saved in the _Keystore_. For every key it is possible to define the access criteria.

In order to implement effective biometric authentication, it is therefore necessary to create a key that can be used only after a successful biometric authentication. This key should be used to encrypt and decrypt a sensitive data such as an authentication token.

  

In order to use the biometric authentication all of the following requirements must be fulfilled:

  

1) **Require** **the proper permission in the Android Manifest**:

  

<uses-permission android:name="android.permission.USE\_FINGERPRINT" />

<uses-permission android:name="android.permission.USE\_BIOMETRIC" />

  

  

2) **Check if the user can authenticate using biometrics:**

This includes having a protected lock screen enabled, a biometric hardware available and a biometric identity registered (For instance a fingerprint). The following piece of code shows a sample implementation:

  

import androidx.biometric.BiometricManager;

. . .

BiometricManager biometricManager = BiometricManager.from(this);

switch (biometricManager.canAuthenticate()) {

    case BiometricManager.BIOMETRIC\_SUCCESS:

        // User can authenticate using biometrics

        break;

    case BiometricManager.BIOMETRIC\_ERROR\_NO\_HARDWARE:

        // No biometric features available on this device

        break;

    case BiometricManager.BIOMETRIC\_ERROR\_HW\_UNAVAILABLE:

        // Biometric features are currently unavailable

        break;

    case BiometricManager.BIOMETRIC\_ERROR\_NONE\_ENROLLED:

        // The user hasn't associated any biometric credentials with their account

        break;

}

  

  

  

3) **Check if the application has the correct permissions**:

  

context.checkSelfPermission(Manifest.permission.USE\_FINGERPRINT) == PermissionResult.PERMISSION\_GRANTED;

  

context.checkSelfPermission(Manifest.permission.USE\_BIOMETRIC) == PermissionResult.PERMISSION\_GRANTED;

  

The most important methods which must be used in order to implement the biometric authentication in Android are the following ones:

  

1) **authenticate** (which starts the authentication flow):

  

biometricPrompt.authenticate(promptInfo, new BiometricPrompt.CryptoObject(cipher));

  

  

2) onAuthenticationSucceeded (which is called upon a successful authentication):

  

@Override

public void onAuthenticationSucceeded(

        @NonNull BiometricPrompt.AuthenticationResult result) {

// . . . .

}

  

  

The cipher referred in the first parameter of the _authenticate_ method should be used in order to decrypt a secret data which has been previously stored in the device. Such cipher can use both asymmetric and symmetric algorithm.

In the following example we are going to create a key for a cipher which uses AES-CBC-PKCS7.

KeyGenerator keyGenerator = KeyGenerator.getInstance(KeyProperties.KEY\_ALGORITHM\_AES, “AndroidKeyStore”);

keyGenerator.init(new KeyGenParameterSpec.Builder (KEY\_ALIAS,

        KeyProperties.PURPOSE\_ENCRYPT | KeyProperties.PURPOSE\_DECRYPT)

        .setBlockModes(KeyProperties.BLOCK\_MODE\_CBC)

        .setEncryptionPaddings(KeyProperties.ENCRYPTION\_PADDING\_PKCS7)

        .setUserAuthenticationRequired(true)

        .setUserAuthenticationValidityDurationSeconds(-1)

        .setInvalidatedByBiometricEnrollment(true)

        .build()

);

keyGenerator.generateKey();

  

  
When creating the key for the cipher that will be used in the biometric authentication flow, the most important options are the following ones:

  

1) **setUserAuthenticationRequired(true)**: available from API level 23, when set to true, the key can be used only if the user has been authenticated. Additionally, the key will become irreversibly invalidated once the secure lock screen is disabled, or when the secure lock screen is forcibly reset. 

  

2) **setUserAuthenticationValidityDurationSeconds(-1)**: available from API level 23, when set to -1 the key can only be unlocked using a biometric identity. If it is set to a different value, the key can be unlocked using a device screenlock too.

  

3) **setInvalidatedByBiometricEnrollment(true)**: available only from API level 24, when set to true, the key is irreversibly invalidated when a new biometric is enrolled, or when all existing biometrics are deleted. Consider that the value is true by default.

  

  

Before authenticating the user with biometrics, the cipher should be initialized in order to check if the key is still valid. This can be done as follows:

  

public Cipher getCipherForBiometrics() {

    try {

        final Cipher cipher = Cipher.getInstance(

                       KeyProperties.KEY\_ALGORITHM\_AES + "/"

                        + KeyProperties.BLOCK\_MODE\_CBC + "/"

                        + KeyProperties.ENCRYPTION\_PADDING\_PKCS7);

        final SecretKey key;

        final KeyStore keyStore =  KeyStore.getInstance(“AndroidKeyStore”);

        keyStore.load(null);

        key = (SecretKey) keyStore.getKey(KEY\_ALIAS, null);

        cipher.init(Cipher.DECRYPT\_MODE, key);

        return cipher;

    } catch (KeyPermanentlyInvalidatedException e) {

        return null;

    } catch (KeyStoreException | CertificateException | UnrecoverableKeyException | IOException

            | NoSuchAlgorithmException | InvalidKeyException | NoSuchPaddingException e) {

        throw new RuntimeException("Failed to init Cipher", e);

}

  

  

Once the cipher is properly initialised it should be used as an argument for the authenticate method in order to start the biometric authentication flow.

. . .

Cipher cipher = getCipherForBiometrics();

if (cipher != null) {

    biometricPrompt.authenticate(promptInfo, new BiometricPrompt.CryptoObject(cipher));

. . .

  

  

The biometric authentication flow is then managed by the Android platform, and the method onAuthenticationSucceeded is called upon a successful authentication.

It is worth considering that this method can also be called by using hooking techniques and tools such as Frida. The difference between a valid authentication flow and a tampered authentication flow is the BiometricPrompt.CryptoObject.

Indeed, when a valid authentication flow is performed the Android platform properly instantiate the cipher contained within the BiometricPrompt.CryptoObject, and then this must be used to decrypt critical data such as the aforementioned authentication token.

Instead, when this method is called by using hooking techniques the cipher is not properly instantiated and when using it to decrypt the data, an exception will be raised.

  

@Override

public void onAuthenticationSucceeded(BiometricPrompt.AuthenticationResult result) {

        Cipher cipher = result.getCryptoObject().getCipher();

        byte\[\] decrypted = cipher.doFinal(// get here authentication token encrypted);

        String authenticationToken = decrypted.toString();

        // save the authentication token somewhere

       . . . 

}

  

This implementation is secure even against hooking techniques because when calling the onAuthenticationSucceeded callback with Frida, the AuthenticationResult object does not contain a valid cipher instance since the used key, that has been defined as accessible only after a biometric authentication, has not been unlocked by the Android OS and the cipher will raise an Exception when trying to decrypt the data.

During the various assessments performed on mobile applications we’ve found different insecure implementation of the biometric authentication that looks like the following one:

@Override

public void onAuthenticationSucceeded(BiometricPrompt.AuthenticationResult result) {

        enterApplication(); 

}

  

This kind of implementation is insecure since does not make use of the  BiometricPrompt.CryptoObject contained in the AuthenticationResult object, but it assumes that the authentication has been properly validated since the method onAuthenticationSucceeded has been called and allows the user to enter the application.

It is worth considering that even implementation that makes use of the BiometricPrompt.CryptoObject could be insecure if they do not decrypt data that are necessary to login the user (such as an authentication token, JWTs and so on). Indeed even Exceptions could be captured using hooking techniques and could be ignored in order to continue the application flow. 

### Biometric Authentication in iOS

The iOS platform introduced the biometric authentication starting from iPhone 5s in 2013. At that time it supported only the fingerprint authentication known as Touch ID.

When Apple released the iPhone X, the Face ID was added as biometric option that could be used to authenticate a user.

The biometric authentication flow is usually implemented with the LocalAuthentication framework. It is worth considering however that the LocalAuthentication framework is an event-based procedure and can be bypassed with hooking techniques and tools such as Frida or Objection.

Unlike Android, the iOS platform allows to save arbitrary data within the Keychain defining the access criteria for every stored item.

In order to implement an effective biometric authentication, it is suggested to use the Keychain methods instead of the LocalAuthentication framework. Such approach consists in storing sensitive data (such as an authentication token) within the Keychain, and defining the proper access criteria so that the data can be used only after a successful biometric authentication.

In order to use the biometric authentication, it is required to check if the biometric hardware is available and if the user has enrolled biometric identitites. This can be done using the canEvaluatePolicy method as shown below:

  

var error: NSError? if context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: &error) { // handle biometric authentication . . .

  

The canEvaluatePolicy method with the deviceOwnerAuthenticationWithBiometrics flag, returns _true_ only if the hardware to authenticate the user through biometrics is available and if the user has enrolled biometric factors.

When storing sensitive data for a biometric authentication within the Keychain it is recommended to use the following flags:

1) kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly: requires that a passcode is set on the device. The data is accessible only with the device unlocked and it is deleted when the user deactivates the passcode.

2) kSecAccessControlBiometryCurrentSet/kSecAccessControlBiometryAny: requires a user to authenticate with biometrics (e.g. Face ID or Touch ID) before accessing the data in the Keychain item. When using kSecAccessControlBiometryCurrentSet, whenever the user adds a fingerprint or facial representation to the device, it will automatically invalidate the entry in the Keychain. This makes sure that the keychain item can only be unlocked by users that were enrolled when the item was added to the keychain.

The usage of the other flags should be avoided when storing data relative to biometric authentication since they do not mandatory require the usage of biometric factors to retrieve the data when accessing the application.

  

Following it is reported an example on how to securely save data in the Keychain for biometric authentication:

  

var error: Unmanaged<CFError>?

guard let accessControl = SecAccessControlCreateWithFlags(kCFAllocatorDefault,

                                                          kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly,

                                                          SecAccessControlCreateFlags.biometryCurrentSet,

                                                          &error) else {

    // failed to create AccessControl object

    return

}

var query: \[String: Any\] = \[:\]

query\[kSecClass as String\] = kSecClassGenericPassword

query\[kSecAttrLabel as String\] = "label\_for\_auth\_token" as CFString

query\[kSecAttrAccount as String\] = "App Account" as CFString

query\[kSecValueData as String\] = "here\_goes\_auth\_token".data(using: .utf8)! as CFData

query\[kSecAttrAccessControl as String\] = accessControl

  

let status = SecItemAdd(query as CFDictionary, nil)

if status == noErr {

    // successfully saved

} else {

    // error while saving

}

  

When requesting the sensitive data, the iOS platform will ask for biometric authentication returning data or nil depending if the biometric authentication was successful or not.

  

During the various assessments performed on mobile applications we’ve found different insecure implementation of the biometric authentication that make use of the _evaluatePolicy_ method and are similar to the following one:

let context = LAContext()

var error: NSError?

context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: "Authenticate with biometrrics to access the application") { success, evaluationError in

    guard success else {

        // Authentication Failed

    }

   enterApplication();

}

  

  

This kind of implementation is insecure since does not make use of the Keychain, but it assumes that the authentication has been properly validated since the success condition has been met and allows the user to use the application.

Using hooking techniques or tools such as Frida or Objection this kind of implementation could be bypassed without providing a valid biometric authentication.

It is worth considering that even implementations that make use of the Keychain could be bypassed if the proper flags are not set when storing the data in it.

Specifically the usage of the flag kSecAttrAccessibleWhenUnlockedThisDeviceOnly and kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly should be avoided since they do not require that a passcode has been previously set on the device and does not delete the data when the passcode is disabled. Furthermore if the device has no passcode, the data is always accessible since the device is considered always unlocked.

Finally the usage of the other _SecAccessControlCreateFlags_, except for the aforementioned _kSecAccessControlBiometryCurrentSet/kSecAccessControlBiometryAny_ should be avoided since they do not mandatory require a biometric authentication. Indeed the device passcode could be used as well.

  

  

### Conclusions

When implementing biometric authentication on mobile application it is recommended to always use solutions that relies on cryptography and secure hardware such as the Keystore for Android and the Keychain for iOS.

Event-based authentication implementation should be considered insecure since they could be easily bypassed on rooted or jailbroken devices by using hooking techniques or tools such as Frida or Objection.

Highly sensitive applications such as banking apps or financial related applications should always rely on strong implementations when using biometric authentication and they should delete the sensitive data when the biometric set is changed or completely disabled.

Finally, for sensitive applications it is also suggested to implement frameworks in order to enhance their resiliency by detecting rooted/jailbroken device or attacks that make use of hooking techniques in order to reduce the risks of being exploited.

  

#### References

Android

-   [https://developer.android.com/training/sign-in/biometric-auth](https://developer.android.com/training/sign-in/biometric-auth)
    
-   [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05f-Testing-Local-Authentication.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05f-Testing-Local-Authentication.md)
    
-   [https://source.android.com/security/biometric](https://source.android.com/security/biometric)
    

  

iOS

-   [https://developer.apple.com/documentation/localauthentication/logging\_a\_user\_into\_your\_app\_with\_face\_id\_or\_touch\_id](https://developer.apple.com/documentation/localauthentication/logging_a_user_into_your_app_with_face_id_or_touch_id)
    
-   [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x06f-Testing-Local-Authentication.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x06f-Testing-Local-Authentication.md)
    

  

This article is the result of research through the official Android and iOS developer guides, the OWASP Mobile Security Testing Guide (https://owasp.org/www-project-mobile-security-testing-guide/) and the assessment activities on mobile applications performed by Minded Security’s consultants.

  

  

#### Authors

-   Michele Tumolo 
-   Giuseppe Porcu

#### [Source](https://blog.mindedsecurity.com/feeds/7245193214801615697/comments/default)

<br/>
---
