# PI approximation

<p align="center">
<img src="https://i.imgur.com/llW7yiO.png" height="80%" width="80%" alt="https://img.lovepik.com/free-png/20210918/lovepik-404-page-error-png-image_400217866_wh1200.png"/>
</p>

### [YouTube Explanation with Demonstration](https://google.com)

This project specified by CodeWars wants you to write a function that accepts an array of 10 integers (between 0 and 9), that returns a string of those numbers in the form of a phone number.

## Instructions

The aim of the kata is to try to show how difficult it can be to calculate decimals of an irrational number with a certain precision. We have chosen to get a few decimals of the number "pi" using the following infinite series (Leibniz 1646–1716):

PI / 4 = 1 - 1/3 + 1/5 - 1/7 + ... which gives an approximation of PI / 4.

http://en.wikipedia.org/wiki/Leibniz_formula_for_%CF%80

To have a measure of the difficulty we will count how many iterations are needed to calculate PI with a given precision of epsilon.

There are several ways to determine the precision of the calculus but to keep things easy we will calculate PI within epsilon of your language Math::PI constant.

In other words, given as input a precision of epsilon we will stop the iterative process when the absolute value of the difference between our calculation using the Leibniz series and the Math::PI constant of your language is less than epsilon.

Your function returns an array or a string or a tuple depending on the language (See sample tests) with

your number of iterations
your approximation of PI with 10 decimals
Example :
Given epsilon = 0.001 your function gets an approximation of 3.140592653839794 for PI at the end of 1000 iterations : since you are within epsilon of Math::PI you return

iter_pi(0.001) --> [1000, 3.1405926538]
Notes :
Unfortunately, this series converges too slowly to be useful, as it takes over 300 terms to obtain a 2 decimal place precision. To obtain 100 decimal places of PI, it was calculated that one would need to use at least 10^50 terms of this expansion!

About PI : http://www.geom.uiuc.edu/~huberty/math5337/groupe/expresspi.html

## Function Definition (iter-pi):

Purpose: Calculates an approximation of π using the Leibniz formula.
Parameters: Takes a single parameter epsilon, which specifies the desired precision of the approximation.

Local Variables:
```commandline
$M_PI = 3.14159265358979323846 # True value of π
$approximation = 0.0            # Initial approximation
$iterations = 0                 # Count of iterations
$i = 0                           # Index for series terms
```

## Looping to Calculate π:

A while ($true) loop is used to keep calculating terms until the approximation is within the specified precision (epsilon).

Term Calculation:
```commandline
$term = [math]::Pow(-1, $i) / (2 * $i + 1) # Calculate current term
```

The approximation is updated by adding the current term, and the number of iterations is incremented.
```commandline
$approximation += $term # Update approximation
```

The number of iterations is incremented:
```commandline
$iterations += 1 # Increment iteration count
```

After calculating the approximation of π, check whether it meets the desired precision:
```commandline
if ([math]::Abs($pi_approx - $M_PI) -lt $epsilon) {
    break # Exit the loop if the precision is met
}
```

## Returning the Result:

The function returns a string representation of the number of iterations and the approximation of π, formatted to ten decimal places:
```commandline
return "[${iterations}, $([math]::Round($pi_approx, 10).ToString('F10'))]"
```

## Testing Function (testing):

Takes two parameters: epsilon (the precision value) and expect (the expected output).
Calls iter-pi with the specified epsilon and compares the output to the expected result using an assertion:
```commandline
$ans = iter-pi $epsilon # Call iter-pi with epsilon
$ans | Should -Be $expect # Assert the output matches the expected result
```

## Fixed Tests Function (fixed):

Defines a series of tests with different values of epsilon and their corresponding expected results:
```commandline
testing 0.1 "[10, 3.0418396189]"   # Testing with 10 iterations for epsilon 0.1
testing 0.01 "[100, 3.1315929036]" # Testing with 100 iterations for epsilon 0.01
testing 0.001 "[1000, 3.1405926538]" # Testing with 1000 iterations for epsilon 0.001
```

Each call to testing checks whether the actual output of iter-pi matches the expected output.

## Describe Block:

Organizes the tests in a structured way using Describe and Context:
```commandline
Describe "iter-pi Tests" {
    Context "Fixed Tests" {
        It "Should Pass Fixed Tests" {
            fixed # Call the fixed function to run the tests
        } 
    }
}
```
The It block runs the fixed function to execute the tests, ensuring that all defined test cases pass.
```
##Full Code

```commandline
# Function to calculate an approximation of PI using the Leibniz series
function iter-pi {
    # Specify the output type as a string
    [OutputType([string])]
    
    # Parameter to define the precision (epsilon)
    Param ([double]$epsilon)

    # Constant value of PI for comparison
    $M_PI = 3.14159265358979323846

    # Initialize variables for approximation and iteration count
    $approximation = 0.0
    $iterations = 0
    $i = 0

    # Infinite loop to calculate PI until the desired precision is reached
    while ($true) {
        # Calculate the current term in the series
        $term = [math]::Pow(-1, $i) / (2 * $i + 1)

        # Add the current term to the approximation
        $approximation += $term

        # Increment the number of iterations
        $iterations += 1

        # Calculate the current approximation of PI
        $pi_approx = 4 * $approximation

        # Check if the approximation is within the specified precision
        if ([math]::Abs($pi_approx - $M_PI) -lt $epsilon) {
            break # Exit the loop if the precision is met
        }

        # Increment the index for the next term
        $i += 1
    }

    # Return the result formatted as a string with 10 decimal places
    return "[${iterations}, $([math]::Round($pi_approx, 10).ToString('F10'))]"
}

# Function to test the output of the iter-pi function against expected results
function testing([double]$epsilon, [string]$expect) {
    # Call the iter-pi function with the provided epsilon value
    $ans = iter-pi $epsilon

    # Assert that the actual output matches the expected output
    $ans | Should -Be $expect
}

# Function to run a series of fixed tests
function fixed() {
    # Test cases with specific epsilon values and their expected outputs
    testing 0.1 "[10, 3.0418396189]"   # Testing with 10 iterations for epsilon 0.1
    testing 0.01 "[100, 3.1315929036]" # Testing with 100 iterations for epsilon 0.01
    testing 0.001 "[1000, 3.1405926538]" # Testing with 1000 iterations for epsilon 0.001
}

# Describe block to organize tests and define test context
Describe "iter-pi Tests" {
    Context "Fixed Tests" {
        # Test to ensure the fixed tests pass successfully
        It "Should Pass Fixed Tests" {
            fixed # Call the fixed function to run the tests
        } 
    }
}

```
