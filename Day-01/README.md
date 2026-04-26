<div align="center">

# ☁️ Day 01 — Cloud Computing & Azure DevOps

[![Day](https://img.shields.io/badge/Day-01-blue?style=for-the-badge&logo=azure-devops&logoColor=white)](.)
[![Topic](https://img.shields.io/badge/Topic-Cloud%20Fundamentals-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)](.)


> *Cloud Computing → Agile → DevOps → Azure DevOps → Hands-on Portal Walkthrough*

</div>

---

## 📑 Table of Contents

- [What is Cloud Computing?](#-what-is-cloud-computing)
- [IaaS vs PaaS vs SaaS](#-iaas-vs-paas-vs-saas)
- [Shared Responsibility Model](#-shared-responsibility-model)
- [Traditional Build & Deployment](#-traditional-build--deployment-workflow)
- [Waterfall Model & Its Challenges](#-waterfall-model--its-challenges)
- [What is Agile?](#-what-is-agile)
- [What is DevOps?](#-what-is-devops)
- [What is Azure DevOps?](#️-what-is-azure-devops)
- [Hosting Options](#-hosting-options)
- [Hands-on: Portal Walkthrough](#-hands-on--azure-devops-portal-walkthrough)
- [References](#-references)

---

## ☁️ What is Cloud Computing?

Imagine you want to open a restaurant. You have two choices:

- **Option A:** Buy land, construct a building, set up the kitchen, buy all equipment, hire staff to maintain the building — and *then* start cooking.
- **Option B:** Rent a fully equipped kitchen space, pay only for the hours you use it, and focus entirely on cooking.

**Cloud Computing is Option B — but for software and technology.**

---

In the traditional world of IT, if a company wanted to run any application or website, it had to:
- Purchase expensive **physical servers**
- Set up a **data center** (a room or building to house those servers)
- Buy **networking equipment**, storage drives, and power systems
- Hire a dedicated team to **physically maintain** all of it
- Pay for **electricity, cooling, and space** — 24 hours a day, 7 days a week

And here's the painful part — all of this had to be done **before a single user even accessed the application.** If the application became popular and needed more capacity, you had to go through the entire process again — order new hardware, wait weeks for delivery, set it up, and configure it. If the application failed or the business shut down, all that hardware became a sunk cost.

---

**Cloud Computing** completely changes this model. Instead of owning and managing your own hardware, you access computing resources — **servers, storage, databases, networking, software, analytics** — over the **internet**, provided and maintained by a cloud provider. You pay only for what you actually use, scale up or down instantly based on demand, and never have to worry about physical hardware.

The three major cloud providers are:

| Provider | Company |
|:---|:---|
| **Microsoft Azure** | Microsoft |
| **AWS** | Amazon |
| **GCP** | Google |

---

### 🎯 Core Characteristics of Cloud Computing

| Characteristic | What It Means in Practice |
|:---|:---|
| **On-Demand Self-Service** | You can provision a server or database in minutes through a portal or API — no human approval needed from the provider. |
| **Broad Network Access** | Resources are accessible over the internet from any device — laptop, phone, or tablet. |
| **Resource Pooling** | The provider's infrastructure serves multiple customers simultaneously. Your resources are isolated but physically shared. |
| **Rapid Elasticity** | Scale up during peak load (e.g., a product launch), scale down when traffic drops — automatically and instantly. |
| **Pay As You Go** | You pay only for what you consume — like a utility bill for electricity or water. |

### 💰 Why Companies Move to Cloud

| Problem with On-Premises | How Cloud Solves It |
|:---|:---|
| Huge upfront capital expenditure | No hardware purchase — pay monthly (OpEx model) |
| Takes weeks/months to provision servers | Provision in minutes via portal or API |
| Fixed capacity — hard to scale | Elastic scaling — grow or shrink instantly |
| Risk of hardware failure | Cloud providers offer redundancy and 99.9%+ SLA |
| Maintaining hardware is expensive | Fully managed by the cloud provider |
| One geographic location | Deploy globally across multiple regions in clicks |

> 💡 **Key Takeaway:** Cloud computing shifts the burden of managing physical infrastructure to the provider, so engineering teams can stop worrying about *where* the software runs and focus entirely on *what* the software does.

---

## 🏗️ IaaS vs PaaS vs SaaS

As cloud computing evolved, providers realized that different customers need different levels of control. Some teams want to manage everything themselves. Others just want to run their app without touching any infrastructure. This led to **three cloud service models.**

The best way to understand them is through the pizza analogy:

```
🍕 Pizza Analogy:

  Made at Home     →    Takeaway/Delivery   →    Restaurant
  (On-Premises)         (IaaS or PaaS)           (SaaS)

You manage         You manage some,         You just eat —
everything         provider manages rest    provider handles all
```

---

### 🔹 IaaS — Infrastructure as a Service

The cloud provider gives you **raw, virtualized infrastructure** — virtual machines, storage, and networking. That's it. Everything above the hardware level is **your responsibility.**

You manage: Operating System, security patches, runtime, middleware, application code, and data.

- **Best for:** Teams that need full control over the environment — choosing their own OS, installing custom software, fine-tuning performance.
- **Example on Azure:** Azure Virtual Machines, Azure Blob Storage, Azure Virtual Network

**Real-world analogy:** You rent an empty apartment. The building and utilities are maintained, but you furnish it, maintain it inside, and decide how to use every room.

---

### 🔹 PaaS — Platform as a Service

The cloud provider manages the infrastructure **and** the underlying platform — OS, runtime, middleware, and patching. You bring only your **application code and data.**

You manage: Application logic and data only.

- **Best for:** Developers who want to build and deploy applications quickly without worrying about server configuration.
- **Example on Azure:** Azure App Service, Azure SQL Database, Azure Functions

**Real-world analogy:** You rent a fully furnished apartment. The furniture is there, utilities are managed — you just move in and live your life.

---

### 🔹 SaaS — Software as a Service

The cloud provider manages **everything** — infrastructure, platform, application, updates, and security. You simply access the software through a browser or app and use it.

You manage: Your data and user access settings — nothing else.

- **Best for:** End users and businesses who need software without any technical setup.
- **Example on Azure:** Microsoft 365, Microsoft Teams, Dynamics 365

**Real-world analogy:** You stay at a hotel. You don't manage the building, the room, or the furniture — you just use it and pay per night.

---

### Responsibility Breakdown

| Layer | On-Premises | IaaS | PaaS | SaaS |
|:---|:---:|:---:|:---:|:---:|
| Application | 🧑 You | 🧑 You | 🧑 You | ☁️ Provider |
| Data | 🧑 You | 🧑 You | 🧑 You | ☁️ Provider |
| Runtime | 🧑 You | 🧑 You | ☁️ Provider | ☁️ Provider |
| OS | 🧑 You | 🧑 You | ☁️ Provider | ☁️ Provider |
| Virtualization | 🧑 You | ☁️ Provider | ☁️ Provider | ☁️ Provider |
| Hardware | 🧑 You | ☁️ Provider | ☁️ Provider | ☁️ Provider |

> 💡 **Key Takeaway:** The higher you go (IaaS → PaaS → SaaS), the less you manage — but also the less control you have. Choose the model that matches your team's needs and expertise.

---

## 🔐 Shared Responsibility Model

A dangerous misconception many people have when moving to the cloud is: *"The cloud provider handles everything — I don't need to think about security anymore."*

This is **completely wrong** — and this misunderstanding has caused major security breaches.

Cloud security follows a **Shared Responsibility Model** — meaning both Microsoft and the customer share security duties. The exact split depends on the service model being used.

```
←——————— Customer Responsible ———————→←——— Microsoft Responsible ———→

 On-Premises    IaaS          PaaS              SaaS
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│   Data   │  │   Data   │  │   Data   │  │  (Cloud) │
│  Access  │  │  Access  │  │  Access  │  │  (Cloud) │
│   App    │  │   App    │  │  (Cloud) │  │  (Cloud) │
│    OS    │  │    OS    │  │  (Cloud) │  │  (Cloud) │
│   Infra  │  │  (Cloud) │  │  (Cloud) │  │  (Cloud) │
│ Hardware │  │  (Cloud) │  │  (Cloud) │  │  (Cloud) │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
```

### What Microsoft Always Owns:
- Physical data centers, buildings, and hardware security
- Network infrastructure and DDoS protection
- Host and hypervisor security

### What You Always Own (regardless of service model):
- **Your data** — encrypting it, backing it up, classifying it
- **User identities** — who has access to what (IAM, roles, permissions)
- **Your account security** — enabling MFA, protecting credentials
- **Compliance with your industry's regulations**

> 💡 **Key Takeaway:** Moving to the cloud reduces your infrastructure security burden — but it never eliminates your responsibility for data protection and identity management. You are always accountable for what happens to your own data.

---

## 🔁 Traditional Build & Deployment Workflow

To understand why DevOps matters, you need to understand what software delivery looked like before it.

In the traditional model, Development and Operations were **two completely separate teams** with **conflicting goals:**

```
┌─────────────────────────────────────────────────────────┐
│  DEVELOPMENT TEAM          │  OPERATIONS TEAM           │
│                            │                            │
│  Goal: Ship new features   │  Goal: Keep systems stable │
│  fast and often            │  and avoid changes         │
│                            │                            │
│  "Move fast"               │  "Don't break things"      │
└────────────────────────────┴────────────────────────────┘
                        ⬇
              Constant conflict and blame
```

### The Traditional Workflow:

```
Developers write code for weeks/months in isolation
              ↓
Code is handed off to QA for manual testing
(takes weeks — testers find hundreds of bugs)
              ↓
Bugs sent back to developers to fix
(context lost — devs forgot what they wrote)
              ↓
QA re-tests — more bugs — more cycles
              ↓
Code finally handed to Operations for deployment
(Ops has never seen this code before)
              ↓
Ops deploys manually, usually late Friday night
              ↓
Something breaks in production
              ↓
Dev blames Ops. Ops blames Dev. Customers are angry.
```

**The core problems:**
- Code was written in large batches — huge releases with thousands of changes at once
- By the time a bug was found, the developer had forgotten what they'd written
- No automation — every step was manual and error-prone
- Deployments were so risky that teams avoided them — making the problem worse

> 💡 **Key Takeaway:** The wall between Dev and Ops created a slow, painful, blame-driven process. DevOps tears down that wall permanently.

---

## 🌊 Waterfall Model & Its Challenges

The **Waterfall model** is a traditional project management and software development methodology where work flows **strictly downward** — like water flowing down a waterfall — through a fixed sequence of phases.

```
Phase 1: Requirements   ← Gather ALL requirements upfront
            ↓
Phase 2: System Design  ← Design the complete system
            ↓
Phase 3: Implementation ← Developers write all the code
            ↓
Phase 4: Testing        ← QA tests the complete system
            ↓
Phase 5: Deployment     ← Ship the final product
            ↓
Phase 6: Maintenance    ← Fix issues post-launch
```

The critical rule: **You cannot go back.** Once you leave a phase, it's considered complete.

**When was it used?** Waterfall came from construction and manufacturing, where changing requirements mid-project is genuinely very costly (you can't tear down a half-built bridge). It was borrowed into software development — but software is fundamentally different from physical construction.

### 🚧 Why Waterfall Failed in Software

| Challenge | Real Impact |
|:---|:---|
| **Requirements gathered once, upfront** | Business needs change constantly. What was needed 6 months ago may be irrelevant today. |
| **Testing happens at the very end** | Bugs are found after all development is complete — the most expensive time to fix them. |
| **No working software until the very end** | Customers wait 6–18 months before seeing anything. |
| **No room for feedback mid-project** | If the customer sees the final product and says "this isn't what I wanted" — it's too late. |
| **High risk of total failure** | If the requirements were misunderstood at the start, the entire project can be a complete loss. |

> 💡 **Key Takeaway:** Waterfall treats software like physical construction — rigid, sequential, and unforgiving of change. In a world where business requirements evolve constantly, this model consistently fails. It gave birth to Agile.

---

## ⚡ What is Agile?

**Agile** is a software development philosophy and a set of values published in the **Agile Manifesto (2001)** by a group of software practitioners who were frustrated with the failures of Waterfall.

Agile's core idea is simple but powerful: **instead of building everything at once and delivering at the end, break the work into small deliverable chunks, ship frequently, and adapt based on real feedback.**

### The Agile Manifesto — 4 Core Values:

```
Individuals and Interactions   OVER   Processes and Tools
Working Software               OVER   Comprehensive Documentation  
Customer Collaboration         OVER   Contract Negotiation
Responding to Change           OVER   Following a Plan
```

*(Note: The right side still matters — Agile just values the left side more)*

### How Agile Works — The Sprint Cycle:

```
┌─────────────────────────────────────────────────────┐
│                  PRODUCT BACKLOG                    │
│   (Full list of all features to be built)          │
└─────────────────────────────────────────────────────┘
                        ↓
            Pick top priority items
                        ↓
┌─────────────────────────────────────────────────────┐
│              SPRINT (1–4 weeks)                     │
│  Plan → Design → Build → Test → Review → Ship      │
└─────────────────────────────────────────────────────┘
                        ↓
         Get feedback from customer/stakeholders
                        ↓
         Update backlog based on feedback
                        ↓
              Start the next Sprint
                    (repeat)
```

### Key Agile Concepts

| Concept | What It Means |
|:---|:---|
| **Sprint** | A fixed time period (1–4 weeks) in which a usable piece of software is built and delivered |
| **Product Backlog** | A prioritized list of all features, bug fixes, and improvements to be done |
| **Sprint Backlog** | The subset of backlog items selected for the current sprint |
| **Daily Standup** | A short daily team meeting (15 mins) — what did I do yesterday, what will I do today, any blockers? |
| **Sprint Review** | Demo of completed work to stakeholders at the end of the sprint |
| **Retrospective** | Team reflects on what went well, what didn't, and how to improve — after every sprint |
| **Scrum Master** | Facilitates the Agile process, removes blockers for the team |
| **Product Owner** | Represents the business/customer, owns and prioritizes the backlog |

> 💡 **Key Takeaway:** Agile replaced "plan everything, build everything, deliver once" with "plan a little, build a little, learn a lot, repeat." This dramatically reduced risk and brought customers into the development process continuously.

---

## 🚀 What is DevOps?

**DevOps** is the next evolution after Agile. While Agile made development teams more flexible and collaborative, it didn't fully address the friction between **Development** and **Operations** teams.

The term **DevOps** = **Dev** (Development) + **Ops** (Operations). But it's much more than just combining two teams. DevOps is a **cultural shift + a set of engineering practices + a toolchain** that enables organizations to deliver software **faster, more reliably, and continuously** — with shared ownership across the entire lifecycle.

### How DevOps Evolved from Agile:

```
Waterfall     →      Agile          →       DevOps
─────────            ─────                  ───────
Big releases         Iterative sprints       Continuous delivery
Manual testing       Some automation         Full automation (CI/CD)
Dev ≠ Ops            Dev improved            Dev + Ops = one team
Months to ship       Weeks to ship           Hours/days to ship
```

### The DevOps Infinity Loop:

```
        ┌──── PLAN ─────┐
        │               │
    MONITOR           CODE
        │               │
   OPERATE           BUILD
        │               │
    DEPLOY           TEST
        │               │
        └─── RELEASE ───┘
```

Each stage feeds continuously into the next — there is no "end." This is what makes DevOps a **continuous** practice.

### Key DevOps Practices

| Practice | What It Does |
|:---|:---|
| **Continuous Integration (CI)** | Every developer merges code to a shared branch frequently (multiple times a day). An automated pipeline immediately builds and tests the code. If something breaks, the team knows within minutes. |
| **Continuous Delivery (CD)** | After CI passes, the application is automatically deployed to a staging or test environment. The code is always in a state ready to be deployed to production. |
| **Infrastructure as Code (IaC)** | Servers and infrastructure are defined in code files (like Terraform or ARM templates) and version-controlled — just like application code. No more manual server configuration. |
| **Monitoring & Observability** | Applications are continuously monitored in production. Logs, metrics, and alerts help teams detect issues before users notice them. |
| **Shift Left on Security** | Security is integrated early in the development cycle — not added as an afterthought before deployment. |

> 💡 **Key Takeaway:** DevOps is not a tool or a job title — it is a culture and a set of practices. The goal is to make software delivery so reliable and automated that shipping to production becomes a non-event — something that happens every day without fear.

---

## 🛠️ What is Azure DevOps?

**Azure DevOps** is Microsoft's cloud-based platform that provides all the services and tools a software team needs to plan, develop, build, test, and deploy software — all under one roof.

It is Microsoft's answer to the question: *"What does a team need to fully implement DevOps?"*

Before Azure DevOps, teams had to stitch together multiple separate tools — Jira for tracking, GitHub for code, Jenkins for CI/CD, Artifactory for packages. Azure DevOps brings all of this into a single integrated platform.

### 🧩 Core Services

| Service | What It Does |
|:---|:---|
| 📋 **Azure Boards** | Plan and track work using Kanban boards, backlogs, sprints, and work items (tasks, bugs, user stories) |
| 🗂️ **Azure Repos** | Host and manage source code with full Git support — branching, pull requests, code reviews, and policies |
| ⚙️ **Azure Pipelines** | Automate your entire CI/CD — build, test, and deploy your application on every code commit, across any platform |
| 📦 **Azure Artifacts** | Create private package feeds to host and share npm, NuGet, Maven, and PyPI packages within your organization |
| 🧪 **Azure Test Plans** | Manage manual, exploratory, and automated testing — track test cases, test runs, and defects |
| 📖 **Azure Wikis** | Write and organize project documentation in Markdown directly inside the project |

> 💡 **Key Takeaway:** Azure DevOps is a one-stop DevOps platform — covering the full lifecycle from "what should we build?" to "it's live and monitored in production."

---

## 🌐 Hosting Options 

### Hosting Options

| Option | Type | Description |
|:---|:---|:---|
| **Azure DevOps Services** | ☁️ Cloud | Fully hosted and managed by Microsoft. No infrastructure setup. Automatic updates. Best for most teams. |
| **Azure DevOps Server** | 🏢 On-Premises | Self-hosted inside your own servers. Full control over data and customization. Preferred by organizations with strict data compliance (banking, government, healthcare). |

---

## 🖥️ Hands-on — Azure DevOps Portal Walkthrough

### Step 1: Sign Up & Login

1. Go to 👉 [https://azure.microsoft.com/en-us/products/devops/](https://azure.microsoft.com/en-us/products/devops/)
2. Click **"Start free"**
3. Sign in with your **Microsoft account** (or create one for free)
4. Azure DevOps automatically creates a **default Organization** named after your email
5. Your portal URL will be:
   ```
   https://dev.azure.com/{your-organization-name}
   ```

---

### Step 2: Create Your First Project

1. On the home screen, click **"New Project"**
2. Fill in the details:

| Field | Recommended Value |
|:---|:---|
| Project Name | `Day1_project` |
| Visibility | Private |
| Version Control | Git |
| Work Item Process | Agile |

3. Click **"Create"** — you'll land on your project dashboard 🎉

---

### Step 3: Explore the Left Navigation Panel

Once inside your project, the **left sidebar** gives you access to all Azure DevOps services. Here's a detailed walkthrough of every section:

---

#### 📋 Overview
Your **project homepage**. Three sub-sections:

- **Summary** — High-level snapshot of the project: description, team members, recent activity, and links to Repos/Boards/Pipelines. Good starting point for new team members joining the project.
- **Dashboards** — Fully customizable dashboards. Add, resize, and arrange widgets like build status, sprint burndown, work item charts, deployment frequency, test results, and more. Each team can have its own dashboard. Great for daily monitoring without digging into individual sections.
- **Wiki** — A built-in documentation space. Write structured notes, architecture docs, onboarding guides, runbooks, and SOPs in Markdown — directly inside the project. Supports page hierarchy, images, tables, and code blocks. Replaces the need for external tools like Confluence for many teams.

---

#### 📌 Boards
Where all **work management and sprint planning** happens, based on Agile methodology.

- **Work Items** — The atomic unit of work in Azure DevOps. You can create:
  - *Epic* → large body of work (e.g., "User Authentication System")
  - *Feature* → a feature within an epic (e.g., "Login with Google")
  - *User Story* → a specific user need (e.g., "As a user, I can log in with my Google account")
  - *Task* → a technical to-do to fulfill the story
  - *Bug* → a defect to be fixed
  Each work item has: assignee, status, priority, story points, linked items, comments, and attachments.

- **Boards** — A visual Kanban board. Each column represents a stage (To Do → Active → Resolved → Closed). Drag and drop cards across columns. Teams can customize column names to match their workflow.

- **Backlogs** — A flat, prioritized list of all work items. Used during sprint planning — drag items into the sprint to assign them. Items at the top are highest priority. You can view backlogs at Epic, Feature, Story, or Task level.

- **Sprints** — View and manage the current sprint. See which tasks are assigned to whom, team capacity vs. committed work, and the sprint burndown chart (remaining work over time).

- **Queries** — Create custom saved filters for work items. Example queries:
  - "All open bugs assigned to me"
  - "All user stories in the current sprint that are not started"
  - Queries can be shared across the team and exported to Excel.

---

#### 🗂️ Repos
Your **source code management** space — full Git hosting inside Azure DevOps.

- **Files** — Browse your repository folder structure. View, edit, and even create files directly in the browser. Syntax highlighting supported for all major languages.
- **Commits** — Full history of all commits. Filter by author, date, branch, or message. Click any commit to see the exact lines changed (diff view).
- **Pushes** — See a log of all pushes made to the repository, grouped by push event.
- **Branches** — View all branches. Set **branch policies** on protected branches (like `main`):
  - Require a Pull Request before merging
  - Require a minimum number of reviewers
  - Require linked work items
  - Require a passing build before merge
- **Tags** — Create tags to mark specific commits as release milestones (e.g., `v1.0.0`, `v2.3.1`). Used for versioning.
- **Pull Requests** — The code review workflow. A developer creates a PR when their feature branch is ready to merge into `main`. Reviewers can comment on specific lines, request changes, and approve. PRs can be linked to work items and must pass all configured policies before merging.

---

#### ⚙️ Pipelines
The **CI/CD automation engine** of Azure DevOps — the most critical section for DevOps practice.

- **Pipelines** — Define what happens automatically when code is pushed. A pipeline is written in **YAML** (preferred) or built using the Classic visual editor. A typical pipeline:
  ```
  Trigger: On push to main branch
  Steps:
    1. Restore dependencies
    2. Build the application
    3. Run unit tests
    4. Publish test results
    5. Package and publish artifacts
  ```
  Pipelines run on **agents** — machines (cloud-hosted or self-hosted) that execute the steps.

- **Environments** — Named deployment targets: Dev, QA, Staging, Production. Each environment can have **approvals and checks** — e.g., a manager must manually approve before code goes to Production.

- **Releases** — Classic multi-stage release pipelines (older UI-based approach). Still used in some organizations. YAML-based pipelines with stages are now the recommended approach.

- **Library** — Shared resources for pipelines:
  - **Variable Groups** — Store and reuse variables across pipelines (e.g., `DATABASE_URL`, `API_KEY`). Can be linked to Azure Key Vault for secure secret management.
  - **Secure Files** — Upload sensitive files (SSL certificates, `.p12` files, `.env` files) that pipelines can download securely at runtime.

- **Task Groups** — Package a sequence of commonly used pipeline tasks into a reusable group. Promotes DRY (Don't Repeat Yourself) across multiple pipelines.

- **Deployment Groups** — Register on-premises or VM-based servers as deployment targets. The agent is installed on the target machine, and the pipeline deploys directly to it.

---

#### 🧪 Test Plans
For **structured manual and exploratory testing.**

- **Test Plans** — Organize testing efforts for a specific sprint or release. A test plan contains one or more test suites.
- **Test Suites** — Group related test cases (e.g., "Smoke Tests", "Regression Suite", "Login Feature Tests").
- **Test Cases** — Individual test scenarios with step-by-step instructions and expected results. Testers mark each step as Pass or Fail during execution.
- **Test Runs** — Execute a test suite and record actual outcomes. Bugs can be raised directly from a failed test step and linked automatically.
- **Progress Report** — Visual summary showing how many test cases passed, failed, or are blocked across the plan.

> ⚠️ Requires the **Basic + Test Plan** subscription — not available on the free Basic plan.

---

#### 📦 Artifacts
A **private package management** service — publish, version, and consume packages within your organization.

- **Feeds** — A feed is a private package registry scoped to your organization or project. Your team publishes internal packages to a feed and consumes them in applications or pipelines — just like using a public registry (npmjs, PyPI), but private and secure.
- **Supported package types:** npm, NuGet, Maven, PyPI, Universal Packages
- **Upstream Sources** — A feed can be configured to proxy public registries (like npmjs.com). This means all package requests go through your private feed — giving you control, caching, and security scanning on every package your team uses.
- **Use case:** Your team builds a shared authentication library used across 5 different microservices. Instead of copying code into every repo, publish it as a versioned NuGet/npm package to Artifacts — every service consumes it cleanly with version control.

---

#### 📊 Reports
Analytics built into Boards and Pipelines to track **team performance and project health.**

- **Velocity Chart** — Shows how many story points the team completed in each sprint. Helps predict how much work can realistically fit into upcoming sprints.
- **Burndown Chart** — Tracks remaining work in the sprint against the ideal burndown line. A chart that isn't going down fast enough is an early warning signal.
- **Cumulative Flow Diagram (CFD)** — Visualizes how work items move through stages over time. Wide bands in one stage indicate a bottleneck — work is piling up there.
- **Pipeline Analytics** — Build success/failure rates over time, test pass rates, deployment frequency, and mean time to recovery (MTTR). These are the core **DORA metrics** that measure DevOps maturity.

---

#### ⚙️ Project Settings
Accessible at the **bottom-left corner**. Configure everything about the project and organization:

| Setting | Purpose |
|:---|:---|
| **Teams** | Create and manage sub-teams. Each team gets its own Board, Backlog, and Sprint view. |
| **Repositories** | Set repo-level security permissions and configure branch policies per repo. |
| **Agent Pools** | Manage the machines that run your pipeline jobs. Azure-hosted agents are free within limits; self-hosted agents give more control. |
| **Service Connections** | Connect Azure DevOps to external services — Azure subscription, GitHub, Docker Hub, Kubernetes, AWS, SonarQube, etc. Used by pipelines to deploy and integrate with external systems. |
| **Notifications** | Configure email or Teams alerts for builds, pull requests, work item changes, etc. |
| **Billing** | Manage your subscription plan, add paid parallel pipeline jobs, and view usage. |

---

## 📚 References

- 📖 [Official Azure DevOps Documentation — Microsoft Learn](https://learn.microsoft.com/en-us/azure/devops/)
- 📖 [What is DevOps? — Microsoft Learn](https://learn.microsoft.com/en-us/devops/what-is-devops)
- 📖 [Azure DevOps Services Pricing](https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/)

---

<div align="center">

[![Back](https://img.shields.io/badge/Back-grey?style=for-the-badge)](../README.md)
[![Next ▶](https://img.shields.io/badge/Day_02-0078D4?style=for-the-badge&logo=microsoftazure)](../day-02/README.md)

</div>