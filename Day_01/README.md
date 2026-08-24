# Day 1 — Tcl Fundamentals

## Objective

Learn the fundamental concepts of Tcl and understand how to create, execute and debug a basic Tcl script on Ubuntu Linux.

Day 1 uses the standard Tcl interpreter:

```text
tclsh
```

No Cadence EDA tool is required for Day 1.

---

## Environment

Operating System:

```text
Ubuntu Linux
```

Tcl interpreter:

```text
tclsh
```

Execution methods:

```bash
tclsh
```

and:

```bash
tclsh filename.tcl
```

---

# Topics

## 1. Tcl Introduction

* What is Tcl?
* Tcl interpreter
* Tcl commands
* Command arguments
* Comments

## 2. Basic Commands

* `puts`
* `set`
* `expr`

## 3. Variables

* Creating variables
* Reading variables
* Variable substitution
* Updating variables

## 4. Substitution

* Variable substitution: `$`
* Command substitution: `[ ]`
* Grouping with `{ }`
* Quoting with `" "`

## 5. Arithmetic

* Addition
* Subtraction
* Multiplication
* Division
* Modulus

## 6. Script Execution

* Interactive Tcl
* `.tcl` script
* Running a Tcl script from Linux

---

# Setup

Check Tcl:

```bash
tclsh
```

Inside Tcl:

```tcl
puts "Hello Tcl"
```

Expected output:

```text
Hello Tcl
```

Exit Tcl:

```tcl
exit
```

If Tcl is not installed:

```bash
sudo apt update
sudo apt install tcl
```

Check Tcl version:

```bash
tclsh <<< 'puts [info patchlevel]'
```

---

# First Tcl Program

Create:

```text
hello.tcl
```

Code:

```tcl
puts "Hello Tcl"
```

Run:

```bash
tclsh hello.tcl
```

Output:

```text
Hello Tcl
```

---

# Variables

Tcl uses `set` to create variables.

```tcl
set a 10
set b 20
```

Read the variables:

```tcl
puts $a
puts $b
```

Output:

```text
10
20
```

---

# Arithmetic

Tcl uses `expr` for arithmetic expressions.

```tcl
set a 10
set b 20

set sum [expr {$a + $b}]

puts $sum
```

Output:

```text
30
```

---

# Important Tcl Syntax

## Variable substitution

```tcl
set a 10
puts $a
```

`$a` means:

```text
Get the value stored in variable a
```

---

## Command substitution

```tcl
set a 10
set b 20

puts [expr {$a + $b}]
```

`[ ... ]` means:

```text
Execute the command inside the brackets
and substitute its result.
```

---

## Braces

```tcl
set a 10

puts [expr {$a + 5}]
```

Braces prevent Tcl from performing normal substitutions before `expr` receives the expression.

For now, remember this common style:

```tcl
expr {$a + $b}
```

---

## Comments

A comment begins with `#`.

```tcl
# This is a comment

set a 10
puts $a
```

---

# Practical Program

Create:

```text
basic.tcl
```

Code:

```tcl
# Day 1 Tcl Practice

set name "Tcl"
set a 10
set b 20

set sum [expr {$a + $b}]
set difference [expr {$a - $b}]
set product [expr {$a * $b}]
set division [expr {$a / $b}]

puts "Language = $name"
puts "A = $a"
puts "B = $b"
puts "Sum = $sum"
puts "Difference = $difference"
puts "Product = $product"
puts "Division = $division"
```

Run:

```bash
tclsh basic.tcl
```

---

# VLSI Connection

Although Day 1 uses simple examples, the same Tcl concepts are used later in EDA tools.

For example:

```tcl
set top_design "my_design"
set clock_period 10

puts "Top Design = $top_design"
puts "Clock Period = $clock_period"
```

Later, these variables can be used in Genus/Innovus scripts.

Example concept:

```tcl
set TOP my_design
set REPORT_DIR ./reports

puts "Design = $TOP"
puts "Reports = $REPORT_DIR"
```

This is the beginning of EDA automation.

---

# Execution Flow

The Day 1 execution flow is:

```text
Ubuntu
   |
   v
Terminal
   |
   v
tclsh
   |
   v
day01.tcl
   |
   v
Tcl Interpreter
   |
   v
Output
```

---

# Day 1 Exercises

## Exercise 1

Create three variables:

```text
A = 25
B = 15
C = 10
```

Calculate:

```text
A + B
A - B
A * C
A / C
```

---

## Exercise 2

Create variables:

```text
name
age
city
```

Print them using one `puts` command.

---

## Exercise 3

Calculate the area of a rectangle:

```text
length = 20
width = 10
```

Expected:

```text
Area = 200
```

---

## Exercise 4

Calculate:

```text
frequency = 100 MHz
period = 1 / frequency
```

Use Tcl arithmetic.

---

## Exercise 5 — VLSI

Create:

```text
clock_period = 10
data_delay = 6
```

Calculate:

```text
slack = clock_period - data_delay
```

Expected:

```text
Slack = 4
```

This is only a Tcl arithmetic exercise; actual STA is introduced later.

---

# Commands Learned

| Command | Purpose                         |
| ------- | ------------------------------- |
| `puts`  | Print output                    |
| `set`   | Create/read variable            |
| `expr`  | Evaluate expression             |
| `info`  | Get Tcl interpreter information |
| `exit`  | Exit Tcl                        |

---

# Important Syntax Learned

```text
$variable
```

Variable substitution.

```text
[command]
```

Command substitution.

```text
{expression}
```

Grouping without normal Tcl substitution.

```text
"string"
```

Quoted string with substitution enabled.

```text
# comment
```

Comment.

---

# Day 1 Completion Criteria

Day 1 is complete when I can independently:

* Start `tclsh`
* Create variables
* Print variables
* Perform arithmetic
* Use `expr`
* Understand `$`
* Understand `[ ]`
* Understand `{ }`
* Write a `.tcl` file
* Execute it using `tclsh`
* Read basic Tcl syntax

---

# Next Day

Day 2:

```text
Strings
Lists
Expressions
String commands
List commands
```

Execution environment:

```text
Ubuntu → tclsh → Day 2 Tcl scripts
```
