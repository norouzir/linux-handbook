# locate

Finds files by name, quickly.

`locate` is the original file-search utility. `mlocate` improved its performance and database handling, while `plocate` further optimized search speed and memory usage. In most cases, invoking `locate` runs `plocate`.

When multiple patterns are given, `plocate` will search for files that match all of them. This is the main incompatibility with `mlocate`, which searches for files that match one or more patterns, unless the `-A` option is given.

By default, patterns are taken to be substrings to search for. If at least one unescaped globbing metacharacter (`*`, `?` or `[]`) is given, that pattern is instead taken to be a glob pattern (which means it needs to start and end in `*` for a substring match).

It displays only files that the user running the command is allowed to access, meaning those with read and execute permission on all parent directories. The search index is built by `root` and may contain information about files that ordinary users cannot access. To read this index without running the entire program as root, `plocate` uses the `setgid` bit. However, access to the index does not mean that every file listed in it will be shown: `plocate` checks file visibility using the permissions of the user who ran the command and excludes files that user is not allowed to access.

Exits with `0` to indicate that a match was found or that `--help` or `--version` were passed. Otherwise, exits with status code `1`, indicating that an error occurred or that no matches were found.
