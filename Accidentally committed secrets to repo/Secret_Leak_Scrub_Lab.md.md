# 📂 Incident Response Playbook: Purging Secrets from Git History (Q59)

This playbook establishes standard operational procedures (SOPs) for identifying, trapping, and completely scrubbing exposed secrets or API credentials out of a repository's full commit tracking history.

---

### 🧠 The Core Architecture: The Immutable Git Ledger

Git is architected as a persistent graph mapping content snapshots over time. When a file modification or deletion occurs, Git records a *new* block entry while preserving all ancestral snapshots completely intact. Because of this design, simply executing a standard `git rm` command only clears the file from the latest commit—leaving the exposed keys completely intact in previous history logs.

During an incident remediation window, you must immediately assume that the secret has been compromised. Scrubbing the historical database prevents further exploitation from bad actors scraping public cloud storage trees.

---

### 🚀 Lab Simulation Protocol

#### **1. Generate a Compromised Timeline**
Initialize a local environment tracker and deliberately leak an environment file deep into historical snapshots:
```bash
mkdir -p ~/git-secrets-lab && cd ~/git-secrets-lab
git init -b main

# Baseline stable commit
echo "APP_ENV=production" > config.env
git add config.env && git commit -m "feat: base layout config"

# Compromised leak commit
cat << 'EOF' > secrets.env
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
EOF
git add secrets.env && git commit -m "chore: leak operational access tokens"

# Subsequent progress commit
echo "PORT=8080" >> config.env
git add config.env && git commit -m "feat: expand configuration mapping variables"
```

---

### 🔍 Diagnostic Workflow (The Leak Assessment)

#### **1. Track Historical File References**
Query the object tree database to inspect historical file references across your commits:
```bash
git log --all --name-status | grep "secrets.env"
```

---

### 🧯 Incident Remediation

#### **1. Rotate and Invalidate the Secret Immediately**
> ⚠️ **CRITICAL SECURITY MANDATE:** Changing the Git history does *not* undo an active API leak. Your very first action must always be to revoke and rotate the secret on the cloud provider side (e.g., AWS, GCP, GitHub Personal Tokens) before working on the local Git repository.

#### **2. Rewrite the Repository Commit Graph Database**
Invoke the high-performance utility tool `git-filter-repo` (or native filtering loops) to rewrite history and drop all tracking paths linked to the target file name:
```bash
git filter-branch --force --index-filter \
"git rm --cached --ignore-unmatch secrets.env" \
--prune-empty --tag-name-filter cat -- --all
```

#### **3. Push Overrides to Remote Servers**
Once the local history graph is completely sanitized, execute a hard force push block to overwrite all tracking segments up on the primary remote repository:
```bash
# WARNING: This overwrites remote tracks; coordinate with the engineering team
git push origin --force --all
git push origin --force --tags
```

#### **4. Proactive Production Guardrails**
Integrate pre-commit hooks containing automated static scanners like **`gitleaks`** or **`trufflehog`** directly into your CI/CD pipelines to intercept and block keys before they leave a local workstation:
```bash
# Block commits containing sensitive credential patterns matching specific profiles
pre-commit install
```


<img width="1927" height="1080" alt="Screenshot (86)" src="https://github.com/user-attachments/assets/565eedbf-5b61-4e75-9a5c-05cc6163ac9c" />

