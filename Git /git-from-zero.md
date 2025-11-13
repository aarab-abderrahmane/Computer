# 🧠 Git From Zero – Complete Beginner’s Guide

Learn Git from scratch with clear explanations, examples, and best practices.

---

## **1️⃣ What is Git?**

Git is a **version control system** that lets you:

* Track changes in your code
* Restore old versions
* Work safely with others using branches

💡 Think of Git as a **timeline for your project**.

---

## **2️⃣ Install Git**

**Ubuntu:**

```bash
sudo apt update
sudo apt install git
```

**Windows/Mac:**
Download from [https://git-scm.com/](https://git-scm.com/)

Check installation:

```bash
git --version
```

---

## **3️⃣ Configure Git**

Set your user identity (only once per machine):

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

View all configs:

```bash
git config --list
```

**`--global`** → applies to all repos
**`--local`** → only for current repo

---

## **4️⃣ Initialize a Repository**

```bash
git init
```

Creates a `.git/` folder — Git starts tracking the project.

Example:

```bash
mkdir my-project && cd my-project
git init
```

---

## **5️⃣ Adding and Committing**

### **Add files**

```bash
git add file.txt     # Add one file
git add .            # Add all files
```

### **Commit changes**

```bash
git commit -m "Initial commit"
```

* `-m` → add message inline

### **Add & Commit in one step**

```bash
git commit -a -m "Fix bug"
```

* `-a` → automatically stages all *modified* tracked files.
  (New files still need `git add`.)

---

## **6️⃣ View Status and Differences**

### **Check file state**

```bash
git status
```

### **View unstaged changes**

```bash
git diff
```

### **View committed vs last commit**

```bash
git diff HEAD
```

---

## **7️⃣ Ignore Files**

Create `.gitignore` file:

```
node_modules/
.env
*.log
```

Git will ignore these files in commits.

---

## **8️⃣ Undo Changes**

### **Unstage (remove from staging area)**

```bash
git restore --staged file.txt
```

### **Discard local file changes**

```bash
git restore file.txt
```

Reverts to last committed version.

---

## **9️⃣ Rename or Move Files**

```bash
git mv oldname.txt newname.txt
```

* Equivalent to renaming and staging in one command.
  (`mv` = move)

---

## **🔟 Viewing History**

### **Full log**

```bash
git log
```

### **Short one-line log**

```bash
git log --oneline
```

### **Log with file changes**

```bash
git log -p
```

### **Change last commit message**

```bash
git commit --amend -m "New message"
```

### **Reset to previous commit**

```bash
git reset <commit-id>
```

Removes commits after that ID but keeps files locally.

---

## **11️⃣ Branching**

Branches let you work safely on new features.

### **List branches**

```bash
git branch
```

### **Create a new branch**

```bash
git branch feature-login
```

### **Switch branch**

```bash
git switch feature-login
# or
git checkout feature-login
```

### **Create and switch in one step**

```bash
git switch -c feature-login
```

### **Delete a branch**

```bash
git branch -d feature-login
```

---

## **12️⃣ Merging**

### **Merge branch into current**

```bash
git merge -m "Merge feature-login" feature-login
```

* Combines work from `feature-login` into current branch.
* If there are conflicts, Git will ask you to resolve them.

---

## **13️⃣ Rename the Main Branch**

```bash
git branch -M main
```

* `-M` → force rename, even if branch already exists.

---

## **14️⃣ Create README and Commit Example**

```bash
echo "# My Project" >> README.md
git add README.md
git commit -m "Add README file"
```

---

## **15️⃣ Connect to GitHub Remote**

### **Add remote**

```bash
git remote add origin https://github.com/username/repo.git
```

### **Verify**

```bash
git remote -v
```

---

## **16️⃣ Push and Pull**

### **First push**

```bash
git push -u origin main
```

* `-u` / `--set-upstream` → links local `main` with remote `main`, so you can use `git push` or `git pull` next time without arguments.

### **Push all branches**

```bash
git push --all
```

### **Fetch changes (without merging)**

```bash
git fetch
```

* Downloads remote commits but doesn’t modify your local code.
  Useful to **see what’s new before merging**.

Example:

```bash
git fetch
git log origin/main
```

### **Pull changes (fetch + merge)**

```bash
git pull
```

* `git pull` = `git fetch` + `git merge`
  It updates your local branch to match the remote one.

| Command     | Action                                 |
| ----------- | -------------------------------------- |
| `git fetch` | Download changes only (safe, no merge) |
| `git pull`  | Download + merge immediately           |

---

## **17️⃣ Difference Between Fetch and Pull (Example)**

Assume your teammate pushed new commits to GitHub.

* If you run:

```bash
git fetch
```

You’ll download the commits but **your local files won’t change** until you merge:

```bash
git merge origin/main
```

* If you run:

```bash
git pull
```

You’ll **fetch and merge automatically**, updating your local branch instantly.

---

## **18️⃣ Practical Workflow Example**

```bash
echo "# Demo Project" >> README.md
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/demo.git
git push -u origin main
```

Then for new features:

```bash
git switch -c feature-login
# work on feature
git add .
git commit -m "Add login feature"
git switch main
git merge feature-login
git push
```

---

## **19️⃣ Flags & Symbols Explained**

| Flag       | Meaning                      | Example                         |
| ---------- | ---------------------------- | ------------------------------- |
| `-m`       | Add commit message           | `git commit -m "msg"`           |
| `-a`       | Stage modified tracked files | `git commit -a -m "msg"`        |
| `-u`       | Link local & remote branches | `git push -u origin main`       |
| `-d`       | Delete branch                | `git branch -d feature`         |
| `-M`       | Force rename branch          | `git branch -M main`            |
| `-p`       | Show patch details           | `git log -p`                    |
| `--staged` | Affect staged files          | `git restore --staged file`     |
| `--global` | Apply config globally        | `git config --global user.name` |
| `--amend`  | Modify last commit           | `git commit --amend`            |

---

## **20️⃣ Authentication Fix (HTTPS Token or SSH)**

GitHub removed password login for `git push`.
Use **Personal Access Token (PAT)** or **SSH**.

### **Option 1 – Personal Access Token**

1. Create token at
   GitHub → Settings → Developer settings → Personal Access Tokens → Generate Token
2. Replace password with the token when pushing.

```bash
git push https://github.com/username/repo.git
Username: username
Password: <paste your token>
```

### **Option 2 – SSH (Recommended)**

```bash
ssh-keygen -t ed25519 -C "you@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

Copy and add it in GitHub → Settings → SSH keys
Then update your remote:

```bash
git remote set-url origin git@github.com:username/repo.git
git push
```

---

## **21️⃣ Common Mistakes**

| Problem                           | Solution                                           |
| --------------------------------- | -------------------------------------------------- |
| Pushed with wrong commit message  | `git commit --amend`                               |
| Accidentally deleted a file       | `git restore file.txt`                             |
| Can’t push to GitHub (auth error) | Use SSH or PAT                                     |
| Merge conflicts                   | Edit conflicted files → `git add .` → `git commit` |

---

## **22️⃣ Helpful Resources**

* [Git Documentation](https://git-scm.com/doc)
* [GitHub Docs](https://docs.github.com)
* [Learn Git Branching](https://learngitbranching.js.org/)

---

This README gives you:

* All core Git commands
* Examples and workflow
* Explanations of every flag
* Fetch vs Pull differences
* SSH / Token authentication setup


