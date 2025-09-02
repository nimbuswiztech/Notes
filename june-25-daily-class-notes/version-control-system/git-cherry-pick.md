# git cherry-pick

### ✅ What is `git cherry-pick`?

`git cherry-pick` is a Git command that **copies a specific commit** from one branch and **applies it to another branch** — without merging everything.

👉 It lets you pick just _one cherry_ (commit) from the whole tree (branch)!

***

### ✅ Real-Time Scenario: Hotfix in Production (DevOps Context)

Let’s say your team is working on a **Terraform IaC repo**. You’re in a feature branch:

```
feature/improve-eks-module
```

Meanwhile, a **critical hotfix** was applied directly to the `main` branch to fix an S3 bucket policy:

```bash
git checkout main
# Fix applied
git commit -m "Fix: Removed public access from S3 bucket policy"
```

Now, you realize this fix is **also needed in your current feature branch**, but you **don’t want to merge all changes** from `main`.

***

### ✅ Solution: Use `git cherry-pick`

This allows you to bring **just that one commit** into your feature branch.

***

### 🧪 Step-by-Step: Cherry-Pick Workflow

#### 🔹 Step 1: Find the commit hash

Switch to `main` and find the commit you want:

```bash
git log --oneline
```

Example output:

```
a1b2c3d Fix: Removed public access from S3 bucket policy
...
```

#### 🔹 Step 2: Switch to your feature branch

```bash
git checkout feature/improve-eks-module
```

#### 🔹 Step 3: Cherry-pick the commit

```bash
git cherry-pick a1b2c3d
```

This **copies that one commit** and applies it to your current branch.

***

### ✅ When to Use `git cherry-pick`

| Use Case                                      | Description                                                    |
| --------------------------------------------- | -------------------------------------------------------------- |
| 🔥 Hotfix needs to go to multiple branches    | E.g., Fix in `main` needed in `develop` and a `release` branch |
| 🧪 Reuse a small piece of code                | Without merging the full branch                                |
| 💡 Pick only good commits from a messy PR     | Selectively apply only approved changes                        |
| 🧹 Pick commits during release branch cleanup | Move specific improvements without all noise                   |

***

### ✅ What Happens Internally?

Visual before and after:

#### 🛠 Before:

```
main:    A---B---C---[Fix: S3 issue]
                      \
feature:               D---E---F
```

#### ✅ After cherry-pick:

```
main:    A---B---C---[Fix: S3 issue]
                      \
feature:               D---E---F---[Fix: S3 issue]
```

The commit is **duplicated**, not moved.

***

### ✅ Conflict Handling

If there's a conflict:

```bash
CONFLICT (content): Merge conflict in terraform/s3.tf
```

Resolve it manually, then:

```bash
git add terraform/s3.tf
git cherry-pick --continue
```

If you want to cancel the operation:

```bash
git cherry-pick --abort
```

***

### ✅ Teaching Tip

Use this **classroom activity**:

1. Have 3 branches: `main`, `feature1`, `feature2`
2. Add a unique commit to `main`
3. Let students cherry-pick it into `feature1`
4. Use `git log` and `git show` to compare commit messages

Also show:

```bash
git cherry-pick <start-commit>^..<end-commit>  # Multiple commits
```

***

### ✅ Bonus: Cherry-pick Multiple Commits

```bash
git cherry-pick a1b2c3d f6g7h8i
```

Or a range:

```bash
git cherry-pick a1b2c3d^..f6g7h8i
```

***

### ✅ Analogy&#x20;

\*\*“Cherry-picking is like copying one special topping from a pizza and putting it on another without ordering the whole pizza again.” 🍒🍕

***

### ✅ Caution

* Avoid cherry-picking large groups of commits — prefer rebase/merge.
* Cherry-picking introduces **duplicate commits** if you're not careful.
* Use only when you **don’t want full branch history**.
