# locate

Finds files by name, quickly. It does this by means of an index made by [`updatedb`](08-updatedb.md).

`locate` is the original file-search utility. `mlocate` improved its performance and database handling, while `plocate` further optimized search speed and memory usage. In most cases, invoking `locate` runs `plocate`.

When **multiple patterns** are given, `plocate` searches for files that match **all** of them (AND logic). This is the main incompatibility with `mlocate`, which searches for files that match **one or more** patterns (OR logic), unless the `-A` option is given.

By default, patterns are treated as substrings. If at least one unescaped globbing metacharacter (`*`, `?`, or `[]`) is present, the pattern is treated as a glob pattern (which means it needs to start and end with `*` for a substring match).

It displays only files that the user running the command is allowed to access (read and execute permission on all parent directories). The search index is built by `root` and may contain information about files that ordinary users cannot access. To read this index without running the entire program as root, `plocate` uses the `setgid` bit. However, access to the index does not mean that every file listed in it will be shown: `plocate` checks file visibility using the permissions of the user who ran the command and excludes files that user is not allowed to access.

Exit status: `0` if a match was found or `--help`/`--version` were passed; `1` if an error occurred or no matches were found.





## Common Options

| Short | Long | Description |
| --- | --- | --- |
| `-i` | `--ignore-case` | Case-insensitive search |
| `-c` | `--count` | Print only the count of matching entries |
| `-l N` | `--limit=N` | Limit output to N entries |
| `-r` | `--regexp` | Interpret pattern as a regular expression |
| `-b` | `--basename` | Match only the basename (not the full path) |
| `-w` | `--wholename` | Match the whole path (default) |
| `-e` | `--existing` | Only print entries for files that currently exist |
| `-E` | `--non-existing` | Only print entries for files that no longer exist |
| `-S` | `--statistics` | Print database statistics |
| `-d PATH` | `--database=PATH` | Use a specific database file |

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
