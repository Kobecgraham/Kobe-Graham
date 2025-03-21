
# Create Phone Number - CodeWars

<p align="center">
<img src="https://i.imgur.com/NRhIsoS.png" height="80%" width="80%" alt="https://img.lovepik.com/free-png/20210918/lovepik-404-page-error-png-image_400217866_wh1200.png"/>
</p>

### [YouTube Explanation with Demonstration](https://google.com)

This project specified by CodeWars wants you to write a function that accepts an array of 10 integers (between 0 and 9), that returns a string of those numbers in the form of a phone number.

## Function

This defines a new function named New-PhoneNumber that takes one parameter, $numbers, which is expected to be an array of integers.

```commandline
function New-PhoneNumber([int[]]$numbers)
```

## Input Validation (Range Check)

This checks if the length of the $numbers array is not equal to 10. If the length is not 10, the function throws an error with the message "Array must contain exactly 10 integers." This ensures that the function only processes arrays that have exactly 10 integers.

```commandline
if ($numbers.Length -ne 10) { throw "Array must contain exactly 10 integers." }
```

## Input Validation (Range Check):

This loop iterates through each element ($number) in the $numbers array. For each element, it checks if the value is less than 0 or greater than 9. If any element is outside the range 0 to 9, the function throws an error with the message "Array must contain integers between 0 and 9." This ensures that all integers in the array are within the valid range (0 to 9).

```commandline
foreach ($number in $numbers) { if ($number -lt 0 -or $number -gt 9) { throw "Array must contain integers between 0 and 9." } }
```
## Formatting the Phone Number

This line creates a formatted string using the -f format operator. The format string ({0}{1}{2}) {3}{4}{5}-{6}{7}{8}{9} specifies the desired phone number format, where {0} to {9} are placeholders for the corresponding elements in the $numbers array. The -f operator replaces each placeholder with the respective element from the $numbers array to create the final formatted phone number string.

```commandline
$phoneNumber = "({0}{1}{2}) {3}{4}{5}-{6}{7}{8}{9}" -f $numbers[0], $numbers[1], $numbers[2], $numbers[3], $numbers[4], $numbers[5], $numbers[6], $numbers[7], $numbers[8], $numbers[9]
```
## Return the Result:
This line returns the formatted phone number string from the function.

```commandline
return $phoneNumber
```

## Example Usage:

This calls the New-PhoneNumber function with an array of 10 integers as the argument.

```commandline
New-PhoneNumber -numbers @(1, 2, 3, 4, 5, 6, 7, 8, 9, 0)
```
This prints the returned phone number string to the output, which will be (123) 456-7890 based off the Example.

```commandline
Write-Output $formattedPhoneNumber
```
## Full Code

```commandline
function New-PhoneNumber([int[]]$numbers)
{
    # Step 1: Check if the input array has exactly 10 integers
    if ($numbers.Length -ne 10) {
        throw "Array must contain exactly 10 integers."
    }
    
    # Step 2: Check if all elements in the array are integers between 0 and 9
    foreach ($number in $numbers) {
        if ($number -lt 0 -or $number -gt 9) {
            throw "Array must contain integers between 0 and 9."
        }
    }

    # Step 3: Format the numbers into a phone number string
    $phoneNumber = "({0}{1}{2}) {3}{4}{5}-{6}{7}{8}{9}" -f $numbers[0], $numbers[1], $numbers[2], $numbers[3], $numbers[4], $numbers[5], $numbers[6], $numbers[7], $numbers[8], $numbers[9]
    
    # Step 4: Return the formatted phone number
    return $phoneNumber
}

# Example usage
$formattedPhoneNumber = New-PhoneNumber -numbers @(1, 2, 3, 4, 5, 6, 7, 8, 9, 0)
Write-Output $formattedPhoneNumber  # Output: (123) 456-7890
```
