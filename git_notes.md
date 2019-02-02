# Git Notes

## config user.name and user.email
```
git config --global user.name "username"
git config --global user.email "useremail"

# setting for a single repository
git config user.name "username"
git config user.email "useremail"
```

## update password
```
# Mac OS
git config --global credential.helper osxkeychain

# Windows
git config --global credential.helper wincred

# after setting user.password
git config --global --unset user.password
```

* [Updating credentials from the OSX Keychain](https://help.github.com/articles/updating-credentials-from-the-osx-keychain/)

## Rename a git repository's directory propertly
* wrong operation -- *using os move command*, actually get a delete then add new files operation
* should using `git mv <old name> <new name>`
* case sensitive rename
  ```
  git mv caseSensitiveDir tmpName
  git mv tmpName case_sensitive_dir
  ```


## Changing Git Author

### using git config

### specify author when git commit
```shell
git commit --author="authorName <author@eamil.com>"
```

### using --amend for the very last commit
```shell
git commit --amend --author="authorName <author@email.com>"
```

### using interactive rebase
```shell
git rebase -i -p last_good_commit_hash_like_0ad14fa5
# editor will open, mark all the commits you want to change with the "edit" keyword
# then will give you tips
# stop at a commit_has, you can amend the commit now, with
# git commit --amend
# once you are satisfied with your changes, run
# git rebase --continue
git commit --amend --author="authorName <author@email.com>"
git rebase --continue
```

### using git filter-branch
