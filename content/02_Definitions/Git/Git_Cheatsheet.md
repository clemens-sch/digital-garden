#Definitions 

[[Git_Introduction]]

---

| Command                | Explanation                                                            | Use-Case                                     |
| ---------------------- | ---------------------------------------------------------------------- | -------------------------------------------- |
| git add .              | "Staging" of changes in current directory                              | --                                           |
| git commit -m          | Create new commit with message-flag                                    | --                                           |
| git branch newBranch   | Creates a new branch                                                   | Here: creates 'newBranch'                    |
| git checkout newBranch | Move to other branch                                                   | Here: moves to 'newBranch'                   |
| git merge newBranch    | Combines two branches                                                  | Here: combining 'newBranch' & **'main'**     |
| git rebase main        | Moves commits from current branch in other branch - new linear history | Here: combining **'newBranch'** & 'main'<br> |
