# git

## Real-Time Scenario-Based Git Interview Questions and Detailed Answers <a href="#id-20-real-time-scenario-based-git-interview-question" id="id-20-real-time-scenario-based-git-interview-question"></a>

**Key Takeaway:** Mastering these scenario-based questions will help students demonstrate practical Git proficiency—ranging from conflict resolution and branching strategies to advanced workflows and recovery techniques—ensuring they’re prepared for real-world DevOps interviews.

### 1. Collaborative Feature Development <a href="#id-1-collaborative-feature-development" id="id-1-collaborative-feature-development"></a>

**Question:** Your team is working on a new feature. How do you structure branches and collaborate to keep `main` stable?\
**Answer:**\
Use a **feature branch** workflow:

*   Create a feature branch off `main`:

    ```
    git checkout -b feature/awesome-feature main
    ```
* Push to remote and open a pull request (PR).
*   Regularly pull changes from `main` into the feature branch to minimize conflicts:

    ```
    git fetch origin
    git merge origin/main
    ```
*   After review, merge back into `main` with `--no-ff` to preserve history:

    ```
    git checkout main
    git merge --no-ff feature/awesome-feature
    git push origin main
    ```

### 2. Hotfix in Production <a href="#id-2-hotfix-in-production" id="id-2-hotfix-in-production"></a>

**Question:** A critical bug is found in production. Describe your Git steps to patch it without disrupting ongoing work.\
**Answer:**

*   Checkout `main` (production):

    ```
    git checkout main
    ```
*   Create a hotfix branch:

    ```
    git checkout -b hotfix/urgent-fix
    ```
*   Apply fix, commit, and push:

    ```
    git commit -am "Fix critical production bug"
    git push origin hotfix/urgent-fix
    ```
*   Open PR, merge into `main`, then tag the release:

    ```
    git tag -a v1.0.1 -m "Production hotfix"
    git push origin --tags
    ```
* Finally, merge `main` back into `develop` or feature branches to propagate the fix.

### 3. Complex Merge Conflict <a href="#id-3-complex-merge-conflict" id="id-3-complex-merge-conflict"></a>

**Question:** Two long-lived branches diverged heavily. How do you merge and resolve conflicts?\
**Answer:**

*   Ensure both branches are up-to-date:

    ```
    git checkout branchA && git fetch && git rebase origin/main
    git checkout branchB && git fetch && git rebase origin/main
    ```
*   Merge one into the other locally:

    ```
    git checkout branchA
    git merge branchB
    ```
* For each conflict marker (`<<<<<<<`), manually edit the file to combine or choose changes.
*   Stage and commit resolutions:

    ```
    git add <resolved-files>
    git commit
    ```
* Run full test suite to verify integrity before pushing.

### 4. Bisecting a Bug <a href="#id-4-bisecting-a-bug" id="id-4-bisecting-a-bug"></a>

**Question:** A bug was introduced sometime last week. How do you find the offending commit?\
**Answer:**\
Use `git bisect`:

```
git bisect start
git bisect bad            # current commit
git bisect good v1.2.3    # last known good tag
```

Git checks out midway; test each version and mark as good/bad:

```
git bisect good
git bisect bad
```

Repeat until Git pinpoints the bad commit. Finish with:

```
git bisect reset
```

### 5. Interactive Rebase for Cleanup <a href="#id-5-interactive-rebase-for-cleanup" id="id-5-interactive-rebase-for-cleanup"></a>

**Question:** You have 10 “WIP” commits. How do you squash them into logical units?\
**Answer:**

*   Start interactive rebase onto the parent of the first WIP:

    ```
    git rebase -i HEAD~10
    ```
* Mark the first commit as `pick` and others as `squash` or `fixup`.
* Edit the combined commit message when prompted.
*   Resolve any conflicts, then continue:

    ```
    git rebase --continue
    ```

### 6. Undoing a Public Commit <a href="#id-6-undoing-a-public-commit" id="id-6-undoing-a-public-commit"></a>

**Question:** You accidentally pushed sensitive data. How do you remove it from history?\
**Answer:**

*   Use `git filter-repo` or BFG Repo-Cleaner (modern replacement for `filter-branch`):

    ```
    git filter-repo --path sensitive.txt --invert-paths
    ```
*   Force-push rewritten history to remote:

    ```
    git push --force-with-lease origin main
    ```
* Inform collaborators to re-clone or reset their local copies.

### 7. Temporarily Stashing Work <a href="#id-7-temporarily-stashing-work" id="id-7-temporarily-stashing-work"></a>

**Question:** You need to switch branches but have uncommitted changes. How do you proceed?\
**Answer:**

*   Stash current work:

    ```
    git stash push -m "WIP: experiment"
    ```
*   Switch branch:

    ```
    git checkout other-branch
    ```
*   After finishing, reapply stash:

    ```
    git stash pop
    ```
* If conflicts arise, resolve them and commit.

### 8. Cherry-Picking a Commit <a href="#id-8-cherry-picking-a-commit" id="id-8-cherry-picking-a-commit"></a>

**Question:** A useful bugfix in another branch needs to go into `main`. How?\
**Answer:**

* Identify commit hash in source branch.
*   Checkout `main` and cherry-pick:

    ```
    git checkout main
    git cherry-pick ab12cd34
    ```
* Resolve any conflicts, then push.

### 9. Reverting a Merge <a href="#id-9-reverting-a-merge" id="id-9-reverting-a-merge"></a>

**Question:** A merge introduced multiple bugs. How do you revert it cleanly?\
**Answer:**

*   Use `git revert` with the `-m` flag to specify the parent:

    ```
    git revert -m 1 <merge-commit-hash>
    ```
* This creates a new commit undoing the merge changes while preserving history.

### 10. Detached HEAD Recovery <a href="#id-10-detached-head-recovery" id="id-10-detached-head-recovery"></a>

**Question:** You checked out a specific commit and realize you need to preserve your work. What do you do?\
**Answer:**

*   Create a new branch at detached HEAD:

    ```
    git checkout -b recover-work
    ```
* Continue working on that branch, then merge or push as needed.

### 11. Branch Protection and Pull Requests <a href="#id-11-branch-protection-and-pull-requests" id="id-11-branch-protection-and-pull-requests"></a>

**Question:** How do you enforce code reviews before merging into `main`?\
**Answer:**

* On Git hosting (e.g., GitHub), enable **branch protection rules** for `main` requiring:
  * Status checks (e.g., CI pipeline passing).
  * At least one approval on PRs.
* Developers fork or branch, open PRs, and only merged after automated checks and reviews.

### 12. Handling Large Files <a href="#id-12-handling-large-files" id="id-12-handling-large-files"></a>

**Question:** Your repo has binaries exceeding 100 MB. What’s the solution?\
**Answer:**\
Use **Git LFS**:

```
git lfs install
git lfs track "*.mp4"
git add .gitattributes
git commit -m "Enable LFS for videos"
git add largefile.mp4
git commit -m "Add video via LFS"
git push
```

Large files are stored externally, pointers remain in Git.

### 13. Submodule Management <a href="#id-13-submodule-management" id="id-13-submodule-management"></a>

**Question:** You maintain a third-party library as a Git submodule. How do you update it?\
**Answer:**

*   Enter submodule directory:

    ```
    cd libs/thirdparty
    git fetch origin
    git checkout v2.0.0
    git pull
    cd ../..
    ```
*   Update submodule reference in main repo:

    ```
    git add libs/thirdparty
    git commit -m "Update submodule to v2.0.0"
    git push
    ```

### 14. CI Integration Scenario <a href="#id-14-ci-integration-scenario" id="id-14-ci-integration-scenario"></a>

**Question:** You want every PR to trigger tests in Jenkins before merging. How is Git configured?\
**Answer:**

* Push PRs to a remote development branch.
* Jenkins polls Git or receives webhooks on PR events.
* Jenkinsfile in the repo defines stages (checkout, build, test).
* Jenkins reports back pass/fail status on the PR.
* Branch protection blocks merge unless checks pass.

### 15. Reset vs. Revert <a href="#id-15-reset-vs-revert" id="id-15-reset-vs-revert"></a>

**Question:** What’s the difference, and when do you use each?\
**Answer:**

* **`git reset --hard <commit>`**: Moves HEAD and branch pointer, rewriting history. Use on **private** branches to discard local commits.
* **`git revert <commit>`**: Creates a new commit that undoes changes. Safe for **public** branches; history remains intact.

### 16. Backup and Recovery <a href="#id-16-backup-and-recovery" id="id-16-backup-and-recovery"></a>

**Question:** Your local repo is corrupted. How do you restore it?\
**Answer:**

*   Clone a fresh copy from remote:

    ```
    git clone <repo-url>
    ```
* If unpushed local commits exist, back up `.git` directory before corruption and recover with `git fsck` or by copying objects into new clone’s `.git/objects`.

### 17. Tagging for Releases <a href="#id-17-tagging-for-releases" id="id-17-tagging-for-releases"></a>

**Question:** How do you mark version 2.0.0 and ensure it’s on remote?\
**Answer:**

*   Create annotated tag:

    ```
    git tag -a v2.0.0 -m "Release v2.0.0"
    ```
*   Push tag:

    ```
    git push origin v2.0.0
    ```

### 18. Monorepo Workflow <a href="#id-18-monorepo-workflow" id="id-18-monorepo-workflow"></a>

**Question:** You use a monorepo with multiple services. How do you isolate changes?\
**Answer:**

* Use path-based Jenkins/GitHub Actions triggers to run only affected service tests.
* Create feature branches scoped to service directories.
* Merge via PRs that run CI only on changed paths.

### 19. Pre-commit Hooks for Quality <a href="#id-19-pre-commit-hooks-for-quality" id="id-19-pre-commit-hooks-for-quality"></a>

**Question:** How do you enforce linting and tests before every commit?\
**Answer:**

*   Add a **pre-commit** hook in `.git/hooks/pre-commit`:

    ```
    #!/bin/sh
    npm run lint || exit 1
    npm test  || exit 1
    ```
*   Make it executable:

    ```
    bashchmod +x .git/hooks/pre-commit
    ```
* Now `git commit` fails if lint/tests fail, ensuring code quality.

### 20. Remote-Only Workflows <a href="#id-20-remote-only-workflows" id="id-20-remote-only-workflows"></a>

**Question:** You joined a project with no local write access; only PRs are allowed. How do you contribute?\
**Answer:**

* Fork the repo to your personal namespace.
*   Clone your fork:

    ```
    git clone git@github.com:you/project.git
    ```
*   Add upstream remote:

    ```
    git remote add upstream git@github.com:org/project.git
    ```
*   Sync with upstream main:

    ```
    git fetch upstream && git checkout main && git merge upstream/main
    ```
* Create topic branch, commit, push to your fork, and open a PR against `upstream/main`.
