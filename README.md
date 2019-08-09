# Repro for git-lfs status issue

`git lfs status` fails when status includes file deleted and and directory created with same name

## Repro steps:

1. `git lfs ls-files`
2. observe that `test/oops.png` is an LFS object
3. undo the most recent commit but re-stage all the changes:
    * `git reset HEAD~ && git add --all && git status`
4. observe that git status output reports:
    * deleted file named test
    * added files under new directory of same name
    * including `test/oops.png` which is LFS-tracked
5. `git lfs status || echo $?`
6. observe:
    * "Git LFS objects to be committed" section is empty despite `test/oops.png` to be committed with LFS
    * last line of output "read <path>/git-lfs-status-repro/test: is a directory"
    * exit code is 2
