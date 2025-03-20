---
title: "Creating Custom Inputs for Regex Validation in React and React Native"
date: 2025-01-12
---

Validation is a key aspect of form handling, ensuring that user input adheres to specific rules. In this article, we'll explore how to create reusable custom input components with regex validation for React and React Native, using examples like phone numbers, credit cards, and CVCs.

### Table of Contents

- Why Custom Inputs?
- Getting Started
- Custom Input Component
- Adding Regex Validation with `zod`
- Building the Form
- Conclusion

### Links

- Source Code
- Live Demo

### Why Custom Inputs?

Using custom inputs lets you:

- Standardize validation logic across forms.
- Enhance the user experience with input masks and clear error messages.
- Reuse components in both React and React Native.

### Getting Started

#### Prerequisites

Make sure you have the following dependencies installed:

- `react-hook-form` for form handling.
- `zod` and `@hookform/resolvers/zod` for schema-based validation.
- `react-native-mask-input` for masked inputs in React Native.
- `styled-components` for styling.

```
npm install react-hook-form zod @hookform/resolvers zod react-native-mask-input styled-components
```

### Custom Input Component

Here is the reusable `StyledInput` component:

#### Code for React Native

```
import React from "react";
import {
  Controller,
  FieldPath,
  FieldValues,
  useFormContext,
} from "react-hook-form";
import { TextInputProps, View } from "react-native";
import styled from "styled-components/native";

const InputContainer = styled.View`
  width: 100%;
`;

const Label = styled.Text`
  font-size: 16px;
  color: ${({ theme }) => theme.colors.primary};
`;

const InputBase = styled.TextInput`
  flex: 1;
  font-size: ${({ theme }) => `${theme.fontSizes.base}px`};
  color: ${({ theme }) => theme.colors.primary};
  height: ${({ theme }) => `${theme.inputSizes.base}px`};
  border: 1px solid ${({ theme }) => theme.colors.border};
  border-radius: 8px;
  padding: 8px;
`;

const ErrorMessage = styled.Text`
  font-size: 12px;
  color: ${({ theme }) => theme.colors.error};
`;

interface StyledInputProps<TFieldValues extends FieldValues>
  extends TextInputProps {
  label: string;
  name: FieldPath<TFieldValues>;
}

export function StyledInput<TFieldValues extends FieldValues>({
  label,
  name,
  ...inputProps
}: StyledInputProps<TFieldValues>) {
  const { control, formState } = useFormContext<TFieldValues>();
  const { errors } = formState;
  const errorMessage = errors[name]?.message as string | undefined;

  return (
    <InputContainer>
      <Label>{label}</Label>
      <Controller
        control={control}
        name={name}
        render={({ field: { onChange, onBlur, value } }) => (
          <InputBase
            onBlur={onBlur}
            onChangeText={onChange}
            value={value}
            {...inputProps}
          />
        )}
      />
      {errorMessage && <ErrorMessage>{errorMessage}</ErrorMessage>}
    </InputContainer>
  );
}
```

#### Code for React

The same logic applies, but replace `TextInput` with HTML's `<input>` and style it accordingly.

### Adding Regex Validation with `zod`

Define masks and validators for inputs like phone numbers and credit cards:  

```
import * as zod from "zod";
import { Mask } from "react-native-mask-input";

const turkishPhone = {
  mask: [
    "+",
    "(",
    "9",
    "0",
    ")",
    " ",
    /d/,
    /d/,
    /d/,
    " ",
    /d/,
    /d/,
    /d/,
    "-",
    /d/,
    /d/,
    "-",
    /d/,
    /d/,
  ],
  validator: /^+(90) d{3} d{3}-d{2}-d{2}$/,
  placeholder: "+(90) 555 555-55-55",
};

const creditCard = {
  mask: [
    /d/,
    /d/,
    /d/,
    /d/,
    "-",
    /d/,
    /d/,
    /d/,
    /d/,
    "-",
    /d/,
    /d/,
    /d/,
    /d/,
    "-",
    /d/,
    /d/,
    /d/,
    /d/,
  ],
  validator: /^d{4}-d{4}-d{4}-d{4}$/,
  placeholder: "4242-4242-4242-4242",
};

const cvc = {
  mask: [/d/, /d/, /d/],
  validator: /^d{3}$/,
  placeholder: "123",
};

const schema = zod.object({
  phone: zod.string().regex(turkishPhone.validator, "Invalid phone number"),
  creditCard: zod
    .string()
    .regex(creditCard.validator, "Invalid credit card number"),
  cvc: zod.string().regex(cvc.validator, "Invalid CVC"),
});
```

### Building the Form

Use `react-hook-form` with `zod` for validation:  

```
import { FormProvider, useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

export default function FormScreen() {
  const form = useForm({
    resolver: zodResolver(schema),
    mode: "onBlur",
  });

  return (
    <FormProvider {...form}>
      <StyledInput
        name="phone"
        label="Phone"
        placeholder={turkishPhone.placeholder}
      />
      <StyledInput
        name="creditCard"
        label="Credit Card"
        placeholder={creditCard.placeholder}
      />
      <StyledInput name="cvc" label="CVC" placeholder={cvc.placeholder} />
      <button type="submit">Submit</button>
    </FormProvider>
  );
}
```

### Conclusion

By creating reusable custom inputs with regex validation, you can simplify form handling and maintain consistency across your projects. These components work seamlessly in both React and React Native, providing a great user experience.

Feel free to customize the masks, validators, and styles to suit your application's requirements. Happy coding!

Go to Source
