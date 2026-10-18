# Brace Expansion

Generates strings. Therefore, unlike globbing, files and directories do not need to exist. This makes brace expansion useful for creating multiple files or directories with similar pathnames.

```bash
$ ls p[ae]n
pen
$ ls p{a,e}n
ls: cannot access 'pan': No such file or directory
pen
$ cat passw{,or}d   # equivalent to: cat passwd password
$ touch {a,b{02..4},c}.txt   # creates: a.txt b02.txt b03.txt b04.txt c.txt
$ mkdir dir_{d..h..2}   # creates: dir_d/ dir_f/ dir_h/
$ touch {routes,models}/{users,events}.py
$ mkdir -p customer-churn-analysis/{data/{raw,processed,external},notebooks,sql,outputs}
$ tree
.
└── customer-churn-analysis
    ├── data
    │   ├── external
    │   ├── processed
    │   └── raw
    ├── notebooks
    ├── outputs
    └── sql

9 directories, 0 files
```
