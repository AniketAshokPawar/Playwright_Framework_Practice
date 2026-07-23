# Test Data Management in Playwright Framework

## What is Test Data Management?

Test Data Management is a practice of separating **test data** from **test logic**.

Instead of writing test data directly inside test cases, we maintain it separately and use it in our tests.

Example:

❌ Hardcoded test data:

```ts
await loginPage.login(
    "admin",
    "admin123"
);
```

✅ Using test data:

```ts
await loginPage.login(
    loginData.username,
    loginData.password
);
```

---

# Why do we need Test Data Management?

## 1. Reusability

The same test data can be used in multiple test cases.

Example:

```
tests/
 |
 ├── login.spec.ts
 ├── dashboard.spec.ts
 └── profile.spec.ts
```

All tests can use:

```
test-data/loginData.json
```

---

## 2. Easy Maintenance

Without test data management:

```ts
login("admin","admin123")
```

If username changes, we need to update multiple test files.

With separate test data:

```json
{
 "username":"admin01",
 "password":"admin123"
}
```

Change once and all tests use the updated value.

---

## 3. Separation of Concerns

Test file should contain only:

- Test flow
- Validation
- Business logic

Test data should contain:

- Usernames
- Passwords
- Input values
- Expected values

---

# Test Data Management Approaches

## 1. Hardcoded Data (Not Recommended)

Example:

```ts
await loginPage.login(
    "admin",
    "admin123"
);
```

Problems:

- Difficult maintenance
- Duplicate data
- Not reusable

---

# 2. Data Object Inside Test File

Example:

```ts
const loginData = {

    username: "admin",
    password: "admin123"

};
```

Usage:

```ts
await loginPage.login(
    loginData.username,
    loginData.password
);
```

Problem:

- Data is still inside the test file.
- Cannot easily share between tests.

---

# 3. Separate TypeScript Data File

Folder structure:

```
test-data/

    loginData.ts

    studentData.ts
```

---

## loginData.ts

```ts
export const loginData = {

    username: "admin",
    password: "admin123"

};
```

---

## Import in Test

```ts
import {loginData} from "../../test-data/loginData";
```

Usage:

```ts
await loginPage.login(
    loginData.username,
    loginData.password
);
```

Advantages:

- Reusable
- Easy maintenance
- Better framework structure

---

# 4. JSON Test Data (Most Common)

In many Playwright frameworks, JSON files are used to store test data.

Folder structure:

```
test-data/

    loginData.json

    studentData.json
```

---

# loginData.json

```json
{
    "username": "admin",
    "password": "admin123"
}
```

---

# studentData.json

```json
{
    "studentName": "Aniket",
    "email": "aniket.pawar@gmail.com",
    "mobile": "1234567890",
    "gender": "male",
    "course": "Playwright",
    "country": "India",
    "terms": true
}
```

---

# Import JSON Data in Test

```ts
import loginData from "../../test-data/loginData.json";

import studentData from "../../test-data/studentData.json";
```

Important:

The import variable name is decided by us.

Example:

```ts
import data from "../../test-data/loginData.json";
```

also works.

Here:

```ts
loginData
```

is only a variable name.

It does not come from JSON.

---

# Using JSON Data

Example:

```ts
await loginPage.login(
    loginData.username,
    loginData.password
);
```

Student registration:

```ts
await studentRegistrationPage.register(

    studentData.studentName,

    studentData.email,

    studentData.mobile,

    studentData.gender,

    studentData.course,

    studentData.country,

    studentData.terms

);
```

---

# Single User vs Multiple Users

## Single User JSON

Structure:

```json
{
    "username":"admin",
    "password":"admin123"
}
```

This is an object.

Access:

```ts
loginData.username
```

---

## Multiple Users JSON

Structure:

```json
[
    {
        "username":"admin",
        "password":"admin123"
    },

    {
        "username":"user1",
        "password":"user123"
    },

    {
        "username":"guest",
        "password":"guest123"
    }
]
```

This is an array of objects.

---

## Import remains same

```ts
import loginData from "../../test-data/loginData.json";
```

Only the data structure changes.

---

# Accessing Multiple Users

## Not Recommended in Framework

```ts
loginData[0].username

loginData[1].username
```

This is mainly for understanding arrays.

---

## Recommended Industry Approach

Use loops:

```ts
for(const user of loginData){

    await loginPage.login(
        user.username,
        user.password
    );

}
```

Here every iteration gets one user object.

---

# Data Flow in Framework

```
JSON File

    |
    ↓

Import Data

    |
    ↓

Test File

    |
    ↓

Page Object Method

    |
    ↓

Playwright Action
```

Example:

```
loginData.json

      ↓

loginData.username

      ↓

loginPage.login()

      ↓

username textbox fill
```

---

# Test Data Management with Framework Components

Current framework:

```
Test

 |

Fixture

 |

Page Object

 |

BasePage

 |

Playwright
```

With Test Data:

```
Test Data JSON

 |

Test

 |

Fixture

 |

Page Object

 |

BasePage

 |

Playwright
```

---

# Advantages of JSON Test Data

- Easy to read
- Easy to update
- Separates data and code
- Reusable across tests
- Supports Data Driven Testing
- Language independent

---

# Interview Questions

## Q1. Why do we separate test data from test scripts?

Answer:

To improve maintainability, reusability, and readability by keeping test logic separate from input data.

---

## Q2. Where do you maintain test data in Playwright framework?

Answer:

Usually in a separate `test-data` folder using JSON files, TypeScript files, environment files, or external sources.

---

## Q3. Difference between TypeScript data file and JSON data file?

TypeScript file:

- Can contain logic
- Can contain variables/functions
- Uses export/import

JSON file:

- Contains only data
- No logic
- Used mainly for test data management

---

## Q4. Does import name come from JSON file?

No.

Example:

```ts
import loginData from "./loginData.json";
```

`loginData` is a variable name created by the developer.

---

# Current Framework Structure After Test Data Management

```
Playwright Practice

├── pages
│   ├── BasePage.ts
│   ├── LoginPage.ts
│   └── StudentRegistrationPage.ts
│
├── fixtures
│   └── test-fixtures.ts
│
├── test-data
│   ├── loginData.json
│   └── studentData.json
│
├── tests
│   └── test_with_POM.spec.ts
│
└── playwright.config.ts
```

---

# Summary

Test Data Management helps us:

- Keep test data separate from test logic
- Avoid hardcoded values
- Reuse data across tests
- Maintain large automation frameworks easily

Common industry approach:

```
JSON Test Data
        +
POM
        +
Fixtures
        +
BasePage
        =
Scalable Playwright Framework
```
