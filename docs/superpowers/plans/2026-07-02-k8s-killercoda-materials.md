# K8S Killercoda Materials Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite the Kubernetes Killercoda scenario into a concise, professional, internal SRE quest-style training flow.

**Architecture:** The scenario remains Markdown-driven through `k8s/index.json`. Each page is a focused task panel with narrative context, essential concepts, commands, observation points, hints, and one reflective question. Image prompts are included only where visuals materially improve understanding.

**Tech Stack:** Killercoda scenario JSON, Markdown, `kubectl` command examples, PowerShell validation for local checks.

---

### Task 1: Align Scenario Metadata

**Files:**
- Modify: `k8s/index.json`
- Create: `k8s/finish.md`

- [ ] **Step 1: Update titles and finish pointer**

Change Stage 0 title to `Stage 0：Kubernetes 架構導覽` and set `details.finish.text` to `finish.md`.

- [ ] **Step 2: Validate JSON**

Run: `Get-Content -Raw k8s/index.json | ConvertFrom-Json | Out-Null`

Expected: command exits with code 0 and no parser error.

### Task 2: Rewrite Intro and Stage 0

**Files:**
- Modify: `k8s/intro.md`
- Modify: `k8s/stage0-architecture.md`

- [ ] **Step 1: Rewrite `intro.md`**

Use a professional internal-training tone with NPC livestream incident flavor. Keep it short and task-oriented.

- [ ] **Step 2: Rewrite `stage0-architecture.md`**

Introduce the desired state model, minimal control plane/node components, and an image prompt for a Kubernetes architecture diagram. Do not reference missing image files.

- [ ] **Step 3: Scan tone**

Run: `rg -n "5-10|短講|講師|口頭|課後|講義|講稿|分鐘" k8s docs/superpowers/specs`

Expected: no matches except unrelated historical text if intentionally retained.

### Task 3: Rewrite Stage 1 and Stage 2

**Files:**
- Modify: `k8s/step1-cluster.md`
- Modify: `k8s/step2-node.md`

- [ ] **Step 1: Rewrite `step1-cluster.md`**

Guide learners to inspect nodes, roles, status, Kubernetes version, and internal IPs.

- [ ] **Step 2: Rewrite `step2-node.md`**

Guide learners through `cordon` and `uncordon`, emphasizing scheduling behavior instead of machine failure.

- [ ] **Step 3: Verify command examples are present**

Run: `rg -n "kubectl get nodes|kubectl cordon|kubectl uncordon" k8s/step1-cluster.md k8s/step2-node.md`

Expected: all three command families appear.

### Task 4: Rewrite Stage 3, Stage 4, and Finish

**Files:**
- Modify: `k8s/step3-pod-debug.md`
- Modify: `k8s/step4-deployment.md`
- Modify: `k8s/finish.md`

- [ ] **Step 1: Rewrite `step3-pod-debug.md`**

Make pod debugging a decision path: status, describe, logs, events. Include an image prompt for a debug decision tree.

- [ ] **Step 2: Rewrite `step4-deployment.md`**

Show Deployment desired state through create, scale, inspect, delete pod, and observe replacement. Include an image prompt for the reconciliation loop.

- [ ] **Step 3: Write `finish.md`**

Close with a concise mission debrief and a checklist of what the learner can now do.

- [ ] **Step 4: Verify required concepts**

Run: `rg -n "ImagePullBackOff|CrashLoopBackOff|Pending|Deployment|desired state|current state|replicas" k8s`

Expected: core debugging and deployment concepts appear in the lesson files.

### Task 5: Final Validation

**Files:**
- Validate: `k8s/*.md`
- Validate: `k8s/index.json`

- [ ] **Step 1: Validate scenario references**

Run: `$idx = Get-Content -Raw k8s/index.json | ConvertFrom-Json; @($idx.details.intro.text) + @($idx.details.steps.text) + @($idx.details.finish.text) | ForEach-Object { if (-not (Test-Path (Join-Path 'k8s' $_))) { throw "Missing step file: $_" } }`

Expected: command exits with code 0.

- [ ] **Step 2: Confirm image prompt policy**

Run: `rg -n "生圖提示詞|assets/" k8s`

Expected: prompt sections appear only in Stage 0, Stage 3, and Stage 4; image paths are mentioned as target filenames, not embedded broken image links.

- [ ] **Step 3: Review changed files**

Run: `git diff -- k8s docs/superpowers/plans/2026-07-02-k8s-killercoda-materials.md`

Expected: diff shows concise Killercoda task panels, no unrelated changes.
