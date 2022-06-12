# Shell Scripting Notes

## **Table of Contents**
  - [Introduction](#introduction)
    - [What’s a shell?](#whats-a-shell)
    - [Factors that drove the evolution of shell:](#factors-that-drove-the-evolution-of-shell)
    - [Why Shell Script?](#why-shell-script)
  - [How to ?](#how-to-)
    - [Create a script](#create-a-script)
    - [Execute script](#execute-script)
    - [configure script to run as command](#configure-script-to-run-as-command)
  - [Shebang](#shebang)
  - [Variables](#variables)
    - [Introduction](#introduction-1)
    - [Set value to variable](#set-value-to-variable)
    - [Convention](#convention)
    - [Access value of variable](#access-value-of-variable)
  - [Command line arguments](#command-line-arguments)
    - [Introduction](#introduction-2)
    - [Different ways to process the arguments](#different-ways-to-process-the-arguments)
  - [Read Inputs](#read-inputs)
  - [Arithmetic Operations](#arithmetic-operations)
  - [Flow Control](#flow-control)
    - [conditional-logic](#conditional-logic)
    - [Loops](#loops)
  - [Additional Concepts](#additional-concepts)
    - [Exit codes](#exit-codes)
    - [Functions](#functions)
  - [Best Practices](#best-practices)

## Introduction

### What’s a shell?

1. It is a program(interpreter) whose job is to execute other programs on behalf of users
2. Types of shell:
   1. Thompson Shell
   2. Bourne Shell (sh)
   3. c shell (csh)
   4. Korn Shell (ksh)
   5. Bourne Again Shell (bash)

### Factors that drove the evolution of shell:

1. user convenience
2. programming

### Why Shell Script?

- Automate repetitive / complex tasks
  - Daily backups
  - Installation and patching of software on multiple servers
- monitoring
- Troubleshooting
- auditing
- Increase productivity

## How to ?

### Create a script

- Create a file with **`.sh`** or with out `.sh`
  ```bash
  $ vi script
  ```
  ```bash
  $  vi script.sh
  ```

### Execute script

- To make it executable, run
  ```bash
  $ chmod +x script.sh
  ```
  ```bash
  $ chmod +x script
  ```
- Execute script in below ways:
  ```bash
  $ bash script ## possible with or without '.sh' extension
  ```
  ```bash
  $ script ## possible with or without '.sh' extension
  ```
  ```bash
  $ bash script.sh
  ```
  ```bash
  $ script.sh
  ```
  ```sh
  $ sh script.sh ## runs in Bourne shell instead of bash
  ```

### configure script to run as command

- Whenever a command is run at a linux system, the O.S looks at the path configured in the **`$PATH`** environment variable to locate the executable or script for the command.
- If it cannot find the command in the **`$PATH`** then a **`command not found`** error will be thrown.

- To add our script as a command, append the path to the directory containing the script to the end of the $PATH variable.
  ```bash
  $ export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/me
  ```
- A better way to do this is to append the path to the directory to $PATH variable
  ```bash
  $ export PATH=$PATH:/home/me
  ```
- Run the command
  ```bash
  $ script
  ```
- To see the location of the command
  ```bash
  $ which script
  ```

## Shebang
- A `Shebang` statement is a line that you specify at the top of the shell script and is used to specify what shell a script must run in.
- Shebang instructs the shell to use a particular `shell` or interpreter for the script.
  ```bash
  #!/bin/bash 
  ## above line is to execute in bash
  ```
  ```bash
  #!/bin/sh 
  ## above line is to execute in sh
  ```
- 

## Variables

### Introduction

- A variable is a value that can vary or change
- Variable referes to memory location where value is stored

### Set value to variable

- To set the value for a variable
  ```bash
  $ name=Don
  ```

### Convention

- Variable name should consist of alphanumeric or underscore
- Case Sensitive

### Access value of variable

- Should access by prefixing $ before it's name
- To access the value for a variable
  ```bash
  $ echo $name
  ```

## Command line arguments

### Introduction

- Arguments passed to script while executing are known as command line arguments

### Different ways to process the arguments

1. Positional Arguments
   1. Arguments passed to a script are processed in the same order in which they’re sent.
   2. The indexing of the arguments starts at **`one`**, and the first argument can be accessed inside the script using **`$1`**.
   3. **`$0`** holds script name
      ```bash
      ## file contents of 'print_name' script
      echo "name: $1";
      ```
      ```bash
      $ print_name Don
      name: Don ##This is an output
      ```
2. Flags
   1. Flags is a common way of passing input to a script
   2. It improves user convenience and readability
   3. Passing input to the script, there’s a flag (usually a single letter) starting with a hyphen (-) before each argument.
   4. **`getopts`** function reads the flags in the input
   5. **`OPTARG`** refers to the corresponding values
      ```bash
      ## file contents of 'print_name' script
      while getopts u:p: flag
      do
        case "${flag}" in
          u) username=${OPTARG};;
          p) password=${OPTARG};;
        esac
      done
      echo "Username: $username";
      echo "Password: $password";
      ```
      ```bash
      $ print_name -u 'don' -p 'root'
      Username: don
      Password: root
      ```
3. Additional Practice of accessing arguments
   1. **`$@`** is the array of all the input parameters
   2. **`$#`** gives the number of parameters
   3. **`shift {index}`** will iterate to next argument mostly used with while loop
   ```bash
   i=1;
   for name in "$@"
   do
     echo "Roll - $i: $name";
     i=$((i + 1));
   done
   ```
   ```bash
   i=1;
   j=$#
   while [ $i -le $j ]
   do
     echo "Roll - $i: $1";
     i=$((i + 1));
     shift 1
   done
   ```

## Read Inputs

- To read input from user, use **`read`** command
  ```bash
  read -p "Enter name:" name
  ```
- **`-p`** flag is use to read input by prompting

## Arithmetic Operations

1. Can be calculated using **`expr`**
   ```bash
   $ expr 6 + 2   # output : 8
   $ expr 6 - 2   # output : 4
   $ expr 6 \* 2  # output : 12
   $ expr 7 / 2   # output : 3
   $ expr 7 % 2   # output : 1
   ```
2. Can be calculated using **`double parantesis`**
   ```bash
   $ echo $((6 + 2 )) # output : 8
   $ echo $((6 - 2 )) # output : 4
   $ echo $((6 * 2))  # output : 12
   $ echo $((7 / 2 )) # output : 3
   $ echo $((7 % 2 )) # output : 1
   $ echo $(( ++3 ))  # output : 4
   $ echo $(( --3 ))  # output : 2
   $ echo $(( 3++ ))  # output : 3
   $ echo $(( 3-- ))  # output : 3
   ```
3. `expr` and `double parentheses` only return decimal output, they do not support floating values.
4. For floating point, we use **`bc`**(basic calculator)
5. It works in an interactive mode and also you can input to another command as well
   ```bash
   $ echo 7 / 2 | bc -l  # output : 3.50000000000000000000
   ```

## Flow Control

### conditional-logic
- if
```bash
 if [ {condition} ]
 then
  # code if condition is true
  fi
```
- if-else
```bash
if [ {condition} ]
then
 # code if condition is true
else
 # code if conditions are false
fi
```
- if-elif-else
```bash
if [ {condition} ]
then
 # code if condition 1 is true
elif [ condition2 ]
then
 # code if condition 1 is false and condition 2 is true
elif [ condition3 ]
then
 # code if condition 1 is false and condition 2 is false and condition 3 is true
else
 # code if conditions are false
fi
```
- case (similar to `switch` in other languages)
  - Case statement is used for simplifying multiple condition check with multiple different choices.


```bash
case $var in
[a-z])
    echo "You entered a lowercase character."
    ;;
[A-Z])
    echo "You entered an uppercase character."
    ;;
[0-9])
    echo "You entered a digit."
    ;;
?)
    echo "You entered a special character."
    ;;
*)
    echo "You entered more than one character."
    ;;
esac
```

**_Conditional Operators_**

- Numbers:  
  | Command | Description |
  |---|---|
  | `[ $a -eq $b ]` | equal |
  | `[ $a -lt $b ]` | less than |
  | `[ $a -gt $b ]` | greater than |
  | `[ $a -le $b ]` | less than and equal |
  | `[ $a -ge $b ] `| greater than and equal |
  | `[ $a -ne $b ]` | not equal |
- String:  
  | Command | Description |
  |---|---|
  | `[ "$str1" = "$str2" ]` | checks if string are equal |
  | `[ "$str1" != "$str2" ]` | checks if string are not equal |
  | `[ -n "$str1" ]` | checks if string length is greater than zero |
  | `[ -z "$str1" ]` | checks if string length is zero | 
- File:  
  | Command | Description |
  |---|---|
  |`[ -f $fname ]`| checks file or not |
  |`[ -d $fname ]`| checks directory or not |
  |`[ -c $fname ]`| checks character space file or not |
  |`[ -b $fname ]`| checks block space file or not |
  |`[ -r $fname ]`| checks has read permission or not |
  |`[ -w $fname ]`| checks has write permission or not |
  |`[ -x $fname ]`| checks has execute permission or not |
  |`[ -s $fname ]`| checks size is greater than 0 or not |
- Logical:  
  - OR:

    `$a -eq $b -o  $str1" = "$str2` 

    `$a -eq $b | $str1" = "$str2` 
  - AND:

    `$a -eq $b -a  $str1" = "$str2` 

    `$a -eq $b & $str1" = "$str2` 

### Loops

- While
  - Commands gets executed till the condition is **true**
    ```bash
    i = 0
    while [ $i -le 10 ] 
    do
      echo $i
      i=`expr $i+1` 
    done
    ```

- Until
  - Commands gets executed till the condition gets **true**
    ```bash
    i = 0
    until [ $i -ge 10 ] 
    do
      echo $i
      i=`expr $i+1` 
    done
    ```

- For
  - `for` loops is used when you want to run same command multiple times
  - Execute a command or a set of commands many times
  - Iterate through files
  - Iterate through lines within a file
  - Iterate through the output of a command
    ```bash
    for i in {1..10}
    do
      echo $i
    done
    ```
    ```bash
    for name in $( cat names.txt)
    do
      echo $name
    done
    ```
    ```bash
    for i in 1 2 3 4 5
    do
      echo $i
    done
    ```

## Additional Concepts

### Exit codes
- Some times we need to know the exit code of previous command either to retry or alert
- Command to check the exit code of the the last executed command is echo `$?`

### Functions
- Functions within a shell script is a piece of code or a block of code that perform a particular function that can be reused.
- As shell is an interpreter, your Function must always be defined **first** before calling it, if not then it will give error.
- When to use Functions?
  - Break up large script that performs many different tasks
  - Installing packages
  - Adding users
  - Configuring firewalls
  - Perform Mathematical calculations
  ```bash
  function add() {
    return $(($1 + $2))
  }

  add 2 3 # returns 5 
  ```

## Best Practices

- Don't use extension for file if you want it to make it a command
- Variable names must be in lower-case with underscores to separate words
- Design script to be reusable
- Avoid neccessity of editing before executing by using Command line arguments
