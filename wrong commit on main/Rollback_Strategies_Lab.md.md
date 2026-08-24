# 📂 Incident Response Playbook: Wrong Commit on Main - Reset vs Revert (Q66)

This playbook establishes standard operational procedures (SOPs) for safely rolling back broken deployment commits injected directly into primary tracking lines based on synchronization states.

---

### 🧠 The Core Architecture: History Rewriting vs. Safe Appending

Undoing errors within a Git repository graph database requires evaluating a single crucial architectural question: **Has the target broken commit block been shared with the remote server platform?**

1.  **Local Isolation Boundary:** If a commit exists only on your local machine workstation, you own the tree context completely. You can safely leverage **`git reset`** to scrub the commit from existence, restoring your branch pointer backward in time.
2.  **Shared Distributed Boundary:** If a commit has already been pushed up to a shared central branch (like `main` or `release`), it is immutable public history. You must leverage **`git revert`** to append a *new* corrective inverse snapshot commit on top. This protects your team's tracking environments from diverging history paths.

---

### 🚀 Lab Simulation Protocol

#### **1. Deploy a Vulnerable Branch Base State**
Initialize a local testing block and inject historical states to simulate parallel disaster types:
```bash
mkdir -p ~/git-rollback-lab && cd ~/git-rollback-lab
git init -b main

# Foundation stable footprint
echo "STABLE ARTIFACT" > app.java
git add app.java && git commit -m "feat: initial safe production release"
```

---

### 🔍 Diagnostic & Mitigation Workflows

#### **Workflow 1: Recovering from Local-Only Accidents**
If your terminal logs confirm that the broken commit block has **not** been pushed to a remote destination tracking tree:
```bash
# Execute a Soft Reset to dissolve the commit block while keeping staging files intact
git reset --soft HEAD~1
```

#### **Workflow 2: Recovering from Publicly Pushed Accidents**
If the broken code has already been pushed up to a shared branch architecture where other engineers are actively pulling dependencies:
```bash
# Append a public inverse corrective patch commit onto the branch timeline
git revert <BROKEN_COMMIT_SHA_HASH> --no-edit
```

---

### 🧯 Production Enforcement Guardrails

| Tool Constraint Modifier | Preferred Execution Scope | Operational System Impact |
| :--- | :--- | :--- |
| **`git reset --soft`** | Local Workstations Only | Erases the target commit block completely from history but leaves code intact in your staging index layer. |
| **`git reset --hard`** | High-Risk Local Destructive Work | Forcefully destroys both the commit block records AND all local modified working code files permanently. |
| **`git revert`** | Public Shared Feature/Main Lines | Non-destructive. Maintains an absolute audit trail while pushing seamless, clean fixes down to your team's workstations. |

#### **Continuous Branch Security Mandate**
To ensure team-wide compliance and eliminate human error risks completely, enforce Git branch protection rules at your platform management level (GitHub/GitLab organization dashboards):
1.  **Block direct commits to `main`:** Force all code alterations to traverse individual short-lived feature branch environments reviewed via Pull Requests.
2.  **Explicitly block force pushes (`--force`):** Disable history deletion capabilities across central core branches entirely.
