# Azure, Azure DevOps, Docker, GitHub Actions, and CI/CD — Complete Beginner Guide

The **most important idea** to understand first is this:

> **Azure and AWS are cloud platforms where your application can run.**
>
> **Docker packages your application.**
>
> **GitHub Actions / Azure Pipelines automate the process of building, testing, packaging, and deploying it.**
>
> **Azure DevOps is a collection of development/DevOps tools, and Azure Pipelines is one of those tools.**

Once you understand this separation, most of the confusion disappears.

---

# 1. First: The Big Picture

Imagine you are building an ASP.NET Core Web API.

You write:

```text
ASP.NET Core Web API
        ↓
       Code
```

You need several different things:

```text
Where do I store my code?
        ↓
GitHub / Azure Repos

How do I automatically build and test it?
        ↓
GitHub Actions / Azure Pipelines

How do I package it?
        ↓
Docker

Where do I store the Docker image?
        ↓
Docker Hub / Azure Container Registry

Where does my application actually run?
        ↓
Azure / AWS / another cloud
```

So these technologies are **not all competitors**.

They solve **different problems**.

A useful mental model is:

```text
                    YOUR APPLICATION
                           │
                           ▼
                    Source Code
                           │
              ┌────────────┴────────────┐
              │                         │
           GitHub                  Azure Repos
              │                         │
              ▼                         ▼
       GitHub Actions            Azure Pipelines
              │                         │
              └────────────┬────────────┘
                           │
                     Build + Test
                           │
                           ▼
                        Docker
                           │
                           ▼
                     Docker Image
                           │
              ┌────────────┴────────────┐
              │                         │
          Docker Hub                  ACR
              │                         │
              └────────────┬────────────┘
                           │
                           ▼
                    Azure / AWS
                           │
                           ▼
                   Running Application
```

---

# 2. What Is Azure?

Microsoft Azure is Microsoft's **cloud computing platform**.

Think of Azure as a huge collection of computers, storage systems, databases, networking systems, and other infrastructure available over the internet.

Instead of buying your own physical server:

```text
You
 ↓
Buy physical server
 ↓
Install OS
 ↓
Configure networking
 ↓
Install .NET
 ↓
Deploy application
 ↓
Maintain server
```

you can use Azure:

```text
You
 ↓
Create Azure resource
 ↓
Deploy application
 ↓
Azure provides/manages infrastructure
```

Azure has many services.

For example:

```text
Azure App Service
→ Run web applications/APIs

Azure SQL
→ Database

Azure Storage
→ Store files/blobs

Azure Container Registry
→ Store Docker images

Azure Virtual Machines
→ Virtual servers

Azure Kubernetes Service
→ Run/manage Kubernetes

Azure Functions
→ Serverless functions
```

So:

> **Azure = a cloud platform containing many different services.**

---

# 3. What Is AWS?

Amazon Web Services, or **AWS**, is Amazon's cloud platform.

It provides similar categories of services.

For example:

| Requirement             | Azure           | AWS                                |
| ----------------------- | --------------- | ---------------------------------- |
| Virtual machine         | Azure VM        | EC2                                |
| Container registry      | ACR             | ECR                                |
| Managed web application | App Service     | Elastic Beanstalk / other services |
| Object storage          | Blob Storage    | S3                                 |
| Kubernetes              | AKS             | EKS                                |
| Serverless              | Azure Functions | Lambda                             |

Therefore:

```text
Azure
   ↕
AWS
```

are largely **alternatives**.

A company might choose Azure.

Another might choose AWS.

Some companies use **both**.

---

# 4. Why Do Companies Need Cloud?

Suppose you build an API.

You need a computer somewhere to run:

```text
.NET API
     ↓
Needs CPU
Needs RAM
Needs storage
Needs networking
Needs internet connectivity
```

You could buy a physical server.

But then you have to manage:

* hardware
* electricity
* networking
* operating system
* backups
* scaling
* availability
* security
* monitoring

Cloud providers such as Azure and AWS provide infrastructure and managed services so you don't have to build everything yourself.

---

# 5. What Is Azure DevOps?

Azure DevOps is a set of tools from Microsoft for helping teams **plan, develop, test, and deliver software**.

Important:

> **Azure DevOps is NOT the same thing as Azure.**

This is one of the most important distinctions.

```text
Azure
│
└── Cloud platform
    └── Runs applications/infrastructure

Azure DevOps
│
└── Software development/DevOps platform
    ├── Boards
    ├── Repos
    ├── Pipelines
    ├── Test Plans
    └── Artifacts
```

---

# 6. Azure DevOps Services

Azure DevOps has several major services.

## 6.1 Azure Boards

Used for project management.

For example:

```text
Backlog
   ↓
User Story
   ↓
Task
   ↓
Bug
```

Example:

```text
User Story:
"User should be able to reset password"

Tasks:
- Create API
- Create database table
- Create email service
- Create UI
- Write tests
```

Think:

> **Azure Boards = planning and tracking work**

---

# 7. Azure Repos

Azure Repos provides Git repositories.

For example:

```text
Developer
   ↓
git add
   ↓
git commit
   ↓
git push
   ↓
Azure Repos
```

It is similar in purpose to GitHub repositories.

So:

```text
GitHub Repository
       ↕
Azure Repo
```

Both can store Git source code.

---

# 8. Azure Pipelines

This is the part most relevant to your CI/CD question.

**Azure Pipelines** is used to automate:

```text
Build
Test
Package
Deploy
```

For example:

```text
Code pushed
     ↓
Azure Pipeline starts
     ↓
Restore
     ↓
Build
     ↓
Test
     ↓
Docker build
     ↓
Push image
     ↓
Deploy
```

So:

> **Azure Pipelines is Microsoft's CI/CD tool inside Azure DevOps.**

---

# 9. Azure Artifacts

Azure Artifacts is used to store and share **packages**.

For example:

```text
NuGet packages
npm packages
Maven packages
Python packages
```

Think:

```text
Docker image
→ Container Registry

NuGet/npm/etc. package
→ Azure Artifacts
```

These are different types of things.

---

# 10. Azure Test Plans

Azure Test Plans helps teams manage software testing.

For example:

```text
Test Case
    ↓
Execute test
    ↓
Pass / Fail
    ↓
Bug
```

This is more about structured/manual testing and test management.

---

# 11. Which Azure DevOps Services Are Related to CI/CD?

The most directly relevant one is:

```text
Azure Pipelines
```

But the services can work together:

```text
Azure Boards
     ↓
Work planning

Azure Repos
     ↓
Source code

Azure Pipelines
     ↓
CI/CD

Azure Test Plans
     ↓
Testing

Azure Artifacts
     ↓
Package storage
```

---

# 12. Azure vs Azure DevOps

This distinction is critical.

| Azure                     | Azure DevOps                         |
| ------------------------- | ------------------------------------ |
| Cloud platform            | DevOps/development platform          |
| Runs applications         | Helps build/test/deploy applications |
| Provides servers/services | Provides development tools           |
| App Service               | Azure Pipelines                      |
| Azure VM                  | Azure Repos                          |
| Azure SQL                 | Azure Boards                         |
| ACR                       | Azure Artifacts                      |

Simple version:

```text
Azure
=
WHERE your application can run

Azure DevOps
=
TOOLS that help you develop and deliver it
```

---

# 13. Now Compare Everything

Let's define each one in one sentence.

### GitHub

> A platform for hosting Git repositories and collaborating around source code.

### GitHub Actions

> A CI/CD automation system integrated with GitHub.

### Azure DevOps

> Microsoft's collection of software development and DevOps tools.

### Azure Pipelines

> Azure DevOps's CI/CD automation service.

### Docker

> A technology for packaging applications into containers.

### Azure

> Microsoft's cloud platform where applications and infrastructure can run.

### AWS

> Amazon's cloud platform.

### ACR

> Azure's managed container image registry.

---

# 14. Your Important Question: Why Azure/AWS If We Have Docker + GitHub Actions?

Your understanding is basically correct.

You said:

```text
Docker
→ Packages the application

GitHub Actions
→ Automates CI/CD

Azure / AWS
→ Provides cloud infrastructure where application runs
```

**Exactly.**

Let's make it even clearer.

---

## Docker's Responsibility

Docker solves:

> "How do I package my application so it can run consistently?"

For example:

```text
.NET API
.NET runtime
Dependencies
Configuration
Application files
        ↓
    Docker Image
```

---

## GitHub Actions' Responsibility

GitHub Actions solves:

> "How can I automatically execute the steps required to build/test/deploy my application?"

For example:

```text
git push
   ↓
GitHub Actions
   ↓
restore
   ↓
build
   ↓
test
   ↓
docker build
   ↓
docker push
   ↓
deploy
```

---

## Azure's Responsibility

Azure solves:

> "Where can my application actually run?"

For example:

```text
Azure App Service
       ↓
Running application
       ↓
Internet
       ↓
Users
```

---

# 15. A Real-World Analogy

Imagine a restaurant.

### Docker = lunch box

It packages everything needed together.

```text
Food
+ container
+ everything required
```

### GitHub Actions = delivery worker

It automatically performs:

```text
Prepare
 ↓
Package
 ↓
Deliver
```

### Azure = restaurant/building/infrastructure

It provides the place and infrastructure where the application can actually operate.

### ACR = warehouse

It stores the packaged Docker images.

So:

```text
Docker
→ Packaging

GitHub Actions
→ Automation

ACR
→ Storage of Docker images

Azure
→ Running infrastructure
```

---

# 16. Azure DevOps vs GitHub Actions

This is another important distinction.

Both can perform CI/CD.

```text
GitHub Actions
        ↓
      CI/CD
```

and:

```text
Azure Pipelines
        ↓
      CI/CD
```

They are **alternative CI/CD tools**.

---

# 17. GitHub Actions

Suppose your code is in GitHub:

```text
GitHub
   ↓
GitHub Actions
   ↓
Build
   ↓
Test
   ↓
Docker Build
   ↓
Deploy
```

The automation is defined in:

```text
.github/workflows/
```

Example:

```text
.github/
   workflows/
      deploy.yml
```

---

# 18. Azure Pipelines

Azure DevOps can do essentially the same thing:

```text
Azure Repo
    ↓
Azure Pipeline
    ↓
Build
    ↓
Test
    ↓
Docker Build
    ↓
Deploy
```

The pipeline can be defined in a YAML file, commonly:

```text
azure-pipelines.yml
```

---

# 19. Can GitHub Actions Deploy to Azure?

**Yes.**

This is extremely common.

For example:

```text
GitHub
   ↓
GitHub Actions
   ↓
Build
   ↓
Test
   ↓
Docker Build
   ↓
ACR
   ↓
Azure App Service
```

GitHub Actions does not need to be limited to GitHub infrastructure.

It can deploy to Azure.

GitHub's documentation explicitly supports Docker-to-Azure App Service deployment workflows. ([GitHub Docs][1])

---

# 20. Can Azure Pipelines Build Docker Images?

**Yes.**

For example:

```text
Azure Pipeline
      ↓
Docker build
      ↓
Docker image
      ↓
ACR
      ↓
App Service
```

So you can use either:

```text
GitHub Actions
```

or:

```text
Azure Pipelines
```

for the CI/CD automation.

---

# 21. Two Valid CI/CD Architectures

## Architecture A — GitHub Actions

```text
GitHub
   ↓
GitHub Actions
   ↓
Build
   ↓
Test
   ↓
Docker Build
   ↓
Push Image
   ↓
ACR
   ↓
Azure App Service
   ↓
Running API
```

## Architecture B — Azure Pipelines

```text
Azure Repos
   ↓
Azure Pipelines
   ↓
Build
   ↓
Test
   ↓
Docker Build
   ↓
Push Image
   ↓
ACR
   ↓
Azure App Service
   ↓
Running API
```

The **CI/CD tool changes**.

The basic deployment architecture doesn't have to.

---

# 22. Azure Pipeline YAML

A YAML pipeline is simply a file describing:

> "When this pipeline runs, what should it do?"

Example:

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: dotnet restore

- script: dotnet build

- script: dotnet test
```

The pipeline reads these instructions and executes them.

---

# 23. Where Is the YAML File?

Usually, the YAML file is stored along with your source code.

For example:

```text
my-project/
│
├── MyApi/
├── Tests/
├── Dockerfile
├── azure-pipelines.yml
└── README.md
```

You commit it:

```text
git add .
git commit
git push
```

Azure DevOps can then use that YAML definition for the pipeline.

---

# 24. GitHub Actions YAML vs Azure Pipelines YAML

They are both YAML.

But they have different syntax and concepts.

### GitHub Actions

```text
Workflow
   ↓
Job
   ↓
Step
   ↓
Runner
```

### Azure Pipelines

```text
Pipeline
   ↓
Stage
   ↓
Job
   ↓
Step / Task
   ↓
Agent
```

They look similar because they solve similar automation problems.

---

# 25. GitHub Actions Workflow

A workflow is the complete automation definition.

Example:

```yaml
name: Build

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - run: dotnet restore

      - run: dotnet build

      - run: dotnet test
```

Think:

```text
Workflow
=
Complete automation process
```

---

# 26. Job

A job is a group of steps that execute together on a runner.

```text
Job
 ├── Checkout
 ├── Restore
 ├── Build
 └── Test
```

---

# 27. Step

A step is an individual operation.

```text
Step 1 → Checkout
Step 2 → Restore
Step 3 → Build
Step 4 → Test
```

---

# 28. Runner

The runner is the machine executing the job.

```text
GitHub Actions
      ↓
Runner
      ↓
Runs commands
```

---

# 29. Azure Pipeline Stages

Azure Pipelines adds another useful level:

```text
Pipeline
   ↓
Stage
   ↓
Job
   ↓
Step
```

For example:

```text
Pipeline
│
├── Build Stage
│     └── Build Job
│
├── Test Stage
│     └── Test Job
│
└── Deploy Stage
      └── Deploy Job
```

Stages are especially useful for larger pipelines.

---

# 30. Tasks vs Steps

Azure Pipelines has both concepts.

A step can execute a script:

```yaml
- script: dotnet build
```

A task is a reusable predefined action provided by Azure DevOps or extensions.

For example, conceptually:

```text
Task
→ Docker build task
→ Azure deployment task
→ Publish artifact task
```

So don't worry too much about the distinction initially.

Think:

> **Step = one operation**
>
> **Task = a predefined reusable operation that can be used as a step**

---

# 31. Agents vs Runners

This is an excellent question.

They are **very similar concepts**.

### GitHub Actions

```text
Workflow
   ↓
Runner
   ↓
Executes jobs
```

### Azure Pipelines

```text
Pipeline
   ↓
Agent
   ↓
Executes jobs
```

The fundamental idea is almost the same:

> **The CI/CD service needs a computer on which it can execute your commands.**

---

# 32. What Does the Agent Actually Do?

Suppose your pipeline says:

```text
dotnet restore
dotnet build
dotnet test
docker build
docker push
```

Who actually executes these commands?

The agent.

Conceptually:

```text
Azure DevOps
     ↓
"Agent, run this job"
     ↓
Agent machine
     ↓
dotnet restore
     ↓
dotnet build
     ↓
dotnet test
     ↓
docker build
```

Microsoft's documentation describes an agent as computing infrastructure with agent software that runs pipeline jobs. ([Microsoft Learn][2])

---

# 33. Microsoft-Hosted Agent

Microsoft provides the machine.

```text
Azure DevOps
     ↓
Microsoft-hosted agent
     ↓
Your pipeline
```

You don't maintain the machine.

Microsoft handles things such as the VM image and maintenance.

Microsoft-hosted agents are generally fresh VMs for each job and are discarded afterward. ([Microsoft Learn][2])

---

# 34. Self-Hosted Agent

You provide/manage the machine.

For example:

```text
Your VM
   ↓
Install Azure DevOps Agent
   ↓
Register it
   ↓
Azure Pipeline uses it
```

The machine might be:

```text
Your physical server
      OR
Your VM
      OR
Cloud VM
```

You are responsible for maintaining it.

Self-hosted agents give you more control over installed software and machine configuration. ([Microsoft Learn][2])

---

# 35. Microsoft-Hosted vs Self-Hosted

|                        | Microsoft-hosted           | Self-hosted        |
| ---------------------- | -------------------------- | ------------------ |
| Machine provided by    | Microsoft                  | You                |
| Maintenance            | Microsoft                  | You                |
| Software customization | Limited/configured per job | High               |
| Machine persistence    | Usually fresh VM           | Usually persistent |
| Control                | Less                       | More               |
| Setup                  | Easy                       | More work          |

Simple:

```text
Microsoft-hosted
→ "Give me a temporary machine."

Self-hosted
→ "I own/manage the machine."
```

---

# 36. Azure App Service

Now we reach the **actual application hosting** part.

Azure App Service is a managed Azure service for hosting web applications, APIs, and similar web workloads.

Think:

```text
Internet
   ↓
Azure App Service
   ↓
ASP.NET Core API
```

---

# 37. Is App Service a Server?

The simplest answer:

> **App Service provides managed application hosting infrastructure, but you normally don't manage the underlying server like you would with a raw VM.**

This is called **PaaS — Platform as a Service**.

Compare:

### VM

```text
Azure VM
 ↓
You manage OS
 ↓
You install runtime
 ↓
You deploy application
```

### App Service

```text
App Service
 ↓
Azure manages much of the infrastructure
 ↓
You focus mainly on your application
```

---

# 38. Does App Service Build Your Application?

Usually, **no**.

This is a very important distinction.

You have:

```text
BUILD
DEPLOY
RUN
```

They are different things.

### Build

Turn source code into something deployable.

```text
Source code
    ↓
dotnet build
    ↓
Build output
```

### Deploy

Move the application/package/image to the hosting environment.

```text
Build output/image
       ↓
Azure
```

### Run

Actually execute the application.

```text
Azure
  ↓
ASP.NET Core process/container
  ↓
API running
```

---

# 39. App Service Can Run ASP.NET Core

Yes.

You can deploy an ASP.NET Core Web API to App Service.

Conceptually:

```text
ASP.NET Core API
       ↓
Deploy
       ↓
Azure App Service
       ↓
Running API
```

---

# 40. Does App Service Support Docker?

**Yes.**

You can use a custom Docker container with App Service. Azure's documentation describes configuring App Service to use a custom image, including images stored in Azure Container Registry or other private registries. ([Microsoft Learn][3])

So you can have:

```text
Docker Image
      ↓
ACR
      ↓
Azure App Service
      ↓
Container runs
```

---

# 41. Two Ways to Use App Service

You can think of two broad models.

### Model A — Deploy application directly

```text
.NET application
       ↓
App Service
       ↓
Running API
```

App Service provides the managed application environment.

### Model B — Deploy container

```text
.NET application
       ↓
Dockerfile
       ↓
Docker image
       ↓
ACR
       ↓
App Service
       ↓
Container
       ↓
Running API
```

Your requested flow is **Model B**.

---

# 42. What Is Azure Container Registry?

Azure Container Registry is a registry for storing container images.

Think:

```text
Docker Image
      ↓
Container Registry
      ↓
Stored safely
```

ACR is Microsoft's Azure-native container registry service.

---

# 43. Is ACR Similar to Docker Hub?

Yes.

Both can store Docker/OCI container images.

For example:

```text
Docker Hub
   ↓
myusername/myapi:1.0
```

or:

```text
ACR
   ↓
myregistry.azurecr.io/myapi:1.0
```

---

# 44. Very Important: Registry Does NOT Mean Running

This is another common misunderstanding.

### Registry

Stores images.

```text
ACR
 ↓
Docker Image
```

### Container runtime/hosting service

Runs containers.

```text
App Service
 ↓
Container
 ↓
Application
```

So:

```text
ACR
→ STORE

App Service
→ RUN
```

---

# 45. Does Docker Hub Run Your Container?

No.

Docker Hub primarily stores/distributes container images.

Similarly:

```text
ACR
→ stores image
```

It does not mean:

```text
ACR
→ application is running
```

---

# 46. Docker Image vs Container

This distinction is extremely important.

### Image

A packaged template.

```text
Docker Image
=
Blueprint/package
```

### Container

A running instance of an image.

```text
Docker Image
       ↓
   docker run
       ↓
Docker Container
       ↓
Running application
```

Analogy:

```text
Image
=
Class / blueprint

Container
=
Running instance
```

---

# 47. Dockerfile vs Docker Image vs Container

These three are different:

```text
Dockerfile
    ↓
Instructions
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Container
```

Example:

```text
Dockerfile
=
Recipe

Image
=
Prepared package

Container
=
Running application
```

---

# 48. Where Does ACR Fit?

Exactly here:

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker push
    ↓
ACR
    ↓
Stored Image
```

Then:

```text
Azure App Service
       ↓
Pull image from ACR
       ↓
Create/run container
       ↓
API live
```

Azure documents this exact general pattern: build an image, push it to ACR, and configure App Service to use the image. ([Microsoft Learn][3])

---

# 49. Why Use ACR Instead of Docker Hub?

Docker Hub is perfectly valid.

But companies may choose ACR because it integrates naturally with Azure environments.

For example:

```text
Azure
 ├── App Service
 ├── ACR
 ├── Key Vault
 ├── Monitor
 └── Networking
```

Using ACR can simplify Azure-oriented identity, permissions, networking, and governance.

For example:

```text
GitHub Actions
      ↓
ACR
      ↓
App Service
```

is a very natural Azure deployment architecture.

---

# 50. Now Let's Build Your Complete Real-World Example

Suppose you have:

```text
ASP.NET Core Web API
```

and your project looks like:

```text
MyApi/
│
├── Controllers/
├── Services/
├── Models/
├── Program.cs
├── MyApi.csproj
├── Dockerfile
└── ...
```

You are working in VS Code.

---

# 51. Step 1 — Write Code

You write:

```text
ASP.NET Core Web API
```

Example:

```csharp
app.MapGet("/hello", () => "Hello World");
```

At this point:

```text
Location:
Your computer

Technology:
.NET + VS Code

Purpose:
Develop application
```

Nothing is in Azure yet.

---

# 52. Step 2 — Git

You execute:

```bash
git add .
git commit -m "Add API"
git push origin main
```

Now your source code is pushed to GitHub.

```text
Your Computer
      ↓
     Git
      ↓
GitHub Repository
```

Git is responsible for version control.

GitHub hosts the repository.

---

# 53. Step 3 — GitHub Actions Starts

Suppose your workflow says:

```yaml
on:
  push:
    branches:
      - main
```

You push:

```text
main
 ↓
GitHub
 ↓
push event
 ↓
GitHub Actions
```

GitHub Actions notices the event and starts the workflow.

---

# 54. Step 4 — Runner Starts

GitHub needs a computer to execute your workflow.

So:

```text
GitHub Actions
       ↓
GitHub-hosted runner
       ↓
Workflow executes
```

The runner checks out your repository.

Now the runner has:

```text
MyApi/
├── Program.cs
├── Dockerfile
├── MyApi.csproj
└── ...
```

---

# 55. Step 5 — Restore

GitHub Actions runs:

```bash
dotnet restore
```

This downloads the required .NET dependencies.

Conceptually:

```text
.csproj
   ↓
dotnet restore
   ↓
NuGet dependencies
```

---

# 56. Step 6 — Build

Then:

```bash
dotnet build
```

This checks/compiles the application.

```text
Source Code
    ↓
dotnet build
    ↓
Build output
```

---

# 57. Step 7 — Test

Then:

```bash
dotnet test
```

Tests run.

If tests fail:

```text
Test FAILED
    ↓
Pipeline STOPS
    ↓
No deployment
```

If tests pass:

```text
Tests PASSED
    ↓
Continue
```

This is an important CI concept.

---

# 58. Step 8 — Docker Build

Now the workflow executes:

```bash
docker build -t myapi:latest .
```

Docker sees:

```text
Dockerfile
```

and follows its instructions.

For example:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base

WORKDIR /app

EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build

WORKDIR /src

COPY . .

RUN dotnet restore

RUN dotnet publish -c Release -o /app/publish

FROM base AS final

WORKDIR /app

COPY --from=build /app/publish .

ENTRYPOINT ["dotnet", "MyApi.dll"]
```

---

# 59. Who Reads the Dockerfile?

**Docker does.**

Not Azure.

Not GitHub.

The command:

```bash
docker build
```

causes Docker to read the Dockerfile.

However:

> GitHub Actions can execute the `docker build` command.

So the relationship is:

```text
GitHub Actions
      ↓
runs:
docker build
      ↓
Docker
      ↓
reads Dockerfile
      ↓
creates image
```

This distinction is very important.

---

# 60. When Is the Image Created?

Here:

```text
docker build
     ↓
Docker Image
```

For example:

```text
myapi:latest
```

or better:

```text
myapi:abc123
```

where `abc123` might represent a Git commit.

---

# 61. Step 9 — Login to ACR

The pipeline authenticates with Azure/ACR.

Conceptually:

```text
GitHub Actions
       ↓
Authenticate
       ↓
ACR
```

Now GitHub Actions has permission to push the image.

---

# 62. Step 10 — Tag Image

The image needs a registry address.

For example:

```text
myregistry.azurecr.io/myapi:1.0
```

The important structure is:

```text
ACR hostname / repository : tag
```

For example:

```text
myregistry.azurecr.io
        ↓
     myapi
        ↓
       :1.0
```

---

# 63. Step 11 — Push Image

Now:

```bash
docker push myregistry.azurecr.io/myapi:1.0
```

The image goes:

```text
GitHub Actions Runner
        ↓
Docker Image
        ↓
docker push
        ↓
Azure Container Registry
```

Now ACR contains the image.

---

# 64. Where Is the Image Now?

Not on your laptop.

Not only on the GitHub runner.

It is stored in:

```text
Azure Container Registry
```

For example:

```text
ACR
└── myapi
    ├── 1.0
    ├── 1.1
    └── 1.2
```

---

# 65. Step 12 — Deploy to App Service

Now the pipeline tells App Service:

```text
Use this image:

myregistry.azurecr.io/myapi:1.0
```

App Service knows:

```text
Image location:
ACR

Image:
myapi

Tag:
1.0
```

---

# 66. Step 13 — App Service Pulls Image

Conceptually:

```text
Azure App Service
       ↓
ACR
       ↓
Pull myapi:1.0
       ↓
Docker Image
```

Azure's App Service documentation notes that a containerized App Service can pull the configured image from a container registry when the app starts. ([Microsoft Learn][3])

---

# 67. Step 14 — Container Runs

The image is used to create a running container.

```text
Docker Image
      ↓
Container
      ↓
.NET application
      ↓
Listening on port
      ↓
Internet
```

Now your API is live.

For example:

```text
https://myapi.azurewebsites.net
```

---

# 68. The Complete Flow

Now put everything together:

```text
                    DEVELOPMENT
                         │
                         ▼
                       VS Code
                         │
                    Write .NET Code
                         │
                         ▼
                        Git
                         │
                    git push main
                         │
                         ▼
                       GitHub
                         │
                         ▼
                  GitHub Actions
                         │
                         ▼
                     Runner
                         │
              ┌──────────┴──────────┐
              │                     │
         dotnet restore        dotnet build
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                     dotnet test
                         │
                         ▼
                    docker build
                         │
                         ▼
                   Docker Image
                         │
                         ▼
                    docker push
                         │
                         ▼
              Azure Container Registry
                         │
                         ▼
                 Azure App Service
                         │
                    pulls image
                         │
                         ▼
                    Container
                         │
                         ▼
                 ASP.NET Core API
                         │
                         ▼
                      USERS
```

That is the architecture you should remember.

---

# 69. Where Exactly Does Docker Fit?

Docker sits between:

```text
CI/CD
```

and:

```text
Container Registry / Hosting
```

The flow is:

```text
Source Code
     ↓
CI/CD
     ↓
Dockerfile
     ↓
Docker Build
     ↓
Docker Image
     ↓
Registry
     ↓
Container Hosting
```

---

# 70. Dockerfile's Role

The Dockerfile says:

> "Here are the instructions for creating my application's container image."

For example:

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:10.0

WORKDIR /src

COPY . .

RUN dotnet restore

RUN dotnet publish -c Release -o /app

ENTRYPOINT ["dotnet", "/app/MyApi.dll"]
```

Conceptually:

```text
Dockerfile
    ↓
Instructions
    ↓
Docker build
    ↓
Image
```

---

# 71. Dockerfile vs YAML Pipeline File

This is another common confusion.

They are **not the same**.

### Dockerfile

Answers:

> "How do I build my container image?"

```text
Dockerfile
    ↓
Docker Image
```

### GitHub/Azure YAML

Answers:

> "What steps should my CI/CD system perform?"

```text
YAML
 ↓
Checkout
 ↓
Build
 ↓
Test
 ↓
Docker Build
 ↓
Push
 ↓
Deploy
```

So:

```text
Dockerfile
→ Instructions for Docker

YAML
→ Instructions for CI/CD system
```

---

# 72. Realistic GitHub Actions Workflow

Here is a simplified example.

Assume:

```text
GitHub
   ↓
GitHub Actions
   ↓
ACR
   ↓
Azure App Service
```

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

env:
  REGISTRY: myregistry.azurecr.io
  IMAGE_NAME: myapi

jobs:

  build-test-deploy:
    runs-on: ubuntu-latest

    steps:

      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '10.0.x'

      - name: Restore
        run: dotnet restore

      - name: Build
        run: dotnet build --no-restore

      - name: Test
        run: dotnet test --no-build

      - name: Login to Azure
        uses: azure/login@v2
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Login to ACR
        run: az acr login --name myregistry

      - name: Build Docker image
        run: |
          docker build \
            -t $REGISTRY/$IMAGE_NAME:${{ github.sha }} .

      - name: Push Docker image
        run: |
          docker push \
            $REGISTRY/$IMAGE_NAME:${{ github.sha }}

      - name: Deploy to App Service
        run: |
          az webapp config container set \
            --name my-api \
            --resource-group my-resource-group \
            --container-image-name $REGISTRY/$IMAGE_NAME:${{ github.sha }}
```

This is a **simplified learning example**; production authentication and deployment configuration should be hardened appropriately.

---

# 73. Line-by-Line Understanding

## Name

```yaml
name: Build and Deploy
```

Just gives the workflow a name.

---

## Trigger

```yaml
on:
  push:
    branches:
      - main
```

Means:

> When code is pushed to `main`, start this workflow.

---

## Environment Variables

```yaml
env:
  REGISTRY: myregistry.azurecr.io
  IMAGE_NAME: myapi
```

Reusable values.

Instead of writing:

```text
myregistry.azurecr.io
```

everywhere, we use:

```text
$REGISTRY
```

---

## Job

```yaml
jobs:
  build-test-deploy:
```

Defines a job.

---

## Runner

```yaml
runs-on: ubuntu-latest
```

Means:

> Run this job on an Ubuntu GitHub-hosted runner.

---

## Checkout

```yaml
uses: actions/checkout@v4
```

Downloads/checks out your repository into the runner.

Now the runner has your source code.

---

## Setup .NET

```yaml
uses: actions/setup-dotnet@v4
```

Sets up the required .NET SDK.

---

## Restore

```yaml
run: dotnet restore
```

Downloads dependencies.

---

## Build

```yaml
run: dotnet build --no-restore
```

Builds the application.

---

## Test

```yaml
run: dotnet test --no-build
```

Runs tests.

---

## Azure Login

```yaml
uses: azure/login@v2
```

Authenticates the workflow with Azure.

---

## ACR Login

```yaml
az acr login --name myregistry
```

Authenticates Docker/CLI access to the registry.

---

## Docker Build

```yaml
docker build ...
```

This is where:

```text
Docker
   ↓
reads Dockerfile
   ↓
creates image
```

---

## Docker Push

```yaml
docker push ...
```

Moves the image to ACR.

```text
Runner
 ↓
ACR
```

---

## Deployment

```yaml
az webapp config container set ...
```

Tells App Service which container image to use.

---

# 74. Why Use `${{ github.sha }}`?

This is useful:

```yaml
${{ github.sha }}
```

It represents the commit SHA.

So instead of:

```text
myapi:latest
```

you might have:

```text
myapi:4f82a91...
```

Now you can identify exactly which source-code commit produced the image.

This is much better for traceability.

For example:

```text
Git commit
   ↓
Docker image
   ↓
Deployment
```

You can trace the production application back to the exact commit.

---

# 75. Azure DevOps Alternative

Now replace GitHub Actions with Azure Pipelines.

The architecture becomes:

```text
GitHub Repository
        ↓
Azure Pipeline
        ↓
Azure Agent
        ↓
Build
        ↓
Test
        ↓
Docker Build
        ↓
ACR
        ↓
App Service
```

The important thing is:

> **Azure Pipelines doesn't require your code to be stored in Azure Repos.**

You can connect Azure Pipelines to GitHub repositories as well.

So you could have:

```text
GitHub
   ↓
Azure Pipelines
   ↓
Azure
```

This is perfectly possible.

---

# 76. Simplified Azure Pipeline YAML

For example:

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

variables:
  imageName: 'myapi'
  registry: 'myregistry.azurecr.io'

steps:

- checkout: self

- task: UseDotNet@2
  inputs:
    packageType: sdk
    version: '10.0.x'

- script: dotnet restore
  displayName: Restore

- script: dotnet build --no-restore
  displayName: Build

- script: dotnet test --no-build
  displayName: Test

- script: |
    docker build \
      -t $(registry)/$(imageName):$(Build.SourceVersion) .
  displayName: Build Docker image

- script: |
    docker push \
      $(registry)/$(imageName):$(Build.SourceVersion)
  displayName: Push Docker image
```

In a real Azure setup, you would normally use appropriate Azure service connections/tasks or secure authentication rather than putting credentials into scripts.

---

# 77. What Changes Between GitHub Actions and Azure Pipelines?

Mainly the **CI/CD automation layer**.

### GitHub Actions

```text
GitHub
  ↓
GitHub Actions
  ↓
Runner
```

### Azure Pipelines

```text
GitHub/Azure Repos
  ↓
Azure Pipelines
  ↓
Agent
```

But after that:

```text
Build
 ↓
Test
 ↓
Docker
 ↓
ACR
 ↓
App Service
```

can be almost identical.

---

# 78. Same Dockerfile

You can use the **same Dockerfile**.

```text
             Dockerfile
                 ↑
                 │
       ┌─────────┴─────────┐
       │                   │
GitHub Actions       Azure Pipelines
       │                   │
       └─────────┬─────────┘
                 ↓
            docker build
                 ↓
             Image
```

This is a very important concept.

Docker does not care whether:

```text
GitHub Actions
```

or:

```text
Azure Pipelines
```

started the build.

---

# 79. Azure Pipeline Agent Example

Suppose your YAML says:

```yaml
pool:
  vmImage: ubuntu-latest
```

Azure Pipelines requests an appropriate Microsoft-hosted agent.

Conceptually:

```text
Azure Pipeline
      ↓
Agent Pool
      ↓
Microsoft-hosted Ubuntu VM
      ↓
Clone/check out repository
      ↓
Execute jobs
```

Azure Pipelines requests an agent from an agent pool when a job needs to run. ([Microsoft Learn][4])

---

# 80. Agent Pool

Think of an agent pool as a collection of available machines.

```text
Agent Pool
│
├── Agent 1
├── Agent 2
├── Agent 3
└── Agent 4
```

Azure Pipelines chooses an appropriate available agent.

Microsoft documents the `Azure Pipelines` hosted pool as providing Windows, Linux, and macOS images. ([Microsoft Learn][5])

---

# 81. CI vs CD

You asked specifically about CI/CD.

## CI — Continuous Integration

Main idea:

> Frequently integrate code and automatically verify it.

For example:

```text
Developer pushes code
        ↓
Build
        ↓
Test
        ↓
Result
```

CI mainly answers:

> **"Does the new code work?"**

---

# 82. CD — Continuous Delivery / Deployment

CD takes the process further.

```text
Build
 ↓
Test
 ↓
Package
 ↓
Deploy
 ↓
Environment
```

It answers:

> **"How do we reliably get the software into an environment?"**

Depending on the organization, CD can mean continuous delivery or continuous deployment.

---

# 83. Full CI/CD Pipeline

```text
Developer
    ↓
Git Push
    ↓
CI
├── Restore
├── Build
└── Test
    ↓
Package
├── Docker Build
└── Docker Image
    ↓
Registry
    ↓
CD
    ↓
Azure
    ↓
Application
```

---

# 84. Build vs Deploy vs Run

Memorize this.

### Build

```text
Source Code
    ↓
Build
    ↓
Artifact / Image
```

### Deploy

```text
Artifact / Image
       ↓
Move/configure it in target environment
```

### Run

```text
Deployed application
       ↓
Process/container starts
       ↓
Application serves requests
```

---

# 85. Example

Suppose:

```text
Program.cs
Dockerfile
```

### Build

```text
docker build
```

creates:

```text
myapi:1.0
```

### Deploy

Push it:

```text
myapi:1.0
       ↓
ACR
```

Then configure:

```text
App Service
       ↓
Use myapi:1.0
```

### Run

App Service starts the container:

```text
myapi:1.0
   ↓
Container
   ↓
.NET API
   ↓
Running
```

---

# 86. Docker Hub vs ACR

| Docker Hub                  | ACR                             |
| --------------------------- | ------------------------------- |
| Container registry          | Container registry              |
| Docker ecosystem            | Azure ecosystem                 |
| Stores images               | Stores images                   |
| Public/private repositories | Private registry capabilities   |
| Independent of Azure        | Azure service                   |
| Can be used by Azure        | Naturally integrates with Azure |

The key point:

```text
Docker Hub ≈ ACR
```

in the sense that both are registries.

But they belong to different ecosystems.

---

# 87. ACR vs App Service

Do **not** confuse these.

```text
ACR
 ↓
Stores Docker images
```

```text
App Service
 ↓
Hosts/runs web application
```

So:

```text
ACR = Warehouse

App Service = Place where application runs
```

---

# 88. Azure vs Docker

They aren't competitors.

```text
Docker
→ How application is packaged/run as a container

Azure
→ Cloud infrastructure/services
```

You can use Docker **inside Azure**.

For example:

```text
Docker
   ↓
ACR
   ↓
Azure App Service
```

---

# 89. Azure DevOps vs GitHub

These overlap in some areas, but they aren't exactly the same thing.

### GitHub

Primarily known for:

```text
Git repositories
Pull requests
Code collaboration
Issues
GitHub Actions
```

### Azure DevOps

Provides:

```text
Azure Repos
Azure Boards
Azure Pipelines
Azure Test Plans
Azure Artifacts
```

There is overlap.

For example:

```text
GitHub Actions
       ↕
Azure Pipelines
```

are both CI/CD systems.

---

# 90. Azure DevOps vs GitHub Actions

Think:

```text
Azure DevOps
     │
     └── Azure Pipelines
             ↓
            CI/CD
```

versus:

```text
GitHub
   │
   └── GitHub Actions
             ↓
            CI/CD
```

A company may choose based on:

* existing ecosystem
* source-code location
* Microsoft/Azure integration
* GitHub adoption
* enterprise governance
* team familiarity
* existing tooling
* security/network requirements

There is no universal rule saying one is always better.

---

# 91. When Would a Company Use Azure Pipelines Instead?

For example, an organization may already have:

```text
Azure DevOps
 ├── Boards
 ├── Repos
 ├── Test Plans
 ├── Artifacts
 └── Pipelines
```

Then Azure Pipelines naturally fits into the existing ecosystem.

Another company may have:

```text
GitHub Enterprise
      ↓
GitHub Actions
```

and use GitHub Actions for CI/CD.

---

# 92. Important: GitHub Actions Can Deploy Anywhere

Don't think:

```text
GitHub Actions
      ↓
GitHub only
```

That's wrong.

It can do:

```text
GitHub Actions
      ↓
AWS
```

or:

```text
GitHub Actions
      ↓
Azure
```

or:

```text
GitHub Actions
      ↓
GCP
```

or:

```text
GitHub Actions
      ↓
Your own server
```

It is an automation system.

---

# 93. Azure Pipelines Can Also Deploy Many Places

Similarly:

```text
Azure Pipelines
      ↓
Azure
```

is common, but not mandatory.

It can automate deployments to other environments as well.

So don't define Azure Pipelines as:

> "A tool that only deploys to Azure."

Better:

> **Azure Pipelines is a CI/CD automation service that can automate builds, tests, and deployments to various targets, including Azure.**

---

# 94. Most Important Architecture

Now let's make your complete mental model.

```text
                    DEVELOPER
                        │
                        ▼
                     VS Code
                        │
                   Write Code
                        │
                        ▼
                       Git
                        │
                   git push
                        │
                        ▼
                    GitHub
                        │
                        ▼
              ┌───────────────────┐
              │       CI/CD       │
              │                   │
              │ GitHub Actions    │
              │        OR         │
              │ Azure Pipelines   │
              └─────────┬─────────┘
                        │
                        ▼
                  Runner / Agent
                        │
                        ▼
                Restore / Build
                        │
                        ▼
                      Test
                        │
                        ▼
                   Docker Build
                        │
                        ▼
                   Docker Image
                        │
                        ▼
              ┌─────────────────────┐
              │  Container Registry │
              │                     │
              │ Docker Hub OR ACR   │
              └──────────┬──────────┘
                         │
                         ▼
                    Azure / AWS
                         │
                         ▼
                 Container Hosting
                         │
                         ▼
                  Running Container
                         │
                         ▼
                   .NET Web API
                         │
                         ▼
                      USERS
```

---

# 95. The Most Important Separation

Memorize these responsibilities:

| Technology        | Main responsibility                   |
| ----------------- | ------------------------------------- |
| Git               | Version control                       |
| GitHub            | Host Git repositories/collaboration   |
| Azure Repos       | Git repositories in Azure DevOps      |
| GitHub Actions    | CI/CD automation                      |
| Azure Pipelines   | CI/CD automation                      |
| Docker            | Container packaging/runtime           |
| Dockerfile        | Instructions for building image       |
| Docker Image      | Packaged application                  |
| Docker Container  | Running instance of image             |
| Docker Hub        | Container image registry              |
| ACR               | Azure container image registry        |
| Azure App Service | Managed application/container hosting |
| Azure             | Cloud platform                        |
| AWS               | Cloud platform                        |
| Azure Boards      | Work/project tracking                 |
| Azure Artifacts   | Package management                    |
| Azure Test Plans  | Test management                       |

---

# 96. One More Very Important Diagram

Think of your system as **five layers**:

```text
┌─────────────────────────────────┐
│ 1. SOURCE CODE                  │
│                                 │
│ GitHub / Azure Repos            │
└─────────────────────────────────┘
                 ↓
┌─────────────────────────────────┐
│ 2. CI/CD                        │
│                                 │
│ GitHub Actions / Azure Pipeline │
└─────────────────────────────────┘
                 ↓
┌─────────────────────────────────┐
│ 3. PACKAGING                    │
│                                 │
│ Docker + Dockerfile             │
└─────────────────────────────────┘
                 ↓
┌─────────────────────────────────┐
│ 4. IMAGE STORAGE                │
│                                 │
│ Docker Hub / Azure ACR          │
└─────────────────────────────────┘
                 ↓
┌─────────────────────────────────┐
│ 5. APPLICATION HOSTING          │
│                                 │
│ Azure App Service / AWS etc.    │
└─────────────────────────────────┘
```

This is probably the **best mental model to keep in your notes**.

---

# 97. Common Misunderstandings

## ❌ "Azure DevOps is Azure."

No.

```text
Azure
→ Cloud platform

Azure DevOps
→ Development/DevOps tools
```

---

## ❌ "Docker is a cloud platform."

No.

Docker is container technology.

```text
Docker
→ Package/run containers
```

---

## ❌ "GitHub Actions runs my production application."

Usually no.

It executes CI/CD jobs.

```text
GitHub Actions
→ Build/Test/Deploy

Azure
→ Run production application
```

---

## ❌ "ACR runs my application."

No.

```text
ACR
→ Stores image

App Service
→ Runs application/container
```

---

## ❌ "Docker Hub runs my application."

Not in the sense of being your application's hosting environment.

```text
Docker Hub
→ Stores/distributes image
```

---

## ❌ "Dockerfile is the CI/CD pipeline."

No.

```text
Dockerfile
→ How to build Docker image

YAML
→ How CI/CD process should work
```

---

## ❌ "Azure App Service builds my application."

Not necessarily.

The CI/CD system can build it:

```text
GitHub Actions
      ↓
Build
```

and App Service can then host/run the result.

---

## ❌ "Azure Pipeline and GitHub Actions are completely different concepts."

No.

They are different products that solve a very similar problem:

```text
CI/CD automation
```

---

# 98. The One Flow You Should Memorize

If you are learning this for DevOps interviews or real development, remember this:

```text
Developer
    ↓
Write Code
    ↓
Git
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Build
    ↓
Test
    ↓
Docker Build
    ↓
Docker Image
    ↓
ACR
    ↓
Azure App Service
    ↓
Container
    ↓
.NET API
    ↓
Users
```

And the alternative CI/CD tool is:

```text
Developer
    ↓
GitHub / Azure Repos
    ↓
Azure Pipelines
    ↓
Agent
    ↓
Build
    ↓
Test
    ↓
Docker Build
    ↓
ACR
    ↓
Azure App Service
    ↓
Container
    ↓
.NET API
```

---

# 99. Final Mental Model — In One Sentence Each

If you remember only these, you're in good shape:

**GitHub**

> "I store and collaborate on my source code here."

**GitHub Actions**

> "I automatically execute my CI/CD process."

**Azure DevOps**

> "Microsoft gives me a collection of tools for planning, source control, testing, packages, and CI/CD."

**Azure Pipelines**

> "I use this part of Azure DevOps to automate CI/CD."

**Docker**

> "I package my application into a container image."

**Dockerfile**

> "These are the instructions for creating my Docker image."

**Docker Image**

> "This is the packaged application."

**Docker Container**

> "This is a running instance of that image."

**ACR**

> "I store my Docker images here."

**Azure App Service**

> "I host/run my web application or container here."

**Azure**

> "Microsoft provides the cloud infrastructure and services."

**AWS**

> "Amazon provides another cloud platform."

---

# 100. The Complete Picture

Finally, look at this:

```text
                         YOU
                          │
                          ▼
                     VS CODE
                          │
                     Write .NET
                          │
                          ▼
                         GIT
                          │
                          ▼
                       GITHUB
                          │
                     git push
                          │
                          ▼
                ┌──────────────────┐
                │     CI/CD        │
                │                  │
                │ GitHub Actions   │
                │       OR         │
                │ Azure Pipelines  │
                └────────┬─────────┘
                         │
                    Runner / Agent
                         │
                         ▼
                ┌──────────────────┐
                │       CI         │
                │                  │
                │ Restore          │
                │ Build            │
                │ Test             │
                └────────┬─────────┘
                         │
                         ▼
                      Docker
                         │
                    Dockerfile
                         │
                  docker build
                         │
                         ▼
                  DOCKER IMAGE
                         │
                  docker push
                         │
                         ▼
                ┌─────────────────┐
                │     REGISTRY    │
                │                 │
                │ Docker Hub / ACR│
                └────────┬────────┘
                         │
                         ▼
                       DEPLOY
                         │
                         ▼
                ┌─────────────────┐
                │      AZURE      │
                │                 │
                │  App Service    │
                └────────┬────────┘
                         │
                   Pull image
                         │
                         ▼
                    CONTAINER
                         │
                         ▼
                  ASP.NET CORE API
                         │
                         ▼
                      INTERNET
                         │
                         ▼
                       USERS
```

### The core idea

**Docker does not replace Azure.**

**GitHub Actions does not replace Azure.**

**Azure DevOps does not replace Azure.**

They operate at different layers:

```text
                    WHAT?
                     │
                     ▼
              Source Code
                     │
                     ▼
              CI/CD Automation
                     │
                     ▼
                Packaging
                     │
                     ▼
             Image Storage
                     │
                     ▼
              Cloud Hosting
                     │
                     ▼
            Running Application
```

And that is why you can legitimately have all of these in one project:

```text
GitHub
   +
GitHub Actions
   +
Docker
   +
Azure Container Registry
   +
Azure App Service
```

They are **not doing the same job**. Each one handles a different part of getting your code from **your laptop → production → users**.

([Microsoft Learn][2])

[1]: https://docs.github.com/en/actions/how-tos/deploy/deploy-to-third-party-platforms/docker-to-azure-app-service?apiVersion=2022-11-28&utm_source=chatgpt.com "Deploying Docker to Azure App Service - GitHub Docs"
[2]: https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&utm_source=chatgpt.com "Azure Pipelines Agents - Azure Pipelines | Microsoft Learn"
[3]: https://learn.microsoft.com/en-us/azure/app-service/quickstart-custom-container?utm_source=chatgpt.com "Quickstart: Run a Custom Container on App Service - Azure App Service | Microsoft Learn"
[4]: https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runs?view=azure-devops&utm_source=chatgpt.com "Pipeline runs - Azure Pipelines | Microsoft Learn"
[5]: https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues?view=azure-devops&utm_source=chatgpt.com "Create and Manage Agent Pools - Azure Pipelines | Microsoft Learn"
