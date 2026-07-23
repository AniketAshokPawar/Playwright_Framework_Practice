# Data Driven Testing (DDT) in Playwright

## What is Data Driven Testing (DDT)?

Data Driven Testing is a testing approach where we execute the **same test logic with multiple sets of test data**.

Instead of creating separate tests for different data values, we maintain the data separately and run the same test multiple times using different inputs.

---

## Without DDT

Example:

```ts
test("Login with Admin", async({page})=>{

    await login("admin","admin123");

});


test("Login with User", async({page})=>{

    await login("user1","user123");

});
```

Problems:

- Duplicate test code
- Difficult maintenance
- More test files
- Data changes require code changes

---

# With DDT

We separate:

- Test logic
- Test data

Example:

```
Test Data
    |
    ↓
Test Script
    |
    ↓
Execute multiple times
```

---

# Benefits of DDT

## 1. Reusability

Same test can execute with multiple data sets.

Example:

```
Login Test

    |
    ├── Admin User
    |
    ├── Manager User
    |
    └── Guest User
```

---

## 2. Easy Maintenance

If test data changes, we update only the data file.

No need to modify test code.

---

## 3. Better Test Coverage

Multiple scenarios can be covered using different data combinations.

Example:

```
Username     Password

admin        admin123

user1        user123

guest        guest123
```

---

# DDT Using JSON in Playwright

## Folder Structure

```
Playwright Project

├── pages
│
├── fixtures
│
├── test-data
│   └── loginData.json
│
└── tests
    └── login.spec.ts
```

---

# Single Data Object

Example:

`loginData.json`

```json
{
    "username": "admin",
    "password": "admin123"
}
```

Import:

```ts
import loginData from "../../test-data/loginData.json";
```

Usage:

```ts
await loginPage.login(
    loginData.username,
    loginData.password
);
```

Here:

```
loginData
    |
    ├── username
    └── password
```

---

# Multiple Test Data Sets

For DDT, JSON is usually maintained as an array of objects.

Example:

`loginData.json`

```json
[
    {
        "username": "admin",
        "password": "admin123"
    },
    {
        "username": "user1",
        "password": "user123"
    },
    {
        "username": "manager",
        "password": "manager123"
    }
]
```

Structure:

```
loginData

    |
    ├── [0]
    |      |
    |      ├── username
    |      └── password
    |
    ├── [1]
    |      |
    |      ├── username
    |      └── password
    |
    └── [2]
           |
           ├── username
           └── password
```

---

# Import JSON Data

Import remains the same:

```ts
import loginData from "../../test-data/loginData.json";
```

The import variable name is decided by the developer.

Example:

```ts
import data from "../../test-data/loginData.json";
```

also works.

Here:

```
loginData
```

is only a variable name.

It does not come from the JSON file.

---

# Execute Test Using Loop

To execute the same test with multiple data sets:

```ts
for(const data of loginData){

    test(`Login with ${data.username}`, async({page})=>{


    });

}
```

---

# Complete DDT Example

```ts
import {test, expect} from '@playwright/test';
import loginData from '../../test-data/loginData.json';


for(const data of loginData){

    test(`Login with ${data.username}`, async({page})=>{

        await page.goto("login URL");


        await page.locator("#username")
            .fill(data.username);


        await page.locator("#password")
            .fill(data.password);


        await page.locator("#loginBtn")
            .click();


    });

}
```

---

# Execution Result

From one test code:

```ts
test(`Login with ${data.username}`)
```

Playwright creates multiple tests:

```
✓ Login with admin

✓ Login with user1

✓ Login with manager
```

---

# DDT With Page Object Model

In framework projects, DDT is combined with POM.

Flow:

```
loginData.json

        ↓

Test File

        ↓

LoginPage Method

        ↓

BasePage Actions

        ↓

Playwright
```

Example:

```ts
for(const data of loginData){

    test(`Login ${data.username}`, async({loginPage})=>{


        await loginPage.login(
            data.username,
            data.password
        );


    });

}
```

---

# DDT With Fixtures

In a framework:

```
JSON Data

    ↓

Test

    ↓

Fixture

    ↓

Page Object

    ↓

BasePage

    ↓

Playwright
```

Example:

```ts
test(
"Login Test",
async({loginPage})=>{


    await loginPage.login(
        data.username,
        data.password
    );


});
```

---

# Common DDT Data Sources

Apart from JSON, test data can come from:

## JSON

Most common for Playwright.

Example:

```
loginData.json
```

---

## Excel

Used when business users maintain data.

Example:

```
TestData.xlsx
```

---

## CSV

Useful for large data sets.

Example:

```
users.csv
```

---

## Database

Used when test data comes from application databases.

Example:

```
SQL Database
```

---

# Important Interview Questions

## Q1. What is Data Driven Testing?

Answer:

Data Driven Testing is an approach where the same test script is executed with multiple sets of test data stored separately from the test logic.

---

## Q2. How do you implement DDT in Playwright?

Answer:

By maintaining test data in external files like JSON, importing the data into tests, and iterating through the data to execute the same test with different inputs.

---

## Q3. Why do we use JSON for test data?

Answer:

- Easy to maintain
- Simple structure
- Separates data from code
- Reusable across tests
- Supports multiple test scenarios

---

## Q4. Difference between hardcoded data and DDT?

Hardcoded:

```ts
login("admin","admin123");
```

Data Driven:

```ts
login(
data.username,
data.password
);
```

---

# Current Framework Structure After DDT

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

DDT helps us:

- Avoid duplicate test scripts
- Execute tests with multiple datasets
- Improve maintainability
- Increase test coverage

Common Playwright framework combination:

```
POM
 +
BasePage
 +
Fixtures
 +
JSON Test Data
 +
DDT

=

Scalable Automation Framework
```
