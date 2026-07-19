# Fixtures in Playwright Framework

## What are Fixtures?

Fixtures are a mechanism in Playwright that helps to create and manage reusable objects/resources before running tests.

In a framework, fixtures are mainly used to:

- Create Page Objects automatically
- Avoid creating objects manually in every test
- Provide reusable objects directly to test cases

---

# Why do we need Custom Fixtures?

Before using fixtures, we manually created Page Object instances inside every test.

Example:

```ts
test("Login", async({page})=>{

    const loginPage = new LoginPage(page);

    const studentRegistrationPage = 
        new StudentRegistrationPage(page);

    await loginPage.login(
        "admin",
        "admin123"
    );

});
```

Problems:

- Same object creation code repeats in every test.
- Tests become longer.
- Maintenance becomes difficult when the framework grows.

---

# Solution: Custom Fixtures

With custom fixtures, Playwright creates Page Objects automatically.

After using fixtures:

```ts
test("Login", async({
    page,
    loginPage,
    studentRegistrationPage
})=>{

    await loginPage.login(
        "admin",
        "admin123"
    );

});
```

No need to write:

```ts
new LoginPage(page)
```

or

```ts
new StudentRegistrationPage(page)
```

because fixtures handle object creation.

---

# Fixture Flow

```
Test File

     |
     |
     ↓

Custom Fixture

     |
     |
     ↓

Create Page Object

     |
     |
     ↓

Page Class

     |
     |
     ↓

BasePage

     |
     |
     ↓

Playwright Actions
```

---

# Folder Structure

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
└── tests
    └── POM_tests
        └── test_with_POM.spec.ts
```

---

# Creating Custom Fixture File

File:

```
fixtures/test-fixtures.ts
```

Code:

```ts
import {test as base} from '@playwright/test';
import {LoginPage} from '../pages/LoginPage';
import {StudentRegistrationPage} from '../pages/StudentRegistrationPage';


type MyFixtures = {

    loginPage: LoginPage;

    studentRegistrationPage: StudentRegistrationPage;

};


export const test = base.extend<MyFixtures>({

    loginPage: async ({page}, use)=>{

        const loginPage = new LoginPage(page);

        await use(loginPage);

    },


    studentRegistrationPage: async ({page}, use)=>{

        const studentRegistrationPage =
            new StudentRegistrationPage(page);

        await use(studentRegistrationPage);

    }

});
```

---

# Understanding Fixture Code

## Import Playwright Test

```ts
import {test as base} from '@playwright/test';
```

We rename Playwright's default test as `base`.

Reason:

We will extend the existing Playwright test and add our own fixtures.

---

# Fixture Type Definition

```ts
type MyFixtures = {

    loginPage: LoginPage;

    studentRegistrationPage: StudentRegistrationPage;

};
```

This tells TypeScript:

"We are creating two custom fixtures."

Fixtures:

```
loginPage
studentRegistrationPage
```

Their types are:

```
LoginPage object
StudentRegistrationPage object
```

---

# Extending Playwright Test

```ts
export const test = base.extend<MyFixtures>({
```

Meaning:

Create a new custom test by extending Playwright's existing test.

Now our test supports:

```ts
loginPage
```

and

```ts
studentRegistrationPage
```

---

# Creating LoginPage Fixture

```ts
loginPage: async ({page}, use)=>{

    const loginPage = new LoginPage(page);

    await use(loginPage);

}
```

Flow:

```
Playwright creates page

        ↓

new LoginPage(page)

        ↓

use(loginPage)

        ↓

Test receives loginPage
```

---

# Creating StudentRegistrationPage Fixture

```ts
studentRegistrationPage: async ({page}, use)=>{

    const studentRegistrationPage =
        new StudentRegistrationPage(page);

    await use(studentRegistrationPage);

}
```

Flow:

```
Playwright creates page

        ↓

new StudentRegistrationPage(page)

        ↓

use(studentRegistrationPage)

        ↓

Test receives studentRegistrationPage
```

---

# Understanding use()

`use()` passes the created object to the test.

Example:

Fixture:

```ts
await use(loginPage);
```

Test receives:

```ts
async({loginPage})=>{

}
```

Flow:

```
Object Created

      ↓

use(object)

      ↓

Injected into test

      ↓

Test uses object
```

---

# Using Fixtures in Test

Before Fixtures:

```ts
import {test,expect} from '@playwright/test';
import {LoginPage} from '../../pages/LoginPage';


test("Login", async({page})=>{

    const loginPage = new LoginPage(page);

});
```

---

After Fixtures:

```ts
import {expect} from '@playwright/test';
import {test} from '../../fixtures/test-fixtures';


test("Login", async({
    page,
    loginPage,
    studentRegistrationPage
})=>{

});
```

---

# Complete Test Example

```ts
import {expect} from '@playwright/test';
import {test} from '../../fixtures/test-fixtures';


const loginPageUrl =
'file:///C:/Users/z00575dy/OneDrive - Siemens AG/Desktop/PlayWright Practice/POM_Webpages/login.html';


test("Login", async({
    page,
    loginPage,
    studentRegistrationPage
})=>{


    await page.goto(loginPageUrl);


    await loginPage.login(
        "admin",
        "admin123"
    );


    await expect(page.locator("#welcomeMsg"))
        .toContainText("Welcome Admin");


    await studentRegistrationPage.register(
        "Aniket",
        "aniket.pawar@gmail.com",
        "1234567890",
        "male",
        "Playwright",
        "India",
        true
    );


    await expect(page.locator("#successMsg"))
        .toContainText(
            "Student Registered Successfully"
        );

});
```

---

# BasePage vs Fixtures

## BasePage

Purpose:

Reuse common actions.

Examples:

```ts
click()

fill()

check()

selectOption()
```

Flow:

```
Page Class
     |
     ↓
BasePage
     |
     ↓
Playwright
```

---

## Fixtures

Purpose:

Reuse object creation.

Examples:

```ts
loginPage

studentRegistrationPage
```

Flow:

```
Test

 ↓

Fixture

 ↓

Page Object
```

---

# Advantages of Fixtures

## 1. Removes Object Creation Code

Without fixtures:

```ts
const loginPage =
new LoginPage(page);
```

With fixtures:

```ts
async({loginPage})
```

---

## 2. Cleaner Tests

Tests focus only on business actions.

Example:

```ts
await loginPage.login();
```

instead of:

```ts
const loginPage = new LoginPage(page);
await loginPage.login();
```

---

## 3. Better Maintainability

If Page Object creation changes:

Before:

Need to update many test files.

After:

Update only:

```
fixtures/test-fixtures.ts
```

---

## 4. Scalable Framework

For large projects:

```
100+ tests

        ↓

same fixtures

        ↓

same Page Objects
```

---

# Interview Questions

## Q1. Why do we use fixtures in Playwright?

Answer:

Fixtures are used to create and manage reusable objects automatically and provide them to tests. They reduce duplicate code and improve framework maintainability.

---

## Q2. Difference between BasePage and Fixtures?

Answer:

BasePage contains reusable Playwright actions like click, fill, and check.

Fixtures create and provide reusable objects like Page Objects to tests.

---

## Q3. What is use() in Playwright fixtures?

Answer:

`use()` passes the created fixture object to the test so that the test can access and use it.

---

## Q4. Why use base.extend()?

Answer:

`base.extend()` is used to create custom fixtures by extending Playwright's built-in test functionality.

---

# Final Framework Structure

```
Test

 |
 ↓

Custom Fixture

 |
 ↓

Page Object Class

 |
 ↓

BasePage

 |
 ↓

Playwright API
```

Fixtures + POM + BasePage together create a scalable Playwright automation framework.
