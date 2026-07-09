# K8S Pure Beginner Live Rewrite Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the live Kubernetes Killercoda lesson text with a pure-beginner, story-driven, image-supported version.

**Architecture:** The scenario remains Markdown-driven through `k8s/index.json`. Existing lesson files keep the same filenames and order, while new generated PNGs are copied into `k8s/assets/` and referenced by the relevant stages. The rewrite follows the approved preview draft and keeps all commands within the current beginner scope.

**Tech Stack:** Killercoda scenario JSON, Markdown, PNG assets, PowerShell validation commands.

## Global Constraints

- Pure beginners must not need prior Kubernetes knowledge.
- Every first-use concept should have a plain-language explanation.
- Every `kubectl` command must explain what it checks and what output to inspect.
- Each stage must include a visual aid, table, state card, or visual reading guide.
- Each stage must include a small understanding check.
- Do not add Service, Ingress, RBAC, CNI, CSI, or other out-of-scope Kubernetes topics.
- Do not overwrite existing images; add new generated assets with stable filenames.

---

### Task 1: Add Generated Lesson Images

**Files:**
- Create: `k8s/assets/beginner-mission-map.png`
- Create: `k8s/assets/node-cordon-before-after.png`
- Create: `k8s/assets/pod-debug-detective.png`
- Create: `k8s/assets/deployment-self-healing.png`

**Interfaces:**
- Consumes: generated images under `C:\Users\NPCMike\.codex\generated_images\019f459a-df6d-7130-bfee-c829c4175216\`
- Produces: stable project-local image paths referenced by Markdown.

- [ ] **Step 1: Copy the four selected generated images into `k8s/assets/`**

Use `Copy-Item` from the generated image directory to the stable filenames above. Leave originals in `.codex/generated_images/`.

- [ ] **Step 2: Verify image files exist**

Run:

```powershell
$assets = @(
  'k8s/assets/beginner-mission-map.png',
  'k8s/assets/node-cordon-before-after.png',
  'k8s/assets/pod-debug-detective.png',
  'k8s/assets/deployment-self-healing.png'
)
foreach ($asset in $assets) { if (-not (Test-Path $asset)) { throw "Missing $asset" } }
```

Expected: exits 0.

### Task 2: Rewrite Intro, Stage 0, Stage 1, and Stage 2

**Files:**
- Modify: `k8s/intro.md`
- Modify: `k8s/stage0-architecture.md`
- Modify: `k8s/step1-cluster.md`
- Modify: `k8s/step2-node.md`

**Interfaces:**
- Consumes: `docs/superpowers/drafts/2026-07-09-k8s-pure-beginner-preview.md`
- Produces: beginner-friendly opening, architecture, node inspection, and node maintenance lessons.

- [ ] **Step 1: Rewrite `intro.md`**

Use the preview draft Intro. Reference `./assets/beginner-mission-map.png`.

- [ ] **Step 2: Rewrite `stage0-architecture.md`**

Use the preview draft Stage 0. Reference existing `./assets/architecture.png`.

- [ ] **Step 3: Rewrite `step1-cluster.md`**

Use the preview draft Stage 1. Include example `kubectl get nodes` output and column explanations.

- [ ] **Step 4: Rewrite `step2-node.md`**

Use the preview draft Stage 2. Reference `./assets/node-cordon-before-after.png`.

### Task 3: Rewrite Stage 3, Stage 4, and Finish

**Files:**
- Modify: `k8s/step3-pod-debug.md`
- Modify: `k8s/step4-deployment.md`
- Modify: `k8s/finish.md`

**Interfaces:**
- Consumes: `docs/superpowers/drafts/2026-07-09-k8s-pure-beginner-preview.md`
- Produces: beginner-friendly Pod debug, Deployment self-healing, and mission recap lessons.

- [ ] **Step 1: Rewrite `step3-pod-debug.md`**

Use the preview draft Stage 3. Reference `./assets/pod-debug-detective.png` near the visual intro and keep existing `./assets/pod-debug-decision-tree.png` as the chapter cheat sheet.

- [ ] **Step 2: Rewrite `step4-deployment.md`**

Use the preview draft Stage 4. Reference `./assets/deployment-self-healing.png` before the existing `./assets/deployment-reconciliation-loop.png`.

- [ ] **Step 3: Rewrite `finish.md`**

Use the preview draft Finish.

### Task 4: Validate Scenario

**Files:**
- Validate: `k8s/index.json`
- Validate: `k8s/*.md`
- Validate: `k8s/assets/*.png`

**Interfaces:**
- Consumes: completed lesson files and assets.
- Produces: verified scenario with working references and required concepts.

- [ ] **Step 1: Validate JSON and referenced step files**

Run:

```powershell
$idx = Get-Content -Raw k8s/index.json | ConvertFrom-Json
@($idx.details.intro.text) + @($idx.details.steps.text) + @($idx.details.finish.text) |
  ForEach-Object {
    if (-not (Test-Path (Join-Path 'k8s' $_))) { throw "Missing step file: $_" }
  }
```

Expected: exits 0.

- [ ] **Step 2: Validate image references**

Run:

```powershell
$refs = Select-String -Path k8s/*.md -Pattern '!\[[^\]]*\]\(([^)]+)\)' -AllMatches
foreach ($ref in $refs) {
  foreach ($match in $ref.Matches) {
    $target = $match.Groups[1].Value
    if ($target.StartsWith('./')) {
      $full = Join-Path 'k8s' $target.Substring(2)
      if (-not (Test-Path $full)) { throw "Missing image reference $target in $($ref.Path)" }
    }
  }
}
```

Expected: exits 0.

- [ ] **Step 3: Check required concepts and commands**

Run:

```powershell
rg -n "kubectl|get nodes|cordon|uncordon|ImagePullBackOff|CrashLoopBackOff|Pending|Deployment|desired state|current state" k8s
```

Expected: each required command or concept appears.

- [ ] **Step 4: Check no placeholders remain**

Run:

```powershell
$bad = Select-String -Path k8s/*.md -Pattern 'TBD|TODO|待補|未定|placeholder'
if ($bad) { $bad; exit 1 }
```

Expected: exits 0.
