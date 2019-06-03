# Git

Distributed Version Control

## Setup

## Working with Remote Repositories

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

## Branch

## Commit

### Commit Message

- [Commit Message Best Practices](https://chris.beams.io/posts/git-commit/)

## Merge

### Merge Conflict

## Pull Request

### Pull Request Messages
