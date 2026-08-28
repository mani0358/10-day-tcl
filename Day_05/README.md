# Day_05 — Files, Directories and File I/O

## Objective

The objective of Day 05 is to learn how Tcl interacts with files and directories.

After Day 05, I should be able to:

* Create files
* Open files
* Read files
* Write files
* Append data to files
* Read files line by line
* Check whether files exist
* Create directories
* Find files using `glob`
* Process simple reports
* Generate automated reports

These concepts are extremely important for **VLSI/EDA scripting** because Tcl scripts frequently need to:

```text
Read RTL / configuration files
        ↓
Run EDA commands
        ↓
Generate reports
        ↓
Read reports
        ↓
Extract information
        ↓
Generate summary
```

---

# Environment

## Operating System

Ubuntu Linux

## Tcl Interpreter

`tclsh`

## Where will Day 05 run?

All basic programs run directly in:

```text
Ubuntu Terminal
      ↓
    tclsh
      ↓
Day_05/*.tcl
```

No Cadence tool is required for the basic exercises.

Later, the same file-handling concepts will be used inside:

```text
Genus
Innovus
Tempus
```

---

# 1. Day 05 Learning Flow

```text
             Tcl
              |
      +-------+-------+
      |               |
    Files         Directories
      |               |
      v               v
   open/read       file mkdir
   gets/puts       file exists
   close            glob
      |               |
      +-------+-------+
              |
              v
       Report Processing
              |
              v
       VLSI Automation
```

---

# 2. Important File Commands

| Command        | Purpose                   |
| -------------- | ------------------------- |
| `open`         | Open a file               |
| `close`        | Close a file              |
| `gets`         | Read one line             |
| `read`         | Read file contents        |
| `puts`         | Write to file             |
| `eof`          | Check end of file         |
| `file exists`  | Check whether file exists |
| `file mkdir`   | Create directory          |
| `file delete`  | Delete file/directory     |
| `file size`    | Get file size             |
| `file dirname` | Get directory name        |
| `file tail`    | Get filename              |
| `glob`         | Find files                |
| `pwd`          | Show current directory    |
| `cd`           | Change directory          |

---

# 3. pwd

`pwd` shows the current working directory.

In Ubuntu:

```bash
pwd
```

In Tcl:

```tcl
puts [pwd]
```

Example output:

```text
/home/user/10_day_tcl/Day_05
```

The actual path depends on where the project was created.

---

# 4. cd

`cd` changes the current directory.

Example:

```tcl
cd ..
```

Go into another directory:

```tcl
cd Day_05
```

Check:

```tcl
puts [pwd]
```

---

# 5. Creating a File

The `open` command can create a file.

Syntax:

```tcl
open filename mode
```

Example:

```tcl
set fp [open "output.txt" w]

close $fp
```

This creates:

```text
output.txt
```

---

# 6. File Modes

Important modes:

| Mode | Meaning        |
| ---- | -------------- |
| `r`  | Read           |
| `w`  | Write          |
| `a`  | Append         |
| `r+` | Read and write |

### `w`

```tcl
open "file.txt" w
```

Creates a new file or overwrites an existing file.

### `a`

```tcl
open "file.txt" a
```

Adds new content at the end of the file.

### `r`

```tcl
open "file.txt" r
```

Opens an existing file for reading.

---

# 7. Writing to a File

Use `puts`.

Example:

```tcl
set fp [open "output.txt" w]

puts $fp "Hello Tcl"
puts $fp "This is Day 05"

close $fp
```

This creates:

```text
output.txt
```

containing:

```text
Hello Tcl
This is Day 05
```

Important:

```tcl
puts $fp "Hello Tcl"
```

writes to the file instead of the terminal.

---

# 8. Reading a Complete File

Use:

```tcl
read
```

Example:

```tcl
set fp [open "output.txt" r]

set data [read $fp]

close $fp

puts $data
```

Output:

```text
Hello Tcl
This is Day 05
```

---

# 9. Reading a File Line by Line

The `gets` command reads one line at a time.

Example:

```tcl
set fp [open "output.txt" r]

while {[gets $fp line] >= 0} {
    puts $line
}

close $fp
```

This is one of the most important file-processing patterns in Tcl.

---

# 10. Understanding gets

Consider:

```tcl
gets $fp line
```

Tcl:

```text
File
 |
 v
Read one line
 |
 v
Store in variable "line"
 |
 v
Process line
```

Example:

```tcl
while {[gets $fp line] >= 0} {
    puts "LINE = $line"
}
```

---

# 11. eof

`eof` checks whether the end of a file has been reached.

Example:

```tcl
set fp [open "output.txt" r]

while {![eof $fp]} {
    gets $fp line
    puts $line
}

close $fp
```

However, for normal line-by-line processing, this pattern is generally preferred:

```tcl
while {[gets $fp line] >= 0} {
    puts $line
}
```

---

# 12. Append to a File

Use mode:

```text
a
```

Example:

```tcl
set fp [open "output.txt" a]

puts $fp "Another line"

close $fp
```

The new line is added to the end.

---

# 13. File Exists

Use:

```tcl
file exists
```

Example:

```tcl
if {[file exists "output.txt"]} {
    puts "File exists"
} else {
    puts "File does not exist"
}
```

Output:

```text
File exists
```

---

# 14. File Size

Use:

```tcl
file size
```

Example:

```tcl
if {[file exists "output.txt"]} {
    puts "File size = [file size output.txt] bytes"
}
```

---

# 15. File Name and Directory

Given:

```text
/home/user/project/report.txt
```

Use:

```tcl
puts [file tail "/home/user/project/report.txt"]
```

Output:

```text
report.txt
```

Use:

```tcl
puts [file dirname "/home/user/project/report.txt"]
```

Output:

```text
/home/user/project
```

---

# 16. Creating a Directory

Use:

```tcl
file mkdir
```

Example:

```tcl
file mkdir reports
```

This creates:

```text
reports/
```

If multiple directory levels are required:

```tcl
file mkdir reports/timing
```

---

# 17. Checking a Directory

```tcl
if {[file isdirectory "reports"]} {
    puts "reports is a directory"
}
```

---

# 18. Finding Files with glob

`glob` is used to find files matching a pattern.

Example:

```tcl
set files [glob *.tcl]

foreach file $files {
    puts $file
}
```

This finds all `.tcl` files in the current directory.

For Verilog files:

```tcl
set files [glob *.v]
```

For SystemVerilog:

```tcl
set files [glob *.sv]
```

For reports:

```tcl
set reports [glob *.rpt]
```

---

# 19. glob with a Directory

Example:

```tcl
set files [glob reports/*.rpt]

foreach file $files {
    puts $file
}
```

Conceptually:

```text
reports/
├── timing.rpt
├── area.rpt
└── power.rpt
```

The Tcl script can find all three files automatically.

---

# 20. VLSI Example — Create Report Directory

```tcl
set report_dir "reports"

if {![file exists $report_dir]} {
    file mkdir $report_dir
}

puts "Report directory = $report_dir"
```

This is a very common automation pattern.

---

# 21. VLSI Example — Generate a Report

```tcl
set report_dir "reports"

if {![file exists $report_dir]} {
    file mkdir $report_dir
}

set fp [open "$report_dir/summary.rpt" w]

puts $fp "VLSI Design Summary"
puts $fp "-------------------"
puts $fp "Design      : my_design"
puts $fp "Clock       : clk"
puts $fp "Period      : 10 ns"
puts $fp "Status      : PASS"

close $fp

puts "Report generated."
```

Directory:

```text
Day_05/
└── reports/
    └── summary.rpt
```

---

# 22. Reading a VLSI Report

Suppose `summary.rpt` contains:

```text
VLSI Design Summary
-------------------
Design      : my_design
Clock       : clk
Period      : 10 ns
Status      : PASS
```

Read it:

```tcl
set fp [open "reports/summary.rpt" r]

while {[gets $fp line] >= 0} {
    puts $line
}

close $fp
```

---

# 23. Searching a Report

Suppose:

```text
Status      : PASS
```

is present in the report.

We can search for `PASS`.

Example:

```tcl
set fp [open "reports/summary.rpt" r]

while {[gets $fp line] >= 0} {

    if {[string first "PASS" $line] >= 0} {
        puts "PASS found"
    }
}

close $fp
```

---

# 24. Searching for Timing Violations

Suppose a report contains:

```text
Path 1 Slack = 1.2
Path 2 Slack = 0.5
Path 3 Slack = -0.4
Path 4 Slack = 2.0
```

A simple search can identify negative-slack lines if the report format is known.

For example:

```tcl
set fp [open "timing.rpt" r]

while {[gets $fp line] >= 0} {

    if {[string first "Slack" $line] >= 0} {
        puts $line
    }
}

close $fp
```

This demonstrates the basic concept.

Later, we will learn better parsing techniques rather than relying only on string matching.

---

# 25. Writing a Summary Report

Example:

```tcl
set fp [open "timing_summary.rpt" w]

puts $fp "Timing Summary"
puts $fp "=============="
puts $fp "Setup Slack = 1.25 ns"
puts $fp "Hold Slack  = 0.35 ns"
puts $fp "Status      = PASS"

close $fp
```

The result can then be consumed by another script or reviewed by an engineer.

---

# 26. File Processing Flow

This is an important EDA automation pattern:

```text
Input Report
     |
     v
   open
     |
     v
    gets
     |
     v
 Process line
     |
     v
 if / string / expr
     |
     v
Generate Summary
     |
     v
   puts
     |
     v
Output Report
```

---

# 27. Combining Day 01–05

By Day 05, we can combine almost everything learned so far.

Example:

```tcl
proc process_report {filename} {

    if {![file exists $filename]} {
        puts "ERROR: File does not exist"
        return
    }

    set fp [open $filename r]

    while {[gets $fp line] >= 0} {

        if {[string first "PASS" $line] >= 0} {
            puts "PASS LINE: $line"
        }

        if {[string first "FAIL" $line] >= 0} {
            puts "FAIL LINE: $line"
        }
    }

    close $fp
}

process_report "summary.rpt"
```

This combines:

```text
Day 01 → Variables
Day 02 → Strings
Day 03 → if / while
Day 04 → proc
Day 05 → File I/O
```

---

# 28. Important Safety Practice — Close Files

Whenever you open a file:

```tcl
set fp [open "file.txt" r]
```

close it after use:

```tcl
close $fp
```

Standard pattern:

```tcl
set fp [open "file.txt" r]

# Process file

close $fp
```

---

# 29. Handling File Open Errors

For more robust scripts, `catch` can be used.

Example:

```tcl
if {[catch {open "input.txt" r} fp]} {
    puts "ERROR: Cannot open input.txt"
} else {

    while {[gets $fp line] >= 0} {
        puts $line
    }

    close $fp
}
```

This prevents the script from failing without a useful message when a file cannot be opened.

---

# 30. Day 05 File Structure

Create:

```text
Day_05/
│
├── README.md
├── 01_pwd_cd.tcl
├── 02_create_file.tcl
├── 03_write_file.tcl
├── 04_read_file.tcl
├── 05_read_line_by_line.tcl
├── 06_append_file.tcl
├── 07_file_commands.tcl
├── 08_directories.tcl
├── 09_glob.tcl
├── 10_generate_report.tcl
├── 11_read_report.tcl
├── 12_search_report.tcl
└── 13_practice.tcl
```

Create it from Ubuntu:

```bash
cd ~/10_day_tcl

mkdir -p Day_05

cd Day_05

touch README.md
touch 01_pwd_cd.tcl
touch 02_create_file.tcl
touch 03_write_file.tcl
touch 04_read_file.tcl
touch 05_read_line_by_line.tcl
touch 06_append_file.tcl
touch 07_file_commands.tcl
touch 08_directories.tcl
touch 09_glob.tcl
touch 10_generate_report.tcl
touch 11_read_report.tcl
touch 12_search_report.tcl
touch 13_practice.tcl
```

Check:

```bash
ls -l
```

---

# 31. How to Run

Go to Day 05:

```bash
cd ~/10_day_tcl/Day_05
```

Run:

```bash
tclsh 01_pwd_cd.tcl
```

Run the file-writing example:

```bash
tclsh 03_write_file.tcl
```

Check the generated file:

```bash
ls
```

Read it from Ubuntu:

```bash
cat output.txt
```

Run the report example:

```bash
tclsh 10_generate_report.tcl
```

Then:

```bash
cat reports/summary.rpt
```

---

# 32. Practice Exercises

## Exercise 1 — Create a File

Create:

```text
student.txt
```

and write:

```text
Name = Manjinder
Course = VLSI
Day = 05
```

---

## Exercise 2 — Read the File

Read `student.txt` line by line using:

```tcl
gets
```

and print every line.

---

## Exercise 3 — Append

Append:

```text
Tcl Training = 10 Days
```

to the file.

---

## Exercise 4 — Directory

Create:

```text
reports/
```

using Tcl.

Then create:

```text
reports/timing/
reports/area/
reports/power/
```

---

## Exercise 5 — glob

Find all `.tcl` files in `Day_05`.

Hint:

```tcl
glob *.tcl
```

Print every filename using `foreach`.

---

# 33. VLSI Practice

## Exercise 6 — Generate a Timing Report

Create:

```text
reports/timing.rpt
```

with:

```text
Design = my_design
Clock = clk
Period = 10 ns
Setup Slack = 1.5 ns
Hold Slack = 0.4 ns
Status = PASS
```

---

## Exercise 7 — Read the Report

Write a Tcl script that opens:

```text
reports/timing.rpt
```

and prints every line.

---

## Exercise 8 — Search the Report

Write a Tcl script that finds the line containing:

```text
Setup Slack
```

and prints it.

---

## Exercise 9 — PASS/FAIL

Create a report:

```text
Setup Slack = -0.5 ns
Status = FAIL
```

Write Tcl that searches for:

```text
FAIL
```

and prints:

```text
TIMING VIOLATION FOUND
```

---

## Exercise 10 — Automated Summary

Create a procedure:

```tcl
generate_summary
```

that creates:

```text
reports/summary.rpt
```

and writes:

```text
Design Summary
==============
Design = my_design
Area = 10000 um2
Setup Slack = 1.2 ns
Hold Slack = 0.5 ns
Status = PASS
```

---

# 34. Important EDA Pattern

One of the most important patterns from Day 05 is:

```tcl
set fp [open $filename r]

while {[gets $fp line] >= 0} {

    # Analyze line

}

close $fp
```

You should become very comfortable with this pattern.

It is useful for:

```text
Report processing
Log processing
Configuration files
Generated reports
Tool output
Automation
```

---

# 35. Day 05 Commands to Remember

```text
open
close
read
gets
puts
eof
file exists
file mkdir
file delete
file size
file dirname
file tail
file isdirectory
glob
pwd
cd
catch
```

---

# 36. Day 05 Learning Checklist

* [ ] Understand file I/O
* [ ] Use `open`
* [ ] Use `close`
* [ ] Use `read`
* [ ] Use `gets`
* [ ] Use `puts` with files
* [ ] Understand `r`
* [ ] Understand `w`
* [ ] Understand `a`
* [ ] Use `file exists`
* [ ] Use `file mkdir`
* [ ] Use `file size`
* [ ] Use `file dirname`
* [ ] Use `file tail`
* [ ] Use `glob`
* [ ] Use `pwd`
* [ ] Use `cd`
* [ ] Read files line by line
* [ ] Generate a report
* [ ] Read a report
* [ ] Search a report
* [ ] Understand basic report automation

---

# Day 05 Completion Criteria

Day 05 is complete when I can independently:

1. Create a file using Tcl.
2. Write information into a file.
3. Read a complete file.
4. Read a file line by line.
5. Append information.
6. Check whether a file exists.
7. Create directories.
8. Find files using `glob`.
9. Generate a report.
10. Read and process a report.
11. Search for important information inside a report.
12. Combine file I/O with `proc`, `if`, `foreach` and `while`.

---

# The Big Picture

After Day 05:

```text
                 TCL
                  |
    +-------------+-------------+
    |             |             |
Variables      Lists        Procedures
    |             |             |
    +-------------+-------------+
                  |
            Conditions/Loops
                  |
                  v
              File I/O
                  |
                  v
          Report Processing
                  |
                  v
          EDA Automation
```

This is an important milestone.

From here, Tcl starts becoming much more useful for real VLSI automation.

---

# Next

## Day 06 — Arrays, Dictionaries and Advanced Data Handling

Topics:

* Associative arrays
* `array set`
* `array get`
* `array names`
* `array exists`
* `dict`
* Dictionary keys and values
* Nested data
* Practical data structures
* VLSI design-data examples
* When to use list vs array vs dict

Execution:

```text
Ubuntu → tclsh → Day_06 Tcl scripts
```
