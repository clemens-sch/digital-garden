#Definitions 

---
# # Git ?

---
## # Reasons to use version control

- Have a record of **what**, **when**, **whom** and **why** you did something
- **Snapshots** of different development states
- **Compare** different versions of files
- Easily **share** work with others
- Integrate changes from others (**merging**)
- Mark finished product versions (**tags**)
- Try out new ideas (**branches**)
- **Best practice** and **Industry standard**

---

![vc-xkcd.jpg](https://deep-thought.norwin.at//tech-kb/git/assets/vc-xkcd.jpg)

---
## # Glossary

- **Repository**
    - Stores complete history, branches, tags (and other meta-information)
- **Working Copy**
    - Your “playground”, actual source code
- **Commit / Revision**
    - A specific, single version/snapshot
- **Branch**
    - A distinct line of development

---

[

## # Distributed Version Control

](https://deep-thought.norwin.at/tech-kb/git/Git/#distributed-version-control)

- Every user has a full copy of the repository
- Repositories can be synchronized (push/fetch)
- Off-line access

![centr-decentr.png](https://deep-thought.norwin.at//tech-kb/git/assets/centr-decentr.png)

---

![git_xkcd.png](https://deep-thought.norwin.at//tech-kb/git/assets/git_xkcd.png)

---

[

## # Git

](https://deep-thought.norwin.at/tech-kb/git/Git/#git-2)

- Started in 2005 by Linus Torvalds
- Used for Linux Kernel development
- Highly distributed
- Cryptographic integrity

---

[

## # Install & Setup

](https://deep-thought.norwin.at/tech-kb/git/Git/#install--setup)

- Install - [https://www.git-scm.com](https://www.git-scm.com/)
- Set user information

```powershell
git config --global user.name 'Norbert Winkler'
git config --global user.email 'n.winkler@htlkrems.at'
```

---

[

## # Cheat Sheet

](https://deep-thought.norwin.at/tech-kb/git/Git/#cheat-sheet)

[https://education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)

---

[

## # Creating Repositories

](https://deep-thought.norwin.at/tech-kb/git/Git/#creating-repositories)

- Create a new local repository

```bash
$ cd path/to/project
$ git init
# or
$ git init new_dir
$ cd new_dir
```

---

[

## # Adding files

](https://deep-thought.norwin.at/tech-kb/git/Git/#adding-files)

- Adding new files
- Current state of the file is recorded

```bash
# add a new file foo.txt with contents 'bar'
$ >foo.txt echo 'bar'
git add foo.txt
```

---

[

## # Writing history

](https://deep-thought.norwin.at/tech-kb/git/Git/#writing-history)

- Permanently save your work
- [The seven rules of a great Git commit message](https://cbea.ms/git-commit/)

```bash
$ git commit -m "Showcased how to add files to git"
```

---

[

## # Updating Files

](https://deep-thought.norwin.at/tech-kb/git/Git/#updating-files)

- Tell Git about new changes
- Same thing as adding new files (Git only cares about the content of files)

```bash
# add new content to foo.txt
$ >>foo.txt echo 'new content'
$ git add foo.txt
$ git commit -m 'updated file'
```

---

[

## # Browsing history

](https://deep-thought.norwin.at/tech-kb/git/Git/#browsing-history)

```bash
$ git log
commit 87bebde3c4c24c34f8b61d3332ac18df416d28c7
Author: norwin <me@norwin.at>
Date:   Tue Jun 11 04:35:35 2024 +0200

    Updated file

commit f49e31ed76c07887481bea4ba4dffd606b794c45
Author: norwin <me@norwin.at>
Date:   Tue Jun 11 04:32:13 2024 +0200

    Showcased how to add files to git
```

---

[

## # Working copy status

](https://deep-thought.norwin.at/tech-kb/git/Git/#working-copy-status)

- Shows list of modified, new and staged files
- Also reminds you of important commands

```bash
$ git status
On branch main
nothing to commit (working directory clean)
```

---

[

## # Inspecting changes

](https://deep-thought.norwin.at/tech-kb/git/Git/#inspecting-changes)

- Show current changes
- Show differences between commits
- Show differences between branches

```bash
$ git diff
$ git diff HEAD~ HEAD
$ git diff branch1 branch2
```

---

[

## # What the fork?

](https://deep-thought.norwin.at/tech-kb/git/Git/#what-the-fork)

- Try out new features
- In an ideal world: one branch per feature
- Branches are cheap, use them often
- Branches can be deleted

```bash
# create and switch to new branch
$ git branch newbranch
$ git checkout newbranch

# list branches (star marks active branch)
$ git branch
main
* newbranch
```

---

[

## # Merging changes

](https://deep-thought.norwin.at/tech-kb/git/Git/#merging-changes)

- Integrate changes from a branch
- Integrate changes from others

```bash
# Checkout the branch you want the changes to merge into
$ git checkout main
# merge changes from other branch
$ git merge newbranch
```

---

[

## # Merge conflicts

](https://deep-thought.norwin.at/tech-kb/git/Git/#merge-conflicts)

- Merges don’t always go well
- Git inserts conflict markers into conflicting files
- `git status` tells you the current state
- Remove conflicts and add that state of the file with `git add`

```bash
$ git diff
diff --cc foo.txt
…
<<<<<<< HEAD
current content
=======
branch content
>>>>>>> newbranch

# manually resolve conflict
$ vim foo.txt

# add new state to git
$ git add foo
$ git commit
```

---

[

## # Forking existing projects

](https://deep-thought.norwin.at/tech-kb/git/Git/#forking-existing-projects)

- Get a copy of the repository
- `fetch` + `merge` to update your copy

```bash
$ git clone https://github.com/git/git.git git
$ cd git
# ...
$ git fetch
$ git merge origin/branch
```

---

[

## # Syncing changes

](https://deep-thought.norwin.at/tech-kb/git/Git/#syncing-changes)

- Fetch downloads the latest changes from the remote repository
- With merge you can merge these changes with your local branch
- `pull` is a shortcut for `fetch` + `merge`
- `push` uploads your local changes to the remote repository

```bash
$ git pull
# make changes and commit them ...
# afterwards:
$ git push
```

---

[

## # Links and references

](https://deep-thought.norwin.at/tech-kb/git/Git/#links-and-references)

- [git-scm.com](http://git-scm.com/)
- [Pro Git Book](https://git-scm.com/book/de/v2)
- [Linus’ infamous talk at Google](https://www.youtube.com/watch?v=4XpnKHJAok8)
- [fhLUG: Verteilte Versionsverwaltung mit Git (fortgeschritten)](https://fhlug.at/wp/wp-content/uploads/2011/09/dvcs-git.pdf)
- [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
- [Learn Git Branching](https://learngitbranching.js.org/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)