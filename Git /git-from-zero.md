

# 📚 Git from Zero – Beginner’s Guide

Welcome to **Git from Zero**! This README will guide you step by step from understanding Git concepts to using commands, branching, and collaborating with GitHub.

---

## **1️⃣ What is Git?**

Git is a **version control system (VCS)** that helps you track changes in your code over time.

**Key benefits:**

* Save snapshots of your code (commits)
* Go back to previous versions
* Collaborate with other developers safely

💡 **Analogy:** Git is like a “time machine” for your code.

---

## **2️⃣ Git vs GitHub**

* **Git** → Local version control on your computer
* **GitHub** → Online platform for hosting Git repositories, collaborating, and sharing code

---

## **3️⃣ Installing Git**

* **Linux (Ubuntu):**

```bash
sudo apt update
sudo apt install git
```

* **Windows:** Download [Git](https://git-scm.com/) and install

* **Mac:**

```bash
brew install git
```

Check version:

```bash
git --version
```

---

## **4️⃣ Basic Git Workflow**

1. Initialize repository → `git init`
2. Make changes → `git add`
3. Save changes → `git commit`
4. Share online → `git push`
5. Update local → `git pull`

---

## **5️⃣ Creating and Using a Repository**

### **Initialize a repository**

```bash
git init
```

### **Check repository status**

```bash
git status
```

### **Add files to staging area**

```bash
git add filename   # single file
git add .          # all files
```

### **Commit changes**

```bash
git commit -m "Initial commit"
```

### **Connect to remote repository**

```bash
git remote add origin https://github.com/username/repo.git
```

### **Push changes to GitHub**

```bash
git push -u origin main
```

### **Pull updates from remote**

```bash
git pull origin main
```

---

## **6️⃣ Branching in Git**

Branches are **parallel versions of your project**. They let you work on features or fixes without affecting the main code.

### **Common commands**

```bash
git branch            # list branches
git branch feature    # create branch
git checkout feature  # switch branch
git checkout -b feature  # create + switch
git merge feature     # merge into current branch
git branch -d feature # delete branch locally
```

---

### **Example Workflow**

1. Create a new branch for a feature:

```bash
git checkout -b feature-login
```

2. Make changes and commit:

```bash
git add .
git commit -m "Add login feature"
```

3. Switch back to main:

```bash
git checkout main
```

4. Merge feature branch:

```bash
git merge feature-login
```

5. Push branches to GitHub:

```bash
git push origin main
git push origin feature-login
```

**Visual Representation:**

```
main
  |
  o---o---o      <- commits on main
       \
        o---o   <- commits on feature-login
```

After merging:

```
main
  |
  o---o---o---o---o
       \
        o---o
```

---

## **7️⃣ Cloning an Existing Repository**

```bash
git clone https://github.com/username/repo.git
cd repo
```

---

## **8️⃣ Undoing Changes**

* **Discard unstaged changes:**

```bash
git checkout -- filename
```

* **Remove staged changes:**

```bash
git reset HEAD filename
```

* **Revert a commit:**

```bash
git revert <commit_hash>
```

---

## **9️⃣ Recommended Git Workflow**

1. `git pull` → update local repo
2. `git checkout -b feature-branch` → create a branch
3. Make changes → `git add` + `git commit`
4. `git push origin feature-branch` → push branch
5. Create Pull Request (PR) → review & merge

---

## **🔑 Tips for Beginners**

* Keep `main` stable
* Create branches for every new feature
* Use descriptive branch names:

  * `feature-login`
  * `bugfix-header`
  * `hotfix-typo`
* Write clear commit messages:
  `"Add login validation"` instead of `"changes"`

---

## **10️⃣ Resources to Learn More**

* [Official Git Documentation](https://git-scm.com/doc)
* [GitHub Docs](https://docs.github.com/)
* Interactive Tutorial: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)

---

This README is a **complete beginner’s guide**, from zero knowledge to making branches, commits, and pushing to GitHub.


