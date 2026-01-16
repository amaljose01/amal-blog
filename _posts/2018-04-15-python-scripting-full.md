---
layout: post
title: "Python Scripting Full Guide: From Basics to Advanced"
date: 2018-04-19 10:00:00 +0000
categories: [python, scripting, programming, tutorial]
author: Amal
image: /assets/images/posts/2018-04-15-python-scripting-full.svg
---



# Python Scripting Full Guide: From Basics to Advanced

## Introduction

Python is one of the most popular programming languages for scripting, automation, data science, and web development. This comprehensive guide covers everything you need to master Python scripting.

## Part 1: Python Fundamentals

### Installation and Setup

```bash
# Install Python
python3 --version

# Virtual environment
python3 -m venv myenv
source myenv/bin/activate  # Linux/macOS
myenv\Scripts\activate      # Windows
```

### Basic Data Types

```python
# Strings
name = "Alice"
message = 'Hello, World!'
multiline = """This is
a multiline
string"""

# Numbers
age = 25
salary = 50000.50
complex_num = 3 + 4j

# Booleans
is_active = True
is_admin = False

# Lists
fruits = ['apple', 'banana', 'cherry']
mixed = [1, 'two', 3.0, True]

# Tuples (immutable)
coordinates = (10, 20)
point = (1, 2, 3)

# Dictionaries
person = {
    'name': 'Alice',
    'age': 25,
    'email': 'alice@example.com'
}

# Sets (unique elements)
colors = {'red', 'green', 'blue'}
```

### String Operations

```python
text = "Hello, Python!"

# Length and indexing
length = len(text)
first_char = text[0]
last_char = text[-1]

# Slicing
substring = text[0:5]
reversed_text = text[::-1]

# Methods
uppercase = text.upper()
lowercase = text.lower()
replaced = text.replace('Python', 'World')
split_words = text.split(',')
joined = ' '.join(['Hello', 'World'])

# String formatting
name = "Alice"
age = 25
formatted = f"My name is {name} and I'm {age} years old"
formatted2 = "My name is {} and I'm {} years old".format(name, age)
```

## Part 2: Control Flow

### Conditional Statements

```python
age = 20

# if-elif-else
if age < 13:
    print("Child")
elif age < 18:
    print("Teenager")
else:
    print("Adult")

# Ternary operator
status = "Adult" if age >= 18 else "Minor"

# Membership test
if 'a' in "apple":
    print("Found")

# Comparison
if age >= 18 and age <= 65:
    print("Working age")
```

### Loops

```python
# for loop
for i in range(5):
    print(i)

# Loop with list
fruits = ['apple', 'banana', 'cherry']
for fruit in fruits:
    print(fruit)

# Enumerate
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# While loop
counter = 0
while counter < 5:
    print(counter)
    counter += 1

# Loop control
for i in range(10):
    if i == 3:
        continue  # Skip
    if i == 7:
        break     # Exit loop
    print(i)

# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(10) if x % 2 == 0]
```

## Part 3: Functions

### Function Basics

```python
# Simple function
def greet(name):
    return f"Hello, {name}!"

result = greet("Alice")

# Multiple parameters
def add(a, b):
    return a + b

sum_result = add(5, 3)

# Default parameters
def power(base, exponent=2):
    return base ** exponent

result1 = power(5)      # Uses default
result2 = power(5, 3)   # Override default

# Variable-length arguments
def sum_all(*args):
    return sum(args)

total = sum_all(1, 2, 3, 4, 5)

# Keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="NYC")

# Docstrings
def calculate_area(radius):
    """Calculate the area of a circle given radius."""
    return 3.14159 * radius ** 2
```

### Advanced Function Concepts

```python
# Decorators
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@my_decorator
def say_hello(name):
    print(f"Hello, {name}!")

# Lambda functions
square = lambda x: x ** 2
add = lambda x, y: x + y

numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))

# Closures
def multiplier(factor):
    def multiply(number):
        return number * factor
    return multiply

times_three = multiplier(3)
result = times_three(10)  # Returns 30
```

## Part 4: Object-Oriented Programming

### Classes and Objects

```python
class Person:
    # Class variable
    species = "Homo sapiens"
    
    # Constructor
    def __init__(self, name, age):
        # Instance variables
        self.name = name
        self.age = age
    
    # Instance method
    def introduce(self):
        return f"I am {self.name}, {self.age} years old"
    
    # Class method
    @classmethod
    def create_from_birth_year(cls, name, birth_year):
        age = 2024 - birth_year
        return cls(name, age)
    
    # Static method
    @staticmethod
    def is_adult(age):
        return age >= 18
    
    # String representation
    def __str__(self):
        return f"Person({self.name}, {self.age})"

# Create instance
person = Person("Alice", 25)
print(person.introduce())

# Class method usage
person2 = Person.create_from_birth_year("Bob", 1995)

# Static method usage
is_adult = Person.is_adult(20)
```

### Inheritance

```python
# Base class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return f"{self.name} makes a sound"

# Derived class
class Dog(Animal):
    def speak(self):
        return f"{self.name} barks"

class Cat(Animal):
    def speak(self):
        return f"{self.name} meows"

# Usage
dog = Dog("Buddy")
print(dog.speak())  # Buddy barks

# Multiple inheritance
class Pet:
    def play(self):
        return "Playing with pet"

class ServiceDog(Dog, Pet):
    pass

service_dog = ServiceDog("Max")
print(service_dog.speak())
print(service_dog.play())
```

### Polymorphism

```python
class Shape:
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2

shapes = [Rectangle(5, 10), Circle(5)]
for shape in shapes:
    print(f"Area: {shape.area()}")
```

## Part 5: File Handling

### Reading and Writing Files

```python
# Write to file
with open('output.txt', 'w') as file:
    file.write("Hello, World!\n")
    file.write("Python scripting\n")

# Read from file
with open('output.txt', 'r') as file:
    content = file.read()
    print(content)

# Read line by line
with open('output.txt', 'r') as file:
    for line in file:
        print(line.strip())

# Append to file
with open('output.txt', 'a') as file:
    file.write("Appended line\n")

# JSON handling
import json

data = {
    'name': 'Alice',
    'age': 25,
    'skills': ['Python', 'SQL', 'Linux']
}

# Write JSON
with open('data.json', 'w') as file:
    json.dump(data, file, indent=4)

# Read JSON
with open('data.json', 'r') as file:
    loaded_data = json.load(file)
```

## Part 6: Working with External Libraries

### Common Libraries

```python
# OS operations
import os
print(os.getcwd())
os.chdir('/path/to/directory')
files = os.listdir('.')

# System operations
import sys
print(sys.version)
print(sys.path)

# Time and date
import datetime
now = datetime.datetime.now()
today = datetime.date.today()

# Regular expressions
import re
pattern = r'\d+'
text = "The year is 2018"
matches = re.findall(pattern, text)

# Command execution
import subprocess
result = subprocess.run(['ls', '-la'], capture_output=True, text=True)
print(result.stdout)

# HTTP requests
import requests
response = requests.get('https://api.example.com/data')
json_data = response.json()
```

## Part 7: Error Handling

### Exception Handling

```python
# Try-except
try:
    number = int("abc")
except ValueError:
    print("Invalid input")

# Multiple exceptions
try:
    file = open('nonexistent.txt', 'r')
    data = file.read()
except FileNotFoundError:
    print("File not found")
except IOError:
    print("IO error")

# Else and finally
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Cannot divide by zero")
else:
    print(f"Result: {result}")
finally:
    print("Cleanup")

# Custom exceptions
class CustomError(Exception):
    pass

def validate_age(age):
    if age < 0:
        raise CustomError("Age cannot be negative")
    return age
```

## Part 8: Scripting Best Practices

### Script Template

```python
#!/usr/bin/env python3
"""
Script description.

Usage:
    python script.py [options]
"""

import sys
import argparse
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

def main():
    """Main function."""
    parser = argparse.ArgumentParser(description='Script description')
    parser.add_argument('--input', required=True, help='Input file')
    parser.add_argument('--output', help='Output file')
    
    args = parser.parse_args()
    
    logger.info(f"Processing {args.input}")
    # Your code here
    logger.info("Done")

if __name__ == '__main__':
    main()
```

## Summary

You've learned Python scripting from fundamentals to advanced concepts:
- **Basics**: Data types, operators, strings
- **Control Flow**: Conditionals, loops
- **Functions**: Definition, decorators, lambdas
- **OOP**: Classes, inheritance, polymorphism
- **File Handling**: Reading, writing, JSON
- **Best Practices**: Error handling, logging

Python is versatile and powerful. Practice these concepts to become a proficient Python scripter!
