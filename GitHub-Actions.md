
# GitHub Actions — Complete Study Notes

## 1. What is GitHub Actions?

**GitHub Actions** is a CI/CD automation platform built into GitHub.

It allows you to tell GitHub:

> "When something happens in my repository, automatically perform these tasks."

For example:

```text
Developer pushes code
        ↓
GitHub detects the push
        ↓
GitHub starts GitHub Actions
        ↓
Download code
        ↓
Install .NET
        ↓
Restore packages
        ↓
Build application
        ↓
Run tests
        ↓
Publish application
        ↓
Deploy application
```

Instead of doing all these tasks manually, GitHub does them automatically.

### Simple example

Without GitHub Actions:

```text
Developer
   ↓
Push code
   ↓
Manually pull code on server
   ↓
Manually install dependencies
   ↓
Manually build
   ↓
Manually test
   ↓
Manually deploy
```

With GitHub Actions:

```text
Developer
   ↓
git push
   ↓
GitHub Actions
   ↓
Build
   ↓
Test
   ↓
Publish
   ↓
Deploy
```

So the main idea is:

> **GitHub Actions = automation for your GitHub repository.**

---

# 2. Why Do We Need GitHub Actions?

Imagine you have a .NET Web API.

Every time you make a change, you need to:

1. Get the latest code.
2. Install dependencies.
3. Build the project.
4. Run tests.
5. Publish the application.
6. Copy it to a server.
7. Restart the application.

Doing this manually is:

* slow
* repetitive
* easy to forget
* easy to make mistakes
* difficult when many developers work together

GitHub Actions automates this process.

### Main problems it solves

| Problem                     | GitHub Actions solution    |
| --------------------------- | -------------------------- |
| Manual build                | Automatically build        |
| Manual testing              | Automatically run tests    |
| Manual deployment           | Automatically deploy       |
| Human mistakes              | Repeatable process         |
| Code breaks after changes   | CI catches problems        |
| Difficult deployments       | Automated CD               |
| Repeating commands          | Store commands in workflow |
| Need different environments | GitHub Environments        |
| Need secure credentials     | GitHub Secrets             |
| Need to save build output   | Artifacts                  |

---

# 3. What is CI/CD?

Before understanding GitHub Actions, understand **CI/CD**.

## CI = Continuous Integration

Continuous Integration means:

> Developers frequently put their code into a shared repository, and automated checks verify that the code works.

For example:

```text
Developer A ──┐
Developer B ──┼──→ GitHub
Developer C ──┘
                 ↓
              CI
                 ↓
             Build
                 ↓
              Tests
                 ↓
          Code is valid?
```

Suppose you change:

```csharp
public int Add(int a, int b)
{
    return a + b;
}
```

You push the code.

CI can automatically:

```text
Restore
   ↓
Build
   ↓
Test
   ↓
PASS / FAIL
```

If the build or tests fail, GitHub reports the failure.

---

# 4. What is CD?

CD can mean **Continuous Delivery** or **Continuous Deployment**, depending on the setup.

### Continuous Delivery

The application is automatically built, tested, and prepared for deployment.

A human may still approve the production deployment.

```text
Code
 ↓
Build
 ↓
Test
 ↓
Package
 ↓
Ready for production
 ↓
Human approval
 ↓
Deploy
```

### Continuous Deployment

The deployment also happens automatically.

```text
Code
 ↓
Build
 ↓
Test
 ↓
Package
 ↓
Deploy
 ↓
Production
```

### CI/CD together

```text
             CI                         CD
             │                          │
             ↓                          ↓
        Build + Test              Deploy application
             │                          │
Developer → GitHub →──────────────→ Production
```

---

# 5. How GitHub Actions Fits Into CI/CD

GitHub Actions is the tool that performs the automation.

```text
                    CI/CD
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
         CI                       CD
    Build + Test            Package + Deploy
          │                       │
          └───────────┬───────────┘
                      ↓
              GitHub Actions
```

GitHub Actions is **not the same thing as CI/CD**.

* **CI/CD** = the software-development automation process/practice.
* **GitHub Actions** = a platform/tool that can implement that process.

Other CI/CD tools include Jenkins, GitLab CI/CD, Azure DevOps Pipelines, etc.

---

# 6. What is a `.yml` / `.yaml` File?

`.yml` and `.yaml` are two extensions for the same YAML format.

YAML is a human-readable way of writing configuration.

Example:

```yaml
name: My Workflow

on:
  push:
    branches:
      - main
```

YAML is commonly used because it is easy for humans to read.

GitHub Actions uses YAML files to describe workflows.

---

# 7. Where is the GitHub Actions YAML File Stored?

Inside your GitHub repository:

```text
your-project/
│
├── .github/
│   └── workflows/
│       └── dotnet-ci.yml
│
├── MyApi/
│   ├── Program.cs
│   └── MyApi.csproj
│
└── README.md
```

The important location is:

```text
.github/workflows/
```

Every workflow YAML file inside this directory can define a GitHub Actions workflow.

Example:

```text
.github/workflows/ci.yml
.github/workflows/deploy.yml
.github/workflows/security.yml
```

You can have multiple workflows.

---

# 8. The Main GitHub Actions Hierarchy

This is the most important structure to understand:

```text
GitHub Repository
       │
       ↓
   Workflow
       │
       ↓
     Event
       │
       ↓
      Job
       │
       ↓
    Runner
       │
       ↓
     Steps
       │
       ↓
 Action / Command
       │
       ├── Build
       ├── Test
       ├── Publish
       └── Deploy
```

But technically, the **event triggers the workflow**.

A better mental model is:

```text
Repository
    │
    │ contains
    ↓
Workflow
    │
    │ waits for
    ↓
Event
    │
    │ triggers
    ↓
Workflow Run
    │
    ├───────────────┐
    ↓               ↓
  Job 1            Job 2
    │               │
  Runner          Runner
    │               │
  Steps           Steps
```

---

# 9. What is a Workflow?

A **workflow** is an automated process defined in a YAML file.

Example:

```text
.github/workflows/dotnet.yml
```

It might say:

```text
When code is pushed to main:

    Checkout code
    ↓
    Setup .NET
    ↓
    Restore
    ↓
    Build
    ↓
    Test
    ↓
    Publish
```

So:

> **Workflow = complete automation definition.**

Think of a workflow as a **recipe**.

Example real-world analogy:

```text
Recipe
  ↓
Step 1: Prepare ingredients
  ↓
Step 2: Cook
  ↓
Step 3: Check food
  ↓
Step 4: Serve
```

GitHub Actions:

```text
Workflow
  ↓
Step 1: Checkout
  ↓
Step 2: Restore
  ↓
Step 3: Build
  ↓
Step 4: Test
  ↓
Step 5: Deploy
```

---

# 10. What is an Event / Trigger?

An **event** is something that happens in GitHub.

Examples:

```text
push
pull_request
workflow_dispatch
schedule
release
```

The event tells GitHub:

> "Start this workflow when this happens."

For example:

```yaml
on:
  push:
    branches:
      - main
```

Meaning:

> Run this workflow when code is pushed to the `main` branch.

---

# 11. Event vs Trigger

These words are often used almost interchangeably, but it helps to think about them like this:

### Event

The thing that happened.

```text
Someone pushed code
```

### Trigger

The rule that says:

```text
When that event happens,
start this workflow.
```

Example:

```yaml
on:
  push:
    branches:
      - main
```

Here:

```text
Event = push

Trigger rule = push to main
```

---

# 12. Common GitHub Actions Events

## `push`

Runs when commits are pushed.

```yaml
on:
  push:
    branches:
      - main
```

---

## `pull_request`

Runs when a pull request is created or updated.

```yaml
on:
  pull_request:
    branches:
      - main
```

This is very useful for CI.

```text
Developer creates PR
        ↓
GitHub Actions
        ↓
Build
        ↓
Test
        ↓
PASS
```

The PR can then be reviewed knowing automated checks passed.

---

## `workflow_dispatch`

Allows you to manually start a workflow.

```yaml
on:
  workflow_dispatch:
```

Useful when you want:

```text
GitHub
  ↓
Actions
  ↓
Select workflow
  ↓
Run workflow
```

---

## `schedule`

Runs on a schedule using cron.

Example:

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

This can run a workflow periodically.

---

# 13. What Does `branches` Mean?

Example:

```yaml
on:
  push:
    branches:
      - main
```

`branches` filters which branches can trigger the workflow.

For example:

```text
push → main      → workflow runs
push → develop   → workflow does not run
push → feature/x → workflow does not run
```

---

# 14. What is a Job?

A **job** is a group of steps that are executed together on the same runner.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - ...
      - ...
      - ...
```

Here:

```text
Job = build
```

A workflow can contain multiple jobs.

```text
Workflow
   │
   ├── Build Job
   │
   ├── Test Job
   │
   └── Deploy Job
```

By default, independent jobs can run in parallel.

---

# 15. What is a Step?

A **step** is one individual operation inside a job.

Example:

```yaml
steps:
  - uses: actions/checkout@v5

  - uses: actions/setup-dotnet@v5
    with:
      dotnet-version: "8.0.x"

  - run: dotnet restore

  - run: dotnet build

  - run: dotnet test
```

There are four conceptual operations here:

```text
Step 1 → Checkout
Step 2 → Setup .NET
Step 3 → Restore
Step 4 → Build
Step 5 → Test
```

---

# 16. Workflow vs Job vs Step

This is one of the most important differences.

```text
WORKFLOW
│
├── JOB 1
│   ├── Step 1
│   ├── Step 2
│   └── Step 3
│
└── JOB 2
    ├── Step 1
    ├── Step 2
    └── Step 3
```

Think about a company:

```text
Company
   ↓
Department
   ↓
Employee tasks
```

Similarly:

```text
Workflow
   ↓
Job
   ↓
Step
```

### Remember

* **Workflow** = complete automation process
* **Job** = major unit of work
* **Step** = individual task inside a job

---

# 17. What is a Runner?

A **runner** is the machine that actually executes your job.

This is extremely important.

GitHub Actions does not magically execute:

```bash
dotnet build
```

Some computer has to execute that command.

That computer is the **runner**.

```text
GitHub
   │
   │ sends job
   ↓
Runner Machine
   │
   ├── Checkout
   ├── .NET
   ├── dotnet restore
   ├── dotnet build
   └── dotnet test
```

Example:

```yaml
runs-on: ubuntu-latest
```

This means:

> Run this job on a GitHub-hosted Ubuntu runner.

---

# 18. Runner vs Job

These are easy to confuse.

### Job

Defines **what work should be done**.

```yaml
build:
  runs-on: ubuntu-latest
```

### Runner

The machine that **actually performs the work**.

```text
Job
 │
 │ assigned to
 ↓
Runner
 │
 ↓
Commands execute
```

So:

> **Job = work instructions**

> **Runner = computer performing those instructions**

---

# 19. GitHub-Hosted vs Self-Hosted Runners

## GitHub-hosted runner

GitHub provides the machine.

Example:

```yaml
runs-on: ubuntu-latest
```

GitHub creates/provides the runner environment for the job.

Advantages:

* Easy setup
* No server maintenance
* Many operating-system options
* Good for normal CI/CD

---

## Self-hosted runner

You provide and manage the machine.

```text
Your Server
     ↑
Self-hosted GitHub Actions Runner
```

Useful when:

* you need special hardware
* you need private network access
* you need custom software
* company policy requires your own machines

But you must manage:

* updates
* security
* networking
* machine availability

---

# 20. What is an Action?

An **Action** is a reusable piece of automation.

Example:

```yaml
uses: actions/checkout@v5
```

`actions/checkout` is an action.

It performs the work of checking your repository code out onto the runner.

Another action:

```yaml
uses: actions/setup-dotnet@v5
```

This sets up the .NET environment.

Think:

```text
Action = reusable automation component
```

Instead of writing everything yourself, you use existing actions.

---

# 21. Action vs Command

Another important difference.

### Command

A command is something you ask the runner's shell to execute.

Example:

```yaml
run: dotnet build
```

The runner executes:

```bash
dotnet build
```

### Action

An action is a reusable automation component.

Example:

```yaml
uses: actions/checkout@v5
```

### Simple comparison

```text
run:
    "Execute this command"

uses:
    "Use this reusable action"
```

Examples:

```yaml
- run: dotnet restore
- run: dotnet build
- run: dotnet test
```

versus:

```yaml
- uses: actions/checkout@v5
- uses: actions/setup-dotnet@v5
```

---

# 22. What is `uses`?

`uses` tells GitHub:

> Use an existing Action.

Example:

```yaml
- uses: actions/checkout@v5
```

Structure:

```text
actions/checkout
       │
       ↓
    Action
       │
       ↓
     @v5
       │
       ↓
    Version
```

Another example:

```yaml
- uses: actions/setup-dotnet@v5
```

---

# 23. What is `run`?

`run` executes a shell command on the runner.

Example:

```yaml
- run: dotnet restore
```

Equivalent idea:

```text
Runner opens shell
       ↓
Executes:
dotnet restore
```

You can also execute multiple commands:

```yaml
- run: |
    dotnet restore
    dotnet build --no-restore
    dotnet test --no-build
```

The `|` means the following lines are part of a multi-line command block.

---

# 24. What is `with`?

`with` provides input/configuration to an Action.

Example:

```yaml
- uses: actions/setup-dotnet@v5
  with:
    dotnet-version: "8.0.x"
```

Think:

```text
Use setup-dotnet
        ↓
Give it:
dotnet-version = 8.0.x
```

Another example:

```yaml
- uses: actions/checkout@v5
  with:
    fetch-depth: 0
```

---

# 25. What is `runs-on`?

`runs-on` tells GitHub which runner environment should execute the job.

Example:

```yaml
runs-on: ubuntu-latest
```

Meaning:

```text
Job
 ↓
GitHub-hosted Ubuntu runner
```

Other examples can include Windows or macOS runners.

---

# 26. What is `if`?

`if` allows conditional execution.

Example:

```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: ./deploy.sh
```

Meaning:

```text
Is current branch main?
       │
   ┌───┴───┐
  YES      NO
   ↓        ↓
Deploy    Skip
```

You can also use conditions between jobs.

---

# 27. What is `needs`?

`needs` creates a dependency between jobs.

Suppose:

```text
Build
  ↓
Test
  ↓
Deploy
```

You can write:

```yaml
jobs:
  build:
    ...

  test:
    needs: build
    ...

  deploy:
    needs: test
    ...
```

Meaning:

```text
Build must finish successfully
        ↓
Test can start
        ↓
Test must finish successfully
        ↓
Deploy can start
```

Without `needs`, independent jobs may run in parallel.

---

# 28. Multiple Jobs

Example:

```text
Workflow
   │
   ├──────────────┐
   ↓              ↓
Build           Security Scan
   │              │
   └──────┬───────┘
          ↓
        Test
          ↓
       Deploy
```

YAML:

```yaml
jobs:

  build:
    ...

  security:
    ...

  test:
    needs: [build, security]
    ...

  deploy:
    needs: test
    ...
```

`test` waits for both:

```text
build
security
```

---

# 29. How Do Jobs Communicate?

Jobs normally run on separate runners.

Therefore, files created in Job 1 are not automatically available in Job 2.

For example:

```text
Job 1
Runner A
   ↓
publish/
```

Then:

```text
Job 1 finishes
Runner A is removed
```

Job 2 might get:

```text
Runner B
```

So we need a way to transfer data.

Common solutions include:

1. Artifacts
2. Job outputs
3. External storage/services

---

# 30. What is an Artifact?

An **artifact** is a file or collection of files produced by a workflow that you want to save and use later.

Example:

```text
dotnet publish
      ↓
publish/
      ↓
ZIP/package
      ↓
Upload artifact
```

The artifact could contain:

```text
MyApi.dll
MyApi.deps.json
MyApi.runtimeconfig.json
appsettings.json
other dependencies
```

Then another job can download it.

---

# 31. Build vs Artifact

These are not the same.

### Build

The process of converting source code into compiled output.

```text
Source code
    ↓
dotnet build
    ↓
Compiled output
```

### Artifact

Saved output from the workflow.

```text
Compiled/published output
        ↓
   Upload artifact
        ↓
   Stored by GitHub
```

So:

> **Build = process/output creation**

> **Artifact = saved file(s) that can be used later**

---

# 32. Artifact Flow

```text
BUILD JOB
   │
   ↓
dotnet publish
   │
   ↓
publish/
   │
   ↓
Upload Artifact
   │
   ↓
GitHub Artifact Storage
   │
   ↓
DOWNLOAD
   │
   ↓
DEPLOY JOB
   │
   ↓
Deployment
```

Example:

```yaml
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: api
    path: ./publish
```

Then later:

```yaml
- name: Download artifact
  uses: actions/download-artifact@v5
  with:
    name: api
    path: ./publish
```

---

# 33. What is a Secret?

A **Secret** is sensitive information that should not be stored directly in your source code.

Examples:

```text
Database password
Cloud API key
Deployment token
Cloud provider credentials
Private API key
```

Never do this:

```yaml
run: deploy --password=MyPassword123
```

Instead, store the value as a GitHub Secret.

Then reference it:

```yaml
${{ secrets.DEPLOY_PASSWORD }}
```

Example:

```yaml
env:
  DEPLOY_PASSWORD: ${{ secrets.DEPLOY_PASSWORD }}
```

---

# 34. Why Secrets Are Needed

Imagine your deployment requires:

```text
SERVER_PASSWORD
API_KEY
DATABASE_PASSWORD
```

If you put them inside:

```text
.github/workflows/deploy.yml
```

and commit the file:

```text
GitHub Repository
       ↓
Anyone with access to repository history
       ↓
May potentially see credentials
```

Instead:

```text
GitHub Secret
      ↓
Workflow
      ↓
Runner
      ↓
Use secret
```

The actual secret value should not be written into the YAML file.

---

# 35. Secret vs Environment Variable

These are related but different.

## Environment variable

A variable available to a process.

Example:

```yaml
env:
  ASPNETCORE_ENVIRONMENT: Production
```

This is configuration, not automatically a secret.

## Secret

A protected sensitive value.

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

### Important

An environment variable can contain a secret, but **not every environment variable is a secret**.

For example:

```text
ASPNETCORE_ENVIRONMENT=Production
```

is normally not sensitive.

While:

```text
DATABASE_PASSWORD=******
```

is sensitive.

---

# 36. What are GitHub Environments?

GitHub **Environments** represent deployment environments.

Typical environments:

```text
Development
     ↓
Staging
     ↓
Production
```

Each environment can have:

* environment-specific secrets
* environment-specific variables
* protection rules
* required approvals

Example:

```yaml
jobs:
  deploy:
    environment: production
```

This tells GitHub:

> This job is deploying to the `production` environment.

---

# 37. Secret vs Environment

Do not confuse:

```text
Environment
```

with:

```text
Environment variable
```

They are different.

### Environment

A GitHub deployment target/configuration.

```text
production
staging
development
```

### Environment variable

A key/value available to the application/job.

```text
ASPNETCORE_ENVIRONMENT=Production
```

---

# 38. Matrix Strategy

A matrix allows the same job to run with multiple combinations of values.

Suppose you want to test your application against:

```text
.NET 8
.NET 9
.NET 10
```

Instead of writing three jobs:

```yaml
strategy:
  matrix:
    dotnet: ["8.0.x", "9.0.x", "10.0.x"]
```

Then:

```yaml
with:
  dotnet-version: ${{ matrix.dotnet }}
```

GitHub creates multiple job runs.

```text
Matrix
  │
  ├── .NET 8
  ├── .NET 9
  └── .NET 10
```

This is useful for testing multiple versions, operating systems, databases, etc.

---

# 39. Reusable Workflows

Suppose your company has 20 repositories.

Every repository needs:

```text
Build
Test
Security scan
Publish
```

Instead of copying the same YAML into 20 repositories, you can create a reusable workflow.

```text
Reusable Workflow
       │
       ├── Repository A
       ├── Repository B
       ├── Repository C
       └── Repository D
```

This provides centralized CI/CD logic.

A workflow can be designed to be called by another workflow using:

```yaml
on:
  workflow_call:
```

---

# 40. Caching

Some dependencies take time to download.

For .NET:

```text
NuGet packages
```

If every workflow downloads everything from scratch:

```text
Workflow
   ↓
Download packages
   ↓
Build
```

This can be slow.

Caching stores reusable data so future workflow runs can be faster.

Conceptually:

```text
First run:
NuGet packages
     ↓
Download
     ↓
Cache

Next run:
Cache
  ↓
Reuse packages
  ↓
Faster
```

Caching is an **optimization**. It is not required for GitHub Actions to work.

---

# 41. Permissions

GitHub Actions may need permission to access GitHub resources.

For example:

```yaml
permissions:
  contents: read
```

This means the workflow gets read access to repository contents.

Permissions should follow the principle:

> Give only the permissions the workflow actually needs.

Avoid giving unnecessary write access.

For example:

```yaml
permissions:
  contents: read
```

is safer than giving broad write permissions when they are not needed.

---

# 42. Complete GitHub Actions Workflow

Now let's create a realistic CI workflow for an ASP.NET Core Web API.

Project:

```text
RealEstateApi/
│
├── RealEstateApi.sln
│
├── src/
│   └── RealEstateApi/
│       ├── Program.cs
│       ├── appsettings.json
│       └── RealEstateApi.csproj
│
└── tests/
    └── RealEstateApi.Tests/
        └── RealEstateApi.Tests.csproj
```

Workflow:

```yaml
name: .NET CI

on:
  push:
    branches:
      - main
      - develop

  pull_request:
    branches:
      - main
      - develop

permissions:
  contents: read

jobs:
  build-test-publish:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout repository
        uses: actions/checkout@v5

      - name: Setup .NET
        uses: actions/setup-dotnet@v5
        with:
          dotnet-version: "8.0.x"

      - name: Restore dependencies
        run: dotnet restore

      - name: Build
        run: dotnet build --no-restore --configuration Release

      - name: Run tests
        run: dotnet test --no-build --configuration Release

      - name: Publish application
        run: dotnet publish src/RealEstateApi/RealEstateApi.csproj \
          --configuration Release \
          --no-build \
          --output ./publish

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: realestate-api
          path: ./publish
```

---

# 43. Understanding the YAML Structure

The overall structure is:

```text
name
 │
 ↓
on
 │
 ↓
permissions
 │
 ↓
jobs
 │
 └── build-test-publish
       │
       ↓
    runs-on
       │
       ↓
     steps
       │
       ├── checkout
       ├── setup .NET
       ├── restore
       ├── build
       ├── test
       ├── publish
       └── artifact
```

---

# 44. Line-by-Line Explanation

## `name`

```yaml
name: .NET CI
```

This is the workflow's display name.

GitHub will show:

```text
.NET CI
```

in the Actions interface.

It does not control the actual execution.

---

## `on`

```yaml
on:
```

Defines when the workflow should run.

Think:

```text
WHEN should this workflow start?
```

---

## `push`

```yaml
on:
  push:
```

Means:

> Run when a push event occurs.

---

## `branches`

```yaml
branches:
  - main
  - develop
```

Means:

> Only trigger for pushes to `main` or `develop`.

---

## `pull_request`

```yaml
pull_request:
```

Means:

> Run when a pull request event occurs.

This helps catch problems before merging.

---

# 45. What Happens During a Pull Request?

Example:

```text
feature/property-search
          │
          │ Push
          ↓
       GitHub
          │
          ↓
   Create Pull Request
          │
          ↓
 GitHub Actions starts
          │
          ↓
       Build
          ↓
        Tests
          ↓
      PASS / FAIL
```

If tests fail:

```text
PR
 ↓
❌ CI failed
```

The developer can fix the problem before merging.

---

# 46. `permissions`

```yaml
permissions:
  contents: read
```

This gives the workflow read access to repository contents.

It follows a safer permission model.

---

# 47. `jobs`

```yaml
jobs:
```

This starts the job definitions.

A workflow must contain one or more jobs.

---

# 48. Job Name

```yaml
build-test-publish:
```

This is the job's ID.

It represents:

```text
Build + Test + Publish
```

You choose the ID.

---

# 49. `runs-on`

```yaml
runs-on: ubuntu-latest
```

The job will execute on an Ubuntu GitHub-hosted runner.

Conceptually:

```text
GitHub
  ↓
Create/provide Ubuntu runner
  ↓
Run this job
```

---

# 50. `steps`

```yaml
steps:
```

Defines the individual tasks of the job.

Everything underneath `steps` executes in order unless conditions or failures change the flow.

---

# 51. Checkout Step

```yaml
- name: Checkout repository
  uses: actions/checkout@v5
```

The runner initially does not simply have your project files sitting there.

The checkout action gets the repository content onto the runner.

Conceptually:

```text
GitHub Repository
       │
       ↓
Checkout Action
       │
       ↓
Runner workspace
       │
       ├── Program.cs
       ├── .csproj
       ├── Tests
       └── Other files
```

Now later commands can access your source code.

---

# 52. Setup .NET

```yaml
- name: Setup .NET
  uses: actions/setup-dotnet@v5
  with:
    dotnet-version: "8.0.x"
```

This prepares the .NET SDK required by the workflow.

Conceptually:

```text
Runner
  ↓
Setup .NET
  ↓
.NET SDK available
  ↓
dotnet commands can run
```

---

# 53. Restore

```yaml
- name: Restore dependencies
  run: dotnet restore
```

`dotnet restore` reads your project files and restores NuGet dependencies.

For example:

```text
Your API
  │
  ├── ASP.NET Core packages
  ├── Entity Framework Core
  ├── Npgsql
  └── Other NuGet packages
```

The required packages are downloaded/restored.

---

# 54. Build

```yaml
- name: Build
  run: dotnet build --no-restore --configuration Release
```

The project is compiled.

Conceptually:

```text
C# source code
      ↓
C# compiler
      ↓
Compiled .NET output
```

`--no-restore` means:

> Don't restore packages again because restore was already performed.

`--configuration Release` means build using the Release configuration.

---

# 55. Test

```yaml
- name: Run tests
  run: dotnet test --no-build --configuration Release
```

Runs automated tests.

Example:

```text
Build
  ↓
Tests
  ├── Login test
  ├── Property test
  ├── User test
  └── API test
```

If tests fail:

```text
❌ Job fails
```

Later deployment steps should normally not proceed when required checks fail.

---

# 56. Publish

```yaml
- name: Publish application
  run: dotnet publish src/RealEstateApi/RealEstateApi.csproj \
    --configuration Release \
    --no-build \
    --output ./publish
```

Publishing prepares the application for deployment.

The result is placed in:

```text
./publish
```

Conceptually:

```text
Source Code
    ↓
Build
    ↓
Test
    ↓
dotnet publish
    ↓
publish/
    ├── DLL
    ├── dependencies
    ├── configuration files
    └── runtime information
```

### Build vs Publish

```text
Build
 ↓
Checks/compiles the application

Publish
 ↓
Creates deployable application output
```

---

# 57. Upload Artifact

```yaml
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: realestate-api
    path: ./publish
```

This takes:

```text
./publish
```

and uploads it as a GitHub Actions artifact.

```text
Runner
  │
  ↓
./publish
  │
  ↓
Upload Artifact
  │
  ↓
GitHub Artifact Storage
```

Now another job can download it.

---

# 58. Complete CI Flow

The workflow we just created is:

```text
Developer
    │
    │ git push
    ↓
GitHub Repository
    │
    ↓
Push Event
    │
    ↓
GitHub Actions Workflow
    │
    ↓
Create Runner
    │
    ↓
Checkout Repository
    │
    ↓
Setup .NET
    │
    ↓
dotnet restore
    │
    ↓
dotnet build
    │
    ↓
dotnet test
    │
    ↓
dotnet publish
    │
    ↓
Upload Artifact
    │
    ↓
CI completed
```

At this point we have **CI**.

We have not yet deployed the application.

---

# 59. Adding Deployment

Now let's separate build/test from deployment.

A better production-style architecture can be:

```text
                 GitHub
                    │
                    ↓
                Workflow
                    │
             ┌──────┴──────┐
             ↓             ↓
          CI Job        Other checks
             │
             ↓
       Build + Test
             │
             ↓
          Publish
             │
             ↓
       Upload Artifact
             │
             ↓
       Deployment Job
             │
             ↓
        Download Artifact
             │
             ↓
       Deploy to Server
             │
             ↓
         Production
```

---

# 60. Complete CI/CD Example

Here is an example structure:

```yaml
name: .NET CI/CD

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

permissions:
  contents: read

jobs:

  build:
    name: Build and Test

    runs-on: ubuntu-latest

    steps:

      - name: Checkout repository
        uses: actions/checkout@v5

      - name: Setup .NET
        uses: actions/setup-dotnet@v5
        with:
          dotnet-version: "8.0.x"

      - name: Restore dependencies
        run: dotnet restore

      - name: Build
        run: dotnet build --no-restore --configuration Release

      - name: Test
        run: dotnet test --no-build --configuration Release

      - name: Publish
        run: |
          dotnet publish src/RealEstateApi/RealEstateApi.csproj \
            --configuration Release \
            --no-build \
            --output ./publish

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: realestate-api
          path: ./publish


  deploy:
    name: Deploy to Production

    needs: build

    runs-on: ubuntu-latest

    environment: production

    steps:

      - name: Download artifact
        uses: actions/download-artifact@v5
        with:
          name: realestate-api
          path: ./publish

      - name: Deploy application
        run: |
          echo "Deploying application..."
          # Actual deployment commands go here
```

The final deployment command depends on your hosting provider.

For example, it could deploy to:

```text
Azure
AWS
Google Cloud
Render
DigitalOcean
VM
Kubernetes
Docker-based server
etc.
```

---

# 61. Understanding `needs: build`

This:

```yaml
deploy:
  needs: build
```

means:

```text
Build Job
    │
    │ must succeed
    ↓
Deploy Job
```

Without it:

```text
Build ─────────→
                 Deploy
```

They may run independently.

With it:

```text
Build
  │
  ↓
Deploy
```

This prevents deployment from starting before the build job finishes successfully.

---

# 62. Complete Real-World Lifecycle

Now let's follow everything from the developer's computer.

## Step 1 — Developer writes code

Developer changes:

```text
PropertyController.cs
```

Maybe they add:

```text
GET /api/properties
```

---

## Step 2 — `git add`

Developer runs:

```bash
git add .
```

This puts changed files into Git's staging area.

```text
Working Directory
       ↓
    git add
       ↓
Staging Area
```

GitHub Actions has not started yet.

---

# 63. Step 3 — `git commit`

Developer runs:

```bash
git commit -m "Add property search API"
```

Now Git creates a commit locally.

```text
Working Directory
       ↓
Staging Area
       ↓
Commit
```

Still, GitHub Actions has not necessarily started because the commit is still local.

---

# 64. Step 4 — `git push`

Developer runs:

```bash
git push origin main
```

Now the commit is sent to GitHub.

```text
Developer Computer
       │
       │ git push
       ↓
GitHub Repository
```

This is where GitHub can detect the push event.

---

# 65. Step 5 — GitHub Detects Event

Workflow contains:

```yaml
on:
  push:
    branches:
      - main
```

GitHub sees:

```text
Push to main
```

The rule matches.

Therefore:

```text
Push Event
    ↓
Trigger Workflow
```

---

# 66. Step 6 — GitHub Creates a Workflow Run

GitHub starts an execution of the workflow.

Think:

```text
Workflow file
     ↓
Workflow Run #123
```

Every time the workflow runs, GitHub creates a workflow run with its own status and logs.

Example:

```text
.NET CI
   Run #123
   Run #124
   Run #125
```

---

# 67. Step 7 — GitHub Creates/Assigns Runner

For a GitHub-hosted runner:

```yaml
runs-on: ubuntu-latest
```

GitHub provides a suitable runner environment.

```text
Workflow Run
     ↓
Job
     ↓
Ubuntu Runner
```

The runner is where the commands actually execute.

---

# 68. Step 8 — Runner Checks Out Code

The first step:

```yaml
uses: actions/checkout@v5
```

gets the repository content into the runner's workspace.

```text
GitHub Repository
       ↓
Checkout
       ↓
Runner Workspace
```

---

# 69. Step 9 — .NET Is Prepared

```yaml
uses: actions/setup-dotnet@v5
```

The required .NET SDK is made available.

```text
Runner
   ↓
.NET SDK
   ↓
dotnet command available
```

---

# 70. Step 10 — Restore Dependencies

```bash
dotnet restore
```

NuGet dependencies are restored.

```text
.csproj
   ↓
NuGet dependencies
   ↓
Restore
   ↓
Packages available
```

---

# 71. Step 11 — Build

```bash
dotnet build
```

The source code is compiled.

```text
C# Source
   ↓
Compiler
   ↓
Compiled application
```

If compilation fails:

```text
❌ Build failed
```

The job normally stops.

---

# 72. Step 12 — Test

```bash
dotnet test
```

Automated tests run.

```text
Build
  ↓
Test
  ├── Unit tests
  ├── Integration tests
  └── Other automated tests
```

If a required test fails:

```text
❌ CI failed
```

Deployment should not happen.

---

# 73. Step 13 — Publish

```bash
dotnet publish
```

The application is prepared for deployment.

```text
Application
     ↓
dotnet publish
     ↓
publish/
```

---

# 74. Step 14 — Artifact Is Created

The published files are uploaded:

```text
publish/
   ↓
upload-artifact
   ↓
GitHub Artifact Storage
```

Now the deploy job can retrieve the same output.

This is useful because deployment should use the **already tested build output**, rather than building the application again.

---

# 75. Step 15 — Deployment Job Starts

Because:

```yaml
needs: build
```

GitHub waits for:

```text
Build Job = SUCCESS
```

Then:

```text
Deploy Job starts
```

---

# 76. Step 16 — Download Artifact

The deployment runner downloads:

```text
realestate-api
```

from GitHub's artifact storage.

```text
GitHub Artifact Storage
          ↓
    Download Artifact
          ↓
   Deployment Runner
```

---

# 77. Step 17 — Deployment

Now the deployment commands run.

Conceptually:

```text
Published application
       ↓
Deployment command
       ↓
Cloud/server
       ↓
ASP.NET Core application
       ↓
Production
```

The exact command depends on where you host the API.

---

# 78. Step 18 — Application Is Running

Finally:

```text
Internet
   ↓
Load Balancer / Reverse Proxy
   ↓
ASP.NET Core Web API
   ↓
Database
```

The new version is now running.

---

# 79. The Complete Picture

This is the complete lifecycle:

```text
┌─────────────────────────────┐
│ Developer Computer          │
│                             │
│ Write Code                  │
│     ↓                       │
│ git add                     │
│     ↓                       │
│ git commit                  │
│     ↓                       │
│ git push                    │
└──────────────┬──────────────┘
               │
               ↓
┌─────────────────────────────┐
│ GitHub Repository            │
│                             │
│ Push Event                  │
│       ↓                     │
│ Workflow Trigger            │
└──────────────┬──────────────┘
               │
               ↓
┌─────────────────────────────┐
│ GitHub Actions              │
│                             │
│ Workflow Run                │
│       ↓                     │
│ Job                         │
│       ↓                     │
│ Runner                      │
│       ↓                     │
│ Checkout                    │
│       ↓                     │
│ Setup .NET                  │
│       ↓                     │
│ Restore                     │
│       ↓                     │
│ Build                       │
│       ↓                     │
│ Test                        │
│       ↓                     │
│ Publish                     │
│       ↓                     │
│ Upload Artifact             │
└──────────────┬──────────────┘
               │
               ↓
┌─────────────────────────────┐
│ Artifact Storage            │
│                             │
│ Published API files         │
└──────────────┬──────────────┘
               │
               ↓
┌─────────────────────────────┐
│ Deployment Job              │
│                             │
│ Download Artifact            │
│       ↓                     │
│ Deploy                      │
└──────────────┬──────────────┘
               │
               ↓
┌─────────────────────────────┐
│ Production Server           │
│                             │
│ ASP.NET Core Web API        │
└─────────────────────────────┘
```

---

# 80. Why Artifact Instead of Building Again?

Consider:

```text
CI Job
  ↓
Build
  ↓
Test
  ↓
Publish
  ↓
Artifact
```

Then:

```text
Deploy Job
  ↓
Download same artifact
  ↓
Deploy
```

This is better than:

```text
CI Job
  ↓
Build
  ↓
Test

Deploy Job
  ↓
Build again
  ↓
Deploy
```

Why?

Because you want to deploy the **same output that passed testing**.

Conceptually:

```text
Code
 ↓
Build
 ↓
Test
 ↓
✅ Approved artifact
 ↓
Deploy EXACT artifact
```

---

# 81. Environment-Specific Deployment

A real company may have:

```text
Developer
    ↓
Pull Request
    ↓
CI
    ↓
Development
    ↓
Staging
    ↓
Production
```

Example:

```text
feature branch
      ↓
    CI test
      ↓
develop
      ↓
Development
      ↓
main
      ↓
Staging
      ↓
Approval
      ↓
Production
```

GitHub Environments can help protect production.

---

# 82. Secrets in Deployment

Suppose your deployment requires an API token.

Do not write:

```yaml
run: deploy --token=abc123
```

Instead:

```text
GitHub
  │
  └── Secrets
       │
       └── DEPLOY_TOKEN
```

Workflow:

```yaml
env:
  DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

Then the deployment tool can use the environment variable.

The important principle is:

```text
Secret
  ↓
GitHub secret storage
  ↓
Workflow
  ↓
Runner
  ↓
Deployment command
```

Do not print secrets into logs.

---

# 83. Important Secret Safety Rules

### Never hard-code secrets

Bad:

```yaml
run: echo "password123"
```

### Don't commit secrets

Bad:

```text
appsettings.json
```

containing production passwords.

### Use GitHub Secrets

```yaml
${{ secrets.MY_SECRET }}
```

### Don't unnecessarily print secrets

Avoid:

```yaml
run: echo "${{ secrets.MY_SECRET }}"
```

### Give minimum permissions

Only give the workflow the permissions it needs.

---

# 84. Job Dependency Diagram

Suppose you have:

```text
Build
  │
  ├──────────────┐
  ↓              ↓
Unit Tests     Security Scan
  │              │
  └──────┬───────┘
         ↓
       Deploy
```

YAML:

```yaml
jobs:

  build:
    ...

  unit-tests:
    needs: build
    ...

  security:
    needs: build
    ...

  deploy:
    needs: [unit-tests, security]
    ...
```

This means:

```text
Build
  ↓
┌───────────────┐
│               │
↓               ↓
Tests        Security
│               │
└───────┬───────┘
        ↓
      Deploy
```

Notice that tests and security can run in parallel.

---

# 85. `if` + `needs`

You can combine conditions and dependencies.

Example:

```yaml
deploy:
  needs: build
  if: github.ref == 'refs/heads/main'
```

Meaning:

```text
Did build succeed?
        ↓
Is branch main?
        ↓
   YES → Deploy
```

---

# 86. Matrix Example

Suppose your API needs testing on multiple operating systems:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest
```

Then:

```yaml
runs-on: ${{ matrix.os }}
```

GitHub effectively creates:

```text
Job
 │
 ├── Ubuntu
 │
 └── Windows
```

For multiple .NET versions:

```yaml
strategy:
  matrix:
    dotnet:
      - "8.0.x"
      - "9.0.x"
```

Result:

```text
             Test Job
                │
       ┌────────┴────────┐
       ↓                 ↓
    .NET 8             .NET 9
```

---

# 87. Reusable Workflow Diagram

```text
                 Reusable Workflow
                  Build + Test
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       Project A    Project B    Project C
```

This prevents copying the same CI configuration into every repository.

---

# 88. Caching Diagram

Without cache:

```text
Every run
   ↓
Download dependencies
   ↓
Build
```

With cache:

```text
First run
   ↓
Download
   ↓
Save cache

Later run
   ↓
Check cache
   ↓
Reuse
   ↓
Faster build
```

Caching improves speed; it is not a replacement for restore/build.

---

# 89. What Happens If a Step Fails?

Suppose:

```text
Checkout       ✅
Setup .NET     ✅
Restore        ✅
Build          ❌
Test           ⛔
Publish        ⛔
Deploy         ⛔
```

Normally, when a required step fails, later steps are skipped.

So:

```text
Build failed
     ↓
Job failed
     ↓
Dependent deploy job does not run
```

This is one of the most important CI/CD safety mechanisms.

---

# 90. What Happens If Tests Fail?

Example:

```text
Build
  ↓
SUCCESS
  ↓
Test
  ↓
FAIL
  ↓
CI FAILED
  ↓
Deployment blocked
```

This prevents broken code from automatically reaching production.

---

# 91. Workflow Run vs Workflow

Another useful distinction:

### Workflow

The YAML definition.

```text
.github/workflows/ci.yml
```

### Workflow Run

One execution of that workflow.

For example:

```text
ci.yml

Run #101
Run #102
Run #103
```

Every new triggering event can create another workflow run.

---

# 92. Complete Concept Map

Keep this diagram in your notes:

```text
GitHub Repository
       │
       │ contains
       ↓
Workflow YAML
       │
       │ defines
       ├───────────────┐
       ↓               ↓
   Events             Jobs
       │               │
       │               ↓
       │             Runner
       │               │
       │               ↓
       │             Steps
       │               │
       │        ┌──────┴──────┐
       │        ↓             ↓
       │      Action        Command
       │
       ↓
Workflow Run
       │
       ↓
Build / Test / Publish
       │
       ↓
Artifact
       │
       ↓
Deployment
       │
       ↓
Environment
       │
       ↓
Production
```

---

# 93. Most Important Keywords

| Keyword        | Meaning                                    |
| -------------- | ------------------------------------------ |
| `name`         | Name of workflow/job/step                  |
| `on`           | Defines events that trigger workflow       |
| `push`         | Trigger when code is pushed                |
| `pull_request` | Trigger for pull-request events            |
| `branches`     | Limits trigger to specific branches        |
| `jobs`         | Defines jobs                               |
| `runs-on`      | Selects runner                             |
| `steps`        | Defines individual tasks                   |
| `uses`         | Uses an Action                             |
| `run`          | Executes shell command                     |
| `with`         | Gives inputs to an Action                  |
| `env`          | Defines environment variables              |
| `secrets`      | Accesses GitHub Secrets                    |
| `needs`        | Creates job dependency                     |
| `if`           | Conditional execution                      |
| `strategy`     | Controls job strategy                      |
| `matrix`       | Runs combinations of values                |
| `permissions`  | Controls GitHub token permissions          |
| `environment`  | Associates job with deployment environment |

---

# 94. Easy Comparison Table

## Workflow vs Job vs Step

| Concept  | Meaning             |
| -------- | ------------------- |
| Workflow | Complete automation |
| Job      | Major unit of work  |
| Step     | Individual task     |

```text
Workflow
   ↓
Job
   ↓
Step
```

---

## Job vs Runner

| Concept | Meaning               |
| ------- | --------------------- |
| Job     | What needs to be done |
| Runner  | Machine that does it  |

```text
Job
 ↓
Runner
 ↓
Execute steps
```

---

## Action vs Command

| Concept | Example               | Meaning                          |
| ------- | --------------------- | -------------------------------- |
| Action  | `actions/checkout@v5` | Reusable automation              |
| Command | `dotnet build`        | Shell command executed by runner |

---

## Artifact vs Build

| Concept  | Meaning                   |
| -------- | ------------------------- |
| Build    | Compiling the application |
| Artifact | Saved output used later   |

```text
Build
 ↓
Output
 ↓
Artifact
```

---

## Event vs Trigger

| Concept | Meaning                                         |
| ------- | ----------------------------------------------- |
| Event   | Something happens                               |
| Trigger | Rule that starts workflow because of that event |

Example:

```text
Event:
push

Trigger:
push to main
```

---

## Secret vs Environment Variable

| Secret                    | Environment Variable        |
| ------------------------- | --------------------------- |
| Used for sensitive values | Used for configuration/data |
| Protected by GitHub       | Normal variable             |
| Example: API key          | Example: environment name   |

Important:

> An environment variable can contain a secret, but an environment variable is not automatically secret.

---

## CI vs CD

| CI                  | CD                             |
| ------------------- | ------------------------------ |
| Build               | Prepare/release                |
| Test                | Deploy                         |
| Validate code       | Deliver application            |
| Find problems early | Get application to environment |

```text
CI
Code → Build → Test
             ↓
CD
          Publish → Deploy
```

---

## GitHub Actions vs Workflow

| GitHub Actions                                   | Workflow                              |
| ------------------------------------------------ | ------------------------------------- |
| Automation platform                              | One automation definition             |
| Provides runners, actions, workflow engine, etc. | YAML file defining what should happen |
| Platform                                         | Configuration/process                 |

Think:

```text
GitHub Actions
     │
     ├── Workflow A
     ├── Workflow B
     └── Workflow C
```

---

# 95. A Real-World ASP.NET Core CI/CD Architecture

Imagine your project is:

```text
Real Estate Management Platform
             │
             ↓
      ASP.NET Core Web API
             │
       ┌─────┴─────┐
       ↓           ↓
 PostgreSQL      External APIs
```

Development flow:

```text
Developer
    │
    │ writes C# code
    ↓
Local project
    │
    │ git add
    ↓
Staging
    │
    │ git commit
    ↓
Local Git
    │
    │ git push
    ↓
GitHub
    │
    ↓
Pull Request / Push
    │
    ↓
GitHub Actions
    │
    ↓
┌──────────────────────┐
│ CI                   │
│                      │
│ Checkout             │
│ Setup .NET           │
│ Restore              │
│ Build                │
│ Test                 │
│ Publish              │
└──────────┬───────────┘
           │
           ↓
       Artifact
           │
           ↓
┌──────────────────────┐
│ CD                   │
│                      │
│ Download Artifact    │
│ Configure Secrets    │
│ Deploy                │
└──────────┬───────────┘
           │
           ↓
      Production
           │
           ↓
    ASP.NET Core API
           │
           ↓
       PostgreSQL
```

---

# 96. What GitHub Actions Is Actually Doing

It is useful to remove the confusing terminology.

At the simplest level, GitHub Actions is doing this:

```text
1. Wait for an event
        ↓
2. Read workflow instructions
        ↓
3. Start/assign a runner
        ↓
4. Give runner your repository
        ↓
5. Execute commands/actions
        ↓
6. Collect results
        ↓
7. Save artifacts if requested
        ↓
8. Run dependent jobs
        ↓
9. Deploy if configured
        ↓
10. Report success/failure
```

That is the core of GitHub Actions.

---

# 97. One Complete Mental Model

Imagine GitHub Actions as a factory.

```text
                FACTORY
                   │
                   ↓
              WORKFLOW
           "How factory works"
                   │
                   ↓
                 JOB
           "Major department"
                   │
                   ↓
                RUNNER
             "Worker machine"
                   │
                   ↓
                STEPS
          "Individual tasks"
                   │
          ┌────────┴────────┐
          ↓                 ↓
       ACTION             COMMAND
    Reusable tool       Shell command
          │                 │
          └────────┬────────┘
                   ↓
                OUTPUT
                   │
                   ↓
               ARTIFACT
             Saved product
                   │
                   ↓
              DEPLOYMENT
                   │
                   ↓
              PRODUCTION
```

And the event is the thing that says:

```text
"Start the factory now."
```

---

# 98. The Most Important Things to Remember

### 1. GitHub Actions

```text
Automation platform inside GitHub
```

### 2. Workflow

```text
YAML file describing automation
```

Stored in:

```text
.github/workflows/
```

### 3. Event

```text
Something happens in GitHub
```

Examples:

```text
push
pull_request
schedule
workflow_dispatch
```

### 4. Job

```text
Group of steps
```

### 5. Runner

```text
Machine that executes the job
```

### 6. Step

```text
One task inside a job
```

### 7. Action

```text
Reusable automation component
```

### 8. `run`

```text
Execute a shell command
```

### 9. Artifact

```text
Saved output from a workflow
```

### 10. Secret

```text
Sensitive value stored securely
```

### 11. Environment

```text
Deployment target/configuration such as staging or production
```

### 12. `needs`

```text
Defines job dependency
```

### 13. `if`

```text
Defines a condition
```

### 14. Matrix

```text
Run the same job with different combinations
```

### 15. CI/CD

```text
CI = Build + Test + Validate

CD = Deliver/Deploy
```

---

# 99. Final One-Page Diagram

If you remember only one diagram, remember this:

```text
                 DEVELOPER
                     │
                     │
              git add / commit
                     │
                     ↓
                  git push
                     │
                     ↓
              ┌─────────────┐
              │   GITHUB    │
              │ Repository  │
              └──────┬──────┘
                     │
                Push / PR
                     │
                     ↓
              ┌─────────────┐
              │   EVENT     │
              │  TRIGGER    │
              └──────┬──────┘
                     │
                     ↓
              ┌─────────────┐
              │  WORKFLOW   │
              │   .yml      │
              └──────┬──────┘
                     │
              ┌──────┴──────┐
              ↓             ↓
           JOB 1          JOB 2
          Build/Test      Security
              │             │
              ↓             ↓
           RUNNER         RUNNER
              │
              ↓
            STEPS
              │
       ┌──────┼────────┐
       ↓      ↓        ↓
   ACTION   COMMAND   ACTION
       │      │        │
       └──────┼────────┘
              ↓
          .NET CI
              │
       ┌──────┼──────┐
       ↓      ↓      ↓
    Restore  Build  Test
                     │
                     ↓
                  Publish
                     │
                     ↓
                  ARTIFACT
                     │
                     ↓
             Download Artifact
                     │
                     ↓
               DEPLOY JOB
                     │
                     ↓
              GitHub Secrets
                     │
                     ↓
               DEPLOYMENT
                     │
                     ↓
              ┌────────────┐
              │ Production │
              │ ASP.NET    │
              │ Core API   │
              └────────────┘
```

---

# 100. Final Mental Summary

The entire concept can be remembered as:

```text
EVENT
  ↓
WORKFLOW
  ↓
JOB
  ↓
RUNNER
  ↓
STEPS
  ↓
ACTIONS / COMMANDS
  ↓
BUILD
  ↓
TEST
  ↓
PUBLISH
  ↓
ARTIFACT
  ↓
DEPLOY
  ↓
ENVIRONMENT
  ↓
PRODUCTION
```

And for a .NET application:

```text
git push
   ↓
GitHub detects push
   ↓
Workflow starts
   ↓
Runner starts
   ↓
Checkout code
   ↓
Setup .NET
   ↓
dotnet restore
   ↓
dotnet build
   ↓
dotnet test
   ↓
dotnet publish
   ↓
Upload artifact
   ↓
Deploy job
   ↓
Download artifact
   ↓
Use required secrets
   ↓
Deploy
   ↓
ASP.NET Core API running in production
```

**The key idea:** GitHub Actions takes a process that a developer would normally perform manually and turns it into a **repeatable, automatic process defined as code in a YAML workflow**.

If you learn the concepts in this order — **Event → Workflow → Job → Runner → Step → Action/Command → Artifact → Deployment** — the GitHub Actions YAML becomes much easier to understand.
