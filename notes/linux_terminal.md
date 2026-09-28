##  Navigations

### pwd

shows the location
### ls

shows the contents of the folder
#### variants

```cli
ls -l   — detailed list
ls -a   — show hidden files
ls -la  — both options together
ls -lh  — show file sizes
ls /etc — show the contents of the specified folder
```
### cd

go to the folder
#### variants

```cli
cd ..    — go one level higher
cd ../.. — go two level higher
cd ~     — go to your home folder
cd       — go to your home folder
cd /     — go to the root of the fole system
cd -     — go back to the previous folder

cd /home/user/projects — go to the full path
cd projects/python     — go the path relative to the current folder
```
### special designations

```cli
.  — the current folder
.. — parent folder
~  — user's home folder
/  — the root of the file system
```
### tree

show the directory tree

```cli
tree -L 2 — limit the depth
```
### find

find the file
#### variants

```cli
find . -name "scope.md" — find the file in the current folder and subdirectories

find . -iname "readme.md" — find the file in the current folder without case-insensitive

find . -type d -name "lessons" — find the directory in the current folder
```