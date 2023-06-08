# Code Quality Basics

## DIHI Conventions

### Code

1. Use [poetry](https://python-poetry.org/) for dependency and package management in place of pip.
2. Use [pytest](https://docs.pytest.org/en/7.3.x/) to manage and run tests on package code
3. Use [Black](https://pypi.org/project/black/) to autoformat code on save to match PEP as closely as possible. configured in pyproject.toml
   1. [VSCode Black Plugin](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter)
4. Use [iSort](https://pycqa.github.io/isort/) to autosort imports in each file based on PEP. configured in pyproject.toml
   1. [VSCode iSort Plugin](https://marketplace.visualstudio.com/items?itemName=ms-python.isort)
5. Use [mypy](https://mypy-lang.org/) to provide typehints / static typing in your code
6. Use `my_dihi_app/cli.py` as the entrypoint to your code using [click](https://click.palletsprojects.com/en/8.1.x/)

### Package / Repository Structure (High Level)

```
my_dihi_app/
    assets/
    deploy/
    docs/
    images/
    my_dihi_app/
    tests/
    scr/
    .dockerignore
    .env
    .gitignore
    license
    pyproject.toml
    poetry.lock
    README.md
```

This structure fits nicely with how python, internally, expects packages to be structured.  All execution of everything can, and should, be managed from the root of the package.  Running everything with `python -m` having python treat the package as a `module`, ensures correct python pathing of imports within the package, regardless of location (no sister or out of path import errors).

All code on which your application depends should be under your `my_dihi_app` directory.  Files inside of `my_dihi_app` should not try to import or read files outside of `my_dihi_app`.  If a file must be read from outside of the application, the path to the file should be an input to the application and should be allowed to be variable.  Don't depend on paths to external files existing as committed.

1. assets - I use this to hold non-code specific assets and attributions, typically repo iconography.
2. deploy - compose, swarm, helm.  This is the location for all of the deployment code needed to deploy the package into a running environment.
3. docs - documentation, ideally sphinx but .md works.  All docs for the project should be in here.
4. images - docker images in folders by image purpose. `images/model`, `images/airflow-scheduler`, `images/airflow-webserver`
5. my_dihi_app - the application or model being developed.  `my_dihi_app/cli.py` is the command line interface for your application.  All application dependent code and files should be under this directory.
6. tests - the test suite for all of the code inside of `my_dihi_app`.  `python -m pytest tests` should run all tests.  This is configurable in the pyproject.toml
7. scr - bash scripts wrapping more complex functions.  provides a clean interface for variable processes.  `scr/compose.sh up` performs compose up of the development stack.  The development stack may be different by project.
8. .dockerignore - tells docker what files to leave out of any built docker images. This will be similar to the .gitignore but may also include non-package files (.env, `__pycache__`, images, deploy, etc).  A good .dockerignore increases security of your images and reduces their build time.  Don't build what you don't need.
9. .env - a file to hold required environment variables for your application.  **DO NOT COMMIT THIS FILE**.  `.env` should be in your `.gitignore`
10. .gitignore - See [gitignore.io](https://www.toptal.com/developers/gitignore/) - A file used to tell git what not to include in commits.  Typically `.env` files with secrets or other temporary file types (`__pycache__`, etc).  Don't commit what shouldn't be shared.
11. LICENSE - simple "All Rights Reserved" license declaration file, see [License](#license)
12. pyproject.toml - The configuration and package dependency inventory for the project.  This is managed and constructed through `poetry` but can be edited to add configuration.
13. poetry.lock - created by poetry this is the dependency map for all of the packages the project depends on.  This is used for deployment and, when build, ensures we don't have sub-package conflicts
14. README.md - A markdown summary of the contents of your package, what it does, and how to use it.  Should be brief with detail (and links to detail) in `/docs` 

### License

```
Copyright {{ year }}, Duke Institute for Health Innovation (DIHI), 
Duke University School of Medicine, Durham NC. 
All Rights Reserved.
```


## Best Practices

1. Don't write functions with more than 3 arguments (without a good reason).
   1. Functions requiring more than 3 args are probably doing too much and should be split into multiple functions for readability / testability
2. Use verbose names to aid understanding. `convert_string_to_unicode()` vs `clean_data()`
3. Organize code within your package into useful categories. `my_dihi_app/database` for database code / interaction, 
   1. `my_dihi_app/models`
   2. `my_dihi_app/elements`
   3. `my_dihi_app/features`
4. Represent collections of items or collections of functionality with objects where possible.
5. Use Abstract base objects to set the 'required' interface for any derived objects
6. Use Base Objects to extend generic functionality to any derived objects
7. Always pay special attention to your interfaces (for functions, objects, code bases, packages, everything).
   1. Understand what your interface is and choose it deliberately.