# Day_04 — Procedures and Functions

## Objective

The objective of Day 04 is to learn how to create **reusable Tcl procedures** using the `proc` command.

Procedures are very important in Tcl automation because instead of writing the same commands repeatedly, we can create a function once and call it whenever required.

Day 04 builds on:

```text
Day 01 → Variables
Day 02 → Strings and Lists
Day 03 → Conditions and Loops
Day 04 → Procedures
```

The concepts learned today will later be used to create reusable **VLSI/EDA automation functions**.

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

No Cadence Genus, Innovus or Tempus is required for the basic Day 04 exercises.

---

# 1. What is a Procedure?

A Tcl procedure is a reusable block of code.

The Tcl command used to create a procedure is:

```tcl
proc
```

Basic syntax:

```tcl
proc procedure_name {arguments} {
    commands
}
```

Example:

```tcl
proc hello {} {
    puts "Hello Tcl"
}

hello
```

Output:

```text
Hello Tcl
```

---

# 2. Why Procedures Are Important

Without a procedure:

```tcl
puts "Starting synthesis"
puts "Starting synthesis"
puts "Starting synthesis"
```

With a procedure:

```tcl
proc start_message {} {
    puts "Starting synthesis"
}

start_message
start_message
start_message
```

The procedure allows us to write the logic once and reuse it.

---

# 3. Basic Procedure

Example:

```tcl
proc greet {} {
    puts "Hello"
}

greet
```

Output:

```text
Hello
```

The procedure is created using:

```tcl
proc greet {} {
    puts "Hello"
}
```

Then called using:

```tcl
greet
```

---

# 4. Procedure with Arguments

A procedure can receive input values.

Example:

```tcl
proc greet {name} {
    puts "Hello $name"
}

greet Manjinder
```

Output:

```text
Hello Manjinder
```

Here:

```text
name
```

is the procedure argument.

---

# 5. Multiple Arguments

Example:

```tcl
proc add {a b} {
    puts [expr {$a + $b}]
}

add 10 20
```

Output:

```text
30
```

Execution:

```text
add 10 20
  |
  +--> a = 10
  |
  +--> b = 20
  |
  v
expr {$a + $b}
  |
  v
30
```

---

# 6. Return Values

A procedure can return a value using:

```tcl
return
```

Example:

```tcl
proc add {a b} {
    return [expr {$a + $b}]
}

set result [add 10 20]

puts "Result = $result"
```

Output:

```text
Result = 30
```

This is different from simply printing the result.

---

# 7. puts vs return

### Using puts

```tcl
proc add {a b} {
    puts [expr {$a + $b}]
}
```

This displays the result.

### Using return

```tcl
proc add {a b} {
    return [expr {$a + $b}]
}
```

This sends the result back to the caller.

Then:

```tcl
set result [add 10 20]
```

can store the returned value.

Important concept:

```text
puts
  ↓
Display result

return
  ↓
Send result back
```

---

# 8. Procedure with a Calculation

```tcl
proc multiply {a b} {
    return [expr {$a * $b}]
}

set result [multiply 5 10]

puts "Result = $result"
```

Output:

```text
Result = 50
```

---

# 9. Procedure with a Condition

Procedures can contain `if`.

```tcl
proc check_slack {slack} {

    if {$slack >= 0} {
        return "PASS"
    } else {
        return "VIOLATION"
    }
}

puts [check_slack 1.5]
puts [check_slack -0.5]
```

Output:

```text
PASS
VIOLATION
```

---

# 10. Procedure with a Loop

A procedure can contain a loop.

```tcl
proc print_numbers {limit} {

    for {set i 1} {$i <= $limit} {incr i} {
        puts $i
    }
}

print_numbers 5
```

Output:

```text
1
2
3
4
5
```

---

# 11. Procedure with a List

```tcl
proc print_cells {cells} {

    foreach cell $cells {
        puts "Cell = $cell"
    }
}

set cells {NAND2_X1 NOR2_X1 INV_X1 BUF_X1}

print_cells $cells
```

Output:

```text
Cell = NAND2_X1
Cell = NOR2_X1
Cell = INV_X1
Cell = BUF_X1
```

This is very close to the type of reusable logic used in EDA Tcl scripts.

---

# 12. Default Arguments

A procedure can have a default value.

Example:

```tcl
proc greet {{name "User"}} {
    puts "Hello $name"
}

greet
greet Manjinder
```

Output:

```text
Hello User
Hello Manjinder
```

Here:

```text
name = User
```

is used when no argument is supplied.

---

# 13. Multiple Arguments with a Default

Example:

```tcl
proc clock_info {name {period 10}} {

    puts "Clock = $name"
    puts "Period = $period ns"
}

clock_info clk
clock_info clk2 5
```

Output:

```text
Clock = clk
Period = 10 ns

Clock = clk2
Period = 5 ns
```

---

# 14. Local Variables

Variables created inside a procedure are normally local to that procedure.

Example:

```tcl
proc test {} {

    set a 10

    puts "Inside = $a"
}

test
```

The variable `a` belongs to the procedure's local scope.

---

# 15. Global Variables

A global variable can be accessed using:

```tcl
global
```

Example:

```tcl
set clock_period 10

proc show_clock {} {

    global clock_period

    puts "Clock Period = $clock_period"
}

show_clock
```

Output:

```text
Clock Period = 10
```

However, excessive use of global variables should generally be avoided because it makes large scripts harder to maintain.

---

# 16. Procedure Calling Another Procedure

A procedure can call another procedure.

Example:

```tcl
proc add {a b} {
    return [expr {$a + $b}]
}

proc calculate {a b} {

    set result [add $a $b]

    puts "Result = $result"
}

calculate 10 20
```

Output:

```text
Result = 30
```

This allows large automation scripts to be divided into smaller reusable functions.

---

# 17. VLSI Example — Timing Check Procedure

Create a reusable timing-check procedure:

```tcl
proc check_slack {slack} {

    if {$slack >= 0} {
        puts "PASS: Slack = $slack ns"
    } else {
        puts "FAIL: Slack = $slack ns"
    }
}

check_slack 2.5
check_slack 0.5
check_slack -0.3
```

Output:

```text
PASS: Slack = 2.5 ns
PASS: Slack = 0.5 ns
FAIL: Slack = -0.3 ns
```

---

# 18. VLSI Example — Count Violations

```tcl
proc count_violations {slacks} {

    set count 0

    foreach slack $slacks {

        if {$slack < 0} {
            incr count
        }
    }

    return $count
}

set slacks {2.1 -0.5 1.2 -1.0 0.5 -0.2}

set violations [count_violations $slacks]

puts "Timing Violations = $violations"
```

Output:

```text
Timing Violations = 3
```

This combines:

```text
proc
  +
foreach
  +
if
  +
incr
  +
return
```

These are exactly the kinds of combinations we want to become comfortable with before moving into EDA automation.

---

# 19. VLSI Example — Count Cells

```tcl
proc count_cells {cells} {

    return [llength $cells]
}

set cells {
    NAND2_X1
    NAND2_X2
    NOR2_X1
    INV_X1
    BUF_X1
}

puts "Number of cells = [count_cells $cells]"
```

Output:

```text
Number of cells = 5
```

---

# 20. VLSI Example — Find NAND Cells

```tcl
proc find_nand_cells {cells} {

    set nand_cells {}

    foreach cell $cells {

        if {[string first "NAND" $cell] >= 0} {
            lappend nand_cells $cell
        }
    }

    return $nand_cells
}

set cells {
    NAND2_X1
    NOR2_X1
    NAND3_X1
    INV_X1
    NAND2_X2
}

set result [find_nand_cells $cells]

puts "NAND cells:"
puts $result
```

Expected:

```text
NAND cells:
NAND2_X1 NAND3_X1 NAND2_X2
```

---

# 21. EDA Automation Concept

Eventually, instead of passing a manually created list:

```tcl
set cells {
    NAND2_X1
    NOR2_X1
    INV_X1
}
```

an EDA tool can provide design objects.

Conceptually:

```tcl
set cells [get_cells *]
```

Then a reusable procedure could process them:

```tcl
proc process_cells {cells} {

    foreach cell $cells {
        puts "Processing $cell"
    }
}
```

Call:

```tcl
process_cells [get_cells *]
```

The exact `get_cells` syntax and returned-object behavior depends on the EDA tool, so those details will be covered when we start the Cadence section.

---

# 22. Procedure Execution Flow

```text
Procedure Definition
        |
        v
     proc name
        |
        v
   Arguments
        |
        v
     Commands
        |
        v
      return
        |
        v
   Calling Program
```

Example:

```text
calculate(10,20)
       |
       v
     proc
       |
       v
   a = 10
   b = 20
       |
       v
  calculate result
       |
       v
     return
       |
       v
      30
```

---

# 23. Day 04 File Structure

Create:

```text
Day_04/
│
├── README.md
├── 01_basic_proc.tcl
├── 02_proc_arguments.tcl
├── 03_return.tcl
├── 04_default_arguments.tcl
├── 05_local_global.tcl
├── 06_proc_with_if.tcl
├── 07_proc_with_loop.tcl
├── 08_proc_with_list.tcl
├── 09_vlsi_timing.tcl
├── 10_vlsi_cells.tcl
└── 11_practice.tcl
```

Create the directory from Ubuntu:

```bash
cd ~/10_day_tcl

mkdir -p Day_04

cd Day_04

touch README.md
touch 01_basic_proc.tcl
touch 02_proc_arguments.tcl
touch 03_return.tcl
touch 04_default_arguments.tcl
touch 05_local_global.tcl
touch 06_proc_with_if.tcl
touch 07_proc_with_loop.tcl
touch 08_proc_with_list.tcl
touch 09_vlsi_timing.tcl
touch 10_vlsi_cells.tcl
touch 11_practice.tcl
```

Check:

```bash
ls
```

---

# 24. How to Run Day 04

All basic Day 04 programs run using:

```bash
tclsh filename.tcl
```

Example:

```bash
tclsh 01_basic_proc.tcl
```

Another example:

```bash
tclsh 09_vlsi_timing.tcl
```

Interactive mode:

```bash
tclsh
```

Then:

```tcl
proc add {a b} {
    return [expr {$a + $b}]
}

puts [add 10 20]
```

---

# 25. Practice Exercises

## Exercise 1 — Add

Create:

```tcl
proc add {a b} {
    ...
}
```

Return the sum.

Expected:

```text
30
```

for:

```text
10 + 20
```

---

## Exercise 2 — Maximum

Create a procedure:

```tcl
max_value
```

that accepts two numbers and returns the larger number.

Example:

```text
max_value 20 50
```

Expected:

```text
50
```

---

## Exercise 3 — Even/Odd

Create:

```text
check_even
```

that returns:

```text
EVEN
```

or:

```text
ODD
```

---

## Exercise 4 — List Count

Create:

```text
count_items
```

that accepts a list and returns the number of elements.

---

## Exercise 5 — Timing

Create:

```text
check_timing
```

that accepts slack and returns:

```text
PASS
```

or:

```text
VIOLATION
```

---

## Exercise 6 — Count Violations

Create:

```text
count_violations
```

Input:

```tcl
{1.2 -0.4 2.1 -1.2 0.5}
```

Expected:

```text
2
```

---

## Exercise 7 — VLSI Cell Filter

Create a procedure that accepts:

```tcl
{NAND2_X1 NOR2_X1 NAND3_X1 INV_X1 BUF_X1}
```

and returns only cells containing:

```text
NAND
```

Expected:

```text
NAND2_X1 NAND3_X1
```

---

# 26. Important Commands Learned

| Command   | Purpose                |
| --------- | ---------------------- |
| `proc`    | Create procedure       |
| `return`  | Return a value         |
| `global`  | Access global variable |
| `incr`    | Increment variable     |
| `foreach` | Iterate through list   |
| `if`      | Conditional execution  |
| `llength` | Get list length        |
| `lappend` | Add element to list    |

---

# 27. Most Important Concepts

### Procedure

```tcl
proc name {arguments} {
    commands
}
```

### Call procedure

```tcl
name arguments
```

### Return value

```tcl
return value
```

### Store returned value

```tcl
set result [name arguments]
```

---

# 28. Day 04 Learning Checklist

* [ ] Understand procedures
* [ ] Create a procedure using `proc`
* [ ] Call a procedure
* [ ] Pass arguments
* [ ] Use multiple arguments
* [ ] Return values
* [ ] Understand `puts` vs `return`
* [ ] Use default arguments
* [ ] Understand local variables
* [ ] Understand global variables
* [ ] Create procedures containing `if`
* [ ] Create procedures containing loops
* [ ] Create procedures that process lists
* [ ] Create VLSI-oriented procedures
* [ ] Count timing violations using a procedure
* [ ] Filter cell names using a procedure

---

# Day 04 Completion Criteria

Day 04 is complete when I can independently write a reusable Tcl procedure that:

1. Accepts arguments.
2. Performs calculations.
3. Uses conditions.
4. Uses loops.
5. Processes lists.
6. Returns a result.
7. Can be called multiple times.
8. Can be reused for VLSI automation.

The key pattern to remember is:

```text
Input
  |
  v
Procedure
  |
  +--> Variables
  |
  +--> Conditions
  |
  +--> Loops
  |
  v
Return
  |
  v
Result
```

---

# Day 04 VLSI Pattern

A very important future EDA automation pattern is:

```tcl
proc check_objects {objects} {

    foreach object $objects {

        if {condition} {
            # process object
        }
    }
}
```

Later, the `objects` argument may come from an EDA database.

Conceptually:

```tcl
check_objects [get_cells *]
```

Therefore, the skills from Days 1–4 combine as:

```text
Variables
   +
Lists
   +
Conditions
   +
Loops
   +
Procedures
   |
   v
EDA Automation
```

---

# Next

## Day 05 — Files, Directories and File I/O

Topics:

* `open`
* `close`
* `read`
* `gets`
* `puts`
* File modes
* `file exists`
* `file mkdir`
* `glob`
* Directory handling
* Reading reports
* Writing reports
* VLSI report-processing examples

Execution environment:

```text
Ubuntu → tclsh → Day_05 Tcl scripts
```
