---
title: "How to Build a Centered Input Field for Amounts in React Native"
date: 2025-01-07
---

A clean, centered input field for entering amounts is a small but significant part of any financial app. It enhances user experience, provides clarity, and ensures ease of use. In this article, we’ll walk through creating a reusable **`CurrencySection`** component in React Native.

This component is simple yet powerful, featuring:

- Dynamic height adjustment for the input field.
- Automatic formatting for amounts in a specified currency.
- Inline error messaging for better user feedback.

Let’s dive into the details!

### What the `CurrencySection` Does

The `CurrencySection` component is designed to:

1. Allow users to input amounts in a visually appealing, centered format.
2. Automatically format amounts into the specified currency.
3. Provide flexibility to customize elements such as the currency symbol, placeholder text, and error messages.

### Implementation

Here’s the full implementation of the `CurrencySection` component:  

```
/* eslint-disable react-native/no-inline-styles */
import React, {useCallback, useState} from 'react';
import {
  Keyboard,
  NativeSyntheticEvent,
  Platform,
  StyleSheet,
  TextInput,
  TextInputContentSizeChangeEventData,
  TextInputProps,
  View,
} from 'react-native';
import {
  formatAmountInCurrency,
  MAX_AMOUNT_LENGTH_CURRENCY_SECTION,
  trimNumber,
} from '../../utils/functions';
import colors from '../../utils/helpers/colors';
import {InlineError} from '../../utils/helpers/Reusables';
import AppText from '../texts/appText';

interface CurrencySectionProps extends TextInputProps {
  title?: string;
  amount?: string;
  error?: string;
  onChangeAmount?: (amount: string) => void;
  isEditable?: boolean;
  currency?: string;
}

const CurrencySection: React.FC<CurrencySectionProps> = ({
  title = 'Enter amount',
  amount = '',
  error = '',
  onChangeAmount,
  isEditable = true,
  currency = '₦',
  ...props
}) => {
  const [height, setHeight] = useState(42);

  const onChangeText = useCallback(
    (text: string) => {
      onChangeAmount?.(trimNumber(text));
    },
    [onChangeAmount],
  );

  const onBlur = useCallback(() => {
    if (amount) {
      const formattedAmount = formatAmountInCurrency(
        Number(amount.replaceAll(',', '')),
      ).replaceAll(',', '');
      onChangeAmount?.(formattedAmount);
    }
  }, [amount, onChangeAmount]);

  const handleContentSizeChange = useCallback(
    (e: NativeSyntheticEvent<TextInputContentSizeChangeEventData>) => {
      setHeight(e.nativeEvent.contentSize.height);
    },
    [],
  );

  return (
    <View style={styles.amountContainerStyle}>
      {title ? <AppText size={15}>{title}</AppText> : null}
      <View style={[styles.rowContainer, {alignItems: 'center'}]}>
        <AppText size={36} weight="bold">
          {currency}
        </AppText>
        <TextInput
          value={amount}
          style={[
            styles.amountInputTextStyle,
            !amount?.length && styles.emptyInputFieldStyle,
            {height},
          ]}
          onContentSizeChange={handleContentSizeChange}
          placeholderTextColor={colors.grey}
          underlineColorAndroid="transparent"
          placeholder="0.00"
          keyboardType="numeric"
          returnKeyType="done"
          onChangeText={onChangeText}
          onBlur={onBlur}
          editable={isEditable}
          multiline
          maxLength={MAX_AMOUNT_LENGTH_CURRENCY_SECTION}
          onSubmitEditing={Keyboard.dismiss}
          {...props}
        />
      </View>

      {error?.toString()?.length > 0 && (
        <InlineError
          style={{marginTop: 10, padding: 10, borderRadius: 18}}
          message={error}
        />
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  amountContainerStyle: {
    alignItems: 'center',
    backgroundColor: 'transparent',
  },
  rowContainer: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  amountInputTextStyle: {
    fontSize: 36,
    fontWeight: '600',
    backgroundColor: 'transparent',
    ...(Platform.OS === 'ios' ? {paddingBottom: 6} : {}),
  },
  emptyInputFieldStyle: {width: 100},
});

export default CurrencySection;
```

![Example of the centered input field for the currency section](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7xk3z7wdgl7vch14fpu0.png)

### Highlights of the `CurrencySection` Component

1. **Dynamic Height Adjustment**  
      
    The input field dynamically resizes to match its content size, ensuring a clean and adaptive design.
    
2. **Automatic Currency Formatting**  
      
    The `formatAmountInCurrency` function ensures amounts are formatted appropriately, making them easier to read.
    
3. **Customizable and Reusable**  
      
    With props for titles, currency symbols, and error messages, this component is flexible enough for various use cases.
    
4. **Error Feedback**  
      
    Inline error messages provide immediate feedback without disrupting the flow of the UI.
    

### Styling Choices

- **Alignment**: Centering the input and currency symbol creates a clean, balanced layout.
- **Font Size and Weight**: The bold and large font for the currency and input fields ensures they stand out.
- **Colors**: Neutral placeholder colors and transparent backgrounds maintain focus on the content.

### Conclusion

This `CurrencySection` component is a simple yet effective addition to any financial app. It’s designed to handle core input requirements while keeping the UI visually appealing and functional.

Give it a try, customize it to suit your app’s style, and let your users enjoy a seamless experience!

_Feel free to share your thoughts or questions in the comments._

Go to Source
