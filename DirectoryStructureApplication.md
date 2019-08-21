# Initial Repository Structure

Common directory structures are great for simplifying the spin up of new projects and for transfer of knowledge between developers. When we all use similarly built applications we develop a fundamental understanding of how the many pieces of our application interact.

## Common Elements

- `app/` - this is your primary application directory and contains all the code required to run your app.  
    - `__init__.py` files are required in all directories used to store importable code
- `docs/` - thorough documentation of your application.  What it does, how it works, how it may fail and what to do when it does
- `tests/` - Holds all of your software tests (Unit, Integration, etc.) including fixtures and mocks required to run them
    - `pytest tests` should run all tests available on for your application
- `.env` - DO NOT COMMIT!, this file contains all environment specific, sometimes sensitive, variables required to run your application. This is designed to be a drop-in replacable file when an environment changes, i.e. going from DEV to TEST or PROD. (see [https://12factor.net/config](https://12factor.net/config)
- `.gitignore` - a list of files or regex-like file search strings to exclude from git.  Examples of things that should be in this file:
    - `.env` to prevent accidental commit of potentially sensitive information
    - `.DS_Store` if you're using OSX this can creep in
    - `*.csv` if you have large CSV files for local development that aren't part of the application
- `contributing.md` - a concise explanation of how best to contribute to the project if it's being opened up for external contribution.
- `license` - very important, but a topic all its own. There are different degrees of 'Open Source' determined by the type of declared license. One should exist in every publically facing repository where code is being actively developed.
- `README.md` - an attractive, high level summary of your application, its purpose, how to install and use it.

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

