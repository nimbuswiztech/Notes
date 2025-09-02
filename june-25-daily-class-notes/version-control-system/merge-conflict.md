# Merge conflict

### ✅ What is a Merge Conflict?

A **merge conflict** occurs when Git **cannot automatically merge changes** from two branches — usually when **two or more people edit the same part of a file** differently.

Git stops the merge and asks **you to resolve the conflict manually**.

***

### ✅ Real-Time Scenario: Two DevOps Engineers Editing Same File

Imagine you're in a DevOps team working on a project. You and your teammate are editing the same file `deploy.yaml` in different branches.

***

### ✅ Scenario Breakdown

#### 🔹 Initial State:

The `develop` branch contains this line in `deploy.yaml`:

```yaml
replicas: 2
```

***

#### 🔹 Step 1: You create a new branch and edit it

```bash
git checkout -b feature/scale-up
```

You update it to:

```yaml
replicas: 5
```

You commit the change:

```bash
git commit -am "Increased replicas to 5"
```

***

#### 🔹 Step 2: Teammate also edits the same line in `develop` branch

Your teammate directly changes `develop`:

```yaml
replicas: 3
```

And commits:

```bash
git commit -am "Updated replicas to 3 for staging"
```

***

#### 🔹 Step 3: You try to merge `develop` into your branch

You want the latest changes from `develop`, so you run:

```bash
git checkout feature/scale-up
git merge develop
```

***

#### ❌ Git Error:

```
Auto-merging deploy.yaml
CONFLICT (content): Merge conflict in deploy.yaml
Automatic merge failed; fix conflicts and then commit the result.
```

***

### ✅ How Git Marks the Conflict

Your `deploy.yaml` file now looks like this:

```yaml
<<<<<<< HEAD
replicas: 5
=======
replicas: 3
>>>>>>> develop
```

This means:

* `HEAD` = your current branch (feature/scale-up)
* `develop` = incoming changes

***

### ✅ Step-by-Step: Resolving Merge Conflict

#### 🔹 Step 1: Open the conflicting file

You’ll see the conflict markers as above.

#### 🔹 Step 2: Edit and resolve

You decide the final version:

```yaml
replicas: 4  # or 5, or 3 — your choice based on team discussion
```

#### 🔹 Step 3: Add the resolved file

```bash
git add deploy.yaml
```

#### 🔹 Step 4: Complete the merge

```bash
git commit
```

Now your merge is complete 🎉

***

### ✅ Commands Recap

| Command              | Purpose                           |
| -------------------- | --------------------------------- |
| `git merge <branch>` | Attempt to merge the branch       |
| `git status`         | Show conflicted files             |
| `git add <file>`     | Mark conflict as resolved         |
| `git merge --abort`  | Cancel merge process              |
| `git log --merge`    | Show commits causing the conflict |

***

### ✅ Visual Git History

#### 🛠 Before Merge

```
develop:  A---B---C (replicas: 3)
             \
feature:       D---E (replicas: 5)
```

#### ⚠ Conflict occurs during merge!

***

### ✅ Analogy

🧠 **“Two cooks are editing the same recipe — one says ‘add 3 spoons of sugar’, the other says ‘add 5 spoons’. Git is the chef who says: ‘I can’t decide, you tell me what to do.’”**

***

### ✅ Best Practices to Avoid Merge Conflicts

| Tip                      | Description                                        |
| ------------------------ | -------------------------------------------------- |
| Pull frequently          | Avoid diverging too much from `develop`            |
| Communicate              | Let team members know if you're editing same files |
| Use smaller commits      | Easier to isolate and fix                          |
| Avoid editing same lines | Break work into components/files                   |
