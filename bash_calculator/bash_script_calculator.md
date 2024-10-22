# Simple Calculator Script

This Bash script acts as a simple calculator, allowing users to perform basic arithmetic operations. It prompts the user for two numbers and an operator, performs the calculation, and displays the result.

## Script Features

- **Basic Arithmetic Operations**: Supports addition, subtraction, multiplication, and division.
- **User Interaction**: Prompts the user for inputs and provides output in a user-friendly format.
- **Colored Output**: Uses colored messages to indicate the start and end of the script, as well as to display error messages.

## Bash Script

```bash
#!/bin/bash

##############################################################################
### Genius Marion
## 18th October 2024
######## Develop a simple calculator script
### - Create a script that:
### - Prompts the user to input two numbers and an operator (+, -, *, /)
### - Performs the calculation using conditional statements and `expr` or `bc`
### - Displays the result to the user
###############################################################################

## Functions to echo part of the script in color
```
#!/bin/bash
red_echo() {
    echo -e "\e[32m$1\e[0m"
}

green_echo() {
    echo -e "\e[31m$1\e[0m"
}

red_echo "Script is starting now"

read -p "Enter any number: " num1
read -p "Enter any number: " num2
read -p "Enter an operator(+, -, *, /): " op

if [[ "$op" = "+" ]]; then
    result=$(expr $num1 + $num2)
elif [ "$op" = "-" ]; then
    result=$(expr $num1 - $num2)
elif [ "$op" = "*" ]; then
    result=$(expr $num1 \* $num2)  # Note the backslash to escape *
elif [ "$op" = "/" ]; then
    result=$(echo "scale=2; $num1 / $num2" | bc)
else
    green_echo "Invalid operation"
    exit 1
fi

echo "Result: $result"

green_echo "End of script"

```

Script Breakdown
Colored Echo Functions:

red_echo(): Displays a starting message in green (using \e[32m escape code).
green_echo(): Displays ending or error messages in red (using \e[31m escape code).
User Input:

Prompts the user to enter two numbers and an operator (+, -, *, /).
Conditional Statements:

Performs the selected arithmetic operation using expr for addition, subtraction, and multiplication.
Uses bc for division to handle floating-point arithmetic.
Error Handling:

If an invalid operator is entered, the script displays an error message and exits.
Result Display:

Outputs the result of the calculation to the user.

