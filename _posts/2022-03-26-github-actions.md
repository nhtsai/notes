---
layout: post
title: "Github Actions"
description: "Adding CI/CD pipelines using Github Actions."
toc: false
comments: false
# image: 
hide: false
search_exclude: false
show_tags: true
# sticky_rank: 1
categories: [dev-ops]
permalink: /github-actions
---

## Components

A [Github Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions) *workflow* is a set of *jobs* run sequentially or in parallel, inside separate VM **runner** machines. Each job is a series of *steps* that either run a script or run an *action*, or a reusable extension used to simplify workflows.

A **workflow** is a YAML file that will run when triggered by an **event**, or a specific activity in a repository that triggers a workflow run, e.g. on push, pull request, or new issue. Workflows can build and test pull requests (CI), deploy applications upon release (CD), and add labels to triage issues.

A **job** is a set of steps in a workflow that executes on the same runner. Steps are shell scripts or **actions** that are executed in order and are dependent on each other. Since steps of a job run on the same runner, data can be shared across steps, e.g. steps to build an application then test that application.

An **action** is a custom application that performs a complex but frequently repeated task. An action can pull a repository, set up and install environment dependencies, or authenticate with cloud providers.

A **runner** is a server that runs workflows a single job at a time, on fresh virtual machines running Ubuntu Linux, Windows, or macOS.

## Example CI Workflow

```yaml
# name of workflow
name: CI

# events that trigger the workflow
on: [push, pull_request]

# list of jobs in this workflow
jobs:

  # build and test job
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.7", "3.8", "3.9", "3.10"] 
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v3
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install test dependencies
      run: |
        python -m pip install --upgrade pip
        python -m pip install pytest
    - name: Run tests
      run: |
        pytest src/tests/

  # format and lint job
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python 3.9
        uses: actions/setup-python@v3
        with:
          python-version: 3.9
      - name: Install lint dependencies
        run: |
          python -m pip install --upgrade pip
          python -m pip install --no-cache-dir black isort pylint
      - name: Format the code with black/isort
        run: |
          black $(git ls-files '*.py')
          isort --profile black $(git ls-files '*.py')
      - name: Lint the code with pylint
        run: |
          pylint $(git ls-files '*.py')

  # docs job
  docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python 3.9
        uses: actions/setup-python@v3
        with:
          python-version: 3.9
      - name: Install docs dependencies
        run: |
          python -m pip --no-cache-dir install sphinx
      - name: Generate docs
        run: |
          sphinx-build -b html src/ build/
```
