# Documentation for DEPI Final Python CSV Project

## Overview

This project is a Python-based Employee Management System implemented as a Jupyter Notebook (`Employee_Manager.ipynb`). It provides a command-line interface (CLI) to manage employee records stored in a CSV file (`Employees.csv`). The system supports operations like adding, updating, deleting, searching, and displaying employee records. The code uses a dictionary-based data structure for in-memory storage and includes input validation for IDs, salaries, and email addresses.

## Table of Contents
1. Dependencies
2. Helper Functions
3. EmployeeManager Class
4. Main CLI Program
5. File Structure
6. Error Handeling

--- 

## Dependencies

The project relies on the following Python standard libraries:
- `csv`: For reading from and writing to the `Employees.csv` file.
- `os`: To check for the existence of the CSV file.
- `re:` For email validation using regular expressions.
No external libraries are required.

---

## Helper Functions

These functions validate user inputs to ensure data integrity:
- `idInput(s)`:
  - __Purpose__: Prompts the user for an ID and ensures it is a positive integer.
  - __Input:__ A string prompt (`s`) to display to the user.
  - __Output__: A valid integer ID as a string.
  - __Validation__: Checks if the input is numeric and greater than 0. If invalid, it prompts again with an error message: "Please enter a valid integer".
- `salaryInput(s)`:
  - __Purpose__: Prompts the user for a salary and ensures it is a positive numeric value.
  - __Input__: A string prompt (`s`) to display to the user.
  - __Output__: A valid salary as a string.
  - __Validation__: Checks if the input is numeric and greater than 0. If invalid, it prompts again with an error message: "Please enter a valid numeric value".

- `emailInput(s)`:
  - __Purpose__: Prompts the user for an email and ensures it matches a valid email pattern.
  - __Input__: A string prompt (`s`) to display to the user.
  - __Output__: A valid email address as a string.
  - __Validation__: Uses the regular expression `r"^[\\w\\.-]+@\\w+\\.\\w+$"` to validate the email format. If invalid, it prompts again with an error message: "Please enter a valid email".
  - __Note__: The commented-out alternative pattern `r"^[a-zA-Z0-9_\\.-]+@[a-zA-Z0-9_]+\\.[a-zA-Z0-9_]$"` is equivalent but more restrictive (not used in the final implementation). 

---

## EmployeeManager Class

The `EmployeeManager` class is the core of the system, managing employee data in memory and persisting it to a CSV file.

### Attributes
- `__data`:
  - __Type__ : Dictionary
  - __Description__: Stores employee records with the employee ID as the key and a dictionary of employee details (`Id`, `Name`, `Position`, `Salary`, `Email`) as the value.
  - __Initial Value__: Empty dictionary (`{}`).
  - __Note__: Using a dictionary instead of a list (as in the commented-out code) optimizes lookup operations by ID.
- `__fields`:
  - __Type__: List
  - __Description__: Defines the CSV file header and structure: ['`Id`', '`Name`', '`Position`', '`Salary`', '`Email`']
  - __Initial Value__: Fixed list as shown above.

### Methods
- `__init__()`:
  - __Purpose__: Initializes the `EmployeeManager` instance.
  - __Behavior__:
    - Checks if `Employees.csv` exists `using os.path.exists()`.
    - If the file exists, reads it using `csv.DictReader` and populates `__data` with employee records, using the `Id` as the key.
    - If the file does not exist, calls `__createFile()` to create an empty CSV file with headers.
  - __Parameters__: None.

- `__createFile()`:
  - __Purpose__: Creates or overwrites the `Employees.csv` file and writes the current `__data` to it.
  - __Behavior__:
    - Opens `Employees.csv` in write mode (`'w'`) with `newline=''` to ensure consistent CSV formatting.
    - Uses `csv.DictWriter` to write the header (from `__fields`) and all employee records (from `__data.values()`).
   - __Parameters__: None.

- `addEmployee(id, name, position, salary, email)`:
  - __Purpose__: Adds a new employee to `__data` and updates the CSV file.
  - __Parameters__:
    - `id` (str): Employee ID.
    - `name` (str): Employee name.
    - `position` (str): Employee position.
    - `salary` (str): Employee salary.
    - `email` (str): Employee email.
  - __Behavior__:
    - Checks if the `id` already exists in `__data`. If it does, prints "id {id} already exists" and returns.
    - Creates a new dictionary for the employee with the provided details.
    - Adds the employee to `__data` with the `id` as the key.
    - Prints a success message: "Employee {name} with ID {id} was added successfully".
    - Calls `__createFile()` to update the CSV file.
  - __Note__: An alternative implementation (commented out) directly appends to the CSV file but is less efficient as it doesn't use the in-memory `__data`.

- `updateEmployee(id)`:
  - __Purpose__: Updates the details of an existing employee.
  - __Parameters__:
    - `id`(str): Employee ID to update.
  - __Behavior__:
    - Checks if the `id` exists in `__data`. If not, catches the exception and prints "Id {id} was not found".
    - If the `id` exists, displays the employee's name and ID, then enters a loop to prompt for updates:
      - Options: `1` (Name), `2` (Position), `3` (Salary), `4` (Email), `0` (Done).
      - For options `1` and `2`, accepts any string input.
      - For option `3`, uses `salaryInput()` for validation.
      - For option `4`, uses `emailInput()` for validation.
      - If an invalid choice is entered, prints "Please enter a valid input".
      - After each update, prints a separator and asks if the user wants to edit more.
    - Calls `__createFile()` to save changes to the CSV file.

- `deleteEmployee(id)`:
  - __Purpose__: Deletes an employee from `__data` and updates the CSV file.
  - __Parameters__:
    - `id` (str): Employee ID to delete.
  - __Behavior__:
    - Checks if the `id` exists in `__data.` If not, catches the exception and checks if the `id` is numeric:
      - If numeric, prints "Id {id} was not found".
      - If non-numeric, prints "Please enter a valid integer".
    - If the `id` exists, prints a success message: "Employee {name} with id {id} was deleted successfully".
    - Removes the employee from `__data` using `pop(id)`.
    - Calls `__createFile()` to update the CSV file.

- `searchEmployee(id)`:
  - __Purpose__: Searches for and displays an employee's details by ID.
  - __Parameters__:
    - `id` (str): Employee ID to search for.
  - __Behavior__:
    - Checks if the `id` exists in `__data`. If not, catches the exception and checks if the id is numeric:
      - If numeric, prints "Id {id} was not found".
      - If non-numeric, prints "Please enter a valid integer".
    - If the `i`d exists, prints "Employee was found" and displays the employee's details (`Id`, `Name`, `Position`, `Salary`, `Email`).

- `dataGetter()` and `dataSetter(value)`:
  - __Purpose__: Getter and setter for the `__data` attribute.
  - __Note__: These methods are not used in the program but are included for potential future use.

- `fieldsGetter()` and `fieldsSetter(value)`:
  - Purpose: Getter and setter for the `__fields` attribute.
  - __Note__: These methods are not used in the program but are included for potential future use.

---

## Main CLI Program
The main program provides a command-line interface for interacting with the `EmployeeManager` class.

### Structure
- Initializes an `EmployeeManager` instance (`system = EmployeeManager()`).
- Displays a welcome message: "Welcome to Employee Manager".
- Enters a loop that displays a menu with the following options:
  - `1`: Show all employees.
  - `2`: Add employee.
  - `3`: Update an employee.
  - `4`: Delete an employee.
  - `5`: Search for an employee.
  - `0`: Exit.
- Prompts the user for a choice and processes it:
  - __Option 1__: Calls `system.showEmployees()`.
  - __Option 2__: Prompts for employee details using `idInput()`, `input()`, `salaryInput()`, and `emailInput()`, then calls `system.addEmployee()`.
  - __Option 3__: Prompts for an ID using `idInput()`, then calls `system.updateEmployee()`.
  - __Option 4__: Prompts for an ID using `idInput()`, then calls `system.deleteEmployee()`.
  - __Option 5__: Prompts for an ID using `idInput()`, then calls `system.searchEmployee()`.
  - __Option 0__: Breaks the loop to exit.
  - __Invalid Input__: Prints "please enter a vaild input" and continues the loop.
- After each operation (except exit), prints a separator ("======================================================================================") and asks "Do you want to do anything else".
- Upon exit, prints "Thank you, Good bye".

### Example Workflow
```
Welcome to Employee Manager
1: Show all employees
2: Add employee
3: Update an employee
4: Delete an employee
5: Search for an employee
0: Exit

Please choose what you want to do: 2

Enter the id of the employee:  10
Enter the name of the employee:  Hassan
Enter the position of the employee:  Data Engineer
Enter the salary of the employee:  hassan@outlook.com

Employee Hassan with ID 10 was added successfuly
```

---

## File Structure
- `Employee_Manager`.ipynb: Main code and execution output.
- `Employees.csv`: Stores employee data with headers Id, Name, Position, Salary, Email.

---

## Error Handling
The program includes mechanisms to handle errors gracefully, ensuring robust operation:
- __Input Validation__:
  - `idInput(s)`: Ensures IDs are positive integers. Invalid inputs (e.g., "ff") trigger "Please enter a valid integer" and prompt again.
  - `salaryInput(s)`: Ensures salaries are positive numeric values. Invalid inputs (e.g., "dd") trigger "Please enter a valid numeric value".
  - `emailInput(s)`: Validates emails using the regex `r"^[\\w\\.-]+@\\w+\\.\\w+$"`. Invalid emails (e.g., "hassan", "hassan.com") trigger "Please enter a valid email".
  - Example: When adding an employee with ID "ff", salary "dd", and email "hassan", the program prompts for correct inputs (e.g., ID "10", salary "30000", email "hassan@outlook.com").
- __Exception Handling__:
  - Methods `updateEmployee(id)`, `deleteEmployee(id)`, and `searchEmployee(id)` use `try-except` blocks to handle non-existent IDs:
    - If the ID is not found, prints "Id {id} was not found".
    - If the ID is non-numeric, `deleteEmployee(id) `and `searchEmployee(id)` print "Please enter a valid integer".
  - Example: Searching for ID "99" (non-existent) outputs "Id 99 was not found"; entering "abc" outputs "Please enter a valid integer".
- __ID Uniqueness__: `addEmployee()` checks for duplicate IDs, printing "id {id} already exists" if the ID is taken, preventing data conflicts.
- __CSV File Management__: The program checks for `Employees.csv` existence with `os.path.exists()` and creates it if missing, avoiding file access errors.
