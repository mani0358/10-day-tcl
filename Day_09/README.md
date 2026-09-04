# 10-Day Tcl Learning Plan — Day 09

# Tcl for EDA Tools — Design Objects, Queries, Collections & Automation

---

## 1. Day 09 Objective

So far we learned Tcl as a programming language.

Today we start learning Tcl as an **EDA automation language**.

The basic idea is:

```text
                 EDA TOOL
                    │
                    ▼
              Design Database
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
        Cells      Pins      Nets
          │         │         │
          └─────────┼─────────┘
                    ▼
                Tcl Query
                    │
                    ▼
              Filter Objects
                    │
                    ▼
             Extract Properties
                    │
                    ▼
              Generate Report
```

This is the foundation of Tcl scripting in tools such as:

* Cadence Genus
* Cadence Innovus
* Cadence Tempus
* Synopsys Design Compiler
* Synopsys PrimeTime
* OpenROAD

**Important:** EDA command syntax is tool-specific. A command that works in Genus may not work in PrimeTime or OpenROAD. Therefore, today we first learn the general Tcl/EDA concept and then look at tool-specific examples.

---

# 2. What We Will Learn

Day 09 topics:

1. Tcl commands inside EDA tools
2. Tool command vs normal Tcl command
3. Design objects
4. Cells/instances
5. Pins
6. Nets
7. Ports
8. Clocks
9. Collections
10. Querying objects
11. Filtering objects
12. Object properties
13. `foreach`
14. `lsearch`
15. `lmap`
16. `regexp` with design data
17. Basic EDA reporting
18. Cadence-style `get_db`
19. Tool command discovery
20. Building an EDA-style automation script

---

# 3. Where Will We Run Day 09?

There are two stages.

## Stage 1 — Ubuntu + tclsh

We will practice the Tcl concepts using:

```text
Ubuntu
tclsh
```

Directory:

```text
~/10_day_tcl/Day_09
```

Run:

```bash
cd ~/10_day_tcl/Day_09
tclsh script.tcl
```

---

## Stage 2 — EDA Tool

Later, some examples can be run inside an EDA tool such as:

```text
Cadence Genus
Cadence Innovus
```

or another supported EDA environment.

For example, a Cadence command such as:

```tcl
get_db
```

is **not a normal standalone `tclsh` command**.

It is provided by the EDA tool.

Therefore:

```text
Ubuntu tclsh
      │
      ├── Tcl commands
      └── Our practice scripts

EDA tool Tcl shell
      │
      ├── Tcl commands
      └── EDA-specific commands
```

---

# 4. Day 09 Directory Structure

Create:

```bash
mkdir -p ~/10_day_tcl/Day_09
cd ~/10_day_tcl/Day_09
```

Recommended structure:

```text
Day_09/
│
├── README.md
│
├── 01_eda_concept.tcl
├── 02_design_objects.tcl
├── 03_collections.tcl
├── 04_foreach_objects.tcl
├── 05_filter_objects.tcl
├── 06_lsearch_objects.tcl
├── 07_object_properties.tcl
├── 08_regexp_objects.tcl
├── 09_mock_cells.tcl
├── 10_mock_timing.tcl
├── 11_mock_qor.tcl
│
├── cadence/
│   ├── 01_get_db.tcl
│   ├── 02_query_cells.tcl
│   ├── 03_query_pins.tcl
│   └── 04_query_design.tcl
│
└── 12_practice.tcl
```

---

# 5. What Is an EDA Design Database?

When you load a design into an EDA tool, the tool internally stores information about the design.

For example:

```text
Design
│
├── Modules
│
├── Instances / Cells
│
├── Pins
│
├── Nets
│
├── Ports
│
├── Clocks
│
├── Constraints
│
└── Libraries
```

Suppose your RTL contains:

```verilog
module top (
    input  clk,
    input  a,
    input  b,
    output y
);

wire n1;

AND2_X1 U1 (
    .A(a),
    .B(b),
    .Y(n1)
);

INV_X1 U2 (
    .A(n1),
    .Y(y)
);

endmodule
```

The EDA tool can represent objects such as:

```text
top
│
├── U1
│    ├── A
│    ├── B
│    └── Y
│
├── U2
│    ├── A
│    └── Y
│
├── a
├── b
├── clk
└── y
```

Tcl allows us to query this information.

---

# 6. What Is a Design Object?

A design object is an entity stored by the EDA database.

Common examples:

```text
Cell / Instance
Pin
Net
Port
Clock
Module
Library Cell
Timing Arc
```

For example:

```text
U1
```

may be an instance.

Its master/reference cell may be:

```text
AND2_X1
```

Its pins may be:

```text
U1/A
U1/B
U1/Y
```

---

# 7. Why Query Design Objects?

Suppose a chip contains:

```text
500,000 cells
2,000,000 pins
700,000 nets
```

You cannot manually inspect them.

Tcl can automate:

```text
Find all cells
       ↓
Filter buffers
       ↓
Count them
       ↓
Find their area
       ↓
Generate report
```

This is one of the main reasons Tcl is so important in VLSI.

---

# 8. Normal Tcl vs EDA Tcl

Normal Tcl:

```tcl
set a 10
set b 20

puts [expr {$a + $b}]
```

EDA Tcl:

```text
Query design
      ↓
Get cells
      ↓
Get properties
      ↓
Filter objects
      ↓
Generate report
```

The EDA tool extends Tcl by providing its own commands.

For example, depending on the tool:

```text
get_cells
get_pins
get_nets
get_ports
get_clocks
get_db
```

The exact commands and object model depend on the EDA tool.

---

# 9. Collections

One of the most important EDA Tcl concepts is a **collection**.

Suppose a design has:

```text
U1
U2
U3
U4
U5
```

A query may return a group of objects.

Conceptually:

```text
cells = {U1 U2 U3 U4 U5}
```

We can then process them:

```tcl
foreach cell $cells {
    puts $cell
}
```

This idea is central to EDA automation.

---

# 10. Practice Collections Using Lists

Before using an actual EDA tool, let's simulate a collection using a Tcl list.

Create:

```text
03_collections.tcl
```

Code:

```tcl
set cells {
    U1
    U2
    U3
    U4
    U5
}

puts "Number of cells = [llength $cells]"
```

Run:

```bash
tclsh 03_collections.tcl
```

Output:

```text
Number of cells = 5
```

---

# 11. Iterate Through Cells

```tcl
set cells {
    U1
    U2
    U3
    U4
    U5
}

foreach cell $cells {
    puts "Cell = $cell"
}
```

Output:

```text
Cell = U1
Cell = U2
Cell = U3
Cell = U4
Cell = U5
```

This is the basic pattern for processing design objects.

---

# 12. Add Cell Types

Now create a mock database.

```tcl
set cell_type(U1) "NAND2_X1"
set cell_type(U2) "INV_X1"
set cell_type(U3) "BUF_X4"
set cell_type(U4) "NAND2_X2"
set cell_type(U5) "DFF_X1"

set cells {U1 U2 U3 U4 U5}

foreach cell $cells {

    puts "$cell -> $cell_type($cell)"
}
```

Output:

```text
U1 -> NAND2_X1
U2 -> INV_X1
U3 -> BUF_X4
U4 -> NAND2_X2
U5 -> DFF_X1
```

This simulates the idea of:

```text
Instance → Property
```

---

# 13. Object Properties

A design object can have many properties.

For example:

```text
Cell U1
│
├── name = U1
├── type = NAND2_X1
├── area = 1.20
├── delay = 0.15
└── power = 0.05
```

We can represent this using a dictionary.

```tcl
set cell_db [dict create]

dict set cell_db U1 type NAND2_X1
dict set cell_db U1 area 1.20
dict set cell_db U1 delay 0.15

puts [dict get $cell_db U1 type]
puts [dict get $cell_db U1 area]
```

Output:

```text
NAND2_X1
1.20
```

This combines:

```text
Day 06 → Dictionary
+
Day 09 → Design object concept
```

---

# 14. Process Object Properties

```tcl
set cells {U1 U2 U3}

set cell_db [dict create]

dict set cell_db U1 type NAND2_X1
dict set cell_db U1 area 1.20

dict set cell_db U2 type INV_X1
dict set cell_db U2 area 0.80

dict set cell_db U3 type BUF_X2
dict set cell_db U3 area 1.50

foreach cell $cells {

    set type [dict get $cell_db $cell type]
    set area [dict get $cell_db $cell area]

    puts "$cell  $type  $area"
}
```

Output:

```text
U1  NAND2_X1  1.20
U2  INV_X1    0.80
U3  BUF_X2    1.50
```

---

# 15. Filtering Objects

Suppose:

```text
U1 → NAND2_X1
U2 → INV_X1
U3 → BUF_X2
U4 → NAND2_X2
U5 → DFF_X1
```

We only want NAND cells.

Use:

```tcl
foreach cell $cells {

    set type [dict get $cell_db $cell type]

    if {[string match "NAND*" $type]} {
        puts "$cell -> $type"
    }
}
```

Output:

```text
U1 -> NAND2_X1
U4 -> NAND2_X2
```

This is exactly the type of operation we perform on real design objects.

---

# 16. Filter by Property

Suppose we want cells with:

```text
area > 1.0
```

Code:

```tcl
foreach cell $cells {

    set area [dict get $cell_db $cell area]

    if {$area > 1.0} {
        puts "$cell Area = $area"
    }
}
```

This gives:

```text
U1 Area = 1.20
U3 Area = 1.50
```

---

# 17. Counting Objects

```tcl
set count 0

foreach cell $cells {

    set type [dict get $cell_db $cell type]

    if {[string match "NAND*" $type]} {
        incr count
    }
}

puts "NAND count = $count"
```

Output:

```text
NAND count = 2
```

---

# 18. Calculating Total Area

```tcl
set total_area 0

foreach cell $cells {

    set area [dict get $cell_db $cell area]

    set total_area [expr {$total_area + $area}]
}

puts "Total area = $total_area"
```

This is a simple example of how a Tcl script can calculate design metrics.

---

# 19. `lsearch`

`lsearch` searches a Tcl list.

Example:

```tcl
set cells {U1 U2 U3 U4 U5}

set index [lsearch $cells U3]

puts "Index = $index"
```

Output:

```text
Index = 2
```

Remember:

```text
U1 → index 0
U2 → index 1
U3 → index 2
```

---

# 20. `lsearch -exact`

```tcl
set cells {U1 U2 U3 U4}

if {[lsearch -exact $cells U3] >= 0} {
    puts "U3 exists"
}
```

Output:

```text
U3 exists
```

---

# 21. `lsearch -glob`

We can search using patterns.

```tcl
set cells {
    NAND2_X1
    NAND2_X2
    INV_X1
    BUF_X2
}

set result [lsearch -all -glob $cells "NAND*"]

puts $result
```

This finds entries matching:

```text
NAND*
```

---

# 22. `regexp` + Design Objects

We learned regex on Day 07.

Now combine it with design objects.

```tcl
set cells {
    NAND2_X1
    NAND2_X2
    INV_X1
    BUF_X2
}

foreach cell $cells {

    if {[regexp {^NAND} $cell]} {
        puts "NAND cell: $cell"
    }
}
```

Output:

```text
NAND cell: NAND2_X1
NAND cell: NAND2_X2
```

---

# 23. Building a Mock Design Database

Let's make a more realistic example.

```tcl
set cells {U1 U2 U3 U4 U5 U6}

set cell_db [dict create]

dict set cell_db U1 type NAND2_X1
dict set cell_db U1 area 1.20
dict set cell_db U1 power 0.05

dict set cell_db U2 type NAND2_X2
dict set cell_db U2 area 2.10
dict set cell_db U2 power 0.08

dict set cell_db U3 type INV_X1
dict set cell_db U3 area 0.80
dict set cell_db U3 power 0.03

dict set cell_db U4 type BUF_X4
dict set cell_db U4 area 2.40
dict set cell_db U4 power 0.10

dict set cell_db U5 type DFF_X1
dict set cell_db U5 area 3.50
dict set cell_db U5 power 0.15

dict set cell_db U6 type NAND2_X1
dict set cell_db U6 area 1.20
dict set cell_db U6 power 0.05
```

---

# 24. Generate Cell Report

```tcl
puts "================================"
puts "CELL REPORT"
puts "================================"

foreach cell $cells {

    set type  [dict get $cell_db $cell type]
    set area  [dict get $cell_db $cell area]
    set power [dict get $cell_db $cell power]

    puts "$cell  $type  Area=$area  Power=$power"
}
```

---

# 25. Count NAND Cells

```tcl
set nand_count 0

foreach cell $cells {

    set type [dict get $cell_db $cell type]

    if {[string match "NAND*" $type]} {
        incr nand_count
    }
}

puts "NAND cells = $nand_count"
```

---

# 26. Calculate Total Area

```tcl
set total_area 0

foreach cell $cells {

    set area [dict get $cell_db $cell area]

    set total_area [expr {$total_area + $area}]
}

puts "Total area = $total_area"
```

---

# 27. Find Largest Cell

```tcl
set largest_cell ""
set largest_area 0

foreach cell $cells {

    set area [dict get $cell_db $cell area]

    if {$area > $largest_area} {

        set largest_area $area
        set largest_cell $cell
    }
}

puts "Largest cell = $largest_cell"
puts "Largest area = $largest_area"
```

Expected:

```text
Largest cell = U5
Largest area = 3.5
```

---

# 28. Mock Timing Database

Now we can represent timing information.

```tcl
set timing_db [dict create]

dict set timing_db U1 slack 0.25
dict set timing_db U2 slack -0.15
dict set timing_db U3 slack 0.10
dict set timing_db U4 slack -0.32
dict set timing_db U5 slack 0.05
```

Check violations:

```tcl
foreach cell {U1 U2 U3 U4 U5} {

    set slack [dict get $timing_db $cell slack]

    if {$slack < 0} {
        puts "$cell -> VIOLATION -> Slack = $slack"
    }
}
```

Output:

```text
U2 -> VIOLATION -> Slack = -0.15
U4 -> VIOLATION -> Slack = -0.32
```

---

# 29. Find Worst Slack

```tcl
set worst_slack 999999
set worst_cell ""

foreach cell {U1 U2 U3 U4 U5} {

    set slack [dict get $timing_db $cell slack]

    if {$slack < $worst_slack} {

        set worst_slack $slack
        set worst_cell $cell
    }
}

puts "Worst slack = $worst_slack"
puts "Worst cell  = $worst_cell"
```

Output:

```text
Worst slack = -0.32
Worst cell  = U4
```

---

# 30. Actual EDA Tools

Now let's move toward the real thing.

An EDA tool provides commands that expose its internal design database.

For example, depending on the tool, you may encounter commands such as:

```text
get_cells
get_pins
get_nets
get_ports
get_clocks
```

or Cadence database commands such as:

```text
get_db
```

These are **EDA-specific commands**.

Do not expect them to work in plain Ubuntu `tclsh`.

---

# 31. Cadence `get_db`

Cadence digital implementation/synthesis tools provide the `get_db` interface for querying database objects.

A typical command has the conceptual form:

```tcl
get_db <object>
```

or:

```tcl
get_db <object> <attribute>
```

The exact available objects/attributes depend on the Cadence tool and design state.

For example, you may query database information after loading/elaborating a design.

The important concept is:

```text
get_db
   ↓
query database
   ↓
return design information
```

---

# 32. Why `get_db` Is Important

Suppose the tool database contains:

```text
100,000 instances
```

Instead of manually looking through them, Tcl can query the database.

Conceptually:

```tcl
set cells [get_db insts]

foreach cell $cells {
    ...
}
```

The exact object/attribute syntax should always be checked in the version of the Cadence tool you are running.

---

# 33. Discovering Tool Commands

When you are inside an EDA Tcl shell, Tcl's introspection commands are useful.

For example:

```tcl
info commands
```

shows commands currently available.

You can search:

```tcl
info commands *get*
```

This can help identify commands containing `get`.

You can also inspect a command's existence:

```tcl
info commands get_db
```

If it returns the command name, that command is available in the current Tcl environment.

---

# 34. `help` in EDA Tools

EDA tools normally provide their own help systems.

For example, depending on the tool:

```text
help
man
```

or tool-specific command documentation may be available.

For a command such as:

```text
get_db
```

always check the documentation/help for the **specific tool and version**.

This is important because:

```text
Genus syntax
≠
Innovus syntax
≠
PrimeTime syntax
≠
OpenROAD syntax
```

---

# 35. Collections — Important Warning

Different EDA tools implement collections differently.

For example:

```text
Tool A → collection object
Tool B → Tcl list-like result
Tool C → database object handles
```

Therefore, don't assume that every tool supports:

```tcl
llength
```

or:

```tcl
foreach
```

in exactly the same way for returned objects.

Always learn the collection mechanism of the tool you are using.

---

# 36. General EDA Query Pattern

The most important pattern is:

```text
Query
  ↓
Collection / Object Set
  ↓
foreach
  ↓
Property
  ↓
Condition
  ↓
Report
```

For example:

```tcl
set cells [EDA_QUERY_FOR_CELLS]

foreach cell $cells {

    set area [EDA_QUERY_FOR_AREA $cell]

    if {$area > 10} {
        puts "$cell : large cell"
    }
}
```

The exact query commands change between EDA tools.

The **programming pattern remains the same**.

---

# 37. VLSI Automation Example

Imagine a synthesized design.

We want to:

```text
1. Get all cells
2. Count cells
3. Find NAND cells
4. Find large cells
5. Calculate area
6. Generate report
```

Pseudo-flow:

```text
                 Design
                   │
                   ▼
              Query cells
                   │
                   ▼
            foreach cell
                   │
          ┌────────┼────────┐
          ▼        ▼        ▼
        Type      Area     Power
          │        │        │
          ▼        ▼        ▼
       Filter   Compare   Sum
          │        │        │
          └────────┼────────┘
                   ▼
               Report
```

---

# 38. Building a Reusable Procedure

From Day 08, we know procedures.

Create:

```tcl
proc check_cell_area {cell area limit} {

    if {$area > $limit} {
        puts "$cell : AREA HIGH : $area"
        return "FAIL"
    }

    return "PASS"
}
```

Use:

```tcl
set status [check_cell_area U1 12.5 10]

puts "Status = $status"
```

Output:

```text
U1 : AREA HIGH : 12.5
Status = FAIL
```

---

# 39. Combining Everything

Now combine:

```text
Day 05 → Files
Day 06 → Dictionaries
Day 07 → Regex
Day 08 → Procedures
Day 09 → EDA objects
```

The resulting automation flow becomes:

```text
EDA Tool
   │
   ▼
Query Design
   │
   ▼
Objects
   │
   ▼
Properties
   │
   ▼
Tcl Processing
   │
   ├── Conditions
   ├── Regex
   ├── Dictionaries
   └── Procedures
   │
   ▼
Report File
```

---

# 40. Example Final Report

A Tcl automation script could generate:

```text
========================================
          DESIGN SUMMARY
========================================

Design       : my_design

Cell Count   : 12500
NAND Count   : 3200
Buffer Count : 850
DFF Count    : 1400

Total Area   : 18542.3

WNS          : -0.25
TNS          : -4.82

Timing       : FAIL
Area         : PASS

========================================
```

This is the kind of result we eventually want from the 10-day Tcl project.

---

# 41. Day 09 Mini Project

## VLSI Design Database Analyzer

Create a mock design database containing at least:

```text
10 cells
```

For every cell store:

```text
Instance name
Cell type
Area
Power
Slack
```

Example:

```text
U1
NAND2_X1
Area = 1.2
Power = 0.05
Slack = 0.25
```

Your program should calculate:

### 1. Total cells

```text
Cell Count = ...
```

### 2. Total area

```text
Total Area = ...
```

### 3. NAND count

```text
NAND Count = ...
```

### 4. Cells with negative slack

```text
Timing Violations = ...
```

### 5. Worst slack

```text
Worst Slack = ...
```

### 6. Largest cell

```text
Largest Cell = ...
Largest Area = ...
```

### 7. Final status

```text
TIMING = PASS/FAIL
AREA   = PASS/FAIL
```

---

# 42. Recommended Mini Project Structure

```text
Day_09/
│
├── design_db.tcl
├── cell_utils.tcl
├── timing_utils.tcl
├── report_utils.tcl
└── main.tcl
```

### `design_db.tcl`

Contains the mock design database.

### `cell_utils.tcl`

Contains:

```text
count_cells
count_nand
calculate_area
find_largest_cell
```

### `timing_utils.tcl`

Contains:

```text
count_violations
find_worst_slack
check_timing
```

### `report_utils.tcl`

Contains:

```text
print_summary
```

### `main.tcl`

Loads everything:

```tcl
source design_db.tcl
source cell_utils.tcl
source timing_utils.tcl
source report_utils.tcl
```

Then executes the flow.

---

# 43. Important Commands to Remember

## Tcl commands

```tcl
foreach
lsearch
llength
lindex
lappend
dict
regexp
proc
return
source
catch
```

## EDA concepts

```text
Design
Cell
Instance
Pin
Net
Port
Clock
Library
Property
Collection
Query
Filter
```

## EDA command families

Depending on the tool:

```text
get_cells
get_pins
get_nets
get_ports
get_clocks
get_db
```

---

# 44. The Most Important EDA Tcl Pattern

Memorize this concept:

```text
GET
 ↓
FILTER
 ↓
LOOP
 ↓
QUERY PROPERTY
 ↓
CHECK
 ↓
REPORT
```

For example:

```text
Get all cells
     ↓
Find NAND cells
     ↓
Loop through NAND cells
     ↓
Get area
     ↓
Check area limit
     ↓
Print result
```

This pattern appears everywhere in VLSI automation.

---

# 45. Common Beginner Mistakes

## Mistake 1 — Running EDA commands in tclsh

This:

```tcl
get_db insts
```

will not work in ordinary:

```bash
tclsh
```

because `get_db` is supplied by the EDA environment.

---

## Mistake 2 — Assuming syntax is universal

Do not assume:

```text
Genus
Innovus
PrimeTime
OpenROAD
```

all use identical commands.

They don't.

---

## Mistake 3 — Treating every result as a normal Tcl list

EDA tools may return specialized collections or database objects.

Learn the collection mechanism of the specific tool.

---

## Mistake 4 — Hard-coding thousands of objects

Don't write:

```tcl
puts U1
puts U2
puts U3
...
```

Instead:

```tcl
foreach cell $cells {
    ...
}
```

---

# 46. Day 09 Checklist

Before moving to Day 10, make sure you understand:

* [ ] What an EDA design database is
* [ ] What a design object is
* [ ] Cell/instance
* [ ] Pin
* [ ] Net
* [ ] Port
* [ ] Clock
* [ ] Object properties
* [ ] Collections
* [ ] Querying
* [ ] Filtering
* [ ] `foreach`
* [ ] `lsearch`
* [ ] `llength`
* [ ] Dictionaries for object data
* [ ] Regex for object-name matching
* [ ] Procedures for reusable analysis
* [ ] Difference between Tcl and EDA-specific commands
* [ ] Why `get_db` cannot run in normal `tclsh`
* [ ] Why EDA command syntax is tool-specific

---

# 47. Day 09 Big Picture

Your Tcl learning has now reached:

```text
                 TCL
                  │
                  ▼
          Programming Basics
                  │
                  ▼
        Files / Data / Regex
                  │
                  ▼
       Procedures / Namespace
                  │
                  ▼
          EDA Tcl Concepts
                  │
                  ▼
        Design Database
                  │
                  ▼
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      Cells      Pins      Nets
        │         │         │
        └─────────┼─────────┘
                  ▼
              Query
                  │
                  ▼
              Filter
                  │
                  ▼
              Analyze
                  │
                  ▼
              Report
```

---

# 48. What You Should Understand After Day 09

You should now understand why Tcl is heavily used in VLSI.

Tcl acts as the **automation layer between you and the EDA tool**.

Instead of manually doing:

```text
Click → Search → Inspect → Calculate → Report
```

you can automate:

```text
Tcl
 │
 ├── Query design
 ├── Find objects
 ├── Filter objects
 ├── Extract properties
 ├── Calculate metrics
 ├── Check constraints
 └── Generate reports
```

That is the real purpose of Tcl in VLSI.

---

# 49. Day 10 Preview

Day 10 will be the final and most practical day.

We will combine everything:

```text
Tcl Basics
     +
File Handling
     +
Arrays / Dictionaries
     +
Regex
     +
Procedures
     +
Namespaces
     +
Error Handling
     +
EDA Queries
     ↓
========================
   VLSI AUTOMATION FLOW
========================
```

The final project will resemble a simplified real-world flow:

```text
             RTL / Design
                  │
                  ▼
             Run Tool
                  │
                  ▼
          Generate Reports
                  │
                  ▼
             Read Reports
                  │
                  ▼
               regexp
                  │
                  ▼
          Extract QoR Metrics
                  │
          ┌───────┼────────┐
          ▼       ▼        ▼
         Area    WNS      TNS
          │       │        │
          └───────┼────────┘
                  ▼
             PASS / FAIL
                  │
                  ▼
          Final QoR Report
```

By the end of Day 10, you should have a small **VLSI Tcl automation project** that you can continue expanding for Genus/Innovus/OpenROAD and other EDA tools.
