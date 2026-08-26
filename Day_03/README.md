# Day_03 — Conditions and Loops

## Objective

The objective of Day 03 is to learn how to make decisions and repeat operations in Tcl.

The main topics are:

* `if`
* `elseif`
* `else`
* `for`
* `foreach`
* `while`
* `break`
* `continue`
* Nested loops
* Conditions with expressions
* VLSI-oriented loop examples

Day 03 still uses **pure Tcl**.

Cadence Genus, Innovus and Tempus are not required yet.

---

# Environment

### Operating System

Ubuntu Linux

### Tcl Interpreter

`tclsh`

### Execution

Interactive mode:

```bash
tclsh
```

Script mode:

```bash
tclsh filename.tcl
```

---

# 1. Day 03 Learning Flow

```text
Variables
    |
    v
Expressions
    |
    v
Conditions
    |
    v
if / elseif / else
    |
    v
Loops
    |
    +------> for
    |
    +------> foreach
    |
    +------> while
    |
    v
Automation
    |
    v
VLSI / EDA Processing
```

---

# 2. Why Conditions and Loops Are Important

Suppose an EDA tool gives Tcl a list of 1000 cells.

We don't want to manually write:

```text
Check cell 1
Check cell 2
Check cell 3
...
Check cell 1000
```

Instead, Tcl can do:

```tcl
foreach cell $cells {
    # process cell
}
```

Similarly, conditions allow us to make decisions:

```tcl
if {$slack < 0} {
    puts "Timing violation"
}
```

This is the foundation of EDA automation.

---

# 3. if Statement

The `if` statement executes code when a condition is true.

Syntax:

```tcl
if {condition} {
    commands
}
```

Example:

```tcl
set a 10

if {$a > 5} {
    puts "A is greater than 5"
}
```

Output:

```text
A is greater than 5
```

---

# 4. Comparison Operators

Important Tcl comparison operators:

| Operator | Meaning               |
| -------- | --------------------- |
| `>`      | Greater than          |
| `<`      | Less than             |
| `>=`     | Greater than or equal |
| `<=`     | Less than or equal    |
| `==`     | Equal                 |
| `!=`     | Not equal             |

Example:

```tcl
set a 10

if {$a == 10} {
    puts "A is 10"
}
```

---

# 5. if / else

Syntax:

```tcl
if {condition} {
    commands
} else {
    commands
}
```

Example:

```tcl
set slack 2.5

if {$slack >= 0} {
    puts "Timing is OK"
} else {
    puts "Timing violation"
}
```

Output:

```text
Timing is OK
```

---

# 6. if / elseif / else

Use `elseif` when there are multiple conditions.

Example:

```tcl
set slack -1.5

if {$slack > 0} {
    puts "Positive slack"
} elseif {$slack == 0} {
    puts "Zero slack"
} else {
    puts "Negative slack"
}
```

Output:

```text
Negative slack
```

---

# 7. Nested if

An `if` can be placed inside another `if`.

Example:

```tcl
set a 20
set b 10

if {$a > 0} {

    if {$b > 0} {
        puts "Both values are positive"
    }

}
```

---

# 8. Logical Operators

Tcl supports logical operators.

Important operators:

| Operator | Meaning |   |    |
| -------- | ------- | - | -- |
| `&&`     | AND     |   |    |
| `        |         | ` | OR |
| `!`      | NOT     |   |    |

Example:

```tcl
set a 10
set b 20

if {$a > 5 && $b > 10} {
    puts "Both conditions are true"
}
```

---

# 9. VLSI Example — Slack Check

```tcl
set slack -0.25

if {$slack >= 0} {
    puts "PASS: Timing is clean"
} else {
    puts "FAIL: Timing violation"
}
```

Output:

```text
FAIL: Timing violation
```

This is a simple Tcl example of the type of decision-making used in timing automation.

---

# 10. for Loop

The `for` loop repeats a block of commands.

Syntax:

```tcl
for {initialization} {condition} {increment} {
    commands
}
```

Example:

```tcl
for {set i 0} {$i < 5} {incr i} {
    puts $i
}
```

Output:

```text
0
1
2
3
4
```

---

# 11. Understanding the for Loop

Consider:

```tcl
for {set i 0} {$i < 5} {incr i} {
    puts $i
}
```

Execution:

```text
set i 0
   |
   v
i < 5 ?
   |
  YES
   |
   v
puts i
   |
   v
incr i
   |
   v
i < 5 ?
```

When:

```text
i = 5
```

the condition becomes false and the loop stops.

---

# 12. incr Command

`incr` increments a variable.

Example:

```tcl
set i 0

incr i

puts $i
```

Output:

```text
1
```

You can increment by a specific value:

```tcl
set i 10

incr i 5

puts $i
```

Output:

```text
15
```

---

# 13. foreach Loop

`foreach` is one of the most important Tcl commands for EDA automation.

Syntax:

```tcl
foreach variable list {
    commands
}
```

Example:

```tcl
set gates {AND OR NAND NOR XOR}

foreach gate $gates {
    puts $gate
}
```

Output:

```text
AND
OR
NAND
NOR
XOR
```

---

# 14. Understanding foreach

Given:

```tcl
set gates {AND OR NAND NOR XOR}
```

Tcl processes:

```text
gates
  |
  +--> AND
  |
  +--> OR
  |
  +--> NAND
  |
  +--> NOR
  |
  +--> XOR
```

For each element:

```tcl
foreach gate $gates {
    puts $gate
}
```

---

# 15. foreach with Index

Example:

```tcl
set gates {AND OR NAND NOR XOR}

set index 0

foreach gate $gates {
    puts "Gate $index = $gate"
    incr index
}
```

Output:

```text
Gate 0 = AND
Gate 1 = OR
Gate 2 = NAND
Gate 3 = NOR
Gate 4 = XOR
```

---

# 16. foreach with Multiple Lists

Tcl can process multiple lists.

Example:

```tcl
set names {A B C}
set values {10 20 30}

foreach name $names value $values {
    puts "$name = $value"
}
```

Output:

```text
A = 10
B = 20
C = 30
```

---

# 17. while Loop

Syntax:

```tcl
while {condition} {
    commands
}
```

Example:

```tcl
set i 0

while {$i < 5} {
    puts $i
    incr i
}
```

Output:

```text
0
1
2
3
4
```

---

# 18. for vs while

### for

Use when the number of iterations or counter structure is known.

```tcl
for {set i 0} {$i < 10} {incr i} {
    puts $i
}
```

### while

Use when you want to continue while a condition remains true.

```tcl
while {$i < 10} {
    puts $i
    incr i
}
```

---

# 19. break

`break` immediately terminates a loop.

Example:

```tcl
for {set i 0} {$i < 10} {incr i} {

    if {$i == 5} {
        break
    }

    puts $i
}
```

Output:

```text
0
1
2
3
4
```

---

# 20. continue

`continue` skips the current iteration and moves to the next iteration.

Example:

```tcl
for {set i 0} {$i < 5} {incr i} {

    if {$i == 2} {
        continue
    }

    puts $i
}
```

Output:

```text
0
1
3
4
```

---

# 21. Nested Loops

A loop can contain another loop.

Example:

```tcl
for {set i 0} {$i < 3} {incr i} {

    for {set j 0} {$j < 3} {incr j} {

        puts "$i $j"
    }
}
```

Output:

```text
0 0
0 1
0 2
1 0
1 1
1 2
2 0
2 1
2 2
```

---

# 22. VLSI Example — Process Standard Cells

```tcl
set cells {
    NAND2_X1
    NAND2_X2
    NOR2_X1
    INV_X1
    BUF_X1
}

foreach cell $cells {
    puts "Processing cell: $cell"
}
```

Output:

```text
Processing cell: NAND2_X1
Processing cell: NAND2_X2
Processing cell: NOR2_X1
Processing cell: INV_X1
Processing cell: BUF_X1
```

Later, the list may come directly from an EDA database instead of being manually created.

Conceptually:

```tcl
foreach cell [get_cells *] {
    # process cell
}
```

This is one of the most important patterns you will encounter in EDA Tcl.

---

# 23. VLSI Example — Check Cell Names

```tcl
set cells {
    NAND2_X1
    NAND2_X2
    NOR2_X1
    INV_X1
}

foreach cell $cells {

    if {[string first "NAND" $cell] >= 0} {
        puts "$cell is a NAND cell"
    } else {
        puts "$cell is not a NAND cell"
    }
}
```

This combines:

```text
foreach
   +
if
   +
string command
```

This combination is very important for automation.

---

# 24. VLSI Example — Check Timing Paths

Suppose we have:

```tcl
set slacks {1.2 0.5 -0.2 2.1 -1.0}
```

Check every slack:

```tcl
foreach slack $slacks {

    if {$slack < 0} {
        puts "VIOLATION: Slack = $slack ns"
    } else {
        puts "PASS: Slack = $slack ns"
    }
}
```

Output:

```text
PASS: Slack = 1.2 ns
PASS: Slack = 0.5 ns
VIOLATION: Slack = -0.2 ns
PASS: Slack = 2.1 ns
VIOLATION: Slack = -1.0 ns
```

---

# 25. Combining Loops and Conditions

Example:

```tcl
set values {10 20 30 40 50}

foreach value $values {

    if {$value > 30} {
        puts "$value is greater than 30"
    }
}
```

Output:

```text
40 is greater than 30
50 is greater than 30
```

This pattern is fundamental to automation:

```text
Get objects
     |
     v
Loop through objects
     |
     v
Check condition
     |
     v
Perform action
```

---

# 26. Day 03 File Structure

Create:

```text
Day_03/
│
├── README.md
├── 01_if.tcl
├── 02_if_else.tcl
├── 03_elseif.tcl
├── 04_for_loop.tcl
├── 05_foreach.tcl
├── 06_while.tcl
├── 07_break_continue.tcl
├── 08_nested_loop.tcl
├── 09_vlsi_cells.tcl
├── 10_vlsi_timing.tcl
└── 11_practice.tcl
```

Run from Ubuntu:

```bash
tclsh 01_if.tcl
```

For example:

```bash
tclsh 05_foreach.tcl
```

---

# 27. Important Commands Learned

| Command    | Purpose                        |
| ---------- | ------------------------------ |
| `if`       | Conditional execution          |
| `elseif`   | Additional condition           |
| `else`     | Alternative condition          |
| `for`      | Counter-based loop             |
| `foreach`  | Iterate through a list         |
| `while`    | Repeat while condition is true |
| `break`    | Exit loop                      |
| `continue` | Skip current iteration         |
| `incr`     | Increment variable             |

---

# 28. Comparison Operators

```text
>       Greater than
<       Less than
>=      Greater than or equal
<=      Less than or equal
==      Equal
!=      Not equal
```

Logical operators:

```text
&&      AND
||      OR
!       NOT
```

---

# 29. Practice Exercises

## Exercise 1 — if

Create:

```tcl
set a 50
```

Check whether `a` is greater than 25.

Expected:

```text
A is greater than 25
```

---

## Exercise 2 — Even/Odd

Create:

```tcl
set number 17
```

Determine whether the number is even or odd.

Hint:

```tcl
expr {$number % 2}
```

---

## Exercise 3 — for Loop

Print numbers:

```text
1
2
3
4
5
6
7
8
9
10
```

using a `for` loop.

---

## Exercise 4 — foreach

Create:

```tcl
set gates {AND OR NAND NOR XOR XNOR}
```

Print each gate.

---

## Exercise 5 — while

Print numbers from 10 down to 1 using a `while` loop.

---

## Exercise 6 — break

Print numbers from 1 to 10 but stop when the number reaches 6.

Expected:

```text
1
2
3
4
5
```

---

## Exercise 7 — continue

Print numbers from 1 to 10 but skip number 5.

Expected:

```text
1
2
3
4
6
7
8
9
10
```

---

# 30. VLSI Practice

## Exercise 8 — Cell Classification

Given:

```tcl
set cells {
    NAND2_X1
    NOR2_X1
    NAND3_X1
    INV_X1
    NAND2_X2
}
```

Use `foreach` and `if` to print:

```text
NAND2_X1 -> NAND
NOR2_X1  -> NOT NAND
NAND3_X1 -> NAND
INV_X1   -> NOT NAND
NAND2_X2 -> NAND
```

---

## Exercise 9 — Timing Check

Given:

```tcl
set slacks {2.1 1.5 0.4 -0.2 3.0 -1.1}
```

Print:

```text
PASS
```

for positive/zero slack and:

```text
VIOLATION
```

for negative slack.

---

## Exercise 10 — Count Violations

Given:

```tcl
set slacks {2.1 -0.5 1.2 -1.0 0.5 -0.2}
```

Use `foreach` and `if` to count how many timing violations exist.

Expected:

```text
Timing Violations = 3
```

---

# 31. Day 03 Learning Checklist

* [ ] Understand `if`
* [ ] Understand `elseif`
* [ ] Understand `else`
* [ ] Use comparison operators
* [ ] Use logical operators
* [ ] Understand `for`
* [ ] Understand `foreach`
* [ ] Understand `while`
* [ ] Use `incr`
* [ ] Use `break`
* [ ] Use `continue`
* [ ] Understand nested loops
* [ ] Combine loops with conditions
* [ ] Process a list of cells
* [ ] Check timing values
* [ ] Count timing violations

---

# Day 03 Completion Criteria

Day 03 is complete when I can independently write a Tcl program that:

1. Makes decisions using `if`.
2. Handles multiple conditions using `elseif`.
3. Uses `for` loops.
4. Uses `foreach` loops.
5. Uses `while` loops.
6. Uses `break` and `continue`.
7. Processes a list of values.
8. Checks conditions for every element.
9. Counts matching or violating elements.
10. Understands how these concepts will be used with EDA collections.

The most important EDA pattern to remember is:

```text
Collection / List
       |
       v
    foreach
       |
       v
    Object
       |
       v
      if
       |
       v
 Check / Process
```

---

# Day 03 Key Example

The following pattern is extremely important for future Genus/Innovus Tcl:

```tcl
foreach object $objects {

    if {condition} {
        # perform action
    } else {
        # alternative action
    }
}
```

Later, `$objects` may be replaced by an EDA-tool collection such as:

```tcl
foreach cell [get_cells *] {
    # process cell
}
```

This is where the Tcl knowledge from Days 1–3 starts connecting directly to VLSI automation.

---

# Next

## Day 04 — Procedures and Functions

Topics:

* `proc`
* Arguments
* Return values
* Local variables
* Global variables
* Default arguments
* Reusable Tcl functions
* VLSI automation procedures

Execution environment:

```text
Ubuntu → tclsh → Day_04 Tcl scripts
```
