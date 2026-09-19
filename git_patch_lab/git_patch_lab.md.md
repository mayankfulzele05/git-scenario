# DevOps & CloudOps Lab: Mastering Git Patch Mode (`git add -p`)

This hands-on lab demonstrates how to maintain a professional, atomic Git history. You will simulate a scenario where you accidentally made **two unrelated changes in a single configuration file** and use Git's patch mode to split them into clean, separate commits.

---

## 🛠️ Step 1: Set Up the Lab Environment

Open your terminal or Git Bash, copy-paste this block of commands, and press **Enter**. This creates a fresh folder, initializes Git, and creates a mock deployment file.

```bash
# 1. Create a new directory and move into it
mkdir git-patch-lab && cd git-patch-lab

# 2. Initialize a fresh Git repository
git init

# 3. Create a starting configuration file
cat << 'EOF' > deploy.conf
# This config handles the application deployment settings.
# Ensure the backend endpoint is secure.
PORT=8080
ENVIRONMENT=production
EOF

# 4. Make an initial commit so Git has a baseline history
git add deploy.conf
git commit -m "Initial commit with deployment config"
```

---

## 📝 Step 2: Make Two Unrelated Changes

Simulate a "flow state" where you accidentally fix a typo **AND** change an infrastructure port at the exact same time before committing. 

Run this command to overwrite the file with your multi-tasked edits:

```bash
cat << 'EOF' > deploy.conf
# This config handles the application deployment settings.
# Ensure the backend endpoint is secure. (Fixed typo: end-point -> endpoint)
PORT=443
ENVIRONMENT=production
EOF
```

### What changed?
1. **Documentation Change:** You fixed a typo in the comment line (`end-point` ➔ `endpoint`).
2. **Infrastructure Change:** You changed the port from `8080` to `443` (Production HTTPS).

---

## ⚡ Step 3: Run the Patch Command

To surgically separate these changes instead of staging the whole messy file, launch the interactive patch mode:

```bash
git add -p deploy.conf
```

---

## 🎮 Step 4: Complete the Interactive Prompt

Git will present the modified lines and ask: `Stage this hunk [y,n,q,a,d,j,J,g,/,e,?]?` 

Execute these inputs in your terminal session:

1. **Type `s` and press Enter:** This tells Git to **(s)plit** the file changes into smaller chunks because the typo and the port change are separate issues.
2. **Type `y` and press Enter:** Git will display the typo fix first. Type **(y)es** to stage this documentation change.
3. **Type `n` and press Enter:** Git will then display the `PORT=443` line. Type **(n)o** to skip staging it for now.

The interactive prompt will close automatically.

---

## 🔎 Step 5: Verify and Commit Separately

Run `git status`. You will see `deploy.conf` listed under **both** staged changes and unstaged changes.

Now, finalize your clean, atomic history by committing them one by one:

```bash
# 1. Commit the typo fix (which is currently staged)
git commit -m "docs: fix typo in deployment configuration comment"

# 2. Stage the remaining infrastructure change
git add deploy.conf

# 3. Commit the port change
git commit -m "infra: update application port to production HTTPS 443"
```

### 🏆 The Grand Reveal
Run the following command to see your beautiful, professional, sequential commit log:

```bash
git log --oneline
```
