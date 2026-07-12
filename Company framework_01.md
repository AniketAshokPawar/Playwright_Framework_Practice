# Playwright Framework Architecture - Company Project Analysis

## Overview

Based on the Playwright configuration and project structure, the company is using a **Hybrid Playwright Automation Framework**.

The framework is built using:

- Playwright Test Runner
- Page Object Model (POM)
- Custom Fixtures
- Allure Reporting
- Environment-based execution
- Custom Playwright wrapper
- User pool management
- AWS region handling

---

# 1. Core Framework: Playwright Test Runner

## Technology Used

```
@playwright/test
```

Example:

```
@playwright/test v1.59.0
```

Playwright Test is the official testing framework provided by Microsoft.

It handles:

- Test execution
- Browser management
- Assertions
- Fixtures
- Parallel execution
- Retries
- Screenshots
- Videos
- Trace viewer

Example:

```typescript
import { test, expect } from '@playwright/test';

test("Login Test", async ({ page }) => {

    await page.goto("/login");

    await expect(page).toHaveTitle("Application");

});
```

Framework base:

```
Playwright Test Runner
```

---

# 2. Design Pattern: Page Object Model (POM)

The project follows the **Page Object Model design pattern**.

## Why POM?

POM separates:

- Test logic
- Page locators
- Page actions

This improves:

- Code reusability
- Maintainability
- Readability
- Easy updates when UI changes

---

## Typical Structure

```
project

├── tests
│   └── login.spec.ts
│
├── page-objects
│   └── LoginPage.ts
│
├── fixtures
│   └── base-test.ts
│
├── utils
│
├── config
│
└── playwright.config.ts
```

---

## Example Page Object

LoginPage.ts

```typescript
export class LoginPage {

constructor(private page){}


username = this.page.getByLabel("Username");

password = this.page.getByLabel("Password");


async login(username,password){

await this.username.fill(username);

await this.password.fill(password);

}

}
```

---

## Test File

login.spec.ts

```typescript
test("Login Test", async({page})=>{

const loginPage = new LoginPage(page);

await loginPage.login(
"admin",
"password"
);

});
```

---

# 3. Custom Fixture Framework

The project uses:

```
Custom base-test fixtures
```

Fixtures are one of the important Playwright features.

They allow dependency injection.

Instead of creating objects manually:

```typescript
const loginPage = new LoginPage(page);
```

Framework automatically provides page objects.

---

## Example Without Fixture

```typescript
test("Login", async({page})=>{

const loginPage = new LoginPage(page);

await loginPage.login();

});
```

---

## Example With Fixture

```typescript
test("Login", async({loginPage})=>{

await loginPage.login();

});
```

---

## Fixture Flow

```
Test Case

    |
    ↓

Custom Fixture

    |
    ↓

Create Page Objects

    |
    ↓

Inject Objects into Test
```

Advantages:

- Less duplicate code
- Cleaner tests
- Better scalability
- Central object management

---

# 4. Reporting Framework: Allure

The project uses:

```
allure-playwright
```

for test reporting.

---

## Reporting Flow

```
Test Execution

       ↓

Playwright

       ↓

Allure Adapter

       ↓

Allure HTML Report
```

---

## Allure Report Provides

- Test execution status
- Passed/failed tests
- Error details
- Screenshots
- Attachments
- Execution steps
- Test history

---

# 5. Custom Playwright Wrapper

The project uses:

```
run-playwright.js
```

as a custom execution wrapper.

Normal Playwright execution:

```
npx playwright test
```

Company execution:

```
run-playwright.js

        |

        ↓

npx playwright test
```

---

## Purpose of Custom Wrapper

The wrapper adds company-specific functionality.

Features:

- Multi-environment execution
- User pool management
- AWS region switching
- Custom reporting enhancements

---

# 6. Multi Environment Support

The framework supports multiple environments:

```
dev
qa
preprod
prod
```

---

## Example

Instead of changing code:

```
Application URL
Database
Credentials
Configuration
```

The framework loads environment-specific values.

Example:

```
ENV=qa
```

Loads:

```
qa configuration
```

---

## Benefits

- Same test code for multiple environments
- Easy deployment testing
- Less configuration changes

---

# 7. User Pool Management

The framework manages multiple test users.

Example:

```
users.json

{
 "admin":
 {
    "username":"admin_user"
 },

 "tester":
 {
    "username":"tester_user"
 }
}
```

---

Benefits:

- Avoid hardcoded users
- Supports parallel execution
- Easy user rotation
- Better test isolation

---

# 8. AWS Region Switching

The framework supports AWS region-based execution.

Example:

```
Regions:

US

EU

APAC
```

The framework handles:

```
Region

   ↓

Environment

   ↓

Configuration

   ↓

Test Execution
```

---

Benefits:

- Validate application in multiple regions
- Reduce manual configuration
- Support global deployments

---

# 9. Final Framework Classification

The company framework can be classified as:

```
================================================

        Hybrid Playwright Framework

================================================


Core Framework:

    Playwright Test Runner


Design Pattern:

    Page Object Model (POM)


Architecture:

    Custom Fixtures
    Page Object Injection


Reporting:

    Allure Reporting


Execution:

    Custom Playwright Wrapper


Configuration:

    Multi Environment Support


Additional Features:

    User Pool Management
    AWS Region Switching


================================================
```

---

# 10. Interview Answer

## Question:
"What Playwright framework have you worked on?"

## Answer:

I have worked on a **Hybrid Playwright Automation Framework** built using the Playwright Test Runner.

The framework follows the **Page Object Model architecture** with custom fixtures for dependency injection and page object management.

It uses **Allure reporting** for execution reports and supports multiple environments like dev, QA, pre-production, and production.

The framework also includes a custom execution wrapper for handling environment configuration, user pool management, AWS region switching, and customized reporting.

---

# Framework Learning Mapping

To understand this framework completely, learn in this order:

```
Playwright Core
        |
        ↓
Page Object Model
        |
        ↓
Custom Fixtures
        |
        ↓
Test Data Management
        |
        ↓
Configuration Management
        |
        ↓
Reporting (Allure)
        |
        ↓
API Integration
        |
        ↓
CI/CD Pipeline
        |
        ↓
Complete Hybrid Framework
```
