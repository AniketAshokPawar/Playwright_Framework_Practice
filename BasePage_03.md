
# Base Page in Playwright Framework

## What is Base Page?

Base Page is a parent class that contains common reusable Playwright actions used by multiple Page Classes.

In POM, each page has its own class:

```

pages/

├── BasePage.ts\
├── LoginPage.ts\
└── StudentRegistrationPage.ts

```

Relationship:

```
          BasePage
              |
    ---------------------
    |                   |
LoginPage StudentRegistrationPage

```

`LoginPage` and `StudentRegistrationPage` inherit common methods from `BasePage`.

---

# Why do we need Base Page?

Without Base Page, every Page Class directly uses Playwright actions.

Example:

LoginPage.ts

```ts
await this.username.fill(username);
await this.password.fill(password);
await this.loginBtn.click();
```

StudentRegistrationPage.ts

```
await this.studentName.fill(name);
await this.registerBtn.click();
```

Problems:

-   Same actions are repeated in many files.
-   If we want to modify click/fill behavior, we need to update every page.
-   Maintenance becomes difficult in large frameworks.

* * * * *

Purpose of Base Page
====================

Base Page centralizes common technical actions.

Example:

If tomorrow we want to add:

-   Custom wait before click
-   Screenshot after action
-   Logging
-   Retry mechanism
-   Reporting

We only modify BasePage.

All Page Classes automatically get the updated behavior.

* * * * *

Base Page vs Page Classes
=========================

BasePage responsibility
-----------------------

BasePage handles:

-   How to click
-   How to fill
-   How to select dropdown
-   How to check checkbox/radio
-   How to wait
-   How to get text

Example:

```
click()
fill()
check()
selectOption()
```

* * * * *

Page Class responsibility
-------------------------

Page Classes handle:

-   Which element to interact with
-   Business actions

Example:

LoginPage:

```
login()
```

StudentRegistrationPage:

```
registerStudent()
```

* * * * *

Creating BasePage.ts
====================

```
import {Page, Locator} from '@playwright/test';

export class BasePage{

    page: Page;

    constructor(page: Page){

        this.page = page;

    }

    async fill(locator: Locator, text:string){

        await locator.fill(text);

    }

    async click(locator: Locator){

        await locator.click();

    }

    async selectOption(locator: Locator, option:string){

        await locator.selectOption(option);

    }

    async check(locator: Locator){

        await locator.check();

    }

    async uncheck(locator: Locator){

        await locator.uncheck();

    }

}
```

* * * * *

Inheritance in Base Page
========================

Page Classes extend BasePage.

Example:

```
export class LoginPage extends BasePage{

}
```

Meaning:

LoginPage gets all methods from BasePage.

It can use:

```
this.fill()

this.click()

this.check()
```

* * * * *

super() in Base Page
====================

When a child class extends a parent class, we use:

```
super(page);
```

Example:

```
constructor(page: Page){

    super(page);

}
```

Purpose of super()
------------------

`super()` calls the parent class constructor.

BasePage constructor:

```
constructor(page: Page){

    this.page = page;

}
```

When LoginPage creates an object:

```
const loginPage = new LoginPage(page);
```

Flow:

```
new LoginPage(page)

        |
        ↓

LoginPage constructor

        |
        ↓

super(page)

        |
        ↓

BasePage constructor

        |
        ↓

this.page = page
```

Now LoginPage can use:

```
this.page.locator()
```

* * * * *

LoginPage using BasePage
========================

Before BasePage:

```
await this.username.fill(username);

await this.password.fill(password);

await this.loginBtn.click();
```

After BasePage:

```
await this.fill(this.username, username);

await this.fill(this.password, password);

await this.click(this.loginBtn);
```

* * * * *

StudentRegistrationPage using BasePage
======================================

Example:

```
await this.click(this.registerStudentBtn);

await this.fill(
    this.studentName,
    studentName
);

await this.selectOption(
    this.course,
    course
);

await this.check(this.terms);

await this.click(this.registerBtn);
```

* * * * *

Important Rule
==============

Do not put business actions in BasePage.

Wrong:

```
async login(username,password){

}
```

because login belongs to LoginPage.

Wrong:

```
async registerStudent(){

}
```

because registration belongs to StudentRegistrationPage.

* * * * *

Correct:

BasePage:

```
click()
fill()
check()
selectOption()
wait()
```

Page Classes:

```
login()
registerStudent()
searchEmployee()
createUser()
```

* * * * *

BasePage Architecture
=====================

```
Test File
    |
    |
    ↓
Page Class
(LoginPage / StudentRegistrationPage)
    |
    |
    ↓
BasePage
(Common actions)
    |
    |
    ↓
Playwright APIs
(click, fill, check)
```

* * * * *

Advantages of Base Page
=======================

1\. Code Reusability
--------------------

Common actions are written once.

Example:

```
click()
fill()
check()
```

can be reused by all pages.

2\. Easy Maintenance
--------------------

Change once:

```
BasePage.ts
```

instead of changing:

```
LoginPage.ts
DashboardPage.ts
EmployeePage.ts
ReportPage.ts
```

3\. Cleaner Page Classes
------------------------

Page Classes focus only on business flow.

Example:

```
login()
registerStudent()
```

instead of low-level Playwright code.

4\. Scalable Framework
----------------------

For large projects with hundreds of pages, BasePage keeps the framework organized.

* * * * *

Common Methods Usually Added in BasePage
========================================

Typical company frameworks contain:

```
click()

fill()

selectOption()

check()

uncheck()

waitForElement()

getText()

isVisible()

hover()

screenshot()

pressKey()
```

* * * * *

Key Interview Questions
=======================

Q1. Why do we use BasePage in Playwright?
-----------------------------------------

Answer:

BasePage is used to maintain common reusable Playwright actions in one place. It reduces code duplication and improves framework maintainability.

Q2. What is the difference between BasePage and Page Classes?
-------------------------------------------------------------

Answer:

BasePage contains common technical operations like click, fill, wait, and check.

Page Classes contain page-specific locators and business workflows.

Q3. Why do we use extends BasePage?
-----------------------------------

Answer:

`extends` provides inheritance. It allows Page Classes to reuse common methods from BasePage.

Q4. Why is super(page) required?
--------------------------------

Answer:

`super(page)` initializes the parent class constructor and passes the Playwright Page object from the child class to BasePage.

Q5. Does BasePage remove all code from Page Classes?
----------------------------------------------------

Answer:

No.

BasePage removes duplicate technical actions, not business logic.

Page Classes still contain page-specific actions like login, registration, and search.
