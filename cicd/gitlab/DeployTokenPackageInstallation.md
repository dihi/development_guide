<!--
 Copyright 2021, Duke Institute for Health Innovation (DIHI), Duke University School of Medicine, Durham NC. All Rights Reserved.
-->

# Gitlab Deploy Token Package Installation

## Topics

  - Docker
  - Python
  - Gitlab Deploy Token
  - CICD

## Pipenv/Pip Install -e

Local package installation with Pip/Pipenv is straightforward:

`pipenv install -e /path/to/package-setup-for-pip-install`

This can even be used to install 'remote' private packages using https authentication to a git repo.  This clones the repo and runs pip install behind the scenes.

`pipenv install -e git+https://username:password@git-repo-url.git`

### Gitlab / Version Control

We shouldn't store personal authentication information in VCS, so first we have to work around the username and password requirements for the 'private remote' `pipenv install -e`.  This is best done by replacing the username and password with a `gitlab-deploy-user` and `gitlab-deploy-token`.  These can be generated at a group level or, more commonly, within the repository of the private package you're attempting to pip install.

TODO: Add link for how to create gitlab-deploy-tokens

Once these tokens are generated our command line `pipenv install` using them would look like:

`pipenv install -e git+https://gitlab-deploy-user@gitlab-deploy-token@git-repo-url.git`

## ENV

We've substituted our personal login information with gitlab deploy tokens, but that still doesn't help us with our security issue.  Committing these tokens as part of a `requirements.txt` or a `Pipfile` is still bad practice.  We need to be able to use a non-committed file or Environment variable to populate the `requirements.txt` or `Pipfile`.  Luckily `pip` and `pipenv` both support environment variable substitution.

ENV substitution in these files is done by wrapping the variable name like this: `${VARIABLE_NAME}` and putting that ENV substitution in your pip source file where you need the value of that ENV to appear.

```# Pipfile
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[[packages]]
my_package = {editable = true, git = "git+https://${GITLAB_DEPLOY_USER}:${GITLAB_DEPLOY_TOKEN}@git-repo-url.git"}
```

With this implemented, we can now provide these variables in our environment and a `pipenv install` or `pip install -r requirements.txt` should work using those ENV variables.

## Docker

Doing this is Docker adds a little more complexity.  We don't want these ENV variables in our final image or in the layers generated during the image build.  ENV variables in Docker containers persist, so we can no longer use ENV explicitly.  Luckily, Docker provides ARGs that do not persist after build either in layers or in the final image.  ARGS are declared (and sometimes assigned) within the Dockerfile and are referenced farther down the dockerfile with syntax `$VARIABLE`.

To implement this, we need to pass ARGs in the Dockerfile to the ENV of the command being run to do our package installation. This ends up looking something like:

```# Dockerfile

ARG GITLAB_DEPLOY_USER
ARG GITLAB_DEPLOY_TOKEN

RUN GITLAB_DEPLOY_USER=$GITLAB_DEPLOY_USER \
    GITLAB_DEPLOY_TOKEN=$GITLAB_DEPLOY_TOKEN \
    pipenv install
```

### Build

These ARGS will be empty unless we pass them to the `docker build` command via:

`docker build --build-arg GITLAB_DEPLOY_USER=gitlab-deploy-user --build-arg GITLAB_DEPLOYT_TOKEN=gitlab-deploy-token`

## Gitlab CI Builds and Kaniko

This is essentially how we'll pass this information to the build during a Gitlab CI build of an image using Kaniko.

Our kaniko build step will look like this:

```# .gitlab-ci.yml

build-image:
  stage: build
  before_script:
    - export THE_DOCKERFILE="${DOCKERFILE}"
    - export THE_IMAGE="${IMAGE}"
    - |
      export BUILD_ARGS="--context ${CONTEXT}
      --build-arg GITLAB_PULL_USER=${GITLAB_PULL_USER}
      --build-arg GITLAB_PULL_TOKEN=${GITLAB_PULL_TOKEN}"
  extends: .kaniko_build
```

gitlab-ci uses variables declared in the `Settings > CI/CD > Variables` part of the repository during execution. The familiar `${VARIABLE}` syntax is used again here.  

## Putting it all together

1. We add our package install target repository's `GITLAB_DEPLOY_USER` and `GITLAB_DEPLOY_TOKEN` to our current repository variables @ `Settings > CI/CD > Variables`. 
2. We add the Kaniko build above with our declared `build-arg` values being imported from our CI Variables. This passes our CI Variables into our Dockerfile Build as ARGs (not ENV), letting us build the image with the variables in place without persisting them into the layers or final image.  
3. Inside the Dockerfile during the build, the ARGs are passed as inline ENV to the `pipenv install` command making them temporarily available.  
4. Since the required variables are in the ENV for that command RUN step, the `Pipfile` or `requirements.txt` pulling that information from the ENV is able to appropriately populate the required ENV substitutions for a successful build.
