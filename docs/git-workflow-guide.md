# Git Workflow Guide: Branching, Merging and Remote Collaboration

## 1. Introduction

Git is a distributed version control system that helps developers track changes to files, collaborate with other developers and manage different versions of a project.

GitHub is a cloud-based platform where developers host Git repositories and collaborate on projects.

This guide documents practical Git workflows completed while building my Cloud Support Engineering portfolio.

## 2. Checking repository status

Before making changes, check the current state of the repository.

```bash
git status
```

This command shows:
- The current branch.
- Whether the branch is ahead of or behind its remote counterpart.
- Modified, staged or untracked files.
- Whether the working tree is clean.

## 3. Creating and switching branches

Branches allow developers to work on features or fixes independently without immediately changing the main branch.

Create a feature branch:

```bash
git branch feature/portfolio-improvements
```

Switch to it:

```bash
git switch feature/portfolio-improvements
```

Create and switch to a branch in one command:

```bash
git switch -c feature/new-feature
```

List available local branches:

```bash
git branch
```

## 4. Staging and committing changes

After modifying files, stage the changes:

```bash
git add branch-practice.txt
```

Commit the staged changes:

```bash
git commit -m "Add branch practice file"
```

A commit records a snapshot of the staged changes in the repository's history.

## 5. Merging branches

Merging brings changes from one branch into another.

First, switch to the branch that should receive the changes:

```bash
git switch main
```

Then merge the feature branch:

```bash
git merge feature/portfolio-improvements
```

### Fast-forward merge

A fast-forward merge happens when the receiving branch has no new commits that diverge from the feature branch.

Git can simply move the receiving branch pointer forward.

## 6. Understanding and resolving merge conflicts

A merge conflict happens when Git cannot automatically combine competing changes, often because different branches modified the same part of a file.

Git may display:

```text
CONFLICT (content): Merge conflict in branch-practice.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Conflict markers look like this:

```text
<<<<<<< HEAD
Changes from the current branch
=======
Changes from the incoming branch
>>>>>>> feature/portfolio-improvements
```

Meaning:
- `<<<<<<< HEAD`: Beginning of the current branch's version.
- `=======`: Separates the two versions.
- `>>>>>>>`: Marks the end of the incoming branch's version.

### Resolving a conflict

1. Open the affected file.
2. Review both versions.
3. Decide which changes to keep, or combine them.
4. Remove all conflict markers.
5. Save the corrected file.
6. Stage the resolved file.
7. Commit the merge resolution.

Commands:

```bash
git add branch-practice.txt
git commit -m "Resolve merge conflict in branch practice file"
```

The commit completes the merge that was already in progress. You do not need to run the original merge command again.

## 7. Understanding Git remote repositories

A remote is a reference to a repository hosted elsewhere, such as GitHub.

List configured remotes:

```bash
git remote -v
```

View local and remote-tracking branches:

```bash
git branch -a
```

The `origin` name commonly refers to the main remote repository.

For example:
- `main`: My local branch.
- `origin/main`: My local record of the remote main branch.

## 8. Pushing changes to GitHub

Push local commits to the remote repository:

```bash
git push origin main
```

This sends committed local changes on `main` to the remote `main` branch.

GitHub uses secure authentication for Git operations. Personal access tokens may be used where required instead of account passwords.

Never share access tokens or commit them to a repository.

## 9. Fetching remote changes

The fetch command downloads new commits and updates remote-tracking references without integrating those changes into the current local branch.

```bash
git fetch origin
```

After fetching, inspect the repository:

```bash
git status
git log --oneline --graph --all -5
```

To inspect differences between local main and the fetched remote main:

```bash
git diff main origin/main
```

Fetching allows a developer to review remote changes before integrating them.

## 10. Pulling remote changes

The pull command fetches remote changes and integrates them into the current branch.

```bash
git pull origin main
```

A pull normally performs a fetch followed by a merge, unless another integration strategy is configured.

After pulling, verify the result:

```bash
git status
```

## 11. Fetch vs Pull vs Push

| Command | Purpose |
|---|---|
| `git fetch origin` | Downloads remote updates without integrating them into the current branch. |
| `git pull origin main` | Fetches and integrates changes into the current branch. |
| `git push origin main` | Sends local commits to GitHub. |

A simple way to remember:

- Fetch: Check what's new remotely.
- Pull: Bring remote changes into my local branch.
- Push: Send my committed changes to the remote repository.

## 12. Practical collaboration exercise

To understand remote collaboration, I simulated another developer making a change directly on GitHub.

The exercise followed these steps:

1. Opened the GitHub repository in a browser.
2. Edited `branch-practice.txt`.
3. Added a new sentence about Git fetch and pull.
4. Committed the change directly to GitHub's `main` branch.
5. Returned to my Ubuntu terminal.
6. Ran `git fetch origin`.
7. Used `git status` and `git log` to confirm that the remote branch was one commit ahead.
8. Ran `git pull origin main`.
9. Verified that the new sentence appeared in my local file.
10. Confirmed that the local branch was up to date with `origin/main`.

The pull completed as a fast-forward update.

## 13. Key lessons learned

Through these practical exercises, I learned how to:

- Create and manage feature branches.
- Work independently without disturbing the main branch.
- Stage and commit changes.
- Merge feature branches.
- Understand fast-forward merges.
- Identify and resolve merge conflicts.
- Push local commits to GitHub.
- Fetch remote updates.
- Pull changes into a local branch.
- Verify that local and remote branches are synchronised.

## 14. Useful Git commands reference

```bash
git status
git branch
git branch -a
git switch main
git switch -c feature/new-feature
git add .
git commit -m "Describe the change"
git merge feature/new-feature
git fetch origin
git pull origin main
git push origin main
git log --oneline --graph --all
git diff main origin/main
git remote -v
```

## 15. Conclusion

Git is an essential tool in software development and DevOps because it supports version control, collaboration, change tracking and controlled integration of code.

Understanding these workflows provides a foundation for working with collaborative development teams and implementing automated CI/CD pipelines.

**Project:** Cloud Support Engineering Portfolio  
**Documentation:** Practical Git workflow training  
**Environment:** Ubuntu on WSL2, Git and GitHub
