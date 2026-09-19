# DevOps Troubleshooting Lab: Git Bisect Bug Hunt

This lab simulates a real-world CloudOps troubleshooting scenario. You will create a mock infrastructure settings file, simulate a history of 10 sequential commits, introduce a bug silently, and use `git bisect` to discover exactly which commit caused the failure.

---

## 🛠️ Step 1: Set Up the Lab Environment

Open your terminal or Git Bash, copy-paste this block of commands, and press **Enter**. This script creates a fresh repository and builds a 10-commit timeline automatically.

```bash
# 1. Create a new directory and move into it
mkdir git-bisect-lab && cd git-bisect-lab

# 2. Initialize a fresh Git repository
git init

# 3. Create the starting safe configuration file
echo "APP_VERSION=1.0" > app.env
echo "MAX_CONNECTIONS=100" >> app.env
git add app.env && git commit -m "Initial working configuration"
git tag v1.0.0

# 4. Automate 8 more commits, silently injecting a bug at Commit #5
for i in {2..9}; do
    if [ $i -eq 5 ]; then
        # The bug is injected here: MAX_CONNECTIONS is corrupted to letters
        echo "MAX_CONNECTIONS=XYZ" >> app.env
        git add app.env && git commit -m "infra: tweak connection limits for performance"
    else
        echo "CONFIG_PARAM_$i=value_$i" >> app.env
        git add app.env && git commit -m "chore: routine configuration update $i"
    fi
done

# 5. Add the final commit (the current broken state)
echo "DEPLOYMENT_STATUS=active" >> app.env
git add app.env && git commit -m "deploy: update final deployment status flag"
```

---

## 🔍 Step 2: Confirm the System is Broken

Let's simulate running our application right now. Check the configuration file to see what it looks like:

```bash
cat app.env
```

You will see `MAX_CONNECTIONS=XYZ` tucked inside the file. Because it is set to letters instead of a number, the application crashes on boot. The current state is definitely **BAD**.

---

## 🎯 Step 3: Start the Git Bisect Hunt

Now, launch the binary search wizard to find out who broke the parameter:

```bash
# 1. Start the troubleshooting mode
git bisect start

# 2. Mark the current state as broken
git bisect bad

# 3. Mark the initial tag (v1.0.0) as known good
git bisect good v1.0.0
```

---

## 🎮 Step 4: Play the Interactive Guessing Game

Git will instantly split your 10 commits in half, drop you into a **Detached HEAD** state right in the middle of your history, and tell you how many steps are left. 

Look at your terminal screen. For each step Git drops you into, check the contents of the file and answer Git:

1. Run **`cat app.env`** to see if the file is healthy or broken at this specific moment in time.
2. If you see `MAX_CONNECTIONS=100`, the file is healthy! Type **`git bisect good`** and press Enter.
3. If you see `MAX_CONNECTIONS=XYZ`, the file is broken! Type **`git bisect bad`** and press Enter.

Repeat this process **2 or 3 times**. Git will continually narrow down the history.

---

## 🏆 Step 5: The Culprit Identified

Within a few prompts, Git will stop bisecting and display a final report on your screen showing the exact commit hash, author name, date, and message:

```text
a1b2c3d4e5f6g7h8i9j0 is the first bad commit
commit a1b2c3d4e5f6g7h8i9j0
Author: Your Name <you@example.com>
Date:   Today

    infra: tweak connection limits for performance
```

### 🧼 Step 6: Clean Up and Exit
Once you have noted down the broken commit information, exit the bisect environment and return safely back to your active `main` branch:

```bash
git bisect reset
```
