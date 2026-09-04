# Day_06 — Arrays, Dictionaries and Advanced Data Handling

## Objective

The objective of Day 06 is to learn Tcl data structures used for storing and processing structured information.

Today we will learn:

* Tcl lists — quick review
* Associative arrays
* `array set`
* `array get`
* `array names`
* `array exists`
* `array size`
* `unset`
* `dict`
* `dict set`
* `dict get`
* `dict exists`
* `dict keys`
* `dict values`
* Nested dictionaries
* Arrays vs lists vs dictionaries
* VLSI-oriented examples

The main idea is:

```text
Day 01 → Variables
Day 02 → Strings / Lists
Day 03 → Conditions / Loops
Day 04 → Procedures
Day 05 → Files / File I/O
Day 06 → Structured Data
```

---

# Environment

## Operating System

Ubuntu Linux

## Tcl Interpreter

`tclsh`

## Where will Day 06 run?

All basic programs run directly in:

```text
Ubuntu Terminal
      ↓
    tclsh
      ↓
Day_06/*.tcl
```

No Cadence tool is required for Day 06.

Later, these concepts will be useful inside:

```text
Genus
Innovus
Tempus
```

---

# 1. Why Data Structures Matter in VLSI

Suppose we have information about several cells:

```text
Cell       Type       Area
NAND2_X1   NAND       1.2
NOR2_X1    NOR        1.5
INV_X1     INV        0.6
```

We need a way to store this information.

Instead of using many unrelated variables:

```tcl
set cell1 "NAND2_X1"
set type1 "NAND"
set area1 1.2

set cell2 "NOR2_X1"
set type2 "NOR"
set area2 1.5
```

we can use Tcl data structures.

This makes automation much easier.

---

# 2. Tcl Data Structures

The three important structures for Day 06 are:

```text
LIST
ARRAY
DICT
```

Conceptually:

```text
List
 ↓
Ordered collection

Array
 ↓
key → value

Dictionary
 ↓
key → value
```

---

# 3. List Review

A Tcl list stores ordered elements.

Example:

```tcl
set cells {NAND2_X1 NOR2_X1 INV_X1 BUF_X1}
```

Access an element:

```tcl
puts [lindex $cells 0]
```

Output:

```text
NAND2_X1
```

Get number of elements:

```tcl
puts [llength $cells]
```

Output:

```text
4
```

---

# 4. Associative Array

A Tcl array stores data using:

```text
key → value
```

Example:

```tcl
set cell_area(NAND2_X1) 1.2
set cell_area(NOR2_X1) 1.5
set cell_area(INV_X1) 0.6
```

Here:

```text
NAND2_X1 → 1.2
NOR2_X1  → 1.5
INV_X1   → 0.6
```

---

# 5. Accessing an Array Element

Example:

```tcl
set cell_area(NAND2_X1) 1.2

puts $cell_area(NAND2_X1)
```

Output:

```text
1.2
```

Notice the syntax:

```tcl
$cell_area(NAND2_X1)
```

The key is inside parentheses.

---

# 6. Another Array Example

```tcl
set age(Manjinder) 25
set age(Rahul) 23
set age(Aman) 24

puts $age(Manjinder)
```

Output:

```text
25
```

The names are keys and the numbers are values.

---

# 7. array set

Instead of setting every element individually, we can initialize an array using `array set`.

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
    NOR2_X1 1.5
    INV_X1 0.6
    BUF_X1 0.8
}
```

Now:

```tcl
puts $cell_area(NAND2_X1)
```

Output:

```text
1.2
```

---

# 8. array get

`array get` returns the array as a list of key/value pairs.

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
    NOR2_X1 1.5
    INV_X1 0.6
}

puts [array get cell_area]
```

Output will contain:

```text
NAND2_X1 1.2 NOR2_X1 1.5 INV_X1 0.6
```

The exact ordering of array elements should not be relied upon.

---

# 9. array names

Use:

```tcl
array names
```

to get all keys.

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
    NOR2_X1 1.5
    INV_X1 0.6
}

puts [array names cell_area]
```

Possible output:

```text
INV_X1 NAND2_X1 NOR2_X1
```

Again, array order is not guaranteed.

---

# 10. Loop Through an Array

This is very important.

```tcl
array set cell_area {
    NAND2_X1 1.2
    NOR2_X1 1.5
    INV_X1 0.6
}

foreach cell [array names cell_area] {
    puts "$cell -> $cell_area($cell)"
}
```

Possible output:

```text
INV_X1 -> 0.6
NAND2_X1 -> 1.2
NOR2_X1 -> 1.5
```

Pattern:

```text
array names
     ↓
foreach
     ↓
key
     ↓
array(key)
```

---

# 11. array exists

Check whether an array exists:

```tcl
array exists cell_area
```

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
}

if {[array exists cell_area]} {
    puts "Array exists"
}
```

Output:

```text
Array exists
```

---

# 12. array size

Get the number of elements:

```tcl
array size cell_area
```

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
    NOR2_X1 1.5
    INV_X1 0.6
}

puts [array size cell_area]
```

Output:

```text
3
```

---

# 13. unset

Remove an array element:

```tcl
unset cell_area(NAND2_X1)
```

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
    NOR2_X1 1.5
    INV_X1 0.6
}

unset cell_area(NAND2_X1)

puts [array names cell_area]
```

Now `NAND2_X1` is removed.

---

# 14. Array with Conditions

Example:

```tcl
array set cell_area {
    NAND2_X1 1.2
    NAND2_X2 2.1
    NOR2_X1 1.5
    INV_X1 0.6
}

foreach cell [array names cell_area] {

    if {$cell_area($cell) > 1.0} {
        puts "$cell has area > 1.0"
    }
}
```

This combines:

```text
Array
+
foreach
+
if
```

---

# 15. Dictionary

A Tcl dictionary also stores:

```text
key → value
```

Example:

```tcl
set cell_area [dict create \
    NAND2_X1 1.2 \
    NOR2_X1 1.5 \
    INV_X1 0.6]
```

Now:

```tcl
puts [dict get $cell_area NAND2_X1]
```

Output:

```text
1.2
```

---

# 16. dict create

A dictionary can be created directly:

```tcl
set student [dict create \
    name Manjinder \
    course VLSI \
    day 6]
```

Access values:

```tcl
puts [dict get $student name]
puts [dict get $student course]
```

Output:

```text
Manjinder
VLSI
```

---

# 17. dict set

Add or modify a dictionary value:

```tcl
set student [dict create]

dict set student name Manjinder
dict set student course VLSI
dict set student day 6
```

Display:

```tcl
puts $student
```

---

# 18. dict get

Get a value:

```tcl
puts [dict get $student name]
```

If:

```text
name → Manjinder
```

the result is:

```text
Manjinder
```

---

# 19. dict exists

Check whether a key exists.

```tcl
if {[dict exists $student course]} {
    puts "Course exists"
}
```

Output:

```text
Course exists
```

---

# 20. dict keys

Get dictionary keys:

```tcl
puts [dict keys $student]
```

Possible output:

```text
name course day
```

Unlike an array, dictionary data is an ordinary Tcl value.

---

# 21. dict values

Get dictionary values:

```tcl
puts [dict values $student]
```

Possible output:

```text
Manjinder VLSI 6
```

---

# 22. Loop Through a Dictionary

Example:

```tcl
set student [dict create \
    name Manjinder \
    course VLSI \
    day 6]

foreach key [dict keys $student] {
    puts "$key = [dict get $student $key]"
}
```

Output:

```text
name = Manjinder
course = VLSI
day = 6
```

---

# 23. Dictionary with VLSI Data

Example:

```tcl
set cell [dict create \
    name NAND2_X1 \
    type NAND \
    area 1.2 \
    delay 0.15]
```

Access:

```tcl
puts "Cell  = [dict get $cell name]"
puts "Type  = [dict get $cell type]"
puts "Area  = [dict get $cell area]"
puts "Delay = [dict get $cell delay]"
```

Output:

```text
Cell  = NAND2_X1
Type  = NAND
Area  = 1.2
Delay = 0.15
```

---

# 24. Nested Dictionary

Dictionaries can contain dictionaries.

Example:

```tcl
set design {}

dict set design name my_design
dict set design clock name clk
dict set design clock period 10
dict set design clock frequency 100
```

Now the structure is conceptually:

```text
design
│
├── name → my_design
│
└── clock
    ├── name → clk
    ├── period → 10
    └── frequency → 100
```

Access:

```tcl
puts [dict get $design name]
puts [dict get $design clock name]
puts [dict get $design clock period]
```

Output:

```text
my_design
clk
10
```

---

# 25. Nested Dictionary — VLSI Example

```tcl
set design {}

dict set design name my_design
dict set design technology 180nm

dict set design clock name clk
dict set design clock period 10
dict set design clock uncertainty 0.2

puts "Design      = [dict get $design name]"
puts "Technology  = [dict get $design technology]"
puts "Clock       = [dict get $design clock name]"
puts "Period      = [dict get $design clock period] ns"
puts "Uncertainty = [dict get $design clock uncertainty] ns"
```

---

# 26. Dictionary Modification

Suppose:

```tcl
set design [dict create name my_design area 10000]
```

Change area:

```tcl
dict set design area 12000
```

Now:

```tcl
puts [dict get $design area]
```

Output:

```text
12000
```

---

# 27. dict unset

Remove a dictionary key:

```tcl
dict unset design area
```

Now `area` no longer exists.

Check:

```tcl
if {[dict exists $design area]} {
    puts "Area exists"
} else {
    puts "Area does not exist"
}
```

---

# 28. Array vs Dictionary

Both can represent:

```text
key → value
```

But they are not the same thing.

### Array

```tcl
set area(NAND2_X1) 1.2
```

The array is a variable with multiple elements.

### Dictionary

```tcl
set area [dict create NAND2_X1 1.2]
```

The dictionary itself is a value stored in variable `area`.

Conceptually:

```text
Array:
variable
 ├── key1 → value1
 ├── key2 → value2
 └── key3 → value3

Dictionary:
variable
 └── dictionary value
      ├── key1 → value1
      ├── key2 → value2
      └── key3 → value3
```

---

# 29. List vs Array vs Dictionary

| Structure         | Best Use                                  |
| ----------------- | ----------------------------------------- |
| List              | Ordered collection                        |
| Array             | Key/value data accessed as array elements |
| Dictionary        | Structured key/value data                 |
| Nested Dictionary | Hierarchical structured data              |

Examples:

### List

```tcl
set cells {A B C D}
```

### Array

```tcl
set area(A) 1.2
set area(B) 2.1
```

### Dictionary

```tcl
set cell [dict create name A area 1.2]
```

---

# 30. VLSI Example — Cell Database

Use a dictionary for one cell:

```tcl
set cell {}

dict set cell name NAND2_X1
dict set cell type NAND
dict set cell area 1.2
dict set cell delay 0.15
dict set cell power 0.05

puts "Name  = [dict get $cell name]"
puts "Type  = [dict get $cell type]"
puts "Area  = [dict get $cell area]"
puts "Delay = [dict get $cell delay]"
puts "Power = [dict get $cell power]"
```

---

# 31. VLSI Example — Multiple Cells

We can store multiple cells as a dictionary of dictionaries.

```tcl
set cells {}

dict set cells NAND2_X1 type NAND
dict set cells NAND2_X1 area 1.2
dict set cells NAND2_X1 delay 0.15

dict set cells NOR2_X1 type NOR
dict set cells NOR2_X1 area 1.5
dict set cells NOR2_X1 delay 0.20

dict set cells INV_X1 type INV
dict set cells INV_X1 area 0.6
dict set cells INV_X1 delay 0.08
```

Structure:

```text
cells
│
├── NAND2_X1
│   ├── type  → NAND
│   ├── area  → 1.2
│   └── delay → 0.15
│
├── NOR2_X1
│   ├── type  → NOR
│   ├── area  → 1.5
│   └── delay → 0.20
│
└── INV_X1
    ├── type  → INV
    ├── area  → 0.6
    └── delay → 0.08
```

---

# 32. Reading Nested Cell Data

```tcl
puts [dict get $cells NAND2_X1 area]
```

Output:

```text
1.2
```

Type:

```tcl
puts [dict get $cells NAND2_X1 type]
```

Output:

```text
NAND
```

Delay:

```tcl
puts [dict get $cells NAND2_X1 delay]
```

Output:

```text
0.15
```

---

# 33. Loop Through Multiple Cells

```tcl
foreach cell [dict keys $cells] {

    set type [dict get $cells $cell type]
    set area [dict get $cells $cell area]
    set delay [dict get $cells $cell delay]

    puts "$cell : type=$type area=$area delay=$delay"
}
```

Possible output:

```text
NAND2_X1 : type=NAND area=1.2 delay=0.15
NOR2_X1 : type=NOR area=1.5 delay=0.20
INV_X1 : type=INV area=0.6 delay=0.08
```

---

# 34. Filtering Dictionary Data

Example:

```tcl
foreach cell [dict keys $cells] {

    set area [dict get $cells $cell area]

    if {$area > 1.0} {
        puts "$cell has area greater than 1.0"
    }
}
```

Expected cells:

```text
NAND2_X1
NOR2_X1
```

---

# 35. VLSI Example — Timing Data

Create:

```tcl
set timing {}

dict set timing path1 slack 1.2
dict set timing path2 slack 0.5
dict set timing path3 slack -0.3
dict set timing path4 slack -1.1
```

Check all paths:

```tcl
foreach path [dict keys $timing] {

    set slack [dict get $timing $path slack]

    if {$slack < 0} {
        puts "$path -> VIOLATION ($slack ns)"
    } else {
        puts "$path -> PASS ($slack ns)"
    }
}
```

---

# 36. Count Timing Violations

```tcl
set count 0

foreach path [dict keys $timing] {

    set slack [dict get $timing $path slack]

    if {$slack < 0} {
        incr count
    }
}

puts "Total Violations = $count"
```

Expected:

```text
Total Violations = 2
```

---

# 37. Procedure + Dictionary

Combine Day 04 and Day 06.

```tcl
proc check_timing {timing} {

    foreach path [dict keys $timing] {

        set slack [dict get $timing $path slack]

        if {$slack < 0} {
            puts "$path -> FAIL"
        } else {
            puts "$path -> PASS"
        }
    }
}

set timing {}

dict set timing path1 slack 1.2
dict set timing path2 slack -0.5
dict set timing path3 slack 0.8

check_timing $timing
```

This is a good example of combining multiple Tcl concepts.

---

# 38. File + Dictionary

Day 05 and Day 06 can also be combined.

Suppose a file contains:

```text
NAND2_X1 1.2
NOR2_X1 1.5
INV_X1 0.6
```

Read it and store it in a dictionary:

```tcl
set fp [open "cells.txt" r]

set cells {}

while {[gets $fp line] >= 0} {

    if {[llength $line] == 2} {

        set name [lindex $line 0]
        set area [lindex $line 1]

        dict set cells $name area $area
    }
}

close $fp

puts $cells
```

This demonstrates:

```text
File
 ↓
gets
 ↓
List
 ↓
Dictionary
 ↓
Structured data
```

---

# 39. Important EDA Concept

In real EDA tools, you will often obtain design objects using commands such as:

```tcl
get_cells
get_ports
get_nets
get_pins
```

The exact behavior depends on the tool.

You may then process the returned objects using:

```tcl
foreach
if
proc
```

and store your own calculated information using Tcl data structures.

Conceptually:

```text
EDA Database
     |
     v
Get Objects
     |
     v
foreach
     |
     v
Extract Properties
     |
     v
Store Results
     |
     +----> Array
     |
     +----> Dictionary
     |
     v
Generate Report
```

---

# 40. Day 06 File Structure

Create:

```text
Day_06/
│
├── README.md
├── 01_list_review.tcl
├── 02_basic_array.tcl
├── 03_array_set.tcl
├── 04_array_get.tcl
├── 05_array_names.tcl
├── 06_array_loop.tcl
├── 07_array_commands.tcl
├── 08_basic_dict.tcl
├── 09_dict_commands.tcl
├── 10_nested_dict.tcl
├── 11_vlsi_cells.tcl
├── 12_vlsi_timing.tcl
├── 13_proc_dict.tcl
├── 14_file_dict.tcl
└── 15_practice.tcl
```

Create from Ubuntu:

```bash
cd ~/10_day_tcl

mkdir -p Day_06

cd Day_06

touch README.md
touch 01_list_review.tcl
touch 02_basic_array.tcl
touch 03_array_set.tcl
touch 04_array_get.tcl
touch 05_array_names.tcl
touch 06_array_loop.tcl
touch 07_array_commands.tcl
touch 08_basic_dict.tcl
touch 09_dict_commands.tcl
touch 10_nested_dict.tcl
touch 11_vlsi_cells.tcl
touch 12_vlsi_timing.tcl
touch 13_proc_dict.tcl
touch 14_file_dict.tcl
touch 15_practice.tcl
```

Check:

```bash
ls -l
```

---

# 41. How to Run

Go to Day 06:

```bash
cd ~/10_day_tcl/Day_06
```

Run:

```bash
tclsh 01_list_review.tcl
```

Run array example:

```bash
tclsh 06_array_loop.tcl
```

Run dictionary example:

```bash
tclsh 10_nested_dict.tcl
```

Run VLSI example:

```bash
tclsh 12_vlsi_timing.tcl
```

---

# 42. Practice Exercises

## Exercise 1 — Student Array

Create an array:

```text
marks(physics)
marks(digital)
marks(verilog)
marks(tcl)
```

Store marks for each subject.

Print all subjects and marks.

---

## Exercise 2 — Find Highest Mark

Using the array above, find the highest mark.

Use:

```text
array names
foreach
if
```

---

## Exercise 3 — Cell Area Array

Create:

```tcl
NAND2_X1 → 1.2
NAND2_X2 → 2.1
NOR2_X1  → 1.5
INV_X1   → 0.6
BUF_X1   → 0.8
```

Find all cells with:

```text
area > 1.0
```

---

## Exercise 4 — Student Dictionary

Create:

```text
name
roll
branch
semester
```

using a dictionary.

Print all information.

---

## Exercise 5 — Clock Dictionary

Create:

```text
clock name
clock period
clock uncertainty
clock frequency
```

using a nested dictionary.

Expected structure:

```text
clock
├── name
├── period
├── uncertainty
└── frequency
```

---

## Exercise 6 — Timing Dictionary

Create:

```text
path1 → slack = 1.2
path2 → slack = -0.5
path3 → slack = 0.8
path4 → slack = -1.1
```

Print:

```text
PASS
```

for positive slack and:

```text
VIOLATION
```

for negative slack.

---

## Exercise 7 — Count Violations

Using Exercise 6, count the total number of timing violations.

Expected:

```text
2
```

---

## Exercise 8 — Cell Database

Create a nested dictionary containing:

```text
NAND2_X1
NOR2_X1
INV_X1
```

For each cell store:

```text
type
area
delay
power
```

Then print the complete database.

---

# 43. Important Commands

## Array

```tcl
array set
array get
array names
array exists
array size
unset
```

## Dictionary

```tcl
dict create
dict set
dict get
dict exists
dict keys
dict values
dict unset
```

---

# 44. Day 06 Checklist

* [ ] Understand lists
* [ ] Understand associative arrays
* [ ] Create an array
* [ ] Use `array set`
* [ ] Use `array get`
* [ ] Use `array names`
* [ ] Use `array exists`
* [ ] Use `array size`
* [ ] Remove array elements
* [ ] Understand dictionaries
* [ ] Create a dictionary
* [ ] Use `dict set`
* [ ] Use `dict get`
* [ ] Use `dict exists`
* [ ] Use `dict keys`
* [ ] Use `dict values`
* [ ] Use `dict unset`
* [ ] Create nested dictionaries
* [ ] Process dictionary data with loops
* [ ] Combine dictionary + conditions
* [ ] Combine dictionary + procedures
* [ ] Combine files + dictionaries
* [ ] Understand VLSI data-handling applications

---

# 45. Key Difference to Remember

## List

Use when you primarily need an ordered collection:

```tcl
set cells {A B C D}
```

## Array

Use Tcl's array variable when you want indexed key/value elements:

```tcl
set area(A) 1.2
```

## Dictionary

Use when you want a key/value structure that behaves as a Tcl value:

```tcl
set cell [dict create name A area 1.2]
```

---

# Day 06 Completion Criteria

Day 06 is complete when I can independently:

1. Create and access an associative array.
2. Iterate through an array.
3. Check whether an array exists.
4. Count array elements.
5. Create and modify dictionaries.
6. Access dictionary values.
7. Check dictionary keys.
8. Iterate through dictionaries.
9. Create nested dictionaries.
10. Store VLSI cell information.
11. Store timing information.
12. Count timing violations.
13. Combine procedures with dictionaries.
14. Combine file I/O with structured data.

---

# The Big Picture

After Day 06:

```text
                    TCL
                     |
      +--------------+--------------+
      |              |              |
 Variables         Lists        Procedures
      |              |              |
      +--------------+--------------+
                     |
             Conditions / Loops
                     |
                     v
                  File I/O
                     |
                     v
             Structured Data
                /         \
             Array        Dict
                \         /
                 \       /
                  VLSI DATA
                     |
                     v
              EDA Automation
```

The goal is not just to memorize commands.

The goal is to understand how to take:

```text
EDA information
      ↓
store it
      ↓
process it
      ↓
filter it
      ↓
calculate something
      ↓
generate a report
```

using Tcl.

---

# Next

## Day 07 — Tcl String Processing and Regular Expressions

Topics:

* `string`
* `string length`
* `string compare`
* `string match`
* `string first`
* `string range`
* `string tolower`
* `string toupper`
* `regexp`
* `regsub`
* Pattern matching
* Parsing EDA reports
* Extracting timing/area information
* VLSI log processing

Execution:

```text
Ubuntu → tclsh → Day_07 Tcl scripts
```

Day 07 will be particularly important for **report parsing and automation**.
