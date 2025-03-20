---
title: "Handling Keyboard in React Native"
date: 2025-01-22
---

Managing the keyboard when it appears on the screen is crucial in mobile app development to ensure a seamless user experience. React Native provides several ways to handle this using components like `KeyboardAvoidingView`, `KeyboardAwareScrollView`, and platform-specific configurations. Below are some common approaches to manage the keyboard behavior effectively.

# Solution 1. Basic Usage with KeyboardAvoidingView

The `KeyboardAvoidingView` component is used to adjust the layout of your UI when the keyboard appears. Below is a typical implementation:  

```
import { SafeAreaView, KeyboardAvoidingView } from 'react-native';

<SafeAreaView style={{ flex: 1 }}>
  <KeyboardAvoidingView
    enabled
    behavior={isAndroid ? 'height' : 'padding'}
    style={{ flex: 1 }}
    keyboardVerticalOffset={isAndroid ? 10 : 0} // Adjust space before keyboard
  >
    {/* Your UI components */}
  </KeyboardAvoidingView>
</SafeAreaView>
```

**Key Props:**

- **behavior:** Specifies how the view will behave. Common values are '`height`', '`padding`', or '`position`'.
- **keyboardVerticalOffset:** Adds extra spacing to avoid overlapping.

# Solution 2. Android-Specific Configurations

For Android, you can configure the `MainActivity` in your app to handle the keyboard correctly.

**Before:**  

```
<activity
  android:name=".MainActivity"
  android:label="@string/app_name"
  android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
  android:windowSoftInputMode="adjustResize">
```

**After:**  

```
<activity
  android:name=".MainActivity"
  android:label="@string/app_name"
  android:configChanges="keyboard|keyboardHidden|orientation|screenSize">
```

Remove `android:windowSoftInputMode="adjustResize"` to let React Native handle the keyboard adjustments.

# Solution 3. Using `KeyboardAwareScrollView`

The `KeyboardAwareScrollView` from `react-native-keyboard-aware-scroll-view` provides additional flexibility and control.

**Example:**  

```
import { KeyboardAwareScrollView } from 'react-native-keyboard-aware-scroll-view';

<KeyboardAwareScrollView
  keyboardShouldPersistTaps='handled'
  showsVerticalScrollIndicator={false}
  contentContainerStyle={{ flex: 1 }}
  bounces={false}
  enableOnAndroid // Adjust based on need
  scrollEnabled={false} // Adjust based on need
  extraHeight={15} // Adjust based on need
  extraScrollHeight={15} // Adjust based on need
>
  {/* Your UI components */}
</KeyboardAwareScrollView>
```

**Key Props:**

- **keyboardShouldPersistTaps:** Ensures taps outside the keyboard dismiss it. Always set to 'handled'.
- **extraHeight:** Adds extra scroll height for better adjustment.
- **enableOnAndroid:** Enables keyboard behavior handling on Android.

# Handling Keyboard in Modals

When working with modals, you can combine `Modal` and `KeyboardAwareScrollView` for better keyboard handling.

**Example:**  

```
import { Modal } from 'react-native';
import { KeyboardAwareScrollView } from 'react-native-keyboard-aware-scroll-view';

<Modal
  isVisible={isModalVisible}
  supportedOrientations={['portrait']}
  transparent={true}
>
  <KeyboardAwareScrollView
    keyboardShouldPersistTaps='handled'
    showsVerticalScrollIndicator={false}
    contentContainerStyle={{ flex: 1 }}
    bounces={false}
    enableOnAndroid // Adjust based on need
    scrollEnabled={false} // Adjust based on need
    extraHeight={15} // Adjust based on need
    extraScrollHeight={15} // Adjust based on need
  >
    {/* Modal content */}
  </KeyboardAwareScrollView>
</Modal>
```

# Best Practices

- Always use keyboardShouldPersistTaps='handled' to ensure smooth interaction.
- Adjust `extraHeight` or `keyboardVerticalOffset` based on your design needs.
- Use platform-specific configurations to handle unique keyboard behaviors on iOS and Android.
- Test thoroughly with different screen sizes and keyboard types to ensure compatibility.

By following these methods, you can effectively handle keyboard behavior in your React Native app, providing a seamless and user-friendly experience.

Go to Source
