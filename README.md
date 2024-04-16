# dpe-dbt-templat #TODO change to your repo name

This repo contains the dbt project for creating views in Snowflake.

* [Developer Setup](#developer-setup)
  * [Prerequisites](#prerequisites)
  * [Getting Started](#getting-started)
* [Development](#development)
  * [Running and Testing](#running-and-testing)
  * [Generating and Reviewing dbt Documentation](#generating-and-reviewing-dbt-documentation)

## Developer Setup

### Prerequisites

In order to get started with development you must already have installed [Python 3](https://www.python.org/downloads/) on your workstation. Currently, we are standardizing on Python 3.11.

Before getting started, please familiarize yourself with the Platform User Guide and complete [Dev Setup from their onboarding checklist](https://confluence.sonos.com/display/DATA/Platform+Guide+-+Getting+Started) to get your machine ready for development. Recommend using Python 3.11 if possible.

### Getting Started

With the prerequisites in place:
1. Clone the repo to your workstation:
```shell
  git clone https://github.com/Sonos-Inc/your-repo-name . #TODO change link to your repo
```
```shell
  cd your-repo-name #TODO change to your repo name
```
2. Create and activate your virtual environment:

    a. if using venv run:
    ```shell
    python3 -m venv envname #change envname to something like your projectname
    ```
    Then run:
    ```shell
    . envname/bin/activate #change envname to something like your projectname
    ```
    b. if using conda run:
    ```shell
    conda create --name envname python=3.11 #change envname to something like your projectname
    ```
    Then run:
    ```shell
    conda activate envname #change envname to something like your projectname
    ```
3. Next, install the required Python packages using the requirements file:
```shell
  pip install -r requirements.txt
```
4. Install pre-commit hooks:
```sh
pre-commit install
```
5. Set the following environment variables as described [here](https://confluence.sonos.com/display/DATA/sonos-data-core-dbt+profile+setup). This can be done by creating a `.profile` or a local `.env` for your favorite shell environment or updating the activate.bat file with the following environment variables:
```sh
DBT_PROFILE_TARGET=dev
DBT_ENV_SECRET_SNOWFLAKE_ACCOUNT=sonos.us-east-1
DBT_ENV_SECRET_USER=<username>@sonos.com
DBT_ENV_SECRET_ROLE=<snowflake role>
DBT_ENV_SECRET_PRIVATE_KEY=/Users/<username>/.ssh/snowflake_key.p8
DBT_ENV_SECRET_PASSPHRASE=<your_passphrase_here>
DBT_ENV_SECRET_WH=<snowflake warehouse>
DBT_SCHEMA=<first_initial><lastname>
```

6. Login to the Sonos VPN with Full-Tunnel profile (or ensure you are running on a Cloud PC or other VM with access to Snowflake) and verify your local repo against Snowflake:
```shell
  dbt deps
  dbt debug
```
Output:
```
  19:12:12  Running with dbt=1.7.1
  ...
  All checks passed!
```

### Building models

Compile and run the entire project:
```shell
   dbt run
```

Compile and run a specific model:
```shell
   dbt run --select stg_raw__ods_gl_account
```

Compile and run a specific model and all of the models downstream of it:
```shell
   dbt run --select stg_raw__ods_gl_account+
```

When running dbt locally, the models will be created in DWM_DEV.<SCHEMAPREFIX_SCHEMA> in Snowflake.

### Generating and Reviewing dbt Documentation

Generate Docs:
```shell
  dbt docs generate
```

View Docs in Browser:
```shell
  dbt docs serve
```

## Workflow

### Development

1. Branch from test to a local feature branch:
```shell
(venv) sonos-data-core-dbt $ git checkout test
(venv) sonos-data-core-dbt $ git pull origin
(venv) sonos-data-core-dbt $ git checkout -b JIRA-XXXXX
```
2. Commit locally in your local feature branch until you are done with your specific development:
```shell
(venv) sonos-data-core-dbt $ git add my_changed_files
(venv) sonos-data-core-dbt $ git commit -m "JIRA-XXXXX: Short description of change"
```

3. Once you have completed your development locally, please squash all local commits into one:
```shell
(venv) sonos-data-customer-dbt $ git rebase -i HEAD~##
```
!! in this case is the number of commits in your local branch you're going to squash. If you're unsure how many commits you have on your feature branch from it's parent, you can ask git:
```shell
(venv) sonos-data-customer-dbt $ git rev-list --count HEAD ^test
```

4. Rebase your local feature branch to the test branch to pick up any commits there:
```shell
(venv) sonos-data-core-dbt $ git checkout test
(venv) sonos-data-core-dbt $ git pull origin
(venv) sonos-data-core-dbt $ git checkout JIRA-XXXXX
(venv) sonos-data-core-dbt $ git rebase test
```

   - If you have merge conflicts, this will require conflict resolution and a subsequent commit to your local feature branch:
```shell
(venv) sonos-data-core-dbt $ git add my_my_changed_files
(venv) sonos-data-core-dbt $ git commit -m "DPLAT-XXXXX: Short description of change"
```
   - Otherwise, you're ready to prepare for a PR request.

5. Push your local feature branch to Github:
```shell
(venv) sonos-data-core-dbt $ git push origin JIRA-XXXXX
```

6. Open a PR on Github to merge to test branch and follow the PR submission guidelines:
   - All commit messages must begin with the JIRA. E.g. "JIRA-XXXXX: This is a commit message"
   - All PRs must follow the template in the repo including:
      - Filling the Description section with a bulleted list of changes you've made
   - Include at least one Peer Reviewer


7. If you have feedback requests:
   - Make changes locally and commit to your local feature branch:
```shell
(venv) sonos-data-core-dbt $ git add my_my_changed_files
(venv) sonos-data-core-dbt $ git commit -m "JIRA-XXXXX: Short description of change"
```
   - Push your commits to the repo feature branch (if you rebased, you may need to add a `--force` flag)
```shell
(venv) sonos-data-core-dbt $ git push origin JIRA-XXXXX
```
  - Update the PR request by responding to the inline comments, and resolve the conversation if warranted. It is also fine to allow the person that requested the change to resolve the conversation.

8. Once the PR is approved for merge, you will need to check again whether there have been any commits to test since your branch was rebased.
  - If there were, you will need to rebase again and resolve any merge conflicts that arise.

9. Otherwise "Squash & Merge" the PR in the Github UI and accept all defaults.


### Deploy

1. Open a PR to merge from test to main
   - Include at least one code owner
2. Once approved, merge the PR to main

---
&copy; Copyright 2022 - 2023 Sonos, Inc. All rights reserved worldwide.
