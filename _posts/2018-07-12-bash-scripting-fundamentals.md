---
layout: post
title: "Bash Scripting Fundamentals: From Basics to Advanced Automation"
date: 2018-07-12 10:00:00 +0530
categories: [Bash, Scripting, Shell, DevOps]
author: Amal Jose
---

## Introduction to Bash Scripting

Bash scripting is fundamental to DevOps and Linux system administration. In this comprehensive guide, we'll explore everything from basic shell commands to advanced automation techniques.

## Basic Concepts

### Variables and Data Types

```bash
#!/bin/bash

# String variables
NAME="Amal"
echo "Hello, $NAME"

# Numeric variables
COUNT=10
RESULT=$((COUNT + 5))

# Arrays
FRUITS=("Apple" "Banana" "Orange")
echo ${FRUITS[0]}

# Associative arrays
declare -A person
person[name]="John"
person[age]="30"
```

## Control Structures

### If-Else Statements

```bash
#!/bin/bash

AGE=25

if [ $AGE -ge 18 ]; then
    echo "Adult"
elif [ $AGE -ge 13 ]; then
    echo "Teenager"
else
    echo "Child"
fi
```

### Loops

```bash
#!/bin/bash

# For loop
for i in {1..5}; do
    echo "Number: $i"
done

# While loop
COUNT=0
while [ $COUNT -lt 5 ]; do
    echo "Count: $COUNT"
    ((COUNT++))
done

# Until loop
COUNT=0
until [ $COUNT -eq 5 ]; do
    echo "Count: $COUNT"
    ((COUNT++))
done
```

## Functions

```bash
#!/bin/bash

# Simple function
greet() {
    echo "Hello, $1!"
}

# Function with return value
add() {
    local sum=$(($1 + $2))
    echo $sum
}

# Function with error handling
safe_divide() {
    if [ $2 -eq 0 ]; then
        echo "Error: Division by zero"
        return 1
    fi
    echo $(($1 / $2))
}

# Usage
greet "DevOps"
RESULT=$(add 10 20)
echo "Sum: $RESULT"
safe_divide 10 2
```

## String Manipulation

```bash
#!/bin/bash

STRING="Hello World"

# String length
echo ${#STRING}

# Substring
echo ${STRING:0:5}

# Replace
echo ${STRING//World/DevOps}

# Remove prefix/suffix
PREFIX="prefix_value"
echo ${PREFIX#prefix_}

SUFFIX="value_suffix"
echo ${SUFFIX%_suffix}
```

## Working with Files and Directories

```bash
#!/bin/bash

# File operations
if [ -f /etc/passwd ]; then
    echo "File exists"
fi

if [ -d /home ]; then
    echo "Directory exists"
fi

if [ -r /etc/passwd ]; then
    echo "File is readable"
fi

# Create directories
mkdir -p /tmp/my_app/data

# Copy files
cp source.txt destination.txt

# List files
ls -la /tmp/

# Find files
find /tmp -name "*.log" -type f
```

## Advanced Techniques

### Process Substitution

```bash
#!/bin/bash

# Process substitution
diff <(ls dir1) <(ls dir2)

# Reading from multiple files
while read line1 && read line2 <&3; do
    echo "File1: $line1, File2: $line2"
done < file1.txt 3< file2.txt
```

### Regular Expressions and Pattern Matching

```bash
#!/bin/bash

STRING="user@example.com"

# Check if matches pattern
if [[ $STRING =~ ^[a-z]+@[a-z]+\.[a-z]+$ ]]; then
    echo "Valid email"
fi

# Case statement with patterns
case $1 in
    *.log)
        echo "Log file"
        ;;
    *.txt)
        echo "Text file"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```

### Error Handling

```bash
#!/bin/bash

set -e  # Exit on error
set -u  # Exit on undefined variable
set -o pipefail  # Exit if any command in pipeline fails

# Trap errors
trap 'echo "Error on line $LINENO"' ERR

# Try-catch pattern
execute_command() {
    if ! command_that_might_fail; then
        echo "Command failed"
        return 1
    fi
}
```

## Practical Examples

### Log File Processing

```bash
#!/bin/bash

LOG_FILE="/var/log/app.log"
ARCHIVE_DIR="/var/log/archive"

# Process and archive logs
process_logs() {
    if [ -f "$LOG_FILE" ]; then
        # Count errors
        ERROR_COUNT=$(grep -c "ERROR" "$LOG_FILE")
        echo "Total errors: $ERROR_COUNT"
        
        # Archive old logs
        tar -czf "$ARCHIVE_DIR/app_$(date +%Y%m%d).log.tar.gz" "$LOG_FILE"
        > "$LOG_FILE"  # Clear the log
    fi
}

process_logs
```

### System Monitoring Script

```bash
#!/bin/bash

# Monitoring script
monitor_system() {
    CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}')
    MEMORY_USAGE=$(free | grep Mem | awk '{print ($3/$2) * 100.0}')
    DISK_USAGE=$(df / | awk 'NR==2 {print ($3/$2) * 100.0}')
    
    echo "CPU Usage: $CPU_USAGE%"
    echo "Memory Usage: $MEMORY_USAGE%"
    echo "Disk Usage: $DISK_USAGE%"
    
    # Alert if thresholds exceeded
    if (( $(echo "$CPU_USAGE > 80" | bc -l) )); then
        echo "WARNING: High CPU usage"
    fi
}

monitor_system
```

### Deployment Script

```bash
#!/bin/bash

DEPLOY_DIR="/opt/app"
BACKUP_DIR="/opt/backups"

deploy_application() {
    local VERSION=$1
    
    if [ -z "$VERSION" ]; then
        echo "Usage: $0 <version>"
        return 1
    fi
    
    # Create backup
    mkdir -p "$BACKUP_DIR"
    tar -czf "$BACKUP_DIR/app_$(date +%Y%m%d_%H%M%S).tar.gz" "$DEPLOY_DIR"
    
    # Deploy new version
    cd "$DEPLOY_DIR"
    git pull origin master
    git checkout "v$VERSION"
    
    # Run deployment tasks
    ./build.sh
    ./migrate.sh
    systemctl restart app
    
    echo "Deployment of version $VERSION completed successfully"
}

deploy_application $1
```

## Best Practices

1. **Use functions** to organize code
2. **Add error handling** with set -e, trap, and error checks
3. **Use local variables** in functions to avoid conflicts
4. **Quote variables** to prevent word splitting
5. **Use [[ ]] instead of [ ]** for safer comparisons
6. **Add comments** to explain complex logic
7. **Use meaningful variable names**
8. **Test edge cases** (empty strings, special characters)
9. **Use shellcheck** to lint your scripts
10. **Document your scripts** with usage examples

## Common Pitfalls to Avoid

- Not quoting variables: `$VAR` should be `"$VAR"`
- Using = instead of == for comparison
- Forgetting to escape special characters
- Not handling errors properly
- Using global variables excessively
- Not testing with different input

## Conclusion

Bash scripting is a powerful skill for DevOps engineers. Master these fundamentals and you'll be able to automate complex infrastructure tasks efficiently.

---
**Tags:** #Bash #Scripting #Shell #Linux #DevOps #Automation
