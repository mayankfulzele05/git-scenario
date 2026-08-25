# 📂 Incident Response Playbook: Cleaning History via Interactive Rebase (Q9)

This playbook establishes standard operational procedures (SOPs) for consolidating fragmented, local Work-In-Progress (WIP) commits into single, atomic features before code review.

---

### 🧠 The Core Architecture: Linear History Squashing

The Git **Interactive Rebase (`git rebase -i`)** tool functions as a direct graph database restructuring utility. By initializing an interactive rebase execution block context, an engineer can step backward along a local branch history graph timeline, re-order snapshots, drop broken nodes entirely, or group multiple sub-commits into a single entry via **Squashing** or **Fixup** operations.

Enforcing this clean ledger strategy keeps shared enterprise repositories simple, clear, and easy to parse during subsequent rollback or forensic auditing cycles.

---

### 🚀 Lab Simulation Protocol

#### **1. Deploy a Fragmented Commit Timeline**
Run the automated script loop sequence inside a local development environment block to build a messy historical tree:
```bash
mkdir -p ~/git-rebase-lab && cd ~/git-rebase-lab
git init -b main

# Baseline target foundation
echo "System Init" > app.py
git add app.py && git commit -m "feat: core release initialization"

# Append noisy developer workspace tracking blocks
git checkout -b feature/metrics-engine
echo "line 1" >> app.py && git add app.py && git commit -m "wip: block 1"
echo "line 2" >> app.py && git add app.py && git commit -m "wip: block 2"
echo "line 3" >> app.py && git add app.py && git commit -m "chore: fix typo"
echo "line 4" >> app.py && git add app.py && git commit -m "wip: finalize code"
```

---

### 🔍 Diagnostic & History Consolidation Workflow

#### **1. Audit the Local Graph Structure**
Query your local branch commit trails to locate the boundary range separating your unique development work from the primary tracking parent trunk line:
```bash
git log --oneline
```

#### **2. Initialize the Interactive Rebase Interface**
Launch the transformation editor matrix specifying the depth parameter boundary count needed to sweep up the messy commits:
```bash
# Open an interactive session covering the targeted history depth parameters
git rebase -i HEAD~4
```

#### **3. Apply Rewriting Directives**
Inside the text tracking editor container framework, modify the command operators prefixing the target commits:
*   **`pick`**: Retains the targeted commit node intact on the timeline.
*   **`squash`**: Collapses the target changes into the previous upper node while prompting to merge commit message text.
*   **`fixup`**: Blends the file change diffs directly into the parent node above while cleanly discarding the redundant commit message.

---

### 🧯 Strict Production Command Guardrails

> ⚠️ **THE GOLDEN RULE OF REBASING:** Never execute an interactive rebase history rewrite against commit blocks that have already been pushed to a remote repository and shared with other team members! Overwriting public histories will desynchronize the local tracking databases of your entire engineering team, leading to widespread duplication errors across development tracks.

#### **Continuous Branch Maintenance Matrix**
*   **Squash Locally:** Always lean on interactive rebases locally on your private machine workstation before running final push synchronization cycles to present clean PR submissions.
*   **Leverage Platform Squashing:** If managing large multi-developer cross-functional teams where local rebasing compliance varies, configure your primary repository platform settings (GitHub/GitLab) to automatically enforce **Squash Merges** directly at the Pull Request approval level. This merges entire feature tracks as a single clean commit automatically.
