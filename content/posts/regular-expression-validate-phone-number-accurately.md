---
title: "Regular Expression Validate Phone Number Accurately"
date: 2025-01-02
categories: 
  - "security"
---

Regular expressions are powerful input validators and are especially so for formats that follow a predictable pattern, such as phone numbers. A well-constructed regex will ensure that the input is according to the expected format and reduces errors, hence improving data quality.

This article explores how the following regex is used in the validation of common phone number formats across Python, Java, and JavaScript:

## The Regular Expression

Here is the common regular expression that helps you to validate common phone number formats.

```js

/^(+?d{1,3}[-.s]?)?((?d{3})?[-.s]?)?d{3}[-.s]?d{4}$/
```

The example phone numbers that matches by the above regex.

```none

123-456-7890
(123) 456-7890
123.456.7890
123 456 7890
+1 123-456-7890
```

### How it works?

This regex validates phone numbers and accommodates various formats. Here’s how it works:

- **^ and $:** Ensures the entire string matches the pattern.
- **(+?d{1,3}\[-.s\]?)?:** Matches an optional country code:
    - `+?`: An optional “+” sign.
    - `d{1,3}`: One to three digits for the country code.
    - `[-.s]?`: Allows an optional separator (dash, dot, or space).
- **((?d{3})?\[-.s\]?)?:** Matches an optional area code:
    - `(?`: An optional opening parenthesis.
    - `d{3}`: Exactly three digits.
    - `)?`: An optional closing parenthesis.
    - `[-.s]?`: Allows an optional separator.
- **d{3}\[-.s\]?d{4}:** Matches the main phone number:
    - `d{3}`: Three digits.
    - `[-.s]?`: An optional separator.
    - `d{4}`: Four digits.

## Examples

Here is an few example of implementing the above regex in Python, Java, and JavaScript programing languages.

### Python Implementation

```python

import re

# Define the regex pattern
pattern = r'^(+?d{1,3}[-.s]?)?((?d{3})?[-.s]?)?d{3}[-.s]?d{4}$'

# Test phone numbers
phone_numbers = [
    "123-456-7890",
    "(123) 456-7890",
    "+1 123-456-7890",
    "123.456.7890",
    "123 456 7890",
    "invalid_number"
]

for number in phone_numbers:
    if re.match(pattern, number):
        print(f"Valid: {number}")
    else:
        print(f"Invalid: {number}")
```

### Java Implementation

```java

import java.util.regex.*;

public class PhoneNumberValidator {
    public static void main(String[] args) {
        String pattern = "^(\+?\d{1,3}[-.\s]?)?(\(?\d{3}\)?[-.\s]?)?\d{3}[-.\s]?\d{4}$";
        String[] phoneNumbers = {
            "123-456-7890",
            "(123) 456-7890",
            "+1 123-456-7890",
            "123.456.7890",
            "123 456 7890",
            "invalid_number"
        };

        for (String number : phoneNumbers) {
            if (Pattern.matches(pattern, number)) {
                System.out.println("Valid: " + number);
            } else {
                System.out.println("Invalid: " + number);
            }
        }
    }
}
```

### JavaScript Implementation

```js

const pattern = /^(+?d{1,3}[-.s]?)?((?d{3})?[-.s]?)?d{3}[-.s]?d{4}$/;

const phoneNumbers = [
    "123-456-7890",
    "(123) 456-7890",
    "+1 123-456-7890",
    "123.456.7890",
    "123 456 7890",
    "invalid_number"
];

phoneNumbers.forEach(number => {
    if (pattern.test(number)) {
        console.log(`Valid: ${number}`);
    } else {
        console.log(`Invalid: ${number}`);
    }
});
```

## Conclusion

The present regex would thus be flexible enough to be able to validate most commonly used phone number formats, while accurately rejecting those in the incorrect format. Various application integrations are easy in Python, Java, and JavaScript and are nicely described by this particular regex pattern. So use regex in order to get consistent results out of your users; deliver better customer experience with the validated phone number data.

The post Regular Expression Validate Phone Number Accurately appeared first on TecAdmin.
