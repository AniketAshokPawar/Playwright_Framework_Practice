# CI/CD Pipeline (GitLab) - Playwright Notes

## What is CI/CD?

**CI (Continuous Integration)**
- Frequently integrate code into a shared repository.
- Every code change is validated by running automated tests.

**CD (Continuous Delivery/Deployment)**
- After successful validation, the application can be delivered or deployed.

---

# Why CI/CD is used?

Without CI/CD:

Developer/Tester
↓
Push Code
↓
Nobody knows if automation is broken

With CI/CD:

Developer/Tester
↓
Push Code
↓
Pipeline Runs
↓
Automation Executes
↓
Reports Generated
↓
Merge Decision

---

# GitLab Pipeline

A pipeline is an automated workflow that executes predefined steps whenever it is triggered.

Example:

```
Create MR
      ↓
Run Pipeline
      ↓
Execute Playwright Tests
      ↓
Generate Reports
      ↓
Merge
```

---

# Stage

A **stage** is a phase of the pipeline.

Example:

```yaml
stages:
  - test
```

Meaning:

```
Pipeline
   ↓
Test Stage
```

A project may contain multiple stages:

```yaml
stages:
  - build
  - test
  - deploy
```

Flow:

```
Build
  ↓
Test
  ↓
Deploy
```

---

# Important Doubt

### Is Stage same as Environment?

**No.**

| Stage | Environment |
|--------|-------------|
| Pipeline activity | Application location |
| build | dev |
| test | qa |
| deploy | prod |

Example:

```
Pipeline
   ↓
Test Stage
   ↓
Run Playwright Tests
   ↓
Against QA Environment
```

Stage = What to do

Environment = Where to do

---

# Job

A job is an individual task executed inside a stage.

Example:

```yaml
playwright-tests:
  stage: test
```

Here,

```
Stage
 ↓
test

Job
 ↓
playwright-tests
```

Job name can be anything:

- playwright-tests
- automation-tests
- smoke-tests

---

# Script

Script contains commands executed by GitLab Runner.

Example:

```yaml
script:
  - npm install
  - npx playwright install
  - npx playwright test
```

Meaning:

1. Install dependencies
2. Install Playwright browsers
3. Execute Playwright tests

Same commands we run locally.

---

# Artifacts

Artifacts are files generated during pipeline execution and stored by GitLab after the job finishes.

Examples:

- HTML Report
- Allure Report
- Screenshots
- Videos
- Traces

Example:

```yaml
artifacts:
  when: always

  paths:
    - playwright-report/
    - allure-results/
    - test-results/
```

Meaning:

After execution, GitLab saves these folders so they can be downloaded later.

---

# Why Artifacts?

Without artifacts:

```
Pipeline Failed
```

No information available.

With artifacts:

```
Pipeline Failed
      ↓
Open Allure Report
      ↓
Download Screenshot
      ↓
Download Video
      ↓
Download Trace
      ↓
Find Root Cause
```

---

# Company Workflow (My Project)

Current workflow:

```
Create Feature Branch
        ↓
Develop Automation Script
        ↓
Create Merge Request
        ↓
Reviewer Reviews Code
        ↓
Go to Pipeline
        ↓
New Pipeline
        ↓
Select Branch / MR Branch
        ↓
Run Pipeline
        ↓
Pipeline Pass
        ↓
Merge
```

After pipeline completion:

- Email with failed test list
- Allure Report link
- Artifacts
    - Screenshots
    - Videos
    - Traces

---

# Important Doubt

### Do we create a new pipeline for every script?

**No.**

We only trigger the **existing pipeline**.

Example:

Already present in repository:

```
.gitlab-ci.yml
```

When we click:

```
Pipeline
    ↓
New Pipeline
    ↓
Select Branch
    ↓
Run
```

GitLab executes the existing pipeline configuration on our selected branch.

We are **not creating a new pipeline configuration**.

---

# Industry Workflow

## Scenario 1 - MR Validation Pipeline ⭐⭐⭐⭐⭐

```
Feature Branch
      ↓
Create MR
      ↓
Pipeline
      ↓
Run Automation
      ↓
Pass
      ↓
Merge
```

Purpose:
Validate new code before merge.

---

## Scenario 2 - Scheduled Regression Pipeline ⭐⭐⭐⭐

```
Nightly Schedule
        ↓
Pipeline
        ↓
Execute Full Regression
        ↓
Generate Report
```

Purpose:
Validate complete application stability.

---

# Company vs Other Companies

### My Company

```
Create MR
      ↓
Reviewer
      ↓
Manually Run Pipeline
      ↓
Merge
```

### Many Companies

```
Create MR
      ↓
Pipeline Starts Automatically
      ↓
Review
      ↓
Merge
```

Both approaches are common in the industry.

---

# Final .gitlab-ci.yml

```yaml
stages:
  - test

playwright-tests:
  stage: test

  script:
    - npm install
    - npx playwright install
    - npx playwright test

  artifacts:
    when: always

    paths:
      - playwright-report/
      - allure-results/
      - test-results/
```

---

# Interview Questions

### What is CI/CD?

CI/CD automates code validation, testing and deployment.

---

### What is a pipeline?

A pipeline is an automated workflow that executes predefined jobs whenever it is triggered.

---

### What is a stage?

A stage is a phase of the pipeline such as build, test or deploy.

---

### What is a job?

A job is an individual task executed inside a stage.

---

### What is script?

The list of commands executed by GitLab Runner.

---

### What are artifacts?

Artifacts are output files generated during pipeline execution, such as HTML reports, Allure reports, screenshots, videos and traces.

---

### Explain your project's CI/CD workflow.

In our project, automation changes are developed on feature branches. After creating an MR and code review, a GitLab pipeline is manually triggered on the MR branch. The pipeline executes Playwright tests, generates Allure reports and uploads artifacts such as screenshots, videos and traces. Once the pipeline passes and approvals are complete, the MR is merged.
