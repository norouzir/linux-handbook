# locate

Finds files by name, quickly. It does this by means of an index made by [`updatedb`](08-updatedb.md).

`locate` is the original file-search utility. `mlocate` improved its performance and database handling, while `plocate` further optimized search speed and memory usage. In most cases, invoking `locate` runs `plocate`.

When **multiple patterns** are given, `plocate` searches for files that match **all** of them (AND logic). This is the main incompatibility with `mlocate`, which searches for files that match **one or more** patterns (OR logic), unless the `-A` (`--all`) option is given.

By default, patterns are treated as substrings. If at least one unescaped globbing metacharacter (`*`, `?`, or `[]`) is present, the pattern is treated as a glob pattern (which means it needs to start and end with `*` for a substring match).

It displays only files that the user running the command is allowed to access (read and execute permission on all parent directories). The search index is built by `root` and may contain information about files that ordinary users cannot access. To read this index without running the entire program as root, `plocate` uses the `setgid` bit. However, access to the index does not mean that every file listed in it will be shown: `plocate` checks file visibility using the permissions of the user who ran the command and excludes files that user is not allowed to access.

Exit status: `0` if a match was found or `--help`/`--version` were passed; `1` if an error occurred or no matches were found.

| Short | Long | Description |
| --- | --- | --- |
| `-i` | `--ignore-case` | Do a case-insensitive search. |
| `-c` | `--count` | Print only the count of matching entries. |
| `-b` | `--basename` | Match only the basename (not the full path). This is the opposite of `-w`. |
| `-w` | `--wholename` | Match the entire pathname. This is the default, so it will not do anything (unless `-b` is given first). This option thus exists only as compatibility with `mlocate`. |
| `-r` | `--regexp` | Interpret patterns as *basic* regular expressions. |
|  | `--regex` | Interpret patterns as *extended* regular expressions. |
| `-d <DBPATH>` | `--database=<DBPATH>` | Find matches in the given database (`<DBPATH>`), instead of the default database (`/var/lib/plocate/plocate.db`). This argument can be given multiple times, to search multiple databases. It is also possible to give multiple databases in one argument, separated by `:`. |
| `-l <N>` | `--limit=<N>` | Stop searching after `<N>` matches have been found. If `--count` is given, the number printed out will be at most `<N>`. |
| `-e` | `--existing` | Print only entries for files that currently exist. Note that unlike `mlocate`, symlinks are not followed by default (and there is no option to change this). |
| `-0` | `--null` | Instead of a newline, write a NUL (ASCII 0) after each match. Filenames may contain embedded newlines, so NUL delimiters produce unambiguous output for tools such as `xargs`. Because NUL-separated output cannot be quoted for a terminal, plocate prints filenames raw when `-0` is given (equivalent to `-N`). |
| `-N` | `--literal` | Print entry names without quoting. When printing to a terminal, plocate normally escapes characters that can cause trouble (control characters and `'`, `"`, `\`, and backtick) — using a GNU `ls`-style `$'...'` quoting — so filenames containing them are safe for a shell. Output to a pipe or other non-terminal is unquoted by default; use `-N` to suppress even the terminal-side quoting. |
| `-q`\* | `--quiet` | Write no messages about errors encountered while reading and processing databases. |
| `-S`\* | `--statistics` | Write statistics about each read database to standard output instead of searching for files and exit successfully. |
| `-h` | `--help` | Print out usage information, then exit successfully. |
| `-V` | `--version` | Print out version information, then exit successfully. |
> \* Not available in `plocate`. Checking `locate --version` can help.

`LOCATE_PATH`: Colon-separated list of database paths. If set, is appended to the databases given by `--database` (or the default). In GNU/BSD `locate` it replaces the default database instead.

Examples:
```bash
$ locate bash_alias
/home/slow/.bash_aliases
$ sudo updatedb -U ~/sandbox/ -o ~/sandbox/my.db
$ tree
.
├── my.db
└── Readmes
    ├── info.txt
    ├── README.md
    └── readme.txt

2 directories, 4 files
$ locate -d ~/sandbox/my.db readme   # equivalent to: locate -d ~/sandbox/my.db *readme*
/home/slow/sandbox/Readmes/readme.txt
$ locate -d ~/sandbox/my.db -i readme
/home/slow/sandbox/Readmes
/home/slow/sandbox/Readmes/README.md
/home/slow/sandbox/Readmes/info.txt
/home/slow/sandbox/Readmes/readme.txt
$ locate -d ~/sandbox/my.db -ib readme
/home/slow/sandbox/Readmes
/home/slow/sandbox/Readmes/README.md
/home/slow/sandbox/Readmes/readme.txt
$ locate -d ~/sandbox/my.db -c Readme
4
$ locate -d ~/sandbox/my.db -l2 Readme
/home/slow/sandbox/Readmes
/home/slow/sandbox/Readmes/README.md
$ locate -d ~/sandbox/my.db -ibr ^readme
/home/slow/sandbox/Readmes
/home/slow/sandbox/Readmes/README.md
/home/slow/sandbox/Readmes/readme.txt
$ rm Readmes/readme.txt
$ locate -d ~/sandbox/my.db -i readme
/home/slow/sandbox/Readmes
/home/slow/sandbox/Readmes/README.md
/home/slow/sandbox/Readmes/info.txt
/home/slow/sandbox/Readmes/readme.txt
$ locate -d ~/sandbox/my.db -ie readme
/home/slow/sandbox/Readmes
/home/slow/sandbox/Readmes/README.md
/home/slow/sandbox/Readmes/info.txt
$ locate -d ~/sandbox/my.db -i readme Info
/home/slow/sandbox/Readmes/info.txt
$ locate .txt | grep -v "\.git"   # filtering results
```
