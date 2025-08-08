# DEPI-Final-Python-Project
DEPI final project for Python basics, focusing on OOP and CSV handling with Python 
Supervised by: Eng/ Baraa Abu Sallout.

This is a command-line Python application that allows users to manage employee records. It supports adding, updating, deleting, searching, and listing employee data — all stored persistently in a CSV file.

## Project Description

This project is designed to manage employee information through a basic CLI (Command Line Interface).
It demonstrates core Python concepts such as:

- Object-Oriented Programming (OOP)
- Dictionaries and Lists
- File Handling using the `csv` module
- Functions and Conditional Logic
- Error Handling and Data Validation

## Features

- Add new employee with ID, name, position, salary, and email.
- View all employee records.
- Update specific fields of an employee by ID.
- Delete employee by ID.
- Search employee by ID.
- All data is stored in and loaded from `Employees.csv`.
  
  Each employee has the following fields:

- `ID`
- `Name`
- `Position`
- `Salary`
- `Email`

## How It Works

1. When the program starts, it loads employee data from `Employees.csv`  into a dictionary in memory. (or creates a new file if it doesn't exist)
2. Each employee is stored as a dictionary entry, using the employee ID as the key.
3. A menu is displayed for the user to interact with the system.
4. Any changes (add/update/delete/search) interact with the in-memory dictionary.
5. Any changes are immediately written back to the CSV file to ensure data persistence.
6. Data is persistent across program runs.

## Validation & Error Handling

- Prevents duplicate employee IDs
- Validates that the salary is a numeric value
- Validates that the email is in proper format
- Checks if records exist before listing, updating, or deleting
- Displays formatted output for better readability
  
---
