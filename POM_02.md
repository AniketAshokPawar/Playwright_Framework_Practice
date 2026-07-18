
# Page Object Model (POM) in Playwright with TypeScript
-------------------------------------------------------
## What is Page Object Model (POM)?

Page Object Model is a design pattern used in automation testing where each application page is represented as a separate class.

Each Page Class contains:

- Locators of that page
- Actions performed on that page
- Reusable methods

The main purpose of POM is to separate:



Test Logic\
|\
↓\
Page Object Class\
|\
↓\
Locators + Actions



# Why do we use POM?
--------------------
## Without POM

All locators and actions are written inside the test file.

Example:

```ts
await page.locator("#username").fill("admin");

await page.locator("#password").fill("admin123");

await page.locator("#loginBtn").click();
```

Problems:

-   Test files become lengthy.
-   Same locators are repeated in multiple tests.
-   UI changes require updating many test files.
-   Code maintenance becomes difficult.

* * * * *

With POM
--------

Test only calls reusable methods.

Example:

```
await loginPage.login("admin", "admin123");
```

The Page Class handles:

-   Finding elements
-   Filling values
-   Clicking buttons
-   Page-specific actions

Benefits:

-   Code reusability
-   Easy maintenance
-   Cleaner test files
-   Better framework structure

* * * * *

POM Folder Structure
====================

Example:

```
PlayWright Practice
│
├── pages
│   ├── LoginPage.ts
│   └── StudentRegistrationPage.ts
│
├── tests
│   └── POM_tests
│       ├── test_without_POM.spec.ts
│       └── test_with_POM.spec.ts
```

* * * * *

Understanding Class and Object in POM
=====================================

Class
-----

A class is a blueprint.

Example:

```
export class LoginPage {

}
```

The class only defines:

-   Properties
-   Methods

No object is created yet.

* * * * *

Object
------

To use a class, we create an object.

Example:

```
const loginPage = new LoginPage(page);
```

Now the object contains:

```
loginPage object

|
|-- page
|-- username locator
|-- password locator
|-- login button locator
|-- login() method
```

* * * * *

Constructor in POM
==================

Constructor is called automatically when an object is created.

Example:

```
const loginPage = new LoginPage(page);
```

This calls:

```
constructor(page: Page){

}
```

The constructor receives the Playwright page object and initializes the locators.

* * * * *

LoginPage.ts
============

```
import { Page, Locator } from "@playwright/test";

export class LoginPage {

    page: Page;

    username: Locator;
    password: Locator;
    loginBtn: Locator;

    constructor(page: Page){

        this.page = page;

        this.username = this.page.locator("#username");

        this.password = this.page.locator("#password");

        this.loginBtn = this.page.locator("#loginBtn");

    }

    async login(username:string, password:string){

        await this.username.fill(username);

        await this.password.fill(password);

        await this.loginBtn.click();

    }

}
```

* * * * *

Explanation of LoginPage.ts
===========================

Import
------

```
import { Page, Locator } from "@playwright/test";
```

Page:

-   Represents the browser page.

Locator:

-   Represents web elements.

* * * * *

Class Creation
--------------

```
export class LoginPage
```

Creates a Page Object blueprint.

* * * * *

Properties
----------

```
username: Locator;
password: Locator;
loginBtn: Locator;
```

These store the locators of the Login page.

* * * * *

Constructor
-----------

```
constructor(page: Page){

    this.page = page;

}
```

Receives the Playwright page object from the test.

* * * * *

Locator Initialization
----------------------

Example:

```
this.username = this.page.locator("#username");
```

The locator is stored inside the Page Object.

Now test files don't need to know:

```
#username
#password
#loginBtn
```

* * * * *

Login Method
------------

```
async login(username:string, password:string){

    await this.username.fill(username);

    await this.password.fill(password);

    await this.loginBtn.click();

}
```

This method performs the complete login action.

* * * * *

StudentRegistrationPage.ts
==========================

```
import {Page, Locator} from '@playwright/test';

export class StudentRegistrationPage {

    page: Page;

    registerStudentBtn: Locator;
    studentName: Locator;
    email: Locator;
    mobile: Locator;
    course: Locator;
    country: Locator;
    terms: Locator;
    registerBtn: Locator;

    constructor(page: Page){

        this.page = page;

        this.registerStudentBtn =
        this.page.locator("//button[text()='Register Student']");

        this.studentName =
        this.page.locator("#studentName");

        this.email =
        this.page.locator("#email");

        this.mobile =
        this.page.locator("#mobile");

        this.course =
        this.page.locator("#course");

        this.country =
        this.page.locator("#country");

        this.terms =
        this.page.locator("#terms");

        this.registerBtn =
        this.page.locator("#registerBtn");

    }

    async registerStudent(
        studentName:string,
        email:string,
        mobile:string,
        gender:string,
        course:string,
        country:string,
        terms:boolean
    ){

        await this.registerStudentBtn.click();

        await this.studentName.fill(studentName);

        await this.email.fill(email);

        await this.mobile.fill(mobile);

        await this.page
            .locator(`#${gender.toLowerCase()}`)
            .check();

        await this.course.selectOption(course);

        await this.country.selectOption(country);

        if(terms){

            await this.terms.check();

        }

        await this.registerBtn.click();

    }

}
```

* * * * *

Dynamic Locator Example
=======================

For gender selection:

Hardcoded approach
------------------

```
this.gender = this.page.locator("#male");
```

Problem:

It only supports Male.

* * * * *

Dynamic approach
----------------

```
await this.page
.locator(`#${gender.toLowerCase()}`)
.check();
```

If:

```
gender = "Male"
```

It becomes:

```
#male
```

If:

```
gender = "Female"
```

It becomes:

```
#female
```

This makes the locator reusable.

* * * * *

Using Page Objects in Test
==========================

test_with_POM.spec.ts
---------------------

```
import {test, expect} from "@playwright/test";

import {LoginPage} from "../../pages/LoginPage";

import {StudentRegistrationPage}
from "../../pages/StudentRegistrationPage";

const loginPageUrl =
"file:///C:/Users/z00575dy/OneDrive - Siemens AG/Desktop/PlayWright Practice/POM_Webpages/login.html";

test("Student Registration", async({page})=>{

    await page.goto(loginPageUrl);

    const loginPage = new LoginPage(page);

    await loginPage.login(
        "admin",
        "admin123"
    );

    await expect(page.locator("#welcomeMsg"))
    .toContainText("Welcome Admin");

    const studentPage =
    new StudentRegistrationPage(page);

    await studentPage.registerStudent(
        "Aniket",
        "aniket@example.com",
        "1234567890",
        "Male",
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

* * * * *

Execution Flow
==============

```
Test File

    |
    ↓

Create LoginPage Object

    |
    ↓

LoginPage Constructor

    |
    ↓

Initialize Login Locators

    |
    ↓

Call login()

    |
    ↓

Login Successful

    |
    ↓

Create StudentRegistrationPage Object

    |
    ↓

StudentRegistrationPage Constructor

    |
    ↓

Initialize Registration Locators

    |
    ↓

Call registerStudent()

    |
    ↓

Registration Successful
```

* * * * *

Important POM Rules
===================

Page Class should contain:
--------------------------

✅ Locators\
✅ Page actions\
✅ Reusable methods

Examples:

```
login()

registerStudent()

searchStudent()
```

* * * * *

Test File should contain:
-------------------------

✅ Test scenario\
✅ Test data\
✅ Assertions

Avoid writing:

```
page.locator("#username")
```

inside test files.

* * * * *

Can one test use multiple Page Objects?
=======================================

Yes.

Example:

```
const loginPage = new LoginPage(page);

const studentPage = new StudentRegistrationPage(page);
```

A single end-to-end test can use multiple Page Objects.

Example:

```
LoginPage
     |
     ↓
DashboardPage
     |
     ↓
StudentRegistrationPage
```

* * * * *

Should Assertions be inside Page Object?
========================================

Generally:

No.

Page Object:

-   Contains actions

Test:

-   Contains validations

Example:

Page Object:

```
login()
```

Test:

```
expect(welcomeMessage)
```

* * * * *

POM Interview Questions
=======================

Q1. What is Page Object Model?
------------------------------

Answer:

Page Object Model is a design pattern where application pages are represented as classes containing locators and reusable actions. It improves code maintainability and reusability.

* * * * *

Q2. What is the advantage of POM?
---------------------------------

Answer:

-   Reduces code duplication
-   Improves maintenance
-   Separates test logic from page logic
-   Makes automation framework scalable

* * * * *

Q3. What is the purpose of constructor in POM?
----------------------------------------------

Answer:

Constructor receives the Playwright page object and initializes all page locators when the Page Object is created.

* * * * *

Q4. Can one test use multiple Page Objects?
-------------------------------------------

Answer:

Yes. A single business flow can involve multiple pages, so multiple Page Objects can be used in one test.

* * * * *

Summary
=======

POM separates:

```
Test Scenario

       ↓

Page Object Classes

       ↓

UI Locators + Actions
```

Using POM makes Playwright frameworks:

-   Cleaner
-   Reusable
-   Maintainable
-   Scalable
