# manage git repos with version control commands. example: clone, commit, push because devs need working history

## repo setup
```bash
git init                    # init new repo
git clone <url>             # clone remote repo
git clone --depth 1 <url>   # shallow clone (faster)
```

## daily workflow
```bash
git status                  # check file status
git add .                   # stage all changes
git add <file>              # stage specific file
git commit -m "msg"         # commit with message
git commit -am "msg"        # add + commit tracked files
git push                    # push to remote
git pull                    # pull + merge remote
git fetch                   # get remote refs only
```

## branch operations
```bash
git branch                  # list branches
git branch <name>           # create branch
git checkout <name>         # switch branch
git checkout -b <name>      # create + switch
git merge <branch>          # merge branch into current
git branch -d <name>        # delete branch
git branch -D <name>        # force delete
```

## history and diffs
```bash
git log --oneline           # compact log
git log --graph             # visual branches
git diff                    # unstaged changes
git diff --staged           # staged changes
git diff HEAD~1             # compare with previous commit
git show <commit>           # show commit details
```

## remote management
```bash
git remote -v               # list remotes
git remote add origin <url> # add remote
git push -u origin main     # push + set upstream
git remote rm origin        # remove remote
```

## emergency commands
```bash
git stash                   # save working changes
git stash pop               # restore stashed changes
git stash list              # list stashes
git reset HEAD~1            # undo last commit (keep changes)
git reset --hard HEAD~1     # [!] nuke last commit + changes
git reflog                  # show all head movements
git cherry-pick <commit>    # apply single commit
git revert <commit>         # create commit that undoes changes
```

## rewriting history
```bash
git rebase -i HEAD~3        # interactive rebase last 3 commits
git rebase main             # rebase current branch onto main
git commit --amend          # modify last commit message
git reset --soft HEAD~1     # undo commit, keep staged
```

## .gitignore examples
```bash
# common ignores
*.log
*.tmp
.env
.DS_Store
node_modules/
__pycache__/
*.pyc
dist/
build/
```

## search and find
```bash
git log --grep="pattern"    # search commit messages
git log -S "text"           # search code changes
git blame <file>            # see who changed what
git ls-files                # list tracked files
```

## danger zone [!]
```bash
git reset --hard origin/main    # [!] nuke local changes, match remote
git clean -fd                   # [!] delete untracked files/dirs
git filter-branch               # [!] rewrite entire history
git push --force                # [!] overwrite remote history
```

## quick fixes
```bash
git commit --fixup <commit>     # create fixup commit
git rebase -i --autosquash      # auto-apply fixups
git bisect start                # binary search for bugs
git bisect bad                  # mark current as bad
git bisect good <commit>        # mark commit as good
```

# TODO: add git hooks examples, submodule management, worktree usage