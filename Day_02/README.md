# Day_02 — Tcl Data Handling

## Objective

The objective of Day 02 is to understand how Tcl handles **strings, lists, and expressions**.

Day 02 builds on the variables and `expr` concepts learned in Day 01.

The main focus is learning how to store, access, modify and process groups of data using Tcl.

Day 02 still uses **pure Tcl**. Cadence Genus, Innovus and Tempus are not required yet.

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

# 1. Topics Covered

Day 02 covers:

* Strings
* String length
* String comparison
* String searching
* String conversion
* String manipulation
* Lists
* Creating lists
* Accessing list elements
* List length
* Adding elements
* Extracting list ranges
* Replacing list elements
* Sorting lists
* Expressions
* Integer and floating-point calculations
* Practical VLSI examples

---

# 2. Day 02 Learning Flow

```text
Tcl Variables
      |
      v
   Strings
      |
      v
    Lists
      |
      v
 List Operations
      |
      v
 String Operations
      |
      v
   Expressions
      |
      v
VLSI Data Processing
```

---

# 3. Strings

A string is a sequence of characters.

Example:

```tcl
set name "Manjinder"
set course "VLSI"
```

Print:

```tcl
puts $name
puts $course
```

Output:

```text
Manjinder
VLSI
```

---

# 4. String Length

Use:

```tcl
string length
```

Example:

```tcl
set name "Manjinder"

puts [string length $name]
```

The command:

```tcl
[string length $name]
```

returns the number of characters in the string.

---

# 5. String Conversion

Convert a string to uppercase:

```tcl
set name "manjinder"

puts [string toupper $name]
```

Output:

```text
MANJINDER
```

Convert to lowercase:

```tcl
set name "VLSI"

puts [string tolower $name]
```

Output:

```text
vlsi
```

---

# 6. String Comparison

Use:

```tcl
string equal
```

Example:

```tcl
set a "VLSI"
set b "VLSI"

puts [string equal $a $b]
```

Output:

```text
1
```

`1` means true.

`0` means false.

Example:

```tcl
set a "VLSI"
set b "ASIC"

puts [string equal $a $b]
```

Output:

```text
0
```

---

# 7. Searching Inside a String

Use:

```tcl
string first
```

Example:

```tcl
set text "Tcl for VLSI"

puts [string first "VLSI" $text]
```

The command returns the position where `VLSI` starts.

---

# 8. Extracting Part of a String

Use:

```tcl
string range
```

Example:

```tcl
set text "VLSI"

puts [string range $text 0 2]
```

Output:

```text
VLS
```

Tcl uses **zero-based indexing**.

Therefore:

```text
V L S I
0 1 2 3
```

---

# 9. Lists

A Tcl list is an ordered collection of elements.

Example:

```tcl
set cells {AND OR NAND NOR XOR}
```

The list contains:

```text
AND
OR
NAND
NOR
XOR
```

Lists are extremely important in Tcl and later become very important when processing EDA tool objects.

---

# 10. Accessing List Elements

Use:

```tcl
lindex
```

Example:

```tcl
set cells {AND OR NAND NOR XOR}

puts [lindex $cells 0]
puts [lindex $cells 2]
```

Output:

```text
AND
NAND
```

Remember:

```text
AND  → index 0
OR   → index 1
NAND → index 2
NOR  → index 3
XOR  → index 4
```

---

# 11. List Length

Use:

```tcl
llength
```

Example:

```tcl
set cells {AND OR NAND NOR XOR}

puts [llength $cells]
```

Output:

```text
5
```

---

# 12. Add an Element to a List

Use:

```tcl
lappend
```

Example:

```tcl
set cells {AND OR NAND}

lappend cells NOR

puts $cells
```

Output:

```text
AND OR NAND NOR
```

Another example:

```tcl
set ports {clk reset}

lappend ports data
lappend ports enable

puts $ports
```

Output:

```text
clk reset data enable
```

---

# 13. Extract a Range from a List

Use:

```tcl
lrange
```

Example:

```tcl
set cells {AND OR NAND NOR XOR}

puts [lrange $cells 1 3]
```

Output:

```text
OR NAND NOR
```

---

# 14. Insert an Element

Use:

```tcl
linsert
```

Example:

```tcl
set cells {AND OR NOR}

set cells [linsert $cells 2 NAND]

puts $cells
```

Output:

```text
AND OR NAND NOR
```

---

# 15. Replace an Element

Use:

```tcl
lreplace
```

Example:

```tcl
set cells {AND OR NAND NOR}

set cells [lreplace $cells 2 2 XOR]

puts $cells
```

Output:

```text
AND OR XOR NOR
```

---

# 16. Sort a List

Use:

```tcl
lsort
```

Example:

```tcl
set numbers {50 10 40 20 30}

puts [lsort -integer $numbers]
```

Output:

```text
10 20 30 40 50
```

Alphabetical sorting:

```tcl
set cells {NOR AND XOR NAND OR}

puts [lsort $cells]
```

---

# 17. List + foreach

Lists become especially useful with `foreach`.

Example:

```tcl
set cells {AND OR NAND NOR XOR}

foreach cell $cells {
    puts "Cell = $cell"
}
```

Output:

```text
Cell = AND
Cell = OR
Cell = NAND
Cell = NOR
Cell = XOR
```

This concept will become extremely important when working with EDA tools.

For example, later:

```tcl
foreach cell [get_cells *] {
    puts $cell
}
```

This allows Tcl to process multiple design objects.

---

# 18. Expressions

The `expr` command evaluates expressions.

Example:

```tcl
set a 20
set b 10

puts [expr {$a + $b}]
puts [expr {$a - $b}]
puts [expr {$a * $b}]
puts [expr {$a / $b}]
```

---

# 19. Floating-Point Calculation

Tcl can perform floating-point calculations.

Example:

```tcl
set a 10.0
set b 3.0

puts [expr {$a / $b}]
```

Output will be approximately:

```text
3.3333333333333335
```

Using decimal values is important when working with quantities such as:

* Clock periods
* Delays
* Frequencies
* Voltages
* Power
* Timing margins

---

# 20. VLSI Example — Cell List

Consider a simple list of standard cells:

```tcl
set cells {
    NAND2
    NAND3
    NOR2
    INV
    BUF
}
```

Get the number of cells:

```tcl
puts "Number of cells = [llength $cells]"
```

Get the first cell:

```tcl
puts "First cell = [lindex $cells 0]"
```

Add another cell:

```tcl
lappend cells XOR2
```

Print the complete list:

```tcl
puts $cells
```

---

# 21. VLSI Example — Ports

```tcl
set ports {clk reset data_in data_out enable}

puts "Number of ports = [llength $ports]"

foreach port $ports {
    puts "Port = $port"
}
```

Output:

```text
Number of ports = 5
Port = clk
Port = reset
Port = data_in
Port = data_out
Port = enable
```

This is conceptually similar to processing ports obtained from an EDA database.

---

# 22. VLSI Example — Timing Data

```tcl
set clock_period 10.0
set data_delay 7.5

set slack [expr {$clock_period - $data_delay}]

puts "Clock Period = $clock_period ns"
puts "Data Delay   = $data_delay ns"
puts "Slack        = $slack ns"
```

Output:

```text
Clock Period = 10.0 ns
Data Delay   = 7.5 ns
Slack        = 2.5 ns
```

Again, this is only a Tcl calculation. Actual STA will be introduced later.

---

# 23. Important Commands Learned

| Command          | Purpose                  |
| ---------------- | ------------------------ |
| `string length`  | Find string length       |
| `string toupper` | Convert to uppercase     |
| `string tolower` | Convert to lowercase     |
| `string equal`   | Compare strings          |
| `string first`   | Search for a string      |
| `string range`   | Extract part of a string |
| `lindex`         | Access list element      |
| `llength`        | Find list length         |
| `lappend`        | Add element              |
| `lrange`         | Extract list range       |
| `linsert`        | Insert list element      |
| `lreplace`       | Replace list element     |
| `lsort`          | Sort list                |
| `expr`           | Evaluate expression      |

---

# 24. Day 02 File Structure

Create:

```text
Day_02/
│
├── README.md
├── 01_strings.tcl
├── 02_string_operations.tcl
├── 03_lists.tcl
├── 04_list_operations.tcl
├── 05_expressions.tcl
├── 06_vlsi_example.tcl
└── 07_practice.tcl
```

Run each file from the Ubuntu terminal:

```bash
tclsh 01_strings.tcl
```

For example:

```bash
tclsh 03_lists.tcl
```

---

# 25. Practice Exercises

## Exercise 1 — Strings

Create:

```tcl
set name "Manjinder Singh"
```

Print:

* String
* Length
* Uppercase
* Lowercase

---

## Exercise 2 — Lists

Create:

```tcl
set gates {AND OR NAND NOR XOR XNOR}
```

Find:

* Number of gates
* First gate
* Last gate
* Third gate

---

## Exercise 3 — Modify a List

Start with:

```tcl
set gates {AND OR NAND}
```

Add:

```text
NOR
XOR
XNOR
```

Print the final list.

---

## Exercise 4 — Sorting

Create:

```tcl
set numbers {90 20 70 10 50 30}
```

Sort the numbers in ascending order.

---

## Exercise 5 — VLSI

Create:

```tcl
set ports {clk reset enable data_in data_out}
```

Print every port using `foreach`.

---

## Exercise 6 — Timing

Given:

```text
clock_period = 8.0 ns
data_delay = 6.3 ns
```

Calculate:

```text
slack = clock_period - data_delay
```

Expected:

```text
Slack = 1.7 ns
```

---

# 26. Day 02 Learning Checklist

* [ ] Understand Tcl strings
* [ ] Use `string length`
* [ ] Use `string toupper`
* [ ] Use `string tolower`
* [ ] Use `string equal`
* [ ] Use `string first`
* [ ] Use `string range`
* [ ] Understand Tcl lists
* [ ] Use `lindex`
* [ ] Use `llength`
* [ ] Use `lappend`
* [ ] Use `lrange`
* [ ] Use `linsert`
* [ ] Use `lreplace`
* [ ] Use `lsort`
* [ ] Use `foreach` with lists
* [ ] Perform integer calculations
* [ ] Perform floating-point calculations
* [ ] Understand the VLSI application of lists

---

# Day 02 Completion Criteria

Day 02 is complete when I can independently:

1. Create and manipulate strings.
2. Create and manipulate lists.
3. Access individual list elements.
4. Add, insert and replace list elements.
5. Sort lists.
6. Iterate through lists.
7. Perform mathematical calculations.
8. Process simple VLSI-related data using Tcl.

The most important concept to remember for future EDA scripting is:

```text
Tcl List
    |
    v
Collection of Objects
    |
    v
foreach
    |
    v
Process Each Object
```

This concept will become especially important from **Day 07**, when Tcl is used with Cadence EDA tools.

---

# Next

## Day 03 — Conditions and Loops

Topics:

* `if`
* `elseif`
* `else`
* `for`
* `foreach`
* `while`
* `break`
* `continue`
* Nested loops
* VLSI-oriented loop examples

Execution environment:

```text
Ubuntu → tclsh → Day_03 Tcl scripts
```
