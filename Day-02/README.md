<div align="center">

# 🌿 Day 02 — Branching & Merging Strategy in Azure DevOps

> _Branches → Merge Types → Environments → Scenarios (ECR & Feature/UAT)_

</div>

---

## 📑 Table of Contents

- [Why Do We Need a Branching Strategy?](#-why-do-we-need-a-branching-strategy)
- [The Four Core Branches](#-the-four-core-branches)
- [Development Environments](#-development-environments)
- [Merge Types in Azure DevOps](#-merge-types-in-azure-devops)
- [The Complete Branching Flow](#-the-complete-branching-flow)
- [Scenario 1 — Normal Development Flow](#-scenario-1--normal-development-flow)
- [Scenario 2 — ECR (Emergency Change Request)](#-scenario-2--ecr-emergency-change-request)
- [Scenario 3 — Feature Branch & UAT](#-scenario-3--feature-branch--uat)
- [Hotfix Branch — Quick Note](#-hotfix-branch--quick-note)
- [Key Rules to Always Remember](#-key-rules-to-always-remember)
- [References](#-references)

---

## 🤔 Why Do We Need a Branching Strategy?

When you're working alone on a personal project, you can push code directly to a single branch and nothing breaks. But in a real organization with 50, 100, or even 150+ developers all working on the same codebase simultaneously — without a clear branching strategy, things fall apart very quickly.

Imagine:

- Developer A pushes half-finished code that breaks the build
- Developer B directly modifies production code to fix a bug — and introduces a new one
- A new feature that was never approved accidentally goes live in production
- No one knows which version of the code is actually running in production

**A branching strategy is a set of rules and conventions that defines:**

- How many branches exist and what each one is used for
- Who is allowed to push to which branch
- How code moves from one branch to another (merge types, pull requests, approvals)
- How branches map to deployment environments (Dev, QA, Stage, Prod)

> This strategy is **designed and managed by the DevOps engineer** — not individual developers. Developers follow the strategy; the DevOps engineer sets it up, enforces it through policies, and maintains it.

> 💡 **Key Takeaway:** A branching strategy is not optional for a medium-to-large team. Without it, your codebase becomes chaotic, releases become risky, and production incidents become common. Think of it as the traffic rules for your codebase.

---

## 🌿 The Four Core Branches

There are **four long-lived branches** in a standard enterprise branching strategy. These branches are permanent — they are never deleted. They each serve a specific purpose and map directly to specific environments and pipelines.

---

### 🟡 1. Temp Branch (Temporary / Developer Branch)

**Purpose:** Personal working branch for each developer, created per task or user story.

This is not a permanent branch. A developer creates this branch when they pick up a task, works on it, and once the code is merged into Develop — this branch is deleted or abandoned. It is **always cloned from the Develop branch** — never from Release or Main.

**Why from Develop and not Main?**

- Main branch holds the _previous stable release_ — it won't have the latest code other developers have been writing
- Release branch holds only _current release code_ — it won't have all the present/future development code
- Develop branch has everything — all present, past, and ongoing development code — which is exactly what a developer needs as the base to build new work on top of

**Naming convention examples:**

```
feature/user-story-123
bugfix/login-issue-456
task/add-payment-module
```

---

### 🟠 2. Develop Branch

**Purpose:** Central hub for all ongoing development activity — present and future code lives here.

This is the most actively used branch. All developers merge their completed work here via Pull Requests. It is the source of truth for the current state of development.

**What happens here:**

- All temp branches merge into Develop after development is complete
- Build pipeline runs automatically on every merge — code is built and tested
- After a successful build, code is deployed automatically to the **Dev environment** and then **QA environment**

**Key characteristics:**

- Multiple developers push here daily
- Always has the most up-to-date code
- Protected with branch policies — no one pushes directly, only via Pull Requests

---

### 🔴 3. Release Branch

**Purpose:** Holds only the code that is going into the **current release** — nothing more, nothing less.

In a company, there is typically a **release cycle** — for example, every Sunday is a production release day. The week leading up to that is called the release week. During release week, only the code that has been approved for the current production release is moved from Develop into Release.

**Why not just merge everything from Develop into Release?**
Because Develop has future code, experimental code, and work-in-progress code that is **not ready** for production yet. Moving everything would introduce untested or unapproved code into your release — which is dangerous.

**This is where Cherry Picking comes in** (covered in detail later).

**What happens here:**

- Cherry-picked commits from Develop are moved here via PR
- Build pipeline runs automatically and deploys to **Stage environment**
- After Stage validation, code is deployed to **Production**

---

### ⬛ 4. Main Branch

**Purpose:** Stores the **golden copy** — the exact version of code that is live in production right now.

This branch is your safety net. Think of it as a locked vault. Once a release is deployed successfully to production and approved by the client, the release code is merged into Main and **tagged with a version number** (e.g., `v1.0`, `v1.1`, `v2.0`).

**Why is this so important?**

Imagine you deployed version 1.2 to production and it caused a critical issue. Instead of panicking, you simply:

1. Go to Main branch
2. Pull version 1.1 (the last stable tag)
3. Re-deploy version 1.1 to production
4. Application is back to normal

Without Main, you'd have no reliable way to revert to a known-good state.

**Key characteristics:**

- Very limited access — only senior DevOps engineers or leads can merge here
- Never used for active development
- Every merge is tagged with a version number
- Also used for **ECR (Emergency Change Request)** scenarios — covered later

---

### 🟢 5. Feature Branch

**Purpose:** Holds new feature code that needs to be **shown to the client before deciding whether to release it.**

Sometimes a client wants to see how a new feature looks and feels before committing to releasing it to all users. For this, the code needs to go to a special environment called **UAT (User Acceptance Testing)** — not production.

**What happens here:**

- Developer builds the new feature in a temp branch → merges into Develop → cherry-picked into Feature branch
- Code is deployed to **UAT environment** for client review
- If the client approves → code moves from Feature to Release → goes to Production in the next release cycle
- If the client rejects → Feature branch is simply dropped — production is unaffected

> 💡 **Key Takeaway:** These four branches (+ Feature) form a clean separation of concerns. Each branch has a single purpose, maps to specific environments, and has specific policies. This structure gives the DevOps engineer full control over what goes where and when.

---

## 🖥️ Development Environments

Each branch maps to one or more deployment environments. Understanding this mapping is critical because your **CI/CD pipelines are built around these environments.**

```
BRANCH              ENVIRONMENT(S)           PURPOSE
──────────────────────────────────────────────────────────────────
Temp Branch    →    Local PC                 Developer tests locally
Develop        →    Dev → QA                 Integration + QA testing
Release        →    Stage → Prod             Pre-prod validation + Release
Main           →    Prod (ECR only)          Emergency production fix
Feature        →    UAT                      Client acceptance testing
```

### Environment Details:

| Environment  | Full Name                | Purpose                                                          |
| :----------- | :----------------------- | :--------------------------------------------------------------- |
| **Local PC** | Developer's own machine  | Developer builds and tests code locally before pushing           |
| **Dev**      | Development Environment  | First shared environment — basic smoke testing after merge       |
| **QA**       | Quality Assurance        | QA team runs full manual and automated test suites here          |
| **Stage**    | Staging / Pre-Production | Exact replica of production — integration testing, load testing  |
| **Prod**     | Production               | Live environment — real users                                    |
| **UAT**      | User Acceptance Testing  | Exact replica of production — for client preview of new features |

**The flow of code through environments:**

```
Local PC → Dev → QA → Stage → Production
                              ↑
                          (release week)

For features:
Local PC → Dev → QA → UAT → (client approves) → Stage → Production
```

> 💡 **Key Takeaway:** Environments act as quality gates. Code must pass each environment before moving to the next. The further right you go, the closer to production you are — and the higher the bar for quality.

---

## 🔀 Merge Types in Azure DevOps

Before understanding the full branching flow, you need to understand the four merge types available in Azure DevOps Pull Requests. Choosing the right merge type for each branch transition is a core part of the branching strategy.

---

### 1. Merge (No Fast Forward)

```
Before:                     After:
main ---A---B               main ---A---B---M
             \                               ↑
feature       C---D         feature  C---D--/
```

- Creates a new **merge commit (M)** that ties the two branches together
- **Preserves the full commit history** of both branches — every individual commit is visible
- Best when you need to know exactly **who did what and when**
- Use when: auditability and full history are important

---

### 2. Squash Merge

```
Before:                     After:
main ---A---B               main ---A---B---S
             \                               ↑
feature       C---D---E     All of C+D+E squashed into one commit S
```

- Takes **all commits from the feature branch** and **squashes them into a single new commit** on the target branch
- The original individual commits (C, D, E) do not appear in the target branch history
- Best when: developers make many small, noisy commits during development (e.g., "fix typo", "wip", "test again") and you don't want that noise polluting the main branch history
- The code is fully preserved — only the commit history is condensed

---

### 3. Rebase and Merge

```
Before:                     After:
main ---A---B               main ---A---B---C'---D'
             \
feature       C---D
```

- Re-applies each feature branch commit **on top of the latest main branch** commit, one by one
- Creates a perfectly **linear history** — as if the feature was built directly on top of main
- No merge commit is created
- History looks clean but is actually **rewritten** — original commit hashes change (C becomes C', D becomes D')
- Use carefully on shared branches — rewriting history can cause problems for other developers

---

### 4. Semi-Linear Merge (Rebase + Merge Commit)

- A hybrid — rebases the commits first (like Rebase), then creates a merge commit (like No Fast Forward)
- Gives you a linear history **plus** a merge commit marker
- Less commonly used — combines benefits of both but also the complexity

---

### Merge Type Decision Guide:

| Branch Transition | Recommended Merge Type    | Reason                                                                       |
| :---------------- | :------------------------ | :--------------------------------------------------------------------------- |
| Temp → Develop    | **Squash**                | Developers make many noisy commits — squash keeps Develop history clean      |
| Develop → Release | **Merge No Fast Forward** | Need full history to track what exactly went into the release                |
| Release → Main    | **Squash or Merge No FF** | Depends on team requirement — squash for clean history, No FF for full audit |
| Main → Prod (ECR) | **Cherry Pick + PR**      | Only specific commits go to production, not everything                       |

> 💡 **Key Takeaway:** There is no universally "best" merge type. The right choice depends on whether you need clean history (Squash) or full audit trail (No Fast Forward). This is a team discussion — the DevOps engineer proposes, the team and product owner agree.

---

## 🗺️ The Complete Branching Flow

Here is the full picture of how all branches, environments, and pipelines connect:

```
                        ┌─────────────────────────────────────────────────────────────┐
                        │                    DEVELOPER WORKFLOW                        │
                        └─────────────────────────────────────────────────────────────┘

 ┌─────────────┐   PR       ┌─────────────┐   CI/CD Pipeline    ┌───────┐    ┌──────┐
 │ Temp Branch │ ────────► │   DEVELOP   │ ──────────────────► │  DEV  │ ──►│  QA  │
 │  (per task) │  Squash    │   Branch    │  (auto deploy)      └───────┘    └──────┘
 └─────────────┘            └──────┬──────┘
  cloned from                      │ PR + Cherry Pick
    Develop                        │ Merge No FF
                                   ▼
                           ┌───────────────┐   CI/CD Pipeline    ┌─────────┐   ┌──────┐
                           │    RELEASE    │ ──────────────────► │  STAGE  │──►│ PROD │
                           │    Branch     │  (auto deploy)      └─────────┘   └──────┘
                           └───────┬───────┘                                       │
                                   │ PR + Squash                                   │ ✅ Client Approval
                                   │ or Merge No FF                                │
                                   ▼                                               ▼
                           ┌───────────────┐   ECR Pipeline                ┌───────────────┐
                           │     MAIN      │ ─────────────────────────────►│     PROD      │
                           │    Branch     │  (cherry pick → prod only)    │  (ECR only)   │
                           │ (Golden Copy) │                                └───────────────┘
                           │  Tagged v1.0  │
                           └───────────────┘

 ┌─────────────┐   PR       ┌─────────────┐   CI/CD Pipeline    ┌───────┐
 │ Temp Branch │ ────────► │   FEATURE   │ ──────────────────► │  UAT  │ ──► Client Review
 │  (per task) │  Cherry    │    Branch   │  (auto deploy)      └───────┘         │
 └─────────────┘  Pick      └─────────────┘                                       │
  via Develop                                                              ✅ Approved?
                                                                                   │
                                                                     Yes → Merge to Release → Prod
                                                                      No → Drop Feature Branch
```

---

## 📋 Scenario 1 — Normal Development Flow

This is the day-to-day flow that every developer follows for regular development tasks.

### Step-by-step:

```
Step 1: Developer picks up a user story / task from Azure Boards

Step 2: Developer clones Develop branch and creates a personal Temp branch
        git checkout -b feature/user-story-123 origin/develop

Step 3: Developer writes code and tests it on their Local PC

Step 4: Developer pushes code to their Temp branch and raises a Pull Request
        Temp Branch → Develop Branch
        Merge Type: SQUASH
        (why squash? developer may have 50-200 noisy commits — we don't want
        all that history polluting the Develop branch)

Step 5: PR is reviewed and approved. Code merges into Develop.

Step 6: CI/CD pipeline triggers automatically:
        Develop branch → Build → Deploy to Dev → Deploy to QA

Step 7: QA team tests the code in QA environment. Signs off.

Step 8: Release week arrives. DevOps engineer raises a PR from Develop → Release
        Merge Type: Cherry Pick + Merge No Fast Forward
        (only the commits going into this release are cherry-picked — not everything)

Step 9: CI/CD pipeline triggers automatically:
        Release branch → Build → Deploy to Stage

Step 10: Integration testing done in Stage. All good.

Step 11: Sunday release — code automatically deployed to Production from Release pipeline.

Step 12: Client confirms everything looks good in Production.

Step 13: Release branch code is merged into Main via PR
         Merge Type: Squash or Merge No FF (team decision)
         Main branch is tagged: v1.0, v1.1, etc.
```

> 💡 **Key Takeaway:** The normal flow enforces quality gates at every step. Code cannot jump from a developer's machine directly to production — it must pass through Dev, QA, and Stage first. This is intentional and non-negotiable.

---

## 🚨 Scenario 2 — ECR (Emergency Change Request)

**ECR = Emergency Change Request.** This is when something critical breaks in production — homepage is down, payment is failing, core features are broken — and the client needs it fixed **immediately.** There is no time to follow the full release cycle.

### What changes in ECR:

The Release Branch is **skipped entirely.** Code goes from Develop directly to Main, and from Main directly to Production via a dedicated ECR pipeline.

```
Step 1: P1 ticket raised. Production is down. Client is escalating.

Step 2: Developer creates a Temp branch from Develop (same as always)
        Works on the fix.

Step 3: Temp → Develop via PR (Squash)

Step 4: Pipeline deploys fix to Dev → QA
        (There is no escape — even in emergencies, QA must verify the fix
        before it goes to production. You don't want to fix one bug and introduce another.)

Step 5: QA gives a quick go-ahead.

Step 6: Cherry Pick the fix commit from Develop directly to Main via PR
        (Release Branch is skipped — it is only for scheduled release activity)

Step 7: Dedicated ECR production pipeline triggers from Main:
        Main → Production only (no Stage)
        (This is a SEPARATE pipeline created only for ECR — not the regular release pipeline)

Step 8: Production is fixed. Crisis resolved.

Step 9 (later): Sync the same fix back into Release branch so all branches stay in sync.
```

### ECR Flow Diagram:

```
  Temp Branch
      │ PR + Squash
      ▼
  DEVELOP ──────────────── Dev ──► QA
      │
      │ PR + Cherry Pick (SKIP RELEASE)
      ▼
    MAIN
      │
      │ ECR Pipeline (dedicated)
      ▼
  PRODUCTION ✅ Fixed
```

### Important rules for ECR:

- The ECR pipeline is a **completely separate pipeline** — it cannot be confused with or accidentally triggered during a normal release
- Even in emergencies, the fix still goes through **Dev and QA** — you never go directly from developer machine to production
- After the emergency is resolved, the fix must be **back-merged into Release branch** to keep all branches in sync — if you don't, the same bug will reappear in the next regular release

> 💡 **Key Takeaway:** ECR does not mean "skip everything." It means "skip Stage and the scheduled release cycle." QA is still mandatory. And always sync your branches after the emergency — branches out of sync will cause bugs to resurface.

---

## 🧪 Scenario 3 — Feature Branch & UAT

Sometimes a client wants to see a **new feature before deciding whether to ship it.** The feature should be visible and testable — but it must **not go to production** until the client explicitly approves it.

This is what the **Feature Branch + UAT environment** is designed for.

```
Step 1: Client requests a new feature but wants to review it first before committing to production.

Step 2: Developer creates a Temp branch from Develop, builds the new feature.

Step 3: Temp → Develop via PR (Squash)

Step 4: From Develop, cherry-pick the feature commits → Feature Branch via PR
        (same concept as Release — only the feature's commits, not everything from Develop)

Step 5: CI/CD pipeline deploys Feature branch code → UAT environment
        UAT is an exact replica of production — same config, same data structure.
        The client sees the feature exactly as it would appear in production.

Step 6a (Client Approves):
        Feature Branch → Release Branch via PR + Cherry Pick
        Release pipeline deploys → Stage → Production in the next release cycle

Step 6b (Client Rejects):
        Feature Branch is simply dropped/deleted.
        Develop and Production are completely unaffected.
        No rollback needed — the feature never touched production.
```

### Feature Branch Flow:

```
  Temp Branch
      │ PR + Squash
      ▼
  DEVELOP
      │ PR + Cherry Pick
      ▼
  FEATURE BRANCH ──────────────────► UAT
                                       │
                              Client Reviews
                                       │
                    ┌──────────────────┴──────────────────┐
                    │ Approved                             │ Rejected
                    ▼                                      ▼
            Feature → Release                    Drop Feature Branch
            Release → Stage → Prod               Nothing changes in Prod
```

> 💡 **Key Takeaway:** The Feature Branch acts as a holding area — a way to show the client a production-accurate preview without any risk to the actual production environment. UAT is always an exact replica of production, so the client's review is meaningful and trustworthy.

---

## 🔧 Hotfix Branch — Quick Note

You'll often hear about a **Hotfix branch** in other branching strategies (like Gitflow). Here's how it fits into what we've learned:

A **Hotfix branch is essentially a Temp branch** — it's a short-lived, developer-created branch used specifically to fix a bug. The naming convention is what differs:

```
Regular task:     feature/user-story-123
Bug fix:          hotfix/login-page-broken
```

Once the bug fix is done, the hotfix branch follows the **exact same flow** as any other Temp branch:

- Merges into Develop via PR (Squash)
- Goes through Dev → QA
- Moves to Release during the next release cycle (unless it's a P1/ECR — then follows ECR flow)

**Important:** Even a hotfix for a production bug must still go through all lower environments. You cannot find a bug in production and directly push the fix there — this would leave Dev, QA, and Stage branches out of sync, and the same bug will come back in the next release.

> 💡 **Key Takeaway:** Hotfix is just a naming convention for a bug-fix temp branch — not a different type of branch. The flow is always the same. Branches out of sync is the worst thing that can happen to a codebase.

---

## 📌 Key Rules to Always Remember

| Rule                                               | Why It Matters                                                                                                      |
| :------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------ |
| **Always create Temp branch from Develop**         | Develop has the most up-to-date code — building on Main or Release will leave you missing recent development        |
| **No direct pushes to Develop, Release, or Main**  | All merges must go through Pull Requests with reviews and approvals                                                 |
| **Cherry Pick when moving to Release or Main**     | Develop has more code than what's going into a single release — you must be selective                               |
| **Release branch is for current release only**     | Never put future or experimental code into Release                                                                  |
| **Main branch is sacred**                          | Very limited access — tagged versions only — your production safety net                                             |
| **ECR pipeline is separate from Release pipeline** | Never share — risk of accidentally deploying emergency fixes during a scheduled release                             |
| **Always keep all branches in sync**               | Fix in production but not in Develop? That bug will be reintroduced in the next release                             |
| **Even in ECR, code must pass Dev + QA**           | No exceptions — going straight to production without testing is how you fix one bug and cause three more            |
| **Feature branch → UAT, not Production**           | UAT is for client preview only — the feature doesn't touch production until the client formally approves            |
| **Branching strategy is a team decision**          | DevOps engineer proposes and enforces — but the strategy is agreed upon with the development team and product owner |

---

## 📚 References

- 📖 [Azure Repos — Branching Strategies — Microsoft Learn](https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-strategies-with-feature-flags)
- 📖 [Pull Request Merge Strategies — Microsoft Learn](https://learn.microsoft.com/en-us/azure/devops/repos/git/merging-with-squash)
- 📖 [Cherry Pick Changes — Microsoft Learn](https://learn.microsoft.com/en-us/azure/devops/repos/git/cherry-pick)
- 📖 [Branch Policies — Microsoft Learn](https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-policies)

---

<div align="center">

[![◀ Previous](https://img.shields.io/badge/◀_Previous-Day_01-grey?style=for-the-badge&logo=microsoftazure)](../day-01/README.md)
[![Next ▶](https://img.shields.io/badge/Next_▶-Day_03-0078D4?style=for-the-badge&logo=microsoftazure)](../day-03/README.md)

</div>
