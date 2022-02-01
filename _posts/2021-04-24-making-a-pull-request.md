---
layout: post
title: "Making a Pull Request"
description: "How to make a pull request through Github CLI."
toc: false
comments: false
hide: true
search_exclude: false
show_tags: true
categories: [git]
permalink: /making-a-pull-request
---

# Making a Pull Request

1. Fork the project you want to work on to get your own copy of the repository.

1. Clone your copy of the repository using HTTPS or SSH.

    ```shell
    git clone forked_repo.git
    ```

1. Create a new branch to work on a separate branch.

    ```shell
    git checkout origin/master -b fix_bug
    ```

1. Make any modifications to the code, e.g. bug fix, new feature, etc.

1. Preview your changes.

    ```shell
    git diff
    ```

1. Check the state of the repository. Files should be modified but untracked.

    ```shell
    git status
    ```

1. Track the modified files.

    ```shell
    git add file.txt
    ```

1. Commit the changes.

    ```shell
    git commit -m "Fixed typo in file.txt"
    ```

1. Check the state of the repository. Files should be modified and changes committed.

    ```shell
    git status
    ```

1. Push your new branch up to your remote forked repository.

    ```shell
    git push origin HEAD
    ```

    `HEAD` is a a shortcut for your current checked-out branch, aka `fix_typo`.

1. Open a pull request from your fork.

1. Read any contributing guidelines and rules of conduct.

1. Review the changes in your pull request.

1. Create a pull request if everything looks good.


## References
* [Anthony Explains #004 Video](https://www.youtube.com/watch?v=cysuuUtbC6E)