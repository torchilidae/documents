Delete all empty directories from current folder
---
```Shell
find . -type d -empty -delete
```

List all files in the current directory (Not sub directories)
---
```Shell
find . -maxdepth 1 -type f
```

List all file paths in the current directory and its sub directories
---
```Shell
find . -type f -follow -print
```