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
