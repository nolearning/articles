# Git Notes

## Rename a git repository's directory propertly
* wrong operation -- *using os move command*, actually get a delete then add new files operation
* should using `git mv <old name> <new name>`
* case sensitive rename
  ```
  git mv caseSensitiveDir tmpName
  git mv tmpName case_sensitive_dir
  ```
