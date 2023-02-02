---
title: How to use custom functions with the MedTech service device mappings - Azure Health Data Services
description: This article describes how to use custom functions with MedTech service device mappings.
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: how-to
ms.date: 1/18/2023
ms.author: jasteppe
---

# How to use custom functions with device mappings

Many functions are available when using **JMESPath** as the expression language. Besides the functions available as part of the JMESPath specification, many more custom functions may also be used. This article describes the MedTech service-specific custom functions for use with the MedTech service [device mapping](how-to-configure-device-mappings.md) during the device message [normalization](understand-service.md#normalize) process.

> [!TIP]
> For more information on JMESPath functions, see the [JMESPath specification](https://jmespath.org/specification.html#built-in-functions).

## Function signature

Each function has a signature that follows the JMESPath specification. This signature can be represented as:

```jmespath
return_type function_name(type $argname)
```

The signature indicates the valid types for the arguments. If an invalid type is passed in for an argument, an error will occur.

> [!NOTE]
> When math-related functions are done, the end result **must** be able to fit within a [C# long](/dotnet/csharp/language-reference/builtin-types/integral-numeric-types#characteristics-of-the-integral-types) value. If the end result is unable to fit within a C# long value, then a mathematical error will occur.

## Exception handling

Exceptions may occur at various points within the event processing lifecycle. Here are the various points where they can occur:

|Action|When|Exceptions that may occur during template parsing|Outcome|
|------|----|-------------------------------------------------|-------|
|**Template parsing**|Each time a new batch of messages is received the Device mapping template is loaded and parsed.|Failure to parse the template.|System will attempt to reload and parse the latest device mapping template until parsing succeeds. No new messages will be processed until parsing is successful.|
|**Template parsing**|Each time a new batch of messages is received the Device mapping template is loaded and parsed.|Failure to parse any expressions.|System will attempt to reload and parse the latest device mapping template until parsing succeeds. No new messages will be processed until parsing is successful.|
|**Function Execution**|Each time a function is executed against data within a message.|Input data doesn't match that of the function signature.|System stops processing that message. The message isn't retried.|
|**Function execution**|Each time a function is executed against data within a message.|Any other exceptions listed in the description of the function.|System stops processing that message. The message isn't retried.|

## Mathematical functions

### add

```jmespath
number add(number $left, number $right)
```

Returns the result of adding the left argument to the right.

Examples:

| Given                       | Expression       | Result |
|-----------------------------|------------------|--------|
| n/a                         | add(10, 10)      | 20     |
| {"left": 40, "right": 50}   | add(left, right) | 90     |
| {"left": 0, "right": 50}    | add(left, right) | 50     |

### divide

```jmespath
number divide(number $left, number $right)
```

Returns the result of dividing the left argument by the right.

Examples:

| Given                       | Expression          | Result                           |
|-----------------------------|---------------------|----------------------------------|
| n/a                         | divide(10, 10)      | 1                                |
| {"left": 40, "right": 50}   | divide(left, right) | 0.8                              |
| {"left": 0, "right": 50}    | divide(left, right) | 0                                |
| {"left": 50, "right": 0}    | divide(left, right) | mathematic error: divide by zero |

### multiply

```jmespath
number multiply(number $left, number $right)
```

Returns the result of multiplying the left argument with the right.

Examples:

| Given                       | Expression            | Result |
|-----------------------------|-----------------------|--------|
| n/a                         | multiply(10, 10)      | 100    |
| {"left": 40, "right": 50}   | multiply(left, right) | 2000   |
| {"left": 0, "right": 50}    | multiply(left, right) | 0      |

### pow

```jmespath
number pow(number $left, number $right)
```

Returns the result of raising the left argument to the power of the right.

Examples:

| Given                         | Expression       | Result                     |
|-------------------------------|------------------|----------------------------|
| n/a                           | pow(10, 10)      | 10000000000                |
| {"left": 40, "right": 50}     | pow(left, right) | mathematic error: overflow |
| {"left": 0, "right": 50}      | pow(left, right) | 0                          |
| {"left": 100, "right": 0.5}   | pow(left, right) | 10                         |

### subtract

```jmespath
number subtract(number $left, number $right)
```

Returns the result of subtracting the right argument from the left.

Examples:

| Given                       | Expression            | Result |
|-----------------------------|-----------------------|--------|
| n/a                         | subtract(10, 10)      | 0      |
| {"left": 40, "right": 50}   | subtract(left, right) | -10    |
| {"left": 0, "right": 50}    | subtract(left, right) | -50    |

## String functions

### insertString

```jmespath
string insertString(string $original, string $toInsert, number pos)
```

Produces a new string by inserting the value of *toInsert* into the string *original*. The string will be inserted at position *pos* within the string *original*.

If the positional argument is zero based, the position of zero refers to the first character within the string. 

If the positional argument provided is out of range of the length of *original*, then an error will occur.

Examples:

| Given                                                     | Expression                                         | Result              |
|-----------------------------------------------------------|----------------------------------------------------|---------------------|
| {"original": "mple", "toInsert": "sa", "pos": 0}          | insertString(original, toInsert, pos)              | "sample"            |
| {"original": "suess", "toInsert": "cc", "pos": 2}         | insertString(original, toInsert, pos)              | "success"           |
| {"original": "myString", "toInsert": "!!", "pos": 8}      | insertString(original, toInsert, pos)              | "myString!!"        |
| {"original": "myString", "toInsert": "!!"}                | insertString(original, toInsert, length(original)) | "myString!!"        |
| {"original": "myString", "toInsert": "!!", "pos": 100}    | insertString(original, toInsert, pos)              | error: out of range |
| {"original": "myString", "toInsert": "!!", "pos": -1}     | insertString(original, toInsert, pos)              | error: out of range |

## Date functions

### fromUnixTimestamp

```jmespath
string fromUnixTimestamp(number $unixTimestampInSeconds)
```

Produces an [ISO 8061](https://en.wikipedia.org/wiki/ISO_8601) compliant time stamp from the given Unix timestamp. The timestamp is represented as the number of seconds since the Epoch (January 1 1970).

Examples:

| Given                 | Expression              | Result                  |
|-----------------------|-------------------------|-------------------------|
| {"unix": 1625677200} | fromUnixTimestamp(unix)  | "2021-07-07T17:00:00+0" |
| {"unix": 0}          | fromUnixTimestamp(unix)  | "1970-01-01T00:00:00+0" |

### fromUnixTimestampMs

```jmespath
string fromUnixTimestampMs(number $unixTimestampInMs)
```

Produces an [ISO 8061](https://en.wikipedia.org/wiki/ISO_8601) compliant time stamp from the given Unix timestamp. The timestamp is represented as the number of milliseconds since the Epoch (January 1 1970).

Examples:

| Given                    | Expression                | Result                  |
|--------------------------|---------------------------|-------------------------|
| {"unix": 1626799080000}  | fromUnixTimestampMs(unix) | "2021-07-20T16:38:00+0" |
| {"unix": 0}              | fromUnixTimestampMs(unix) | "1970-01-01T00:00:00+0" |

> [!TIP]
> See the MedTech service article [Troubleshoot MedTech service errors](troubleshoot-errors.md) for assistance fixing MedTech service errors. 

## Next steps

In this article, you learned how to use the MedTech service custom functions with the device mappings. 

To learn how to configure the MedTech service device mappings, see

> [!div class="nextstepaction"]
> [How to configure device mappings](how-to-configure-device-mappings.md)

FHIR&#174; is a registered trademark of Health Level Seven International, registered in the U.S. Trademark Office and is used with their permission.
