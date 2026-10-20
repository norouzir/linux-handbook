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
| `-0` | `--null` | Instead of writing a newline after every match, write a NUL (ASCII 0). This is useful for creating unambiguous output when it is to be processed by other tools (like `xargs`), as filenames are allowed to contain embedded newlines. |
| `-N` | `--literal` | Print entry names without quoting. Normally, `plocate` will escape special characters in filenames, so that they are safe for consumption by typical shells, unless printing to a pipe, but this option will turn off such quoting. |
| `-q`\* | `--quiet` | Write no messages about errors encountered while reading and processing databases. |
| `-S`\* | `--statistics` | Write statistics about each read database to standard output instead of searching for files and exit successfully. |
| `-h` | `--help` | Print out usage information, then exit successfully. |
| `-V` | `--version` | Print out version information, then exit successfully. |

> \* May not be available in `plocate`. Checking `locate --version` can help.

## Examples

```bash
# Basic substring search
$ locate bashrc
/home/user/.bashrc
/etc/bash.bashrc

# Case-insensitive search
$ locate -i README
/home/user/project/README.md
/home/user/docs/readme.txt

# Limit results
$ locate -l 5 .conf
/etc/dbus-1/session.conf
/etc/fonts/fonts.conf
/etc/host.conf
/etc/ld.so.conf
/etc/resolv.conf

# Match basename only
$ locate -b bashrc
/home/user/.bashrc

# Regular expression search
$ locate -r '^.*\.conf$'

# Count matches
$ locate -c .log
142

# Show database statistics
$ locate -S
Database /var/lib/plocate/plocate.db:
    1,234,567 entries
    89.2 MiB

# Use custom database
$ locate -d ~/my.db pattern

# Only show existing files
$ locate -e pattern

# Combine with grep for more filtering
$ locate .txt | grep -v ".git"
```
