# Bank Management System

A Python command-line banking demo using object-oriented programming, local JSON persistence, and bcrypt-hashed PINs. It supports account creation, deposits, withdrawals, account lookup, profile updates, and account closure through a repeating terminal menu.

> **Educational project, not production banking software.** Known input-validation, authentication-error-handling, and financial-integrity issues are documented below.

## At a glance

| Item | Current implementation |
|---|---|
| Interface | Interactive terminal / CLI |
| Language | Python 3 |
| External dependency | `bcrypt` |
| Storage | Local `data.json` file |
| Entry point | `main.py` |
| Authentication | Account number + PIN verified with `bcrypt.checkpw()` |
| Menu | Repeats until the user chooses `0` |
| GitHub issues | 3 closed, 5 open (as of 24 September 2026) |

## Features

| Menu | Operation | What the code currently does |
|---|---|---|
| 1 | Open Account | Collects name, DOB, gender, email and a four-digit PIN; generates a random 15-digit account number; rejects customers whose calculated age is below 18; stores a bcrypt PIN hash and initial balance of zero. |
| 2 | Deposit Money | Looks up the account and checks the entered PIN; accepts a positive integer amount; updates the balance in JSON. |
| 3 | Whidhraw Money | Authenticates the account, checks whether the amount exceeds the available balance, and updates the balance. **Negative amounts are not rejected**. |
| 4 | View Passbook | Authenticates and displays the stored account record. This is account details, **not a transaction statement**. |
| 5 | Update Details | Authenticates and prompts for name, email and a new PIN; retains DOB, age, gender, account number and balance. |
| 6 | Close Account | Authenticates, shows the record, requests `y` confirmation and removes the account from JSON. |
| 0 | Exit | Ends the repeating menu loop. |

The existing `Whidhraw` spelling is retained above to match the source code. No fund-transfer, transaction-history or database-backend feature is implemented yet.

## Getting started

### Requirements

- Python 3
- `bcrypt`, an external Python package

### Install and run

```bash
git clone https://github.com/vishwas0229/Bank-Management-System.git
cd Bank-Management-System

# Optional: create an isolated environment
python3 -m venv .venv
source .venv/bin/activate

python3 -m pip install bcrypt
python3 main.py
```

On Windows, activate the environment with `.venv\Scripts\activate` and use `python` if that is your Python 3 command.

**Run the program from the repository directory:** `Bank.database` uses the relative path `data.json`. The current loader prints a message if that file does not exist; it does not initialize a new file automatically.

### Actual terminal menu

```text
1. Open Account
2. Deposit Money
3. Whidhraw Money
4. View Passbook
5. Update Details
6. Close Account
0. Exit
```

After each selected operation, the menu appears again unless `0` is chosen. A non-integer menu response currently raises an exception.

## Repository structure

```text
Bank-Management-System/
├── main.py       # CLI, userInfo and Bank classes
├── data.json     # Local account records (demo data)
└── README.md     # Project documentation
```

This layout describes the files verified during the README update; it is not a claim that the repository contains no other metadata.

## Architecture

### `userInfo`

| Method | Behavior |
|---|---|
| `Name()` | Reads and uppercases the name. |
| `dob()` | Reads numeric day, month and year; saves them on the object. |
| `Age()` | Estimates age using elapsed days divided by 365. |
| `Gender()` | Returns `MALE` for `M` and `FEMALE` for every other input. |
| `Email()` | Reads an email string without format validation. |
| `Pin()` | Re-prompts until input is exactly four numeric characters; hashes it using `bcrypt.hashpw(pin.encode('utf-8'), bcrypt.gensalt())` and returns a UTF-8 string. |

### `Bank`

| Method | Behavior |
|---|---|
| `loadDatabase()` | Reads the JSON list from `data.json` when present. |
| `__update()` | Rewrites the full in-memory account list to `data.json`. |
| `__accountGenerate()` | Generates a random 15-digit string; does **not** check for duplicates. |
| `creatAcc()` | Collects account information and saves eligible customers. |
| `moneyDeposit()` | Authenticates and adds a positive integer deposit. |
| `moneyWhidhraw()` | Authenticates and subtracts the entered integer if balance is sufficient. |
| `showDetails()` | Displays an authenticated customer's stored record. |
| `userUpdate()` | Changes selected profile fields and PIN. |
| `closeAccount()` | Deletes a confirmed account and persists the updated list. |

`Bank.data` is a class-level in-memory list. The program reads account data on initialization and writes the whole list when a supported operation changes it. This is a simple local demonstration, not a transactional storage layer.

## PIN handling and data format

A newly created PIN is hashed with bcrypt and stored as a UTF-8 string. On account lookup, the entered PIN is checked against the stored hash using `bcrypt.checkpw()`. The current demo `data.json` also uses the bcrypt-hash format.

Illustrative account record (the hash is a **placeholder**, not a working credential):

```json
[
  {
    "Name": "EXAMPLE CUSTOMER",
    "DOB": "1/1/2000",
    "Age": 26,
    "Gender": "MALE",
    "Email": "example@example.com",
    "Account No.": "123456789012345",
    "PIN": "<bcrypt-hash>",
    "Balance": 0
  }
]
```

Do not replace an existing bcrypt hash with a plain four-digit PIN. Existing legacy plain-text or numeric PIN records are **not migrated automatically** by the current implementation.

**Privacy limitation:** `creatAcc()`, `showDetails()`, `userUpdate()` and `closeAccount()` print the full account record, including the stored PIN hash. A hash is not the original PIN, but credential hashes should still not be displayed or exposed unnecessarily.

## Workflows

### Account creation

```text
Collect customer information
        |
Validate four-digit PIN and hash it with bcrypt
        |
Generate a random 15-digit account number
        |
Check calculated age >= 18
        |
Append account to Bank.data
        |
Rewrite data.json
```

### Authenticated operations

```text
Enter account number and PIN
        |
Match account number + bcrypt.checkpw()
        |
Deposit / withdraw / view / update / close
        |
Write data.json if records changed
```

**Known lookup defect:** account searches return a list, but the code tests `if userData == False` instead of testing whether that list is empty. An incorrect account number or PIN can therefore lead to `userData[0]` raising an `IndexError`; deposit and withdrawal catch it, while several other operations do not. This behavior is not a successful authentication fallback.

## Verified issue tracker

| Issue | Description | Status |
|---|---|---|
| [#1](https://github.com/vishwas0229/Bank-Management-System/issues/1) | Continuous main menu | **Closed** — repeating loop and exit option exist |
| [#2](https://github.com/vishwas0229/Bank-Management-System/issues/2) | Transaction history | **Open** — no ledger or mini statement |
| [#3](https://github.com/vishwas0229/Bank-Management-System/issues/3) | Fund transfer | **Open** — no transfer workflow |
| [#4](https://github.com/vishwas0229/Bank-Management-System/issues/4) | Strong input validation | **Open** — four-digit PIN validation exists; broader validation is incomplete |
| [#5](https://github.com/vishwas0229/Bank-Management-System/issues/5) | Secure PIN storage | **Closed** — bcrypt hashing and verification exist; see remaining security caveats |
| [#6](https://github.com/vishwas0229/Bank-Management-System/issues/6) | Database backend | **Open** — local JSON is still used |
| [#7](https://github.com/vishwas0229/Bank-Management-System/issues/7) | Account-number uniqueness | **Open** — collisions are not checked |
| [#8](https://github.com/vishwas0229/Bank-Management-System/issues/8) | Close-account menu mapping | **Closed** — option 6 calls `closeAccount()` |

**Progress: 3 closed / 5 open.** Closed issues describe the implemented scope, not a claim that all related edge cases are resolved.

## Known limitations and risks

1. **Incorrect account/PIN handling:** the `userData == False` comparison does not detect an empty list. Some actions can crash when credentials do not match.
2. **Negative withdrawal amounts:** a negative amount passes the insufficient-balance check and can increase the account balance. This must be fixed before using the program for anything beyond demonstration.
3. **Partial input validation:** malformed dates, invalid numeric responses, email format, account-number format and unexpected gender inputs are not handled comprehensively. Age is approximated with `days // 365`.
4. **PIN-change behavior:** `userUpdate()` always invokes `Pin()`, so pressing Enter cannot preserve the old PIN as the surrounding code appears to intend. The new PIN hash is generated even when only name or email needs updating.
5. **Sensitive output:** account creation and account-detail workflows print the stored PIN hash alongside other personal information.
6. **No transaction ledger:** deposits and withdrawals update balances only. The "View Passbook" menu displays account details, not a chronological passbook.
7. **JSON storage:** there is no database transaction handling, concurrent-write protection or audit trail.
8. **Account-number collisions:** generated 15-digit numbers are not checked against existing records.
9. **Account closure:** the program removes the account on confirmation without enforcing a zero balance.
10. **CLI error handling:** the top-level menu converts input directly to `int`; malformed responses can terminate the application.

These observations come from static review of the current source. They are not a claim that the application was run through an automated test suite.

## Development roadmap

The remaining tracked work is transaction history (#2), transfers (#3), comprehensive validation (#4), database integration (#6), and account-number uniqueness (#7). The incorrect-credential lookup and negative-withdrawal issues described above are additional defects worth tracking separately.

Potential later improvements include automated tests, safer credential-hash display, account-closure balance checks, structured logging, audit trails, and separation of CLI, business logic and persistence.

## Documentation-only maintenance policy

This README update documents the current code without modifying application or demo-data files.

| File | Policy for this update |
|---|---|
| `README.md` | Updated |
| `main.py` | Read-only; not modified |
| `data.json` | Read-only; not modified |

## Learning scope

The project demonstrates Python classes, class methods, list comprehensions, JSON serialization, file handling, basic CRUD workflows, random account-number generation and bcrypt-based credential verification. It is a starting point for learning application structure rather than a deployable banking service.

## License

Check the repository for its current license terms before reuse. If no license is present, do not assume unrestricted permission to redistribute the code.

**Repository:** https://github.com/vishwas0229/Bank-Management-System
