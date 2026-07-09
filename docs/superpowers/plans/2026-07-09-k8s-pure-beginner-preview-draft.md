# K8S Pure Beginner Preview Draft Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a review-only full draft that shows how the Kubernetes lesson will read for pure beginners.

**Architecture:** The live Killercoda files under `k8s/` remain unchanged. A single Markdown draft under `docs/superpowers/drafts/` mirrors the existing Intro, Stage 0-4, and Finish structure so the user can review tone, pacing, visual guidance, and understanding checks before replacement.

**Tech Stack:** Markdown, existing Kubernetes lesson structure, existing image references under `k8s/assets/`, PowerShell validation commands.

## Global Constraints

- Pure beginners must not need prior Kubernetes knowledge.
- Every first-use concept should have a plain-language explanation.
- Every `kubectl` command must explain what it checks and what output to inspect.
- Each stage must include a visual aid, table, state card, or visual reading guide.
- Each stage must include a small understanding check.
- Do not add Service, Ingress, RBAC, CNI, CSI, or other out-of-scope Kubernetes topics.
- Do not modify live `k8s/` lesson files in this preview task.

---

### Task 1: Create Review Draft

**Files:**
- Create: `docs/superpowers/drafts/2026-07-09-k8s-pure-beginner-preview.md`

**Interfaces:**
- Consumes: `docs/superpowers/specs/2026-07-09-k8s-pure-beginner-design.md`
- Produces: One complete draft document with Intro, Stage 0, Stage 1, Stage 2, Stage 3, Stage 4, and Finish sections.

- [ ] **Step 1: Create the draft file**

Write a single Markdown document that mirrors the existing lesson order:

```text
Intro
Stage 0
Stage 1
Stage 2
Stage 3
Stage 4
Finish
```

- [ ] **Step 2: Validate required sections**

Run:

```powershell
rg -n "^# |^## " docs/superpowers/drafts/2026-07-09-k8s-pure-beginner-preview.md
```

Expected: output includes the lesson title, Intro, Stage 0-4, and Finish headings.

- [ ] **Step 3: Check for placeholders**

Run:

```powershell
rg -n "TBD|TODO|待補|未定|placeholder" docs/superpowers/drafts/2026-07-09-k8s-pure-beginner-preview.md
```

Expected: no matches.

- [ ] **Step 4: Check required Kubernetes concepts**

Run:

```powershell
rg -n "kubectl|get nodes|cordon|uncordon|ImagePullBackOff|CrashLoopBackOff|Pending|Deployment|desired state|current state" docs/superpowers/drafts/2026-07-09-k8s-pure-beginner-preview.md
```

Expected: each required command or concept appears in the draft.
