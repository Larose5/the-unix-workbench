# Bash Programming

The last two chapters have discussed how to use the bash shell. Bash itself is a
little programming langauge, and this chapter we're going to discuss how you can
write your own computer programs in Bash. Programming in Bash is useful to know
because of how seamlessly it integreats with all of the command line programs
you've already learned. By the end of this chapter you should able to write your
own command line tools!

You can use `nano` to write all of the programs we're going to discuss in this
chapter, however I recommend using the [Atom](https://atom.io/) text editor 
because it's more user friendly.

Now let's create a new file called `math.sh` in the `~/Code/` directory
and let's open that file with either `nano` or Atom.


```bash
cd ~/Code/
nano math.sh
```

You should now have a new, clean text file open. Any code block in the following
chapters that starts with the code (or similar) below should indicate to you
that we're working on a particular text file.

```
#!/usr/bin/env bash
# File: math.sh
```

You do not need to add these lines to your file, though you should type exactly
what I have typed below these lines. **Note**: please type all of the lines out
for every program that we're going to write, do not copy and paste. Typing code
is a little different from typing an email, and you should practice typing the
code out yourself as much as possible. Both of these lines start with the pound
symbol (`#`) and in the Bash programming language anything that is typed after
a pound symbol is ignored. The pound symbol allows you to make **comments** in
your code which you can use to annotate code so that another human being who is
reading your code can understand how your program is designed to function.

If you're using `nano` or another shell-based text editor you should perhaps
open up two terminals, one where you can edit and save the programs you're
working on, and one where you can run your programs. One advantage of using Atom
is that you can keep Atom open in a separate window and then run your programs
in your terminal window.

## Math

The Bash programming language can do very basic arithetic, which we'll
demonstrate in this section.
Now that you have `math.sh` open in your prefered text editor type the following
into your text editor:

```
#!/usr/bin/env bash
# File: math.sh

expr 5 + 2
expr 5 - 2
expr 5 \* 2
expr 5 / 2
```

Save `math.sh` and then run this script in your shell:


```bash
bash math.sh
```

```
## 7
## 3
## 10
## 2
```

Let's break down what's going on in the Bash script you just created. Bash
executes programs in order from the fist line in your file to the last line.
The `expr` command can be used to **evaluate** Bash **expressions**.
An expression is just
a valid string of Bash code that, when run, produces a result. The arithmetic
operators that you're already familiar with for addition (`+`), substraction
(`-`), and multiplication (`*`) work like you would expect them to. Notice that
when doing multiplication you need to escape the the star character, otherwise
Bash thinks you're trying to create a regular expression! The division operator
(`/`) does not work as you might expect it to since 5 / 2 = 2.5. Bash does
**integer division**, which means that the result of dividing one number by
another is always rounded down to the nearest integer. Let's take a look at a
few examples on the command line:


```bash
expr 1 / 3
expr 10 / 3
expr 40 / 21
expr 40 / 20
```

```
## 0
## 3
## 1
## 2
```

The other numerical operator you should be aware of that you might not be
familiar with is the modulus operator (`%`). The modulus operator returns the
**remainder** after integer division. In integer division if A / B = C, and
A % B = D, then B * C + D = A. Let's take a look at some examples on the command
line:


```bash
expr 1 % 3
expr 10 % 3
expr 40 % 21
expr 40 % 20
```

```
## 1
## 1
## 19
## 0
```

Notice that when one number is completely divisible by another number then the
result of the modulus is zero.

If you want to more complex math, for example amth with fractions and numbers
with decimals then I highly suggest combining `echo` and **b**ench **c**alculator
program called `bc`. Open up a new file called `bigmath.sh` and type in the
following:

```
#!/usr/bin/env bash
# File: bigmath.sh

echo "22 / 7" | bc -l
echo "4.2 * 9.15" | bc -l
echo "(6.5 / 0.5) + (6 * 2.2)" | bc -l
```

Save `bigmath.sh` and then run this script in your shell:


```bash
bash bigmath.sh
```

```
## 3.14285714285714285714
## 38.430
## 26.2
```

You can pipe any mathematical string to `bc` with the `-l` flag in order to
use decimal numbers in your calculations.

### Summary

- Bash programs are executed in order from the first line in a file until the
last line.
- Anything wirtten after a pound sign (`#`) is a comment and is not executed by
Bash.
- You can do simple arithmetic with the `expr` command.
- For more complicated arithmetic by piping string of equations into `bc` using
`echo`.

### Exercises

1. Look at the `man` pages for `bc`.
2. Try doing some math in `bc` interactively.
3. Try writing some equations in a file and then provide that file as an
argument to `bc`.

## Variables

In Bash you can store data in variables. In chapter 4 we discussed environmental
variables that are set by your operating system. You can also create your own
variables. Make sure you follow these rules when you're naming variables:

- Every character should be lowercase.
- The variable name should start with a letter.
- The name should only contain alphanumeric characters and underscores (`_`).
- Words in the name should be seperated by underscores.

If you follow those rules then you can avoid accidentally overwriting data
stored in environmental variables.

You can assign data to a variable using the equals sign (`=`). The data you
store in a variable can either be a string or a number. Let's create a variable
now on the command line:


```bash
chapter_number=5
```

The variable name is one the left hand side of the equals sign, and the data
which will be stored in that variable is on the right hand side of the equals
sign. Notice that there are no spaces on either side of the equals sign, this
is not allowed when assigning variables:


```bash
chapter_number = 5
```

```
## Error in running command bash
```

In order to print the data in a variable, also called the value of a variable,
we can use `echo`. When you want to retrieve the value of a variable you must
use the dollar sign (`$`) before the name of the variable. Let's try this out:


```bash
echo $chapter_number
```

```
## 5
```

You can modify the value of a variable using arithetic operators by using the
`let` command:


```bash
let chapter_number=$chapter_number+1
echo $chapter_number
```

```
## 6
```

You can also store strings in variables:


```bash
the_empire_state="New York"
echo $the_empire_state
```

```
## New York
```

Occasionally you might want to run a command like you would on the command line
and store the result of that command in a variable. We can do this by wrapping
the command in a dollar sign and parentheses (`$( )`) around a command. For
example if I wanted to store the number of lines in `math.sh`:


```bash
math_lines=$(cat math.sh | wc -l)
echo $math_lines
```

```
## 7
```

Variable names with a dollar sign can also be used inside of other strings in
order to insert the the value of the variable into the string:


```bash
echo "I went to school in $the_empire_state."
```

```
## I went to school in New York.
```

When writing a Bash script, the script gives you a few variables for free. Let's
create a new file called `vars.sh` with the following code:

```
#!/usr/bin/env bash
# File: vars.sh

echo "Script arguments: $@"
echo "First arg: $1. Second arg: $2."
echo "Number of arguments: $#"
```

### Summary

- Variables can be assigned with the equal sign (`=`) operator.
- Strings or numbers can be assigned to variables.
- The value of a variable can be accessed with the dollar sign (`$`) before the
variable name.
- You can use the dollar sign and parentheses syntax to execute a command and
save the output in a variable.

### Exercises

1. Write a Bash program where you assign two numbers to different variables,
and then the program prints the sum of those variables.
2. Write another Bash program where you assign two strings to different
variables, and then the program prints both of those strings. Write a version
where the strings are printed on the same line, and a version where the strings
are printed on different lines.

## User Input

## If/Else

## Arrays

## Braces

## Loops

## Functions

## Writing Programs

### Exercises

Write a program that takes one number as an argument and prints
"the number is even" if the number is even, "the number is odd" if the number
is odd, and "the number is less than 1" if the number is less than 1.


```bash
evenodd 6
```

```
## the number is even
```


```bash
evenodd 19
```

```
## the number is odd
```


```bash
evenodd 0
```

```
## the number is less than 1
```

Write a program that takes many numbers as arguments and prints the largest
number. Add a flag to specify whether it prints the minimum or maximum number.


```bash
findn -max 8 2 9 4 0 3
```

```
## 9
```

Write a program that takes one number as an argument and prints all of the
numbers