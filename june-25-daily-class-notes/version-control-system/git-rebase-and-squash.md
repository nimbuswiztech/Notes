# Git Rebase and squash

### ✅ What is `git rebase`?

`git rebase` is a Git command used to **move or combine a sequence of commits** to a new base commit. It helps in maintaining a **linear and clean commit history**, especially useful in **collaborative projects** or **CI/CD environments**.

***

### 🧪 Real-Time Scenario: Feature Development in a Team Project

#### 👨‍💻 Project Setup

You are working in a **DevOps team** managing infrastructure as code (IaC) for a project. The `main` branch is used for production-ready infrastructure.

Now imagine this scenario:

*   You (DevOps Engineer) created a new feature branch to add **EKS cluster deployment**:

    ```
    git checkout -b eks-cluster-setup
    ```
* While you're working on your feature, someone else merged changes to the `main` branch related to **S3 bucket policies**.

Now your branch is **behind** the main branch. Before you create a pull request, you want to make sure your branch is **up-to-date** and has a **clean commit history**.

***

### 🎯 Goal

* Integrate the latest `main` branch changes into your `eks-cluster-setup` branch.
* Maintain a clean commit history (instead of a messy merge commit).

***

### 🔄 Rebase Flow Step-by-Step

#### 1. Start on your feature branch:

```bash
git checkout eks-cluster-setup
```

#### 2. Fetch the latest changes:

```bash
git fetch origin
```

#### 3. Rebase your feature branch onto the updated main:

```bash
git rebase origin/main
```

#### Behind the scenes, Git does:

* Takes the commits from your branch (`eks-cluster-setup`)
* Temporarily saves them
* Moves your branch pointer to `main`
* Re-applies your commits on top of `main`

This gives a **linear history** like:

```
main
  |
  o---o---o---S3 changes (main)
               \
                o---o---EKS changes (your branch rebased)
```

***

### 👨‍🏫 Teach This Use Case Like This:

> "Imagine you and your teammate are working on different parts of the infrastructure. Your teammate merges an important security fix to the `main` branch. Before you push your EKS setup code, you want to pull in the latest changes and ensure your history is clean. Instead of a merge, which creates a noisy commit graph, you `rebase` — making it appear as if your work happened **after** the security fix, keeping the commit history **linear and logical**."

***

### 🧨 What If There's a Conflict?

If the same file was edited in both branches (e.g., `main.tf`), you might see:

```bash
CONFLICT (content): Merge conflict in main.tf
```

To resolve:

1. Manually fix the file.
2.  Mark it as resolved:

    ```bash
    git add main.tf
    ```
3.  Continue the rebase:

    ```bash
    git rebase --continue
    ```

If you want to abort and go back:

```bash
git rebase --abort
```

***

### ✅ Final Step (After Rebase)

You need to force push since the history changed:

```bash
git push origin eks-cluster-setup --force
```

***

### 📊 When to Use Rebase

| Use Rebase When              | Avoid Rebase When                        |
| ---------------------------- | ---------------------------------------- |
| You want clean history       | On shared branches like `main`           |
| You’re preparing for PR      | If you're unsure about conflicts         |
| You work alone on the branch | If others are working on the same branch |

***

### 📝 Bonus Teaching Tip

Use this **real-world analogy**:

> “Think of `git merge` as taping two roads together. It works, but you can see the joint. `git rebase` is like re-paving your road from start to end — clean and continuous."

***

\
To **see what is inside a Git stash**, there are **multiple ways** depending on how deep you want to go — just the stash summary, or full file diffs. Here's everything in **teaching-friendly detail**:

***

### ✅ 1. **View List of All Stashes**

```bash
git stash list
```

#### Example Output:

```
stash@{0}: WIP on main: 34a8be2 added EKS config
stash@{1}: WIP on feature/login: b12f998 updated login logic
```

This shows all stash entries. Each stash is labeled as `stash@{n}`.

***

### ✅ 2. **View Summary of a Stash (Which Files Were Changed)**

```bash
git stash show stash@{0}
```

#### Example Output:

```
 main.tf | 5 +++--
 outputs.tf | 2 +-
```

This shows the **files changed and how many lines** were added/removed.

***

### ✅ 3. **View Full Diff of a Stash (Detailed File Changes)**

```bash
git stash show -p stash@{0}
```

Or simply:

```bash
git stash show --patch stash@{0}
```

#### Output:

You’ll get a **diff-style output** similar to `git diff`:

```diff
diff --git a/main.tf b/main.tf
index a12b5d3..6f6e8d4 100644
--- a/main.tf
+++ b/main.tf
@@ -1,4 +1,5 @@
 resource "aws_instance" "web" {
+  ami           = "ami-123456"
   instance_type = "t2.micro"
 }
```

***

### ✅ 4. **View Changes in All Stashes**

Loop over and show everything (for advanced/curious students):

```bash
git stash list | while read -r stash; do
  echo "===== $stash ====="
  git stash show -p "${stash%%:*}"
done
```

***

### ✅ 5. **Apply Without Dropping (Optional Demo Step)**

If students want to **see it live in their working directory**, do:

```bash
git stash apply stash@{0}
```

It will apply the stash but **keep it in the stash list**.

To remove it after applying:

```bash
git stash drop stash@{0}
```

***

### 🧑‍🏫 Teaching Tip (Real Time Analogy):

> "Think of Git stash as a temporary locker for your half-written code. You can check the list (`git stash list`), peek inside (`git stash show`), or even open the locker and inspect all details (`git stash show -p`)."



***

### ✅ What is Git Squash?

**Git squash** is the act of **combining multiple commits into a single commit** to keep the Git history clean and meaningful.

> 🔧 It is done using **interactive rebase**:
>
> ```bash
> git rebase -i HEAD~N
> ```

***

### 🎯 Real-Time Scenario: Infrastructure Automation with Terraform

#### 🧑‍💻 You’re working on a branch called `terraform-s3-setup`.

You made **5 small commits** like this:

1. `created main.tf`
2. `added provider block`
3. `added S3 bucket resource`
4. `fixed typo in S3 bucket name`
5. `added versioning to S3`

Now you want to **combine them into one commit**:

> `"Added S3 Terraform script with versioning"`

**Why?**

* It’s cleaner
* Reviewers don’t need to read trivial changes
* Maintains professionalism in production-grade DevOps code

***

### 🔁 Git Squash: Step-by-Step

#### ✅ Step 1: Check your commit history

```bash
git log --oneline
```

Example output:

```
abcde12 (HEAD -> terraform-s3-setup) added versioning to S3
bdcf120 fixed typo in S3 bucket name
cba123d added S3 bucket resource
aaa456e added provider block
1234abc created main.tf
```

#### ✅ Step 2: Start Interactive Rebase (to squash last 5 commits)

```bash
git rebase -i HEAD~5
```

#### ✅ Step 3: You'll see a screen like this:

```
pick 1234abc created main.tf
pick aaa456e added provider block
pick cba123d added S3 bucket resource
pick bdcf120 fixed typo in S3 bucket name
pick abcde12 added versioning to S3
```

#### ✅ Step 4: Change all lines **except the first** from `pick` to `squash` or `s`

```
pick 1234abc created main.tf
squash aaa456e added provider block
squash cba123d added S3 bucket resource
squash bdcf120 fixed typo in S3 bucket name
squash abcde12 added versioning to S3
```

> 🔄 This tells Git: keep the first commit and squash the rest into it.

#### ✅ Step 5: Git will now prompt to **edit the commit message**

Example:

```
# This is a combination of 5 commits.
# The first commit's message is:
created main.tf

# The commit messages of the other commits:
added provider block
added S3 bucket resource
fixed typo in S3 bucket name
added versioning to S3
```

👉 You can **edit this message** to:

```
Added Terraform script for S3 with versioning
```

Save and exit.

***

### ✅ Step 6: Force Push (if branch was pushed before)

```bash
git push origin terraform-s3-setup --force
```

***

### 🔍 Before vs After Squash

| Before Squash             | After Squash                        |
| ------------------------- | ----------------------------------- |
| 5 small commits           | 1 clean, descriptive commit         |
| Noisy commit history      | Clean and easy-to-review history    |
| Possible confusion for PR | Simple PR with a focused change set |

***

### 🧑‍🏫 Teaching Analogy:

> "Squashing commits is like preparing for a presentation. You clean up your rough notes (individual commits) and present a single clean slide (one commit) that conveys your entire idea."
