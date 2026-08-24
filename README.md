# 10-Day Tcl for VLSI / ASIC / EDA

## 📌 Overview

This repository contains my **10-Day Tcl Learning Journey**, focused on learning Tcl from basic programming concepts to practical **VLSI/ASIC EDA automation**.

The goal is not only to learn Tcl syntax, but also to understand how Tcl is used in real VLSI tools such as:

* Cadence Genus
* Cadence Innovus
* Cadence Tempus
* Other Tcl-based EDA environments

By the end of these 10 days, I should be able to write Tcl scripts for:

* RTL automation
* File and directory handling
* EDA tool control
* Synthesis automation
* Timing analysis
* Report generation
* Design database queries
* Basic ASIC flow automation

---

# 🎯 Goals

By completing this 10-Day Tcl course, I aim to:

1. Understand Tcl syntax and programming fundamentals.
2. Write and execute Tcl scripts independently.
3. Understand Tcl variables, lists, strings and expressions.
4. Use conditions and loops.
5. Create and use Tcl procedures.
6. Work with files and directories.
7. Execute Linux commands from Tcl.
8. Understand Tcl inside EDA tools.
9. Write basic Cadence Genus automation scripts.
10. Understand Tcl-based synthesis and timing automation.
11. Automate report generation.
12. Build a basic ASIC flow automation script.

---

# 🗓️ 10-Day Learning Plan

| Day    | Topic                                   | Environment              |
| ------ | --------------------------------------- | ------------------------ |
| Day 1  | Tcl Basics                              | `tclsh`                  |
| Day 2  | Variables, Expressions, Strings & Lists | `tclsh`                  |
| Day 3  | Conditions & Loops                      | `tclsh`                  |
| Day 4  | Procedures & Arguments                  | `tclsh`                  |
| Day 5  | Files & Directories                     | `tclsh` + Linux          |
| Day 6  | Tcl + Linux + Environment Variables     | `tclsh` + Linux          |
| Day 7  | Tcl in EDA Tools                        | Genus / Innovus          |
| Day 8  | Genus Synthesis Automation              | Cadence Genus            |
| Day 9  | Timing & Report Automation              | Genus / Tempus           |
| Day 10 | Complete ASIC Flow Automation           | Genus / Innovus / Tempus |

---

# 🧠 Learning Approach

Each day will follow this structure:

```text
Theory
   ↓
Syntax
   ↓
Simple Tcl Example
   ↓
Run in tclsh
   ↓
VLSI-Oriented Example
   ↓
EDA Tool Example
   ↓
Practice Problems
   ↓
Mini Project
```

The objective is to understand **why a command is used**, not just memorize its syntax.

---

# 💻 Where Tcl Will Be Run

## 1. Tcl Shell

Basic Tcl programs will be executed using:

```bash
tclsh
```

Example:

```bash
tclsh day01.tcl
```

Used mainly for:

* Tcl syntax
* Variables
* Lists
* Strings
* Conditions
* Loops
* Procedures
* File handling

---

## 2. Linux Terminal

Tcl can interact with Linux commands.

Examples:

```tcl
exec ls
exec pwd
exec mkdir results
```

This will be used for automation.

---

## 3. Cadence Genus

Tcl is heavily used to automate synthesis.

Typical execution:

```bash
genus -f run_genus.tcl
```

Examples of tasks:

```text
Read RTL
   ↓
Elaborate
   ↓
Apply constraints
   ↓
Synthesis
   ↓
Generate reports
```

---

## 4. Cadence Innovus

Tcl is also used for physical design automation.

Typical flow:

```text
Floorplan
   ↓
Placement
   ↓
CTS
   ↓
Routing
   ↓
Reports
```

Example commands will include tool-specific commands such as:

```tcl
floorPlan
placeDesign
ccopt_design
routeDesign
```

---

## 5. Cadence Tempus

Tcl can automate static timing analysis.

Typical tasks:

```text
Read Netlist
   ↓
Read Constraints
   ↓
Update Timing
   ↓
Report Timing
```

---

# 📂 Repository Structure

The repository will be organized day-by-day:

```text
10_day_tcl/
│
├── README.md
│
├── day01/
│   ├── README.md
│   ├── basic.tcl
│   ├── variables.tcl
│   └── practice.tcl
│
├── day02/
│   ├── README.md
│   ├── expressions.tcl
│   ├── strings.tcl
│   ├── lists.tcl
│   └── practice.tcl
│
├── day03/
│   ├── README.md
│   ├── if_else.tcl
│   ├── for_loop.tcl
│   ├── foreach.tcl
│   ├── while.tcl
│   └── practice.tcl
│
├── day04/
│   ├── README.md
│   ├── procedures.tcl
│   ├── arguments.tcl
│   └── practice.tcl
│
├── day05/
│   ├── README.md
│   ├── file_read.tcl
│   ├── file_write.tcl
│   ├── directory.tcl
│   └── practice.tcl
│
├── day06/
│   ├── README.md
│   ├── linux_commands.tcl
│   ├── environment.tcl
│   └── practice.tcl
│
├── day07/
│   ├── README.md
│   ├── genus_basic.tcl
│   ├── collections.tcl
│   └── practice.tcl
│
├── day08/
│   ├── README.md
│   ├── rtl/
│   ├── scripts/
│   ├── reports/
│   └── logs/
│
├── day09/
│   ├── README.md
│   ├── scripts/
│   ├── constraints/
│   ├── reports/
│   └── logs/
│
└── day10/
    ├── README.md
    ├── rtl/
    ├── scripts/
    ├── constraints/
    ├── reports/
    ├── logs/
    └── results/
```

---

# 📚 Day-by-Day Topics

## Day 1 — Tcl Fundamentals

Topics:

* What is Tcl?
* Tcl interpreter
* Commands
* Arguments
* `puts`
* `set`
* Variable substitution
* Command substitution
* Comments
* Basic arithmetic
* `expr`

Environment:

```text
Linux → tclsh
```

---

## Day 2 — Data Handling

Topics:

* Variables
* Numbers
* Strings
* Expressions
* Lists
* `lindex`
* `llength`
* `lappend`
* `lrange`
* `string` commands

Environment:

```text
Linux → tclsh
```

---

## Day 3 — Decision Making & Loops

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

VLSI application:

```text
Iterating through cells
Iterating through ports
Iterating through nets
Processing reports
```

Environment:

```text
Linux → tclsh
```

---

## Day 4 — Procedures

Topics:

* `proc`
* Arguments
* Default arguments
* `return`
* Local variables
* Global variables
* Reusable functions

VLSI application:

```text
Reusable report functions
Reusable checking functions
Automation functions
```

Environment:

```text
Linux → tclsh
```

---

## Day 5 — File Handling

Topics:

* `open`
* `close`
* `read`
* `gets`
* `puts`
* `file exists`
* `file mkdir`
* File paths
* Directory handling

VLSI application:

```text
Read RTL files
Read reports
Create report directories
Create log files
Process synthesis results
```

Environment:

```text
Linux → tclsh
```

---

## Day 6 — Tcl + Linux

Topics:

* `exec`
* Environment variables
* `$::env()`
* `pwd`
* `cd`
* `glob`
* Shell commands
* Automation scripts

VLSI application:

```text
Run EDA commands
Create directories
Find files
Process logs
Automate flow execution
```

Environment:

```text
Linux → tclsh
```

---

## Day 7 — Tcl in EDA Tools

Topics:

* Tcl interpreter inside EDA tools
* Tool commands
* Design objects
* Collections
* `get_*` commands
* Querying design information
* Filtering objects
* `foreach`

Concept:

```text
Tcl
 ↓
EDA Command
 ↓
Design Database
 ↓
Objects / Collections
 ↓
Process with Tcl
```

Environment:

```text
Cadence Genus
Cadence Innovus
```

---

## Day 8 — Genus Automation

Topics:

* Reading RTL
* Elaborating design
* Libraries
* Technology setup
* Constraints
* Synthesis
* Optimization
* Area reports
* Timing reports

Basic flow:

```text
RTL
 ↓
Read
 ↓
Elaborate
 ↓
Constraints
 ↓
Synthesis
 ↓
Optimization
 ↓
Reports
```

Environment:

```text
Cadence Genus
```

---

## Day 9 — Timing & Report Automation

Topics:

* Clock constraints
* Input delay
* Output delay
* Timing paths
* Slack
* Setup timing
* Hold timing
* Timing reports
* Automated report generation

Environment:

```text
Genus / Tempus
```

---

## Day 10 — Complete ASIC Automation

Final objective:

```text
             RTL
              │
              ▼
           Genus
              │
              ▼
          Synthesis
              │
              ▼
          Netlist
              │
              ▼
          Innovus
              │
       ┌──────┼──────┐
       ▼      ▼      ▼
   Floorplan Place   CTS
       │      │      │
       └──────┼──────┘
              ▼
           Routing
              │
              ▼
            STA
              │
              ▼
           Reports
```

The final script should automate as much of this flow as practical.

---

# 🛠️ Tools Required

## Basic Tcl

```text
Tcl / tclsh
Linux terminal
Vim / VS Code
Git
```

## EDA

```text
Cadence Genus
Cadence Innovus
Cadence Tempus
```

The EDA portion requires access to the appropriate licensed Cadence installation and technology libraries.

---

# 📝 Practice Philosophy

For every command, I will answer:

```text
What is it?
      ↓
Why is it used?
      ↓
What is the syntax?
      ↓
Simple example
      ↓
How does Tcl execute it?
      ↓
Where is it used in VLSI?
      ↓
EDA example
```

---

# 🎯 Final 10-Day Project

At the end of Day 10, I will create a Tcl-based ASIC automation project capable of:

* Reading RTL
* Setting up directories
* Running synthesis
* Applying constraints
* Generating timing reports
* Generating area reports
* Saving logs
* Organizing results
* Performing basic design checks
* Automating EDA tool execution

Expected structure:

```text
RTL
 │
 ├── constraints
 │
 ▼
Tcl Automation
 │
 ▼
Genus
 │
 ├── timing report
 ├── area report
 ├── power report
 ├── netlist
 └── logs
```

---

# 📈 Expected Skills After 10 Days

After completing this project, I should be comfortable with:

```text
Tcl Basics
    ↓
Tcl Programming
    ↓
Linux Automation
    ↓
EDA Tcl
    ↓
Genus Automation
    ↓
Timing Automation
    ↓
ASIC Flow Automation
```

I should also be able to read and understand existing Tcl scripts used in VLSI projects and modify them for my own designs.

---

# 🚀 Starting Point

Start the project with:

```bash
mkdir -p ~/10_day_tcl
cd ~/10_day_tcl
```

Initialize Git:

```bash
git init
```

Create the README:

```bash
touch README.md
```

Then create the first day's directory:

```bash
mkdir day01
```

The first execution environment is:

```bash
tclsh
```

Verify Tcl:

```bash
tclsh
```

Then:

```tcl
puts "10-Day Tcl Journey Started"
```

Expected output:

```text
10-Day Tcl Journey Started
```

---

# 🔥 Final Objective

The ultimate goal is:

> **Learn Tcl from zero and become capable of writing practical Tcl automation scripts for VLSI/ASIC EDA flows.**

This project will progress from:

```text
BEGINNER
   ↓
Tcl Programming
   ↓
Linux Automation
   ↓
EDA Tcl
   ↓
Genus
   ↓
Innovus
   ↓
Tempus
   ↓
ASIC Flow Automation
```

**10 Days → Tcl + VLSI Automation**
