# Git squash

### ✅ What is Git Squash?

Git squash is the process of **combining multiple commits into a single commit** to keep your Git history **clean, readable, and professional**.

It’s done using:

```bash
git rebase -i HEAD~<number>
```

***

### ✅ Real-Time Scenario: Clean Up Before Merge

Let’s say you’re working on a **feature branch**:

```bash
git checkout -b feature/login-page
```

During development, you made multiple small commits like:

```bash
git commit -m "Added login HTML"
git commit -m "Fixed login form alignment"
git commit -m "Added login validation"
git commit -m "Fixed typo in login button"
```

Now you’re ready to **merge** this branch into `develop`, but you don't want to pollute the `develop` branch with 4 tiny commits. Instead, you want to **squash them into one meaningful commit** like:

> `"Implemented login page with validation and UI fixes"`

***

### ✅ Why Squash?

| 🔍 Before Squash (4 commits) | 🎯 After Squash (1 clean commit) |
| ---------------------------- | -------------------------------- |
| Messy and noisy history      | Clean and meaningful history     |
| Hard to track change sets    | Easier to revert/analyze later   |

***

### ✅ Step-by-Step: Git Squash Workflow

Let’s squash the last 4 commits into 1.

#### 🔹 Step 1: Start interactive rebase

```bash
git rebase -i HEAD~4
```

#### 🔹 Step 2: You’ll see something like this in the editor:

```
pick  a1b2c3d Added login HTML
pick  b2c3d4e Fixed login form alignment
pick  c3d4e5f Added login validation
pick  d4e5f6g Fixed typo in login button
```

#### 🔹 Step 3: Change all commits **after the first** from `pick` to `squash` (or `s`):

```
pick  a1b2c3d Added login HTML
squash b2c3d4e Fixed login form alignment
squash c3d4e5f Added login validation
squash d4e5f6g Fixed typo in login button
```

#### 🔹 Step 4: Git will now ask you to **edit the commit message**

You can either:

* Combine them into one message:

```
Implemented login page with validation and UI fixes
```

Or

* Keep all messages if useful.

Then save and close the editor.

***

#### 🔹 Step 5: Push with `--force` (because rebase rewrites history)

```bash
git push origin feature/login-page --force
```

***

### ✅ Bonus Tips

| Command                | Description                                  |
| ---------------------- | -------------------------------------------- |
| `git log --oneline`    | See commits in short form                    |
| `git rebase -i HEAD~n` | Squash last n commits                        |
| `git push --force`     | Required after rebase (but warn about usage) |

***

### ✅ Analogy for Students

**“Squashing is like taking all rough drafts of a paragraph and combining them into a final polished paragraph before submitting your assignment.”**

***
