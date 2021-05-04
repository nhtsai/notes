---
layout: post
title: "Making a Pull Request"
description: "How to make a pull request through Github CLI."
author: "Nathan Tsai"
toc: false
comments: false
hide: false
search_exclude: true
show_tags: false
categories: [git]
permalink: /making-a-pull-request
---

# Making a Pull Request

1. Fork the project you want to work on to get your own copy of the repository.

2. Clone your copy of the repository using HTTPS or SSH.

```shell
git clone forked_repo.git
```

3. Create a new branch to work on a separate branch.

```shell
git checkout origin/master -b fix_bug
```

4. Make any modifications to the code, e.g. bug fix, new feature, etc.

5. Preview your changes.

```shell
git diff
```

6. Check the state of the repository. Files should be modified but untracked.

```shell
git status
```

7. Track the modified files.

```shell
git add file.txt
```

8. Commit the changes.

```shell
git commit -m "Fixed typo in file.txt"
```

9. Check the state of the repository. Files should be modified and changes committed.

```shell
git status
```

10. Push your new branch up to your remote forked repository.

```shell
git push origin HEAD
```

`HEAD` is a a shortcut for your current checked-out branch, aka `fix_typo`.

11. Open a pull request from your fork.

12. Read any contributing guidelines and rules of conduct.

13. Review the changes in your pull request.

14. Create a pull request if everything looks good.



## References
* [Anthony Explains #004 Video](https://www.youtube.com/watch?v=cysuuUtbC6E)