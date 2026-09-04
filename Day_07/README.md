
# 10-Day Tcl Learning Plan — Day 07

# String Processing & Regular Expressions in Tcl

## 1. Day 07 Objective

Today we will learn how to process and extract information from text using Tcl.

This is extremely important in **VLSI/ASIC automation**, because EDA tools generate large text reports such as:

* Timing reports
* Area reports
* Power reports
* Synthesis reports
* Simulation logs
* DRC reports
* LVS reports
* Warnings/errors
* QoR reports

A Tcl automation script often needs to:

```text
Read report
      ↓
Find required line
      ↓
Extract value
      ↓
Store value
      ↓
Check condition
      ↓
Generate summary
```

For example:

```text
Slack = -0.42
```

A Tcl script should be able to extract:

```text
-0.42
```

and determine:

```text
Timing Violation
```

---

# 2. Prerequisites

You should already know:

* `set`
* `puts`
* `if`
* `for`
* `foreach`
* `while`
* Procedures
* Lists
* Arrays
* Dictionaries
* File handling

From previous days:

```text
Day 01 → Tcl basics
Day 02 → Variables and operators
Day 03 → Conditions and loops
Day 04 → Procedures
Day 05 → File handling
Day 06 → Arrays and dictionaries
Day 07 → Strings + Regular Expressions
```

---

# 3. Where Will We Run the Code?

For Day 07, we will use:

```text
Ubuntu Linux
```

and:

```text
tclsh
```

No Cadence tool is required yet.

Your directory should be:

```text
~/10_day_tcl/Day_07
```

Go there using:

```bash
cd ~/10_day_tcl/Day_07
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

---

# 4. Day 07 File Structure

Create:

```text
Day_07/
│
├── README.md
│
├── 01_string_length.tcl
├── 02_string_compare.tcl
├── 03_string_match.tcl
├── 04_string_range.tcl
├── 05_string_search.tcl
├── 06_string_trim_case.tcl
├── 07_string_map_replace.tcl
│
├── 08_regexp_basic.tcl
├── 09_regexp_groups.tcl
├── 10_regsub.tcl
│
├── 11_parse_timing.tcl
├── 12_parse_area.tcl
├── 13_parse_report_to_dict.tcl
│
└── 14_practice.tcl
```

Create the directory:

```bash
mkdir -p ~/10_day_tcl/Day_07
cd ~/10_day_tcl/Day_07
```

---

# 5. Tcl Strings

A string is simply text.

Example:

```tcl
set name "Manjinder"
set cell "NAND2_X1"
set report "timing_report.rpt"

puts $name
puts $cell
puts $report
```

Output:

```text
Manjinder
NAND2_X1
timing_report.rpt
```

---

# 6. string length

Syntax:

```tcl
string length $string
```

Example:

```tcl
set cell "NAND2_X1"

set len [string length $cell]

puts "Length = $len"
```

Output:

```text
Length = 8
```

### Run

```bash
tclsh 01_string_length.tcl
```

---

# 7. string compare

Used to compare two strings.

```tcl
string compare $str1 $str2
```

Example:

```tcl
set a "hello"
set b "hello"

puts [string compare $a $b]
```

Output:

```text
0
```

Meaning:

```text
0 → strings are equal
```

For example:

```tcl
set a "NAND"
set b "NOR"

puts [string compare $a $b]
```

A non-zero value means they are different.

---

# 8. string equal

A more convenient comparison is:

```tcl
string equal $str1 $str2
```

Example:

```tcl
set a "NAND2"
set b "NAND2"

if {[string equal $a $b]} {
    puts "Same cell"
} else {
    puts "Different cell"
}
```

Output:

```text
Same cell
```

---

# 9. string match

`string match` checks whether text matches a pattern.

Example:

```tcl
set cell "NAND2_X1"

if {[string match "NAND*" $cell]} {
    puts "NAND cell"
}
```

Output:

```text
NAND cell
```

The `*` means:

```text
any number of characters
```

Examples:

```tcl
string match "NAND*" "NAND2_X1"
```

True.

```tcl
string match "*X1" "NAND2_X1"
```

True.

```tcl
string match "*INV*" "INV_X4"
```

True.

---

# 10. string range

Extract part of a string.

Syntax:

```tcl
string range $string start end
```

Example:

```tcl
set cell "NAND2_X1"

puts [string range $cell 0 3]
```

Output:

```text
NAND
```

Remember:

Tcl indexes start from:

```text
0
```

For:

```text
N A N D 2 _ X 1
0 1 2 3 4 5 6 7
```

Therefore:

```tcl
string range $cell 0 3
```

returns:

```text
NAND
```

---

# 11. string index

Extract one character.

```tcl
set cell "NAND2_X1"

puts [string index $cell 0]
puts [string index $cell 4]
```

Output:

```text
N
2
```

---

# 12. string first

Find the first occurrence of a substring.

```tcl
set line "Startpoint: U1/A"

set pos [string first ":" $line]

puts "Position = $pos"
```

This is useful when parsing report lines.

---

# 13. string last

Find the last occurrence.

```tcl
set line "path/to/report/timing.rpt"

puts [string last "/" $line]
```

---

# 14. string trim

Remove spaces from beginning and end.

```tcl
set line "   Slack = -0.42   "

set clean [string trim $line]

puts $clean
```

Output:

```text
Slack = -0.42
```

Very useful when processing reports.

---

# 15. string trimleft

```tcl
set line "     Slack = -0.42"

puts [string trimleft $line]
```

---

# 16. string trimright

```tcl
set line "Slack = -0.42     "

puts [string trimright $line]
```

---

# 17. Convert to lowercase

```tcl
set text "TIMING VIOLATION"

puts [string tolower $text]
```

Output:

```text
timing violation
```

---

# 18. Convert to uppercase

```tcl
set text "timing violation"

puts [string toupper $text]
```

Output:

```text
TIMING VIOLATION
```

---

# 19. string map

Replace multiple strings.

Example:

```tcl
set text "NAND NOR INV"

set result [string map {
    NAND AND
    NOR OR
    INV NOT
} $text]

puts $result
```

Output:

```text
AND OR NOT
```

---

# 20. string replace

Replace part of a string.

```tcl
set text "NAND2_X1"

set result [string replace $text 0 3 "NOR"]

puts $result
```

Output:

```text
NOR2_X1
```

---

# 21. Regular Expressions

Now we reach the most important part of Day 07.

A **regular expression (regex)** is a pattern used to search and extract text.

Tcl provides:

```tcl
regexp
```

and:

```tcl
regsub
```

These are extremely useful for VLSI report parsing.

---

# 22. Basic regexp

Example:

```tcl
set line "Timing analysis completed"

if {[regexp "Timing" $line]} {
    puts "Timing information found"
}
```

Output:

```text
Timing information found
```

Syntax:

```tcl
regexp pattern string
```

It returns:

```text
1 → match found
0 → no match
```

---

# 23. Regex Anchors

Two important symbols:

```text
^
$
```

`^` means beginning of line.

`$` means end of line.

Example:

```tcl
regexp "^Slack" "Slack = -0.42"
```

Match.

But:

```tcl
regexp "^Slack" "Total Slack = -0.42"
```

doesn't match because the line does not start with `Slack`.

---

# 24. Character Classes

Some useful regex patterns:

```text
[0-9]       digit
[a-z]       lowercase letter
[A-Z]       uppercase letter
[0-9]+      one or more digits
.           any character
```

Example:

```tcl
set line "Cell count = 1250"

if {[regexp {[0-9]+} $line]} {
    puts "Number found"
}
```

---

# 25. Extracting a Number

This is extremely important.

Suppose:

```text
Cell count = 1250
```

We want:

```text
1250
```

Use:

```tcl
set line "Cell count = 1250"

regexp {[0-9]+} $line count

puts "Count = $count"
```

Output:

```text
Count = 1250
```

---

# 26. Capture Groups

Suppose:

```text
Slack = -0.42
```

We want:

```text
-0.42
```

Use:

```tcl
set line "Slack = -0.42"

regexp {Slack\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> slack

puts "Slack = $slack"
```

Output:

```text
Slack = -0.42
```

The important part is:

```text
([-+]?[0-9]*\.?[0-9]+)
```

This matches a number such as:

```text
-0.42
0.42
+0.42
10
10.25
```

---

# 27. Understanding the Regex

Consider:

```text
[-+]?
```

Means optional:

```text
+
```

or:

```text
-
```

Then:

```text
[0-9]*
```

means zero or more digits.

Then:

```text
\.
```

means a literal decimal point.

Then:

```text
[0-9]+
```

means one or more digits.

Therefore it can match:

```text
-0.42
```

---

# 28. Extract WNS

Suppose a timing report contains:

```text
WNS = -0.42
```

Code:

```tcl
set line "WNS = -0.42"

if {[regexp {WNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> wns]} {
    puts "WNS = $wns"
}
```

Output:

```text
WNS = -0.42
```

---

# 29. Extract TNS

```tcl
set line "TNS = -12.75"

if {[regexp {TNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> tns]} {
    puts "TNS = $tns"
}
```

Output:

```text
TNS = -12.75
```

---

# 30. Check Timing Violation

```tcl
set wns -0.42

if {$wns < 0} {
    puts "TIMING VIOLATION"
} else {
    puts "TIMING CLEAN"
}
```

Output:

```text
TIMING VIOLATION
```

This is how Tcl starts becoming useful for actual EDA automation.

---

# 31. regsub

`regsub` is used to replace text using a regular expression.

Example:

```tcl
set line "Slack = -0.42"

set result [regsub {Slack\s*=\s*} $line ""]

puts $result
```

Output:

```text
-0.42
```

---

# 32. Remove Multiple Spaces

Suppose:

```text
Cell     Area       12.5
```

We can clean it:

```tcl
set line "Cell     Area       12.5"

set clean [regsub -all {\s+} $line " "]

puts $clean
```

Output:

```text
Cell Area 12.5
```

Here:

```text
-all
```

means replace all occurrences.

And:

```text
\s+
```

means one or more whitespace characters.

---

# 33. VLSI Example — Timing Report

Create a file:

```text
timing.rpt
```

with:

```text
Startpoint: REG_A/Q
Endpoint: REG_B/D
Path Group: CLK
WNS = -0.42
TNS = -5.83
Clock Period = 10.00
```

Now Tcl can extract:

```text
Startpoint
Endpoint
WNS
TNS
Clock Period
```

---

# 34. Parse Timing Report

Example:

```tcl
set fp [open "timing.rpt" r]

while {[gets $fp line] >= 0} {

    if {[regexp {WNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> wns]} {
        puts "WNS = $wns"
    }

    if {[regexp {TNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> tns]} {
        puts "TNS = $tns"
    }

    if {[regexp {Clock Period\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> period]} {
        puts "Clock Period = $period"
    }
}

close $fp
```

This combines:

```text
Day 05 → File handling
+
Day 07 → Regex
```

---

# 35. Parsing Cell Area

Suppose the report contains:

```text
Cell: NAND2_X1 Area: 1.20
Cell: NAND2_X2 Area: 2.10
Cell: INV_X1 Area: 0.80
```

We can extract both cell name and area.

```tcl
set line "Cell: NAND2_X1 Area: 1.20"

if {[regexp {Cell:\s+(\S+)\s+Area:\s+([0-9.]+)} $line -> cell area]} {

    puts "Cell = $cell"
    puts "Area = $area"
}
```

Output:

```text
Cell = NAND2_X1
Area = 1.20
```

---

# 36. Parse Area Report

Suppose:

```text
area.rpt
```

contains:

```text
Cell: NAND2_X1 Area: 1.20
Cell: NAND2_X2 Area: 2.10
Cell: INV_X1 Area: 0.80
Cell: BUF_X2 Area: 1.50
```

Tcl:

```tcl
set fp [open "area.rpt" r]

while {[gets $fp line] >= 0} {

    if {[regexp {Cell:\s+(\S+)\s+Area:\s+([0-9.]+)} $line -> cell area]} {

        puts "Cell = $cell"
        puts "Area = $area"
    }
}

close $fp
```

---

# 37. Regex + Dictionary

Now combine Day 06 and Day 07.

```tcl
set timing [dict create]

set fp [open "timing.rpt" r]

while {[gets $fp line] >= 0} {

    if {[regexp {WNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> wns]} {
        dict set timing WNS $wns
    }

    if {[regexp {TNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> tns]} {
        dict set timing TNS $tns
    }

    if {[regexp {Clock Period\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> period]} {
        dict set timing Clock_Period $period
    }
}

close $fp

puts "Timing Information:"
puts "WNS          = [dict get $timing WNS]"
puts "TNS          = [dict get $timing TNS]"
puts "Clock Period = [dict get $timing Clock_Period]"
```

This is a very important pattern:

```text
File
 ↓
Read line
 ↓
Regex
 ↓
Extract value
 ↓
Dictionary
 ↓
Analysis
```

---

# 38. Complete VLSI Timing Checker

We can now make a small timing checker.

```tcl
set wns 0
set tns 0

set fp [open "timing.rpt" r]

while {[gets $fp line] >= 0} {

    if {[regexp {WNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> value]} {
        set wns $value
    }

    if {[regexp {TNS\s*=\s*([-+]?[0-9]*\.?[0-9]+)} $line -> value]} {
        set tns $value
    }
}

close $fp

puts "=============================="
puts "       TIMING SUMMARY"
puts "=============================="

puts "WNS = $wns"
puts "TNS = $tns"

if {$wns < 0} {
    puts "STATUS = FAIL"
    puts "Timing violation detected."
} else {
    puts "STATUS = PASS"
    puts "Timing is clean."
}
```

---

# 39. Important Regex Symbols

Keep this table for reference.

| Pattern | Meaning        |
| ------- | -------------- |
| `.`     | Any character  |
| `*`     | Zero or more   |
| `+`     | One or more    |
| `?`     | Optional       |
| `^`     | Beginning      |
| `$`     | End            |
| `[0-9]` | Digit          |
| `[a-z]` | Lowercase      |
| `[A-Z]` | Uppercase      |
| `\s`    | Whitespace     |
| `\S`    | Non-whitespace |
| `\d`    | Digit          |
| `(...)` | Capture group  |
| `\.`    | Literal `.`    |

---

# 40. string vs regexp

Use:

```text
string
```

when you already know the exact text operation.

Example:

```tcl
string first ":" $line
```

Use:

```text
regexp
```

when the text has a variable format.

Example:

```tcl
regexp {WNS\s*=\s*([-+]?[0-9.]+)} $line -> wns
```

---

# 41. Important VLSI Automation Pattern

You should remember this:

```tcl
set fp [open "report.rpt" r]

while {[gets $fp line] >= 0} {

    if {[regexp {PATTERN} $line -> value]} {

        # process value

    }
}

close $fp
```

This pattern will be used repeatedly when working with:

* Genus
* Innovus
* Tempus
* PrimeTime
* Design Compiler
* VCS
* OpenROAD

---

# 42. Day 07 Exercises

## Exercise 1 — String Length

Create:

```tcl
set cell "INV_X8"
```

Find its length.

---

## Exercise 2 — String Match

Check whether:

```text
NAND2_X1
```

matches:

```text
NAND*
```

---

## Exercise 3 — Extract Cell Name

Given:

```text
Cell: BUF_X4
```

Extract:

```text
BUF_X4
```

using `regexp`.

---

## Exercise 4 — Extract Slack

Given:

```text
Slack = -0.35
```

extract:

```text
-0.35
```

---

## Exercise 5 — Timing Status

Given:

```text
WNS = -0.25
```

print:

```text
TIMING FAIL
```

If:

```text
WNS = 0.25
```

print:

```text
TIMING PASS
```

---

## Exercise 6 — Parse Multiple Cells

Given:

```text
Cell: NAND2_X1 Area: 1.20
Cell: NAND2_X2 Area: 2.10
Cell: INV_X1 Area: 0.80
Cell: BUF_X2 Area: 1.50
```

Extract:

```text
Cell name
Area
```

for every line.

---

## Exercise 7 — WNS/TNS Checker

Create:

```text
timing.rpt
```

containing:

```text
WNS = -0.42
TNS = -5.83
Clock Period = 10.00
```

Extract all three values and print:

```text
========================
TIMING REPORT
========================

WNS          = -0.42
TNS          = -5.83
Clock Period = 10.00

STATUS       = FAIL
```

---

# 43. How to Run Day 07

Go to:

```bash
cd ~/10_day_tcl/Day_07
```

Run individual programs:

```bash
tclsh 01_string_length.tcl
```

```bash
tclsh 03_string_match.tcl
```

```bash
tclsh 08_regexp_basic.tcl
```

```bash
tclsh 09_regexp_groups.tcl
```

```bash
tclsh 11_parse_timing.tcl
```

```bash
tclsh 13_parse_report_to_dict.tcl
```

---

# 44. Quick Revision

Today you learned:

### String commands

```tcl
string length
string compare
string equal
string match
string first
string last
string range
string index
string trim
string trimleft
string trimright
string toupper
string tolower
string map
string replace
```

### Regex

```tcl
regexp
regsub
```

### Important regex concepts

```text
^
$
.
*
+
?
[]
()
\s
\S
```

### VLSI skills

You can now:

```text
Read report
      ↓
Search report
      ↓
Extract values
      ↓
Store values
      ↓
Check timing
      ↓
Generate result
```

---

# 45. Day 07 Checklist

Before moving to Day 08, make sure you can:

* [ ] Find string length
* [ ] Compare strings
* [ ] Match string patterns
* [ ] Extract part of a string
* [ ] Search inside strings
* [ ] Remove whitespace
* [ ] Change case
* [ ] Replace text
* [ ] Understand regex
* [ ] Use `regexp`
* [ ] Use capture groups
* [ ] Extract numbers
* [ ] Use `regsub`
* [ ] Parse a timing report
* [ ] Parse an area report
* [ ] Store extracted values in a dictionary
* [ ] Detect timing violations

---

# 46. Most Important Concept of Day 07

The most important thing to remember is:

```text
Tcl is not only a scripting language for running commands.

Tcl can also READ, UNDERSTAND and ANALYZE
the text generated by EDA tools.
```

For example:

```text
Genus/Innovus/Tempus
        ↓
    Report File
        ↓
     Tcl read
        ↓
      regexp
        ↓
 Extract WNS/TNS/Area
        ↓
    Dictionary
        ↓
   PASS / FAIL
```

This is the foundation for **VLSI EDA automation**.

---

# 47. Day 08 Preview

In Day 08 we will move toward more advanced Tcl programming and automation.

Topics will include:

```text
Advanced procedures
Error handling
catch
return
global variables
namespace
source
Reusable Tcl libraries
Modular Tcl scripts
```

We will start moving from:

```text
learning individual Tcl commands
```

toward:

```text
building reusable VLSI automation scripts
```

which is much closer to how Tcl is actually used in EDA flows.
