# Bank Management System

A simple **command-line Bank Management System** built with Python. The project demonstrates object-oriented programming, JSON-based data persistence, user input handling, account authentication using an account number and PIN, and basic banking operations.

> **Project type:** Console / CLI application  
> **Language:** Python  
> **Data storage:** JSON file  
> **Entry point:** `main.py`

---

## 📌 Project Overview

This project simulates a basic banking workflow through an interactive terminal menu. A customer can open a bank account, deposit money, withdraw money, view account details, update selected information, or close an account.

The application keeps customer records in `data.json`, so changes made through the program are written back to the local JSON file.

The implementation is intentionally lightweight and is suitable for learning **Python classes, file handling, JSON serialization, exception handling, list/dictionary operations, random number generation, and basic CRUD-style workflows**.

---

## ✨ Features

### 1. Open New Account

The application collects:

- Full name
- Date of birth
- Gender
- Email address
- 4-digit PIN

It then automatically generates a 15-digit account number and creates an initial balance of `0`.

The current implementation checks:

- Customer age must be **18 or above**
- PIN must contain **exactly 4 digits**

When the checks pass, the account is added to `data.json`.

### 2. Deposit Money

A customer authenticates using:

- Account number
- PIN

After successful lookup, the requested amount is added to the account balance and the updated data is saved to the JSON database.

### 3. Withdraw Money

The withdrawal workflow also requires the account number and PIN.

The application checks the available balance before deducting the requested amount. When the balance is insufficient, the transaction is rejected.

### 4. View Passbook / Account Details

The menu option labelled **View Passbook** displays the stored account information after account number and PIN verification.

The current implementation prints the complete stored record, including account details and balance.

### 5. Update Account Details

A verified customer can update:

- Name
- Email
- PIN

The application preserves:

- Date of birth
- Age
- Gender
- Account number
- Balance

The updated record is persisted back to `data.json`.

### 6. Close Account

A customer can locate the account using account number and PIN, review the stored details, and confirm the closure.

When confirmed with `y`, the account record is removed from the in-memory list and the updated data is written to `data.json`.

---

## 🖥️ Application Menu

The current terminal menu is:

```text
1. Open Account
2. Deposit Money
3. Whidhraw Money
4. View Passbook
5. Update Details
6. Close Account
```

The displayed withdrawal label contains the existing spelling **`Whidhraw Money`** from the source code.

---

## 🏗️ Project Structure

```text
Bank-Management-System/
├── main.py      # Main application logic and CLI
├── data.json    # Local JSON database containing account records
└── README.md    # Project documentation
```

---

## 🔧 Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core application language |
| JSON | Local data persistence |
| `pathlib.Path` | Checking whether the database file exists |
| `datetime` | Date-of-birth and age calculation |
| `random` | Account number generation |
| `string` | Numeric character generation |
| OOP / Classes | Organizing user and banking operations |

No external Python packages are required by the current implementation; it uses modules from the Python standard library.

---

## 🧩 Code Architecture

### `userInfo` Class

The `userInfo` class is responsible for collecting customer information through terminal input.

Implemented methods:

- `Name()` — reads and uppercases the full name
- `dob()` — collects day, month, and year
- `Age()` — calculates age from the stored date of birth
- `Gender()` — accepts `M` / `F` input
- `Email()` — reads the email address
- `Pin()` — collects the account PIN

### `Bank` Class

The `Bank` class manages the application and account data.

Important class members:

- `database = 'data.json'` — JSON database filename
- `data = []` — in-memory collection of account records

Important methods:

| Method | Responsibility |
|---|---|
| `loadDatabase()` | Loads JSON records into memory when the application starts |
| `__update()` | Writes the current account list back to `data.json` |
| `__accountGenerate()` | Generates a 15-digit account number |
| `creatAcc()` | Creates a new customer account |
| `moneyDeposit()` | Deposits funds into an authenticated account |
| `moneyWhidhraw()` | Withdraws funds after authentication |
| `showDetails()` | Displays authenticated account details |
| `userUpdate()` | Updates editable customer information |
| `closeAccount()` | Removes an account after confirmation |

---

## 💾 Data Storage

The project uses a local JSON file instead of a relational database.

Each account is represented as an object containing fields such as:

```json
{
  "Name": "CUSTOMER NAME",
  "DOB": "DD/MM/YYYY",
  "Age": 20,
  "Gender": "MALE",
  "Email": "customer@example.com",
  "Account No.": "123456789012345",
  "PIN": 1234,
  "Balance": 0
}
```

The program loads this list at startup and rewrites the complete list whenever account data changes.

### Persistence Flow

```text
Terminal Input
      ↓
Bank Class / Business Logic
      ↓
In-Memory Bank.data
      ↓
JSON Serialization
      ↓
data.json
```

---

## ▶️ How to Run

### Prerequisites

Install **Python 3** on your system.

Check your Python installation:

```bash
python --version
```

or:

```bash
python3 --version
```

### Clone the Repository

```bash
git clone https://github.com/vishwas0229/Bank-Management-System.git
cd Bank-Management-System
```

### Run the Application

On systems where `python` maps to Python 3:

```bash
python main.py
```

Otherwise:

```bash
python3 main.py
```

The program will display the banking menu and wait for the user's selection.

---

## 🔐 Authentication

For account-level actions, the current implementation uses a combination of:

```text
Account Number + PIN
```

The program searches the loaded account list for a record where both values match.

This provides a basic authentication mechanism for the learning project, but it is **not suitable for production banking software**.

---

## 🧠 Concepts Demonstrated

This project is useful for practicing several Python concepts:

- Object-oriented programming
- Classes and methods
- Class variables
- Class methods
- Private / name-mangled helper methods
- Lists and dictionaries
- List comprehensions
- File handling
- JSON serialization/deserialization
- Exception handling with `try` / `except`
- Date and time calculations
- Random number generation
- Conditional statements
- User input validation
- CRUD-style data operations

---

## 🔄 Typical Workflow

### Account Creation

```text
Start Application
      ↓
Select "Open Account"
      ↓
Enter Customer Information
      ↓
Generate Account Number
      ↓
Validate Age + PIN
      ↓
Create Account
      ↓
Save to data.json
```

### Deposit / Withdrawal / View / Update

```text
Start Application
      ↓
Select an Operation
      ↓
Enter Account Number + PIN
      ↓
Find Matching Account
      ↓
Perform Requested Operation
      ↓
Save Changes to data.json
```

### Account Closure

```text
Account Number + PIN
      ↓
Find Account
      ↓
Show Account Details
      ↓
Confirm Closure
      ↓
Remove Record
      ↓
Save data.json
```

---

## ⚠️ Current Implementation Notes

This README documents the repository as it currently exists. Some behaviors should be considered when using or extending the project:

1. **Local JSON storage**  
   `data.json` is a plain local file rather than a secure banking database.

2. **PIN storage**  
   PIN values are stored directly in the JSON data, so the current implementation does not hash or encrypt credentials.

3. **Account number generation**  
   Account numbers are generated randomly from digits. The current code does not perform a database-level uniqueness check before storing a newly generated number.

4. **Input validation**  
   User input is only partially validated. Invalid numeric/date input can raise exceptions, and the program is not structured around a comprehensive validation layer.

5. **Withdrawal validation**  
   The code checks for insufficient balance, but the current withdrawal flow does not explicitly reject every possible invalid amount case such as a negative value.

6. **Menu flow**  
   The program processes a single selected menu operation each time `main.py` is executed; it does not currently contain a continuous loop that returns to the main menu.

7. **Source-level typo**  
   The withdrawal method and menu label contain the existing spelling `Whidhraw`. This README keeps that spelling when referring to the actual implementation.

8. **Close-account menu behavior**  
   The menu labels option 6 as **Close Account**, while the current bottom-level dispatch in `main.py` calls `user.userUpdate()` for option 6. Therefore, the `closeAccount()` method exists, but option 6 does not currently invoke it from the main menu.

9. **Sample data**  
   The repository's `data.json` contains sample account records. Because these records include example PINs and balances, they should be treated as demo data only and replaced before any real-world use.

---

## 🚀 Possible Future Improvements

The current project can be extended into a more complete banking application by adding:

- A continuous main-menu loop
- Stronger input validation
- Unique account-number verification
- Hashed PIN/password storage
- Transaction history
- Transfer between accounts
- Mini statement / transaction history
- Customer search
- Admin functionality
- Account types such as savings/current
- Interest calculation
- Proper database integration such as SQLite or MySQL
- Logging and audit trails
- Unit tests
- Better separation of UI, business logic, and data-access layers
- Safer handling of sensitive customer information

---

## 🎯 Learning Objective

The primary value of this project is educational: it shows how a small Python program can combine **OOP + file handling + JSON storage + authentication + CRUD operations** into a practical application.

It can serve as a foundation for learning software design before moving from a local CLI prototype to a database-backed application with stronger security and maintainability.

---

## 📄 License

No explicit license file is currently present in the repository. Until a license is added, the project's reuse and redistribution terms should be considered unspecified.

---

## 👨‍💻 Repository

**GitHub:** https://github.com/vishwas0229/Bank-Management-System
