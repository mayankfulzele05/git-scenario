# 📂 Incident Response Playbook: Temporary Context Switching via Git Stash (Q13)

This playbook establishes standard operational procedures (SOPs) for safely shelfing uncommitted, half-finished code structures to execute emergency priority hotfixes without generating redundant commit footprints.

---

### 🧠 The Core Architecture: The Stash Storage Shelf

The Git **Stash** infrastructure functions as a temporary internal clipboard storage index area outside your standard working directory tree. When a stash instruction is triggered, the Git plumbing engine aggregates your active modifications, converts them into individual raw blob snapshots, records them inside an internal reference stack file loop located at `.git/refs/stash`, and forcefully resets your local workspace directory parameters to match your branch's latest clean commit.

Using stashes prevents teams from polluting code graphs with dirty "Work-In-Progress" commits that violate conventional tracking standardizations.

---

### 🚀 Lab Simulation Protocol

#### **1. Generate an Active Unclean Working State**
Run the automated shell setup commands to build a base tracker and dirty up local storage matrices:
```bash
mkdir -p ~/git-stash-lab && cd ~/git-stash-lab
git init -b main

# Baseline target foundation file
echo "Stable Core Code" > pipeline.py
git add pipeline.py && git commit -m "feat: core release initialization"

# Expand onto active feature branches
git checkout -b feature/metrics-engine
echo "Experimental line edits" >> pipeline.py
echo "# Untracked new utility file" > untracked_helper.py
```

---

### 🔍 Diagnostic & Stash Remediation Workflows

#### **1. Evaluate Workspace Fragmentation**
Before initiating a context-switch across branches, audit for modified or uncommitted paths:
```bash
git status
```

#### **2. Shelf the Local Workspace State Safely**
To push all active modifications (including brand-new untracked tracking targets) onto the storage shelf with an explicit diagnostic message note, execute:
```bash
# The -u flag ensures untracked, un-added files are preserved alongside modifications
git stash -u -m "wip: metrics logging feature alpha draft parameters"
```

#### **3. Monitor Stash Layer Storage Pools**
To visualize all saved environment configurations currently sitting on the internal clipboard memory stack:
```bash
git stash list
```

---

### 🧯 Operational Recovery: Pop vs. Apply Guardrails

| Recovery Command Option | Core Selection Target Criteria | Storage Table Lifecycle Impact |
| :--- | :--- | :--- |
| **`git stash apply`** | Recommended when restoring highly complex or high-risk changes across experimental trees. | Copies the code data back into files but leaves the stash backup on the storage shelf array as a safety safety net. |
| **`git stash pop`** | Recommended for standard, everyday context-switching workflows. | Restores the file system states immediately and **purges** the backup item off the tracking stack entirely. |

#### **Resolving Stash Conflicts**
If you apply or pop a stash entry onto a branch that has heavily diverged, Git may drop conflict markers (`<<<<<<< HEAD`) directly into your code text blocks. If a stash pop encounters a merge conflict, **Git will refuse to delete the backup from the stash list** to protect you from losing data. You must resolve the conflict markers manually and then explicitly clear the list via:
```bash
# Manually drop the top stash container item once code lines are unified
git stash drop stash@{0}
```
