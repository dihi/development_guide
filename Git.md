# Git

Distributed Version Control

## Setup

## Working with Remote Repositories

Distributed version control provides a convenient, 'safe' backup of our code as well as a nice collaborative space for development.

### High Level Flow

#### Initial Setup

1. `git clone` - create a local copy of the repository

#### Regular Use

1. `git pull remote branch` - pull any changes since last work session
1. Work locally on code
1. `git add filename` - stages file changes for an upcoming commit.
    - Multiple files can be added to a commit
1. `git commit` - commits the staged file changes to the local git history
    - Always include an appropriately formatted [commit message](##Commit)
1. `git pull remote branch` - incorporates changes in the remote repository while you've been working
1. `git push remote branch` - push local commits to the remote repository

### Remote Repository Hosts

- [GitHub](https://github.com/)
- [Duke GitLab](https://gitlab.oit.duke.edu/)

## Repository Structure for Development

### Basic Application

```bash
.github/
app/
    __init__.py
    submodule/
        __init__.py
docs/
tests/
    __init__.py
.env
.gitignore
contributing.md
changelog.txt
license
README.md
```

### Airflow Compatible Model Runner (Python)

```bash
.github/
airflow/
    dags/
        model_runner_dag.py
assets/
    example.model
constants/
    model_constants.json
    groupers/
        data_grouper.json
db/
    create_db.py
    drop_db.py
    models/
        model.py
src/
    data_processing.py
    lib/
        __init__.py
        data_processing_helpers.py
    run_model.py
.env
.gitignore
license
contributing.md
changelog.txt
Pipfile
Pipfile.lock
README.md
settings.py
```

## Cloning

## Issues

"Issues" drive the development process. Issues are numbered notes or discussions about the code in a repository.  They're typically tagged with labels like 'Bug', 'Discussion', 'Feature', 'Extension', 'In Progress', etc.  Issues provide an outline for what we should be working on and surface bugs that need to be fixed in a concise, trackable way.

### Issue Example

```
Issue: getData test failure
Description: 
# Bug Issue

## Primary Issue

`getData` function test is failing

## Expected Behavior

`getData` function test should be passing

## Possible Corrections

Correct the code in getData to fix the failing test
```

If this were the first Issue of our repository it would likely be labeled `1. getData test failure`.

### Working with Issues

When working on an issue, it's best to create a branch specific to that issue and its resolution. Using the example above we would create (or have github/gitlab create) an issue branch: `1-getData-test-failure`. This becomes the working branch for resolving the `1. getData test failure` issue.

Once the issue is resolved (and tested!) on the issue branch, commits can be [rebased](##Rebase) against the parent branch and [squashed](###Squashing) in preparation for the submission of a [pull request](##Pull-Request) and eventual merge back into the parent branch.  Pull requests that are addressing a specific issue should be labeled `userid/issue`, in this example `mnic/1-getData-test-failure`.

This cycle essentially repeats until all issues are resolved.

#### Summary

1. Create a well named, well described issue using an appropriate issue template.
1. Create a branch from the parent to work on matching the number and name of the issue.
1. Perform all work on the working branch in familiar [Regular Use](#Regular-Use) fashion.
1. checkout parent branch and pull to update local parent to match remote.
1. checkout issue branch.
1. Rebase to parent branch and squash commits.
1. prepare and submit pull request.

## Branch

A git branch is a divergence from the primary development commit path. Branches are commonly used to develop improvements or fix bugs for the primary development path in a non-destructive, non-interfereing way. The base commit of a branch will, by default, be the commit the primary development path was on when the branch was created. Any commits applied to the branch will be independent of the primary development path. Once changes on the branch are complete they can be 'merged' back into the primary development path via [git merge](##Merge).

## Commit

### Commit Message

- [Commit Message Best Practices](https://chris.beams.io/posts/git-commit/)

### Squashing

## Rebase

## Merge

### Merge Conflict

## Pull Request

### Pull Request Messages
