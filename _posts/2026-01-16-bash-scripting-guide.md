---
layout: post
title: "Bash Scripting Fundamentals: A Complete Interactive Guide"
date: 2018-07-12 12:00:00 +0000
categories: [bash, scripting, tutorial, shell]
author: Amal
---

# Bash Scripting Fundamentals: An Interactive Tutorial

Welcome to this comprehensive guide on bash scripting! This post covers essential concepts that every developer should know. Each section includes practical examples you can run immediately in your terminal.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started with Shebang](#getting-started)
3. [Variables and Data Types](#variables)
4. [Conditional Statements](#conditionals)
5. [Loops](#loops)
6. [Functions](#functions)
7. [Working with Files](#files)
8. [Log File Processing & Error Detection](#log-processing)
9. [Best Practices](#best-practices)

---

## Introduction {#introduction}

Bash (Bourne Again Shell) is the default shell on most Linux systems and macOS. It's a powerful scripting language that allows you to automate tasks, manage system administration, and streamline your development workflow.

**Why learn Bash?**
- Automate repetitive tasks
- Manage servers and infrastructure
- Process large amounts of data
- Enhance your DevOps skills
- Improve productivity

---

## Getting Started with Shebang {#getting-started}

### What is a Shebang?

The shebang (`#!/bin/bash`) is the first line of a bash script. It tells the system to execute this file with the bash interpreter.

**Try this:** Create your first script

```bash
#!/bin/bash

echo "Hello, World!"
```

Save this as `hello.sh` and run:

```bash
chmod +x hello.sh
./hello.sh
```

**Expected Output:**
```
Hello, World!
```

### Making Scripts Executable

The `chmod +x` command makes the script executable. Here's why:
- Files have three permission types: read, write, execute
- `x` means execute permission
- Without it, you can't run the script directly

---

## Variables and Data Types {#variables}

Bash variables store data values. Unlike many programming languages, bash doesn't have strict data types - everything is treated as text.

### Declaring and Using Variables

```bash
#!/bin/bash

# Variable assignment (no spaces around =)
name="Amal"
age=25
echo "My name is $name and I am $age years old"

# Command substitution
current_date=$(date)
echo "Current date: $current_date"

# Using backticks (older syntax, still works)
user=$(whoami)
echo "Current user: $user"
```

**Try it:**
```bash
name="YourName"
echo "Hello, $name!"
```

### Naming Conventions

- Use UPPERCASE for constants: `CONSTANT_VALUE`
- Use lowercase for regular variables: `variable_name`
- Variable names are case-sensitive
- Can contain letters, numbers, and underscores

### Special Variables

| Variable | Meaning |
|----------|---------|
| `$0` | Script name |
| `$1, $2, ...` | Command-line arguments |
| `$#` | Number of arguments |
| `$@` | All arguments as array |
| `$?` | Exit status of last command |
| `$$` | Process ID of current script |

**Example with Arguments:**

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

Run it:
```bash
./script.sh apple banana cherry
```

---

## Conditional Statements {#conditionals}

Conditionals allow you to make decisions in your scripts based on certain conditions.

### If-Else Statements

```bash
#!/bin/bash

age=20

if [ $age -ge 18 ]; then
    echo "You are an adult"
else
    echo "You are a minor"
fi
```

**Common Comparison Operators:**

| Operator | Meaning |
|----------|---------|
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-lt` | Less than |
| `-le` | Less than or equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |

### String Comparisons

```bash
#!/bin/bash

password="secret123"

if [ "$password" = "secret123" ]; then
    echo "Access granted"
else
    echo "Access denied"
fi
```

### File Testing

```bash
#!/bin/bash

file="/path/to/file"

if [ -f "$file" ]; then
    echo "File exists"
fi

if [ -d "$file" ]; then
    echo "Directory exists"
fi

if [ -r "$file" ]; then
    echo "File is readable"
fi

if [ -w "$file" ]; then
    echo "File is writable"
fi

if [ -x "$file" ]; then
    echo "File is executable"
fi
```

**Common File Test Operators:**

| Operator | Meaning |
|----------|---------|
| `-f` | File exists and is regular file |
| `-d` | Directory exists |
| `-r` | File is readable |
| `-w` | File is writable |
| `-x` | File is executable |
| `-s` | File exists and is not empty |
| `-z` | String is empty |
| `-n` | String is not empty |

### Multiple Conditions

```bash
#!/bin/bash

age=25
is_student=true

if [ $age -ge 18 ] && [ "$is_student" = "true" ]; then
    echo "Eligible for student adult discount"
fi

if [ $age -lt 13 ] || [ $age -gt 60 ]; then
    echo "Eligible for special pricing"
fi
```

### Case Statements

```bash
#!/bin/bash

fruit="apple"

case $fruit in
    apple)
        echo "You chose an apple"
        ;;
    banana)
        echo "You chose a banana"
        ;;
    orange)
        echo "You chose an orange"
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```

---

## Loops {#loops}

Loops execute a block of code multiple times.

### For Loop

```bash
#!/bin/bash

# Loop through numbers
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Using range
for i in {1..5}; do
    echo "Number: $i"
done

# Using C-style syntax
for ((i=1; i<=5; i++)); do
    echo "Number: $i"
done

# Loop through files
for file in *.txt; do
    echo "Processing $file"
done

# Loop through array
fruits=("apple" "banana" "cherry")
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done
```

### While Loop

```bash
#!/bin/bash

counter=1

while [ $counter -le 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done
```

### Until Loop

```bash
#!/bin/bash

counter=1

until [ $counter -gt 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done
```

### Loop Control

```bash
#!/bin/bash

# Break - exit loop
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo "Number: $i"
done

# Continue - skip to next iteration
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo "Number: $i"
done
```

---

## Functions {#functions}

Functions are reusable blocks of code.

### Defining and Calling Functions

```bash
#!/bin/bash

# Define function
greet() {
    echo "Hello, $1!"
}

# Call function
greet "Amal"
greet "Alice"
```

### Functions with Return Values

```bash
#!/bin/bash

add() {
    local sum=$(($1 + $2))
    echo $sum
}

result=$(add 5 3)
echo "5 + 3 = $result"
```

### Returning Exit Status

```bash
#!/bin/bash

is_number() {
    if [[ $1 =~ ^[0-9]+$ ]]; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

if is_number 42; then
    echo "It's a number"
else
    echo "Not a number"
fi
```

### Local Variables

```bash
#!/bin/bash

global_var="I'm global"

my_function() {
    local local_var="I'm local"
    echo "Local: $local_var"
    echo "Global: $global_var"
}

my_function
echo "$global_var"  # Works
echo "$local_var"   # Empty - out of scope
```

---

## Working with Files {#files}

File manipulation is a core bash skill.

### Reading Files

```bash
#!/bin/bash

# Method 1: While loop
while IFS= read -r line; do
    echo "Line: $line"
done < "filename.txt"

# Method 2: Cat with pipe
cat "filename.txt" | while read -r line; do
    echo "Line: $line"
done

# Method 3: Using mapfile
mapfile -t lines < "filename.txt"
for line in "${lines[@]}"; do
    echo "Line: $line"
done
```

### Writing to Files

```bash
#!/bin/bash

# Overwrite file
echo "First line" > output.txt

# Append to file
echo "Second line" >> output.txt

# Multiple lines
cat > output.txt << 'EOF'
This is line 1
This is line 2
This is line 3
EOF
```

### File Operations

```bash
#!/bin/bash

# Check if file exists
if [ -f "file.txt" ]; then
    echo "File exists"
fi

# Create directory
mkdir -p my_directory

# Copy file
cp source.txt destination.txt

# Move/rename file
mv old_name.txt new_name.txt

# Remove file
rm file.txt

# Get file size
size=$(stat -f%z "file.txt")  # macOS
# size=$(stat -c%s "file.txt")  # Linux
echo "File size: $size bytes"
```

### Working with Directories

```bash
#!/bin/bash

# List files
ls -la

# List only files
ls -lF | grep -v /

# Count files
count=$(ls -1 | wc -l)
echo "Number of files: $count"

# Get current directory
current=$(pwd)
echo "Current directory: $current"

# Change directory
cd /path/to/directory

# Go back to previous directory
cd -
```

### Text Processing

```bash
#!/bin/bash

# Search for text
grep "pattern" filename.txt

# Case-insensitive search
grep -i "pattern" filename.txt

# Replace text
sed 's/old/new/' filename.txt

# Replace all occurrences
sed 's/old/new/g' filename.txt

# Delete lines
sed '/pattern/d' filename.txt

# Show specific lines
sed -n '5,10p' filename.txt
```

---

## Log File Processing & Error Detection {#log-processing}

One of the most practical uses of bash scripting is processing log files to find errors, warnings, and anomalies. This is essential for system administration, debugging, and monitoring.

### Scenario: Processing Multiple Log Files

Let's create a real-world scenario where you need to:
- Read two log files (e.g., application log and system log)
- Extract and flag errors
- Generate a summary report

### Sample Log Files

First, let's create sample log files to work with:

```bash
#!/bin/bash

# Create sample log files for demonstration
mkdir -p logs

# Create application.log
cat > logs/application.log << 'EOF'
[2026-01-16 08:00:01] INFO: Application started
[2026-01-16 08:00:15] DEBUG: Database connection established
[2026-01-16 08:05:32] WARNING: High memory usage detected (85%)
[2026-01-16 08:10:45] ERROR: Failed to connect to API endpoint
[2026-01-16 08:11:20] INFO: Retrying connection...
[2026-01-16 08:11:50] ERROR: Connection timeout after 3 retries
[2026-01-16 08:12:15] INFO: Switching to fallback mode
[2026-01-16 08:20:00] WARNING: Cache expiration detected
[2026-01-16 08:25:30] ERROR: Invalid configuration parameter
[2026-01-16 08:30:00] INFO: Cache refreshed successfully
EOF

# Create system.log
cat > logs/system.log << 'EOF'
[2026-01-16 08:00:00] INFO: System boot completed
[2026-01-16 08:05:00] WARNING: Disk space low (10% remaining)
[2026-01-16 08:10:00] ERROR: Network interface eth0 down
[2026-01-16 08:10:30] INFO: Network interface restarted
[2026-01-16 08:15:00] DEBUG: Kernel module loaded
[2026-01-16 08:20:00] ERROR: Temperature sensor malfunction
[2026-01-16 08:20:30] WARNING: Attempting sensor recalibration
[2026-01-16 08:25:00] INFO: Sensor recalibration successful
[2026-01-16 08:30:00] WARNING: Memory fragmentation at 45%
EOF
```

### Basic Log Parsing Script

Here's a simple script to extract and count errors from a single log file:

```bash
#!/bin/bash

log_file="$1"

# Check if file exists
if [ ! -f "$log_file" ]; then
    echo "Error: Log file not found: $log_file"
    exit 1
fi

echo "========== ERROR ANALYSIS =========="
echo "Log file: $log_file"
echo ""

# Count errors by severity
error_count=$(grep -i "ERROR" "$log_file" | wc -l)
warning_count=$(grep -i "WARNING" "$log_file" | wc -l)
info_count=$(grep -i "INFO" "$log_file" | wc -l)

echo "Summary:"
echo "  INFO: $info_count"
echo "  WARNING: $warning_count"
echo "  ERROR: $error_count"
echo ""

# Display all errors
echo "All Errors:"
grep -i "ERROR" "$log_file"
```

**Usage:**
```bash
chmod +x parse_log.sh
./parse_log.sh logs/application.log
```

### Advanced: Processing Multiple Log Files

This script processes two log files and flags errors with detailed analysis:

```bash
#!/bin/bash

# Configuration
LOG_DIR="logs"
APP_LOG="$LOG_DIR/application.log"
SYS_LOG="$LOG_DIR/system.log"
REPORT_FILE="error_report.txt"

# Color codes for terminal output
RED='\033[0;31m'
YELLOW='\033[1;33m'
GREEN='\033[0;32m'
NC='\033[0m' # No Color

# Counters
total_errors=0
total_warnings=0

# Function to analyze a log file
analyze_log() {
    local log_file=$1
    local log_name=$2
    
    echo "========================================" | tee -a "$REPORT_FILE"
    echo "Analyzing: $log_name" | tee -a "$REPORT_FILE"
    echo "File: $log_file" | tee -a "$REPORT_FILE"
    echo "========================================" | tee -a "$REPORT_FILE"
    
    # Check if file exists
    if [ ! -f "$log_file" ]; then
        echo -e "${RED}[ERROR] File not found: $log_file${NC}" | tee -a "$REPORT_FILE"
        return 1
    fi
    
    # Extract errors
    errors=$(grep -i "ERROR" "$log_file")
    error_count=$(echo "$errors" | grep -c "ERROR")
    
    # Extract warnings
    warnings=$(grep -i "WARNING" "$log_file")
    warning_count=$(echo "$warnings" | grep -c "WARNING")
    
    # Update totals
    ((total_errors += error_count))
    ((total_warnings += warning_count))
    
    # Display summary
    echo "" | tee -a "$REPORT_FILE"
    echo "Summary:" | tee -a "$REPORT_FILE"
    echo "  Errors: $error_count" | tee -a "$REPORT_FILE"
    echo "  Warnings: $warning_count" | tee -a "$REPORT_FILE"
    echo "" | tee -a "$REPORT_FILE"
    
    # Display all errors
    if [ $error_count -gt 0 ]; then
        echo -e "${RED}ERRORS FOUND:${NC}" | tee -a "$REPORT_FILE"
        echo "$errors" | tee -a "$REPORT_FILE"
        echo "" | tee -a "$REPORT_FILE"
    fi
    
    # Display all warnings
    if [ $warning_count -gt 0 ]; then
        echo -e "${YELLOW}WARNINGS FOUND:${NC}" | tee -a "$REPORT_FILE"
        echo "$warnings" | tee -a "$REPORT_FILE"
        echo "" | tee -a "$REPORT_FILE"
    fi
}

# Main script
echo "Starting log analysis..."
echo "" > "$REPORT_FILE"

# Analyze each log file
analyze_log "$APP_LOG" "Application Log"
analyze_log "$SYS_LOG" "System Log"

# Final summary
echo "========================================" | tee -a "$REPORT_FILE"
echo "FINAL SUMMARY" | tee -a "$REPORT_FILE"
echo "========================================" | tee -a "$REPORT_FILE"
echo "" | tee -a "$REPORT_FILE"
echo "Total Errors: $total_errors" | tee -a "$REPORT_FILE"
echo "Total Warnings: $total_warnings" | tee -a "$REPORT_FILE"
echo "" | tee -a "$REPORT_FILE"

if [ $total_errors -gt 0 ]; then
    echo -e "${RED}⚠️  CRITICAL: $total_errors error(s) found!${NC}" | tee -a "$REPORT_FILE"
    exit_code=1
else
    echo -e "${GREEN}✅ No errors found!${NC}" | tee -a "$REPORT_FILE"
    exit_code=0
fi

echo "Report saved to: $REPORT_FILE"
exit $exit_code
```

**Run it:**
```bash
chmod +x log_analyzer.sh
./log_analyzer.sh
```

### Filtering Errors by Time Range

Extract errors that occurred within a specific time window:

```bash
#!/bin/bash

log_file="$1"
start_time="$2"  # Format: HH:MM:SS
end_time="$3"    # Format: HH:MM:SS

if [ -z "$log_file" ] || [ -z "$start_time" ] || [ -z "$end_time" ]; then
    echo "Usage: $0 <log_file> <start_time> <end_time>"
    echo "Example: $0 application.log 08:00:00 08:30:00"
    exit 1
fi

echo "Errors between $start_time and $end_time:"
echo "========================================="

grep -E "\[.*[0-9]{2}:[0-9]{2}:[0-9]{2}\].*ERROR" "$log_file" | \
    awk -v start="$start_time" -v end="$end_time" \
    '$0 ~ start, $0 ~ end { if ($0 ~ /ERROR/) print }'
```

### Pattern-Based Error Detection

Extract specific error patterns (e.g., connection errors, timeout errors):

```bash
#!/bin/bash

log_file="$1"

echo "========== ERROR PATTERN ANALYSIS =========="
echo "File: $log_file"
echo ""

# Define error patterns
declare -A error_patterns=(
    ["Connection Errors"]="connection|timeout|refused|unreachable"
    ["Configuration Errors"]="invalid|config|parameter"
    ["Resource Errors"]="memory|disk|cpu|low"
    ["Authentication Errors"]="unauthorized|forbidden|denied"
)

# Analyze each pattern
for pattern_name in "${!error_patterns[@]}"; do
    pattern="${error_patterns[$pattern_name]}"
    count=$(grep -iE "$pattern" "$log_file" | grep -i "ERROR" | wc -l)
    
    if [ $count -gt 0 ]; then
        echo "[$pattern_name] - Count: $count"
        grep -iE "$pattern" "$log_file" | grep -i "ERROR" | sed 's/^/  /'
        echo ""
    fi
done
```

### Real-Time Error Monitoring

Monitor errors as they're written to a log file:

```bash
#!/bin/bash

log_file="$1"

if [ ! -f "$log_file" ]; then
    echo "Error: File not found: $log_file"
    exit 1
fi

echo "Monitoring errors in: $log_file"
echo "Press Ctrl+C to stop"
echo ""

# Use tail -f to follow the file and grep for errors
tail -f "$log_file" | grep --line-buffered "ERROR\|WARNING" | while read line; do
    if [[ $line == *"ERROR"* ]]; then
        echo -e "\033[0;31m[ERROR] $line\033[0m"
    else
        echo -e "\033[1;33m[WARNING] $line\033[0m"
    fi
done
```

**Usage:**
```bash
chmod +x monitor_logs.sh
./monitor_logs.sh logs/application.log
```

### Complete Example: Generate HTML Report

Convert log analysis to an HTML report:

```bash
#!/bin/bash

log_file="$1"
html_file="error_report.html"

if [ ! -f "$log_file" ]; then
    echo "Error: File not found: $log_file"
    exit 1
fi

# Get statistics
error_count=$(grep -c -i "ERROR" "$log_file")
warning_count=$(grep -c -i "WARNING" "$log_file")
info_count=$(grep -c -i "INFO" "$log_file")

# Create HTML file
cat > "$html_file" << EOF
<!DOCTYPE html>
<html>
<head>
    <title>Log Analysis Report</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .summary { background: #f0f0f0; padding: 10px; border-radius: 5px; }
        .error { color: red; }
        .warning { color: orange; }
        .info { color: green; }
        table { border-collapse: collapse; width: 100%; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
    </style>
</head>
<body>
    <h1>Log Analysis Report</h1>
    <p>Log File: $log_file</p>
    <p>Generated: $(date)</p>
    
    <div class="summary">
        <h2>Summary</h2>
        <p class="error">Errors: $error_count</p>
        <p class="warning">Warnings: $warning_count</p>
        <p class="info">Info: $info_count</p>
    </div>
    
    <h2>Error Details</h2>
    <table>
        <tr>
            <th>Timestamp</th>
            <th>Level</th>
            <th>Message</th>
        </tr>
EOF

# Add error rows to HTML
grep -i "ERROR" "$log_file" | while read line; do
    timestamp=$(echo "$line" | grep -oE "\[.*\]" | head -1)
    message=$(echo "$line" | sed 's/.*\] //')
    echo "<tr><td>$timestamp</td><td><span class='error'>ERROR</span></td><td>$message</td></tr>" >> "$html_file"
done

# Add warning rows to HTML
grep -i "WARNING" "$log_file" | while read line; do
    timestamp=$(echo "$line" | grep -oE "\[.*\]" | head -1)
    message=$(echo "$line" | sed 's/.*\] //')
    echo "<tr><td>$timestamp</td><td><span class='warning'>WARNING</span></td><td>$message</td></tr>" >> "$html_file"
done

# Close HTML
cat >> "$html_file" << 'EOF'
    </table>
</body>
</html>
EOF

echo "HTML report generated: $html_file"
```

---

## Best Practices {#best-practices}

### 1. Use Meaningful Names

```bash
# Bad
x=10
for i in $list; do
    echo $i
done

# Good
count=10
for filename in $file_list; do
    echo "$filename"
done
```

### 2. Quote Variables

```bash
# Bad
echo $name  # Fails if $name contains spaces

# Good
echo "$name"  # Always works
```

### 3. Use Local Variables in Functions

```bash
# Bad
my_function() {
    temp_var="something"  # Global scope
}

# Good
my_function() {
    local temp_var="something"  # Function scope
}
```

### 4. Add Comments

```bash
#!/bin/bash

# This function calculates the sum of two numbers
add() {
    local sum=$(($1 + $2))
    echo $sum
}

# Call the function with arguments 5 and 3
result=$(add 5 3)
```

### 5. Use Shellcheck

Shellcheck is a static analysis tool for bash scripts:

```bash
# Install
brew install shellcheck  # macOS
apt install shellcheck   # Linux

# Check your script
shellcheck script.sh
```

### 6. Error Handling

```bash
#!/bin/bash

# Exit on any error
set -e

# Exit on undefined variable
set -u

# Pipe fails if any command fails
set -o pipefail

# Your script here
```

### 7. Use Functions for Reusability

```bash
#!/bin/bash

# Define common functions
log_info() {
    echo "[INFO] $1"
}

log_error() {
    echo "[ERROR] $1" >&2
}

# Use them
log_info "Script started"
log_error "Something went wrong"
```

---

## Practice Exercises

Here are some exercises to test your learning:

### Exercise 1: Interactive Menu
Create a script that displays a menu and handles user input.

```bash
#!/bin/bash

while true; do
    echo "========== Menu =========="
    echo "1. Add"
    echo "2. Subtract"
    echo "3. Exit"
    read -p "Choose an option: " choice
    
    case $choice in
        1)
            echo "Selected: Add"
            ;;
        2)
            echo "Selected: Subtract"
            ;;
        3)
            echo "Exiting..."
            break
            ;;
        *)
            echo "Invalid option"
            ;;
    esac
done
```

### Exercise 2: File Backup Script
Create a script that backs up files from a directory.

```bash
#!/bin/bash

source_dir="/path/to/source"
backup_dir="/path/to/backup"
timestamp=$(date +%Y%m%d_%H%M%S)

mkdir -p "$backup_dir"
cp -r "$source_dir" "$backup_dir/backup_$timestamp"
echo "Backup created: $backup_dir/backup_$timestamp"
```

### Exercise 3: System Information Script
Create a script that displays system information.

```bash
#!/bin/bash

echo "========== System Information =========="
echo "Hostname: $(hostname)"
echo "OS: $(uname -s)"
echo "Kernel: $(uname -r)"
echo "Memory: $(free -h | grep Mem | awk '{print $2}')"
echo "Disk Usage: $(df -h / | tail -1 | awk '{print $5}')"
```

---

## Summary

You've now learned the fundamentals of bash scripting:

- **Variables & Types**: How to store and use data
- **Conditionals**: Making decisions in code
- **Loops**: Repeating code blocks
- **Functions**: Writing reusable code
- **File Operations**: Reading and writing data
- **Best Practices**: Writing clean, maintainable scripts

**Next Steps:**
- Practice each concept with real-world examples
- Use Shellcheck to validate your scripts
- Build a portfolio of useful scripts
- Learn advanced topics like process management and networking

---

## Resources

- [Official Bash Manual](https://www.gnu.org/software/bash/manual/)
- [Shellcheck](https://www.shellcheck.net/)
- [Bash Academy](https://www.bash.academy/)

---

## Interactive Exercise

Try running these commands in your terminal right now:

```bash
# Create a test script
cat > test.sh << 'EOF'
#!/bin/bash
echo "My interactive bash script!"
echo "Enter your name: "
read name
echo "Hello, $name!"
EOF

# Make it executable
chmod +x test.sh

# Run it
./test.sh
```



