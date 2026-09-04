
# 10-Day Tcl Learning Plan — Day 08

# Advanced Procedures, Error Handling, Source & Namespaces

---

## 1. Day 08 Objective

Until Day 07, we learned Tcl commands individually:

```text
Day 01 → Tcl basics
Day 02 → Variables/operators
Day 03 → Conditions/loops
Day 04 → Procedures
Day 05 → File handling
Day 06 → Arrays/dictionaries
Day 07 → Strings/regexp
```

Today we start writing Tcl like an **automation engineer**.

We will learn:

* Advanced procedures
* Procedure arguments
* Default arguments
* Variable arguments
* `return`
* `catch`
* `error`
* `source`
* Global variables
* Local variables
* Namespaces
* Reusable Tcl libraries
* Modular Tcl scripts
* VLSI automation structure

The goal is to move from:

```text
Small Tcl programs
```

to:

```text
Reusable Tcl automation framework
```

---

# 2. Where Will We Run Day 08?

We will continue using:

```text
Ubuntu Linux
```

with:

```text
tclsh
```

Go to:

```bash
cd ~/10_day_tcl
```

Create the directory:

```bash
mkdir -p Day_08
cd Day_08
```

Check Tcl:

```bash
tclsh
```

Then:

```tcl
puts [info patchlevel]
```

Exit:

```tcl
exit
```

No Cadence tool is required for today's basic exercises.

---

# 3. Day 08 Directory Structure

Create:

```text
Day_08/
│
├── README.md
│
├── 01_proc_arguments.tcl
├── 02_default_arguments.tcl
├── 03_variable_arguments.tcl
├── 04_return.tcl
├── 05_global_variable.tcl
├── 06_local_variable.tcl
├── 07_catch.tcl
├── 08_error.tcl
├── 09_source.tcl
├── 10_namespace.tcl
├── 11_namespace_variables.tcl
├── 12_vlsi_utils.tcl
├── 13_main.tcl
├── 14_timing_utils.tcl
├── 15_vlsi_flow.tcl
└── 16_practice.tcl
```

---

# 4. Why Procedures Matter

Imagine we need to check timing many times.

Without a procedure:

```tcl
if {$wns < 0} {
    puts "Timing FAIL"
}
```

Then again:

```tcl
if {$wns2 < 0} {
    puts "Timing FAIL"
}
```

And again:

```tcl
if {$wns3 < 0} {
    puts "Timing FAIL"
}
```

This is repetitive.

Instead create:

```tcl
proc check_timing {wns} {

    if {$wns < 0} {
        return "FAIL"
    } else {
        return "PASS"
    }
}
```

Then:

```tcl
puts [check_timing -0.42]
puts [check_timing 0.15]
```

Output:

```text
FAIL
PASS
```

This is the basic idea behind reusable Tcl automation.

---

# 5. Procedure Arguments

Basic syntax:

```tcl
proc procedure_name {arguments} {
    commands
}
```

Example:

```tcl
proc add {a b} {

    set result [expr {$a + $b}]

    return $result
}

puts [add 10 20]
```

Output:

```text
30
```

---

# 6. Multiple Arguments

Example:

```tcl
proc cell_info {cell area delay} {

    puts "Cell  = $cell"
    puts "Area  = $area"
    puts "Delay = $delay"
}

cell_info NAND2_X1 1.20 0.15
```

Output:

```text
Cell  = NAND2_X1
Area  = 1.20
Delay = 0.15
```

---

# 7. Default Arguments

Sometimes an argument should have a default value.

Example:

```tcl
proc greet {{name "Engineer"}} {

    puts "Hello $name"
}
```

Calling without argument:

```tcl
greet
```

Output:

```text
Hello Engineer
```

Calling with argument:

```tcl
greet Manjinder
```

Output:

```text
Hello Manjinder
```

---

# 8. VLSI Example — Default Clock

```tcl
proc report_clock {{clock "clk"}} {

    puts "Analyzing clock: $clock"
}

report_clock
report_clock clk_100MHz
```

Output:

```text
Analyzing clock: clk
Analyzing clock: clk_100MHz
```

Default arguments are useful when building reusable scripts.

---

# 9. Variable Number of Arguments

Tcl supports:

```tcl
args
```

for accepting any number of arguments.

Example:

```tcl
proc print_cells {args} {

    foreach cell $args {
        puts "Cell = $cell"
    }
}

print_cells NAND2_X1 INV_X1 BUF_X2
```

Output:

```text
Cell = NAND2_X1
Cell = INV_X1
Cell = BUF_X2
```

---

# 10. Understanding `args`

Suppose:

```tcl
print_cells NAND2_X1 INV_X1 BUF_X2
```

Inside the procedure:

```text
args
```

contains:

```text
NAND2_X1 INV_X1 BUF_X2
```

Therefore:

```tcl
foreach cell $args
```

iterates over every cell.

---

# 11. return

`return` sends a value back from a procedure.

Example:

```tcl
proc square {x} {

    return [expr {$x * $x}]
}

set result [square 5]

puts "Result = $result"
```

Output:

```text
Result = 25
```

---

# 12. return for Timing Check

```tcl
proc check_wns {wns} {

    if {$wns < 0} {
        return "FAIL"
    }

    return "PASS"
}

set status [check_wns -0.32]

puts "Timing Status = $status"
```

Output:

```text
Timing Status = FAIL
```

---

# 13. Early return

`return` can stop a procedure immediately.

Example:

```tcl
proc check_area {area} {

    if {$area <= 0} {
        return "INVALID"
    }

    if {$area > 1000} {
        return "AREA TOO LARGE"
    }

    return "AREA OK"
}

puts [check_area 500]
puts [check_area 1500]
puts [check_area -2]
```

Output:

```text
AREA OK
AREA TOO LARGE
INVALID
```

---

# 14. Local Variables

Variables created inside a procedure are normally local to that procedure.

Example:

```tcl
proc test {} {

    set value 100

    puts "Inside = $value"
}

test
```

The variable:

```text
value
```

belongs to the procedure's local scope.

---

# 15. Global Variables

Sometimes a procedure needs access to a variable outside itself.

Example:

```tcl
set design_name "my_design"

proc show_design {} {

    global design_name

    puts "Design = $design_name"
}

show_design
```

Output:

```text
Design = my_design
```

However, avoid excessive use of global variables.

A better solution for larger Tcl projects is often:

```text
namespace
```

---

# 16. Why Global Variables Can Become a Problem

Imagine a large script containing:

```text
100 procedures
50 variables
20 files
```

If everything uses global variables:

```text
design
clock
area
delay
slack
mode
path
report
...
```

different procedures can accidentally modify the same variable.

This makes debugging difficult.

Therefore:

```text
Small script
    ↓
global may be acceptable

Large automation framework
    ↓
namespace is better
```

---

# 17. catch

One of the most important error-handling commands is:

```tcl
catch
```

It allows us to execute a command and detect whether it generated an error.

Example:

```tcl
if {[catch {expr {10 / 0}} result]} {

    puts "Error occurred"
    puts "Message = $result"

} else {

    puts "Result = $result"
}
```

Output will indicate an error instead of terminating the entire script.

---

# 18. Why `catch` Is Important in VLSI

EDA scripts often execute commands such as:

```text
Read RTL
↓
Elaborate
↓
Synthesize
↓
Read constraints
↓
Optimize
↓
Generate reports
```

If one operation fails, we may want to:

```text
Detect failure
↓
Print useful message
↓
Stop or recover cleanly
```

instead of getting an unclear Tcl error.

---

# 19. catch with File Handling

Example:

```tcl
if {[catch {open "missing.rpt" r} fp]} {

    puts "ERROR: Could not open report"

} else {

    puts "Report opened successfully"
    close $fp
}
```

This is much safer than assuming every file exists.

---

# 20. error

We can deliberately generate an error using:

```tcl
error
```

Example:

```tcl
proc check_area {area} {

    if {$area < 0} {
        error "Area cannot be negative"
    }

    puts "Area = $area"
}

check_area -10
```

This generates an error message.

---

# 21. catch + error

This combination is useful.

```tcl
proc check_area {area} {

    if {$area < 0} {
        error "Invalid area: $area"
    }

    return "Area is valid"
}

if {[catch {check_area -10} result]} {

    puts "ERROR: $result"

} else {

    puts $result
}
```

Output:

```text
ERROR: Invalid area: -10
```

---

# 22. source

`source` is extremely important for Tcl automation.

It allows one Tcl file to load another Tcl file.

Suppose:

```text
vlsi_utils.tcl
```

contains:

```tcl
proc hello {} {
    puts "Hello from utility file"
}
```

Another file:

```text
main.tcl
```

contains:

```tcl
source vlsi_utils.tcl

hello
```

Run:

```bash
tclsh main.tcl
```

Output:

```text
Hello from utility file
```

---

# 23. Why `source` Matters

Instead of putting everything in one giant file:

```text
run.tcl
```

we can divide the project:

```text
main.tcl
    ↓
source utilities.tcl
    ↓
source timing.tcl
    ↓
source area.tcl
    ↓
source report.tcl
```

This gives us:

```text
Modular Tcl
```

---

# 24. VLSI Tcl Project Structure

A practical automation project can look like:

```text
vlsi_flow/
│
├── main.tcl
│
├── config.tcl
│
├── utils.tcl
├── timing_utils.tcl
├── area_utils.tcl
│
└── reports/
```

Then:

```text
main.tcl
   │
   ├── source config.tcl
   ├── source utils.tcl
   ├── source timing_utils.tcl
   └── source area_utils.tcl
```

This is much closer to professional scripting.

---

# 25. Namespaces

A namespace provides a separate scope for related procedures and variables.

Basic syntax:

```tcl
namespace eval vlsi {

    variable design

    proc hello {} {
        puts "Hello from VLSI namespace"
    }
}
```

Call the procedure:

```tcl
vlsi::hello
```

Output:

```text
Hello from VLSI namespace
```

---

# 26. Why Namespace?

Suppose we have:

```text
timing::report
area::report
power::report
```

Each module can have its own procedures.

This avoids naming conflicts.

For example:

```tcl
namespace eval timing {

    proc report {} {
        puts "Timing report"
    }
}

namespace eval area {

    proc report {} {
        puts "Area report"
    }
}
```

Call:

```tcl
timing::report
area::report
```

Output:

```text
Timing report
Area report
```

Both procedures are called:

```text
report
```

but they belong to different namespaces.

---

# 27. Namespace Variables

Example:

```tcl
namespace eval design {

    variable name "my_chip"
    variable area 1250

    proc show {} {

        variable name
        variable area

        puts "Design = $name"
        puts "Area   = $area"
    }
}

design::show
```

Output:

```text
Design = my_chip
Area   = 1250
```

---

# 28. Building a VLSI Utility Library

Create:

```text
12_vlsi_utils.tcl
```

Contents:

```tcl
namespace eval vlsi {

    proc check_wns {wns} {

        if {$wns < 0} {
            return "FAIL"
        }

        return "PASS"
    }

    proc check_area {area limit} {

        if {$area > $limit} {
            return "FAIL"
        }

        return "PASS"
    }

    proc print_separator {} {

        puts "=============================="
    }
}
```

---

# 29. Main Program

Create:

```text
13_main.tcl
```

Contents:

```tcl
source 12_vlsi_utils.tcl

vlsi::print_separator

set wns -0.42
set area 850
set area_limit 1000

puts "WNS Status  = [vlsi::check_wns $wns]"
puts "Area Status = [vlsi::check_area $area $area_limit]"

vlsi::print_separator
```

Run:

```bash
tclsh 13_main.tcl
```

Expected output:

```text
==============================
WNS Status  = FAIL
Area Status = PASS
==============================
```

---

# 30. Timing Utility Library

Create:

```text
14_timing_utils.tcl
```

Use:

```tcl
namespace eval timing {

    proc check_wns {wns} {

        if {$wns < 0} {
            return "FAIL"
        }

        return "PASS"
    }

    proc check_tns {tns} {

        if {$tns < 0} {
            return "FAIL"
        }

        return "PASS"
    }

    proc summary {wns tns} {

        puts "=============================="
        puts "TIMING SUMMARY"
        puts "=============================="

        puts "WNS = $wns"
        puts "TNS = $tns"

        puts "WNS STATUS = [check_wns $wns]"
        puts "TNS STATUS = [check_tns $tns]"
    }
}
```

---

# 31. Using the Timing Library

Create:

```text
15_vlsi_flow.tcl
```

Code:

```tcl
source 14_timing_utils.tcl

set wns -0.25
set tns -4.50

timing::summary $wns $tns
```

Run:

```bash
tclsh 15_vlsi_flow.tcl
```

Output:

```text
==============================
TIMING SUMMARY
==============================
WNS = -0.25
TNS = -4.50
WNS STATUS = FAIL
TNS STATUS = FAIL
```

---

# 32. Combining Day 05 + Day 06 + Day 07 + Day 08

Now we can combine everything learned so far.

```text
Day 05
File handling
     ↓
Day 07
Regexp
     ↓
Day 06
Dictionary
     ↓
Day 08
Procedures + namespace
```

This produces a real report-processing flow.

---

# 33. Example Architecture

```text
                 timing.rpt
                     │
                     ▼
              Read report file
                     │
                     ▼
                  regexp
                     │
                     ▼
             Extract WNS/TNS
                     │
                     ▼
                Dictionary
                     │
                     ▼
             Timing procedure
                     │
                     ▼
               PASS / FAIL
                     │
                     ▼
              Final summary
```

---

# 34. A More Professional Main Script

A larger Tcl automation script could look like:

```tcl
# main.tcl

source config.tcl
source utils.tcl
source timing_utils.tcl
source area_utils.tcl

puts "Starting VLSI flow..."

if {[catch {

    # Run flow here

} result]} {

    puts "FLOW FAILED"
    puts "Reason: $result"

} else {

    puts "FLOW COMPLETED SUCCESSFULLY"
}
```

This is a much better structure than putting hundreds of commands into one file.

---

# 35. Important Tcl Concept — Scope

Remember:

```text
Global scope
    │
    ├── variables
    │
    └── procedures
```

Procedure:

```text
proc
 │
 └── local scope
```

Namespace:

```text
namespace
 │
 ├── variables
 └── procedures
```

This becomes increasingly important as your scripts become larger.

---

# 36. Important Tcl Commands of Day 08

You should remember:

```tcl
proc
return
args
global
variable
catch
error
source
namespace
namespace eval
```

---

# 37. Difference Between `source` and `proc`

This is important.

### `proc`

Creates a reusable procedure:

```tcl
proc add {a b} {
    return [expr {$a + $b}]
}
```

### `source`

Loads another Tcl file:

```tcl
source utils.tcl
```

So:

```text
proc   → creates reusable function
source → loads reusable Tcl code
```

---

# 38. Difference Between `catch` and `error`

### `error`

Creates an error:

```tcl
error "Something went wrong"
```

### `catch`

Catches an error:

```tcl
catch {some_command} result
```

Therefore:

```text
error
   ↓
creates error

catch
   ↓
handles/catches error
```

---

# 39. VLSI Use of Error Handling

Consider:

```tcl
set fp [open "timing.rpt" r]
```

If the file doesn't exist, the script may fail.

Better:

```tcl
if {[catch {open "timing.rpt" r} fp]} {

    puts "ERROR: timing report not found"
    exit 1
}
```

This gives the user a clear message.

---

# 40. VLSI Automation Rule

A good automation script should:

```text
1. Check inputs
2. Execute operation
3. Check errors
4. Process output
5. Generate report
6. Clearly report PASS/FAIL
```

For example:

```text
Check RTL
   ↓
Read RTL
   ↓
Elaborate
   ↓
Synthesize
   ↓
Generate timing report
   ↓
Parse report
   ↓
Check WNS/TNS
   ↓
PASS/FAIL
```

---

# 41. Practice Exercises

## Exercise 1 — Procedure

Create:

```tcl
proc multiply {a b} {
    ...
}
```

Expected:

```tcl
puts [multiply 5 4]
```

Output:

```text
20
```

---

## Exercise 2 — Default Argument

Create:

```tcl
proc show_clock {{clock "clk"}} {
    ...
}
```

Test:

```tcl
show_clock
show_clock clk_200MHz
```

---

## Exercise 3 — Variable Arguments

Create:

```tcl
proc show_cells {args} {
    ...
}
```

Call:

```tcl
show_cells NAND2_X1 INV_X1 BUF_X2 XOR2_X1
```

---

## Exercise 4 — Return

Create a procedure:

```text
check_slack
```

It should return:

```text
PASS
```

when slack >= 0.

Otherwise:

```text
FAIL
```

---

## Exercise 5 — catch

Try opening:

```text
does_not_exist.rpt
```

using `catch`.

Print:

```text
ERROR: Report not found
```

---

## Exercise 6 — Namespace

Create:

```text
timing
area
power
```

namespaces.

Each should contain:

```text
report
```

procedure.

Call:

```tcl
timing::report
area::report
power::report
```

---

## Exercise 7 — VLSI Utility

Create:

```text
vlsi_utils.tcl
```

with procedures:

```text
check_wns
check_tns
check_area
print_summary
```

Then load it using:

```tcl
source vlsi_utils.tcl
```

---

# 42. Day 08 Mini Project

Build:

```text
VLSI QoR Checker
```

Input:

```text
WNS  = -0.32
TNS  = -5.40
AREA = 950
```

Limits:

```text
WNS  >= 0
TNS  >= 0
AREA <= 1000
```

Your Tcl program should print:

```text
================================
        VLSI QoR SUMMARY
================================

WNS  = -0.32
TNS  = -5.40
AREA = 950

WNS STATUS  = FAIL
TNS STATUS  = FAIL
AREA STATUS = PASS

OVERALL STATUS = FAIL
================================
```

Use:

* Procedures
* `return`
* Namespace
* `source`

Do not write everything inside one procedure.

---

# 43. Recommended Mini Project Structure

```text
Day_08/
│
├── main.tcl
├── config.tcl
├── vlsi_utils.tcl
├── timing_utils.tcl
└── area_utils.tcl
```

### `config.tcl`

```tcl
set WNS_LIMIT 0
set TNS_LIMIT 0
set AREA_LIMIT 1000
```

### `timing_utils.tcl`

Contains:

```tcl
check_wns
check_tns
```

### `area_utils.tcl`

Contains:

```tcl
check_area
```

### `vlsi_utils.tcl`

Contains:

```tcl
print_summary
```

### `main.tcl`

Loads everything:

```tcl
source config.tcl
source timing_utils.tcl
source area_utils.tcl
source vlsi_utils.tcl
```

This is the beginning of a **modular VLSI Tcl framework**.

---

# 44. Run Commands

Go to:

```bash
cd ~/10_day_tcl/Day_08
```

Run:

```bash
tclsh 01_proc_arguments.tcl
```

Run error handling:

```bash
tclsh 07_catch.tcl
```

Run namespace:

```bash
tclsh 10_namespace.tcl
```

Run the VLSI utility example:

```bash
tclsh 13_main.tcl
```

Run the timing utility:

```bash
tclsh 15_vlsi_flow.tcl
```

---

# 45. Day 08 Checklist

Before moving to Day 09, you should be able to:

* [ ] Create procedures
* [ ] Pass procedure arguments
* [ ] Use default arguments
* [ ] Use variable arguments
* [ ] Use `return`
* [ ] Understand local scope
* [ ] Understand global scope
* [ ] Use `global`
* [ ] Use `catch`
* [ ] Use `error`
* [ ] Use `source`
* [ ] Understand namespaces
* [ ] Create namespace procedures
* [ ] Create namespace variables
* [ ] Build Tcl utility files
* [ ] Build a modular Tcl project
* [ ] Create a basic VLSI QoR checker

---

# 46. Day 08 Big Picture

The evolution of your Tcl knowledge is now:

```text
                Tcl
                 │
                 ▼
           Basic Commands
                 │
                 ▼
             Variables
                 │
                 ▼
         Conditions / Loops
                 │
                 ▼
            Procedures
                 │
                 ▼
          File Processing
                 │
                 ▼
       Arrays / Dictionaries
                 │
                 ▼
        String / Regular Expr.
                 │
                 ▼
      Advanced Procedures
                 │
                 ▼
       Error Handling
                 │
                 ▼
      Source / Namespaces
                 │
                 ▼
       Modular Tcl Scripts
                 │
                 ▼
        VLSI Automation
```

---

# 47. Most Important Concept of Day 08

The key lesson is:

```text
Do NOT write one huge Tcl script.
```

Instead:

```text
main.tcl
   │
   ├── config.tcl
   ├── utils.tcl
   ├── timing_utils.tcl
   ├── area_utils.tcl
   └── report_utils.tcl
```

Each file has a specific responsibility.

That makes the automation:

```text
Reusable
Readable
Maintainable
Debuggable
Scalable
```

and prepares you for actual **EDA tool Tcl scripting**.

---

# 48. Day 09 Preview

Day 09 will move much closer to real EDA automation.

We will learn how Tcl interacts with **EDA/tool commands**, including concepts such as:

```text
Tool command execution
Command options
Collections
get_* style commands
Filtering objects
Object properties
foreach over tool objects
Querying cells
Querying pins
Querying nets
Querying clocks
```

We will begin developing the mindset:

```text
Tcl
 ↓
EDA Tool
 ↓
Query Design
 ↓
Process Objects
 ↓
Generate Reports
```

This is where Tcl starts becoming directly useful for **Genus / Innovus / Tempus / PrimeTime / OpenROAD automation**.
