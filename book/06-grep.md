# grep

Searches for patterns in files and, by default, prints each line that contains a match.

`grep -G PATTERN FILE` is equivalent to `sed -n /PATTERN/p FILE`.

Debian includes the variant programs `egrep`, `fgrep`, and `rgrep` which are the same as `grep -E`, `grep -F`, and `grep -r`, respectively. These variants are deprecated. For portability reasons, it is recommended to use `grep` with the related option only.

| Short | Long | Description |
| --- | --- | --- |
| -F | --fixed-strings | Interpret pattern as normal non-regex text. |
| -G | --basic-regexp | Interpret patterns as basic regular expressions (BREs). This is the default. |
| -E | --extended-regexp | Interpret patterns as extended regular expressions (EREs). |
| -P | --perl-regexp | Interpret patterns as Perl-compatible regular expressions (PCREs). |
| -v | --invert-match | Invert the sense of matching, to select non-matching lines. |
| -i | --ignore-case | Ignore case distinctions in patterns and input data. |
|  | --no-ignore-case | Do not ignore case distinctions in patterns and input data. This is the default. This option is useful for passing to shell scripts that already use -i, to cancel its effects because the two options override each other. |
| -r | --recursive | Read all files under each directory, recursively, following symbolic links only if they are on the command line. Is equivalent to -d recursive. |
| -o | --only-matching | Print only the matched (non-empty) parts of a matching line |
| -e PATTERN | --regexp=PATTERN | Use PATTERN as the patterns. If this option is used multiple times or is combined with the -f option, search for all patterns given. This option can be used to protect a pattern beginning with -. |
| -f FILE | --file=FILE | Obtain patterns from FILE, one per line. |
| -c | --count | Suppress normal output and instead, print a count of matching lines for each input file. |
| -n | --line-number | Prefix each line of output with the 1-based line number within its input file. |
| -b | --byte-offset | Print the 0-based byte offset within the input file before each line of output. If -o is specified, print the offset of the matching part itself. |
| -w | --word-regexp | Select only those lines where the match forms a whole word. Conceptually, the matching substring must be (^\|NWCC)PATTERN($\|NWCC) where NWCC stands for "non-word constituent character". Word-constituent characters are letters, digits, and the underscore. |
| -x | --line-regexp | Select only those matches that exactly match the whole line. Is equivalent to ^PATTERN$. |
| -l | --files-with-matches | Suppress normal output and instead, print the name of each input file from which output would normally have been printed. Scanning each input file stops upon first match. |
| -L | --files-without-match | Suppress normal output and instead, print the name of each input file from which no output would normally have been printed. |
| -m NUM | --max-count=NUM | Stop reading a file after NUM matching lines. |
| -q | --quite --silent | Do not write anything to standard output. Exit immediately with 0 status if any match is found, 1 if no match is found, and 2 for errors. |
| -s | --no-messages | Suppress error messages about nonexistent or unreadable files, but still output matching lines to standard output. |
| -h | --no-filename | Suppress the prefixing of file names on output. This is the default when there is only one file (or only standard input) to search. |
| -H | --with-filename | Print the file name for each match. This is the default when there is more than one file to search. |
| -T | --initial-tab | Make sure that the first character of actual line content lies on a tab stop, so that the alignment of tabs looks normal. This is useful with options that prefix their output to the actual content (-H, -n and -b). |
| -A NUM | --after-context=NUM | Print NUM lines of trailing context after matching lines. |
| -B NUM | --before-context=NUM | Print NUM of leading context before matching lines. |
| -C NUM | --context=NUM | Print NUM lines of output context. |
|  | --group-separator=SEP | When -A, -B, or -C are in use, print SEP instead of -- between groups of lines. |
|  | --no-group-separator | When -A, -B, or -C are in use, do not print a separator between groups of lines. |
| -d ACTION | --directories=ACTION | If an input file is a directory, use ACTION to process it. By default, ACTION is read, i.e., read directories just as if they were ordinary files.  If ACTION is skip, silently skip directories. If ACTION is recurse, read all files under each directory, recursively (is equivalent to -r). |
|  | --exclude=GLOB | Skip any file with a name that matches GLOB (using wildcards). |
|  | --exclude-dir=GLOB | Skip any directory with a name that matches GLOB (using wildcards). |
|  | --exclude-from=FILE | Same as --exclude, but reading from the FILE. |
|  | --include=GLOB | Search only files whose base name matches GLOB. If contradictory --include and --exclude options are given, the last matching one wins. If no --include or --exclude options match, a file is included unless the first such option is --include. |

Normally the exit status is `0` if a line is selected, `1` if no lines were selected, and `2` if an error occurred. However, if `-q` is used and a line is selected, the exit status is `0` even if an error occurred.

Examples (for `-G`, `-E`, and `-P` examples, check the [RegEx](05-regex.md) section):

```bash
$ grep error app.log log.txt
$ grep user *.txt
$ grep -e "boot" -e "root" -e "debus" /etc/passwd   # equivalent to: grep -E "[br]oot|debus" /etc/passwd
$ cat users.txt
mail
cups
$ grep -f users.txt /etc/passwd
$ grep -f users.txt -e root /etc/passwd
$ echo 'way' | grep -F 'w.'
$ echo 'w.y' | grep -F 'w.'
w.y
$ tree
.
├── grep_dir_1
│   └── grep_file_2.txt
└── grep_file_1.txt

2 directories, 2 files
$ cat grep_file_1.txt
cat
catfish
There is a rat.
(bat)
_cat
$ cat grep_dir_1/grep_file_2.txt
dog3
dog#3
dog_3
$ grep -v cat grep_file_1.txt
There is a rat.
(bat)
$ grep there grep_file_1.txt
$ grep -i there grep_file_1.txt
There is a rat.
$ grep [br]at grep_file_1.txt
There is a rat.
(bat)
$ grep -o [br]at grep_file_1.txt
rat
bat
$ grep -c [br]at grep_file_1.txt   # equivalent to: grep [br]at grep_file_1.txt | wc -l
2
$ grep -n [br]at grep_file_1.txt
3:There is a rat.
4:(bat)
$ grep -b [cr]at. grep_file_1.txt
4:catfish
12:There is a rat.
$ grep -bo [cr]at. grep_file_1.txt
4:catf
23:rat.
$ grep -nbo [cr]at. grep_file_1.txt
2:4:catf
3:23:rat.
$ grep -nboT [cr]at. grep_file_1.txt
 2: 4:  catf
 3:23:  rat.
$ grep -w .at grep_file_1.txt
cat
There is a rat.
(bat)
$ grep -x .at grep_file_1.txt
cat
$ grep -w do grep_dir_1/grep_file_2.txt
$ grep -w dog grep_dir_1/grep_file_2.txt
dog#3
$ grep -w dog[#_] grep_dir_1/grep_file_2.txt
$ grep -w dog[#_]3 grep_dir_1/grep_file_2.txt
dog#3
dog_3
$ grep ^....$ *
grep: grep_dir_1: Is a directory
grep_file_1.txt:_cat
$ grep -d read ^....$ *
grep: grep_dir_1: Is a directory
grep_file_1.txt:_cat
$ grep -d skip ^....$ *
grep_file_1.txt:_cat
$ grep -d recurse ^....$ *
grep_dir_1/grep_file_2.txt:dog3
grep_file_1.txt:_cat
$ grep -r ^....$ *
grep_dir_1/grep_file_2.txt:dog3
grep_file_1.txt:_cat
$ grep -rh ^....$ *
dog3
_cat
$ grep -H fish grep_file_1.txt
grep_file_1.txt:catfish
$ grep -rl dog *
grep_dir_1/grep_file_2.txt
$ grep -rL dog *
grep_file_1.txt
$ grep -d skip -r ^.....$ *
grep_dir_1/grep_file_2.txt:dog#3
grep_dir_1/grep_file_2.txt:dog_3
grep_file_1.txt:(bat)
$ grep -r -d skip ^.....$ *
grep_file_1.txt:(bat)
$ egrep -r .{4} *
grep_dir_1/grep_file_2.txt:dog3
grep_dir_1/grep_file_2.txt:dog#3
grep_dir_1/grep_file_2.txt:dog_3
grep_file_1.txt:catfish
grep_file_1.txt:There is a rat.
grep_file_1.txt:(bat)
grep_file_1.txt:_cat
$ egrep -rm2 .{4} *
grep_dir_1/grep_file_2.txt:dog3
grep_dir_1/grep_file_2.txt:dog#3
grep_file_1.txt:catfish
grep_file_1.txt:There is a rat.
$ grep -nm1 ERROR logfile   # getting the first occurrence
$ grep -q cat grep_file_1.txt
$ echo $?
0
$ grep -q hat grep_file_1.txt
$ echo $?
1
$ grep -q cat grep_file_3.txt
grep: grep_file_3.txt: No such file or directory
$ echo $?
2
$ grep _cat grep_file_{1,3}.txt
grep_file_1.txt:_cat
grep: grep_file_3.txt: No such file or directory
$ echo $?
2
$ grep -s _cat grep_file_{1,3}.txt
grep_file_1.txt:_cat
$ echo $?
2
$ grep -A2 rat grep_file_1.txt
There is a rat.
(bat)
_cat
$ grep -B2 rat grep_file_1.txt
cat
catfish
There is a rat.
$ grep -C1 rat grep_file_1.txt
catfish
There is a rat.
(bat)
$ grep -B1 cat grep_file_1.txt
cat
catfish
--
(bat)
_cat
$ grep -B1 --group-separator=********* cat grep_file_1.txt
cat
catfish
*********
(bat)
_cat
$ grep -B1 --no-group-separator cat grep_file_1.txt
cat
catfish
(bat)
_cat
$ echo 'red fish' > grep_file_4.txt
$ echo 'yellow fish' > grep_file_5
$ grep -s fish *
grep_file_1.txt:catfish
grep_file_4.txt:red fish
grep_file_5:yellow fish
$ grep -s --exclude=grep_file_4.txt fish *
grep_file_1.txt:catfish
grep_file_5:yellow fish
$ grep -s --exclude='*.txt' fish *
grep_file_5:yellow fish
$ mkdir grep_dir_2 && echo 'cute dog' > grep_dir_2/grep_file_6.txt
$ egrep -r '^.{4}$|cute' *
grep_dir_1/grep_file_2.txt:dog3
grep_dir_2/grep_file_6.txt:cute dog
grep_file_1.txt:_cat
$ egrep -r --exclude-dir=grep_dir_1 '^.{4}$|cute' *
grep_dir_2/grep_file_6.txt:cute dog
grep_file_1.txt:_cat
$ egrep -r --exclude=grep_file_1.txt --exclude-dir=grep_dir_1 '^.{4}$|cute' *
grep_dir_2/grep_file_6.txt:cute dog
$ echo -e '*.txt\n*.log' > grep_file_exclude
$ egrep -r 'fish|cute|dog3' *
grep_dir_1/grep_file_2.txt:dog3
grep_dir_2/grep_file_6.txt:cute dog
grep_file_1.txt:catfish
grep_file_4.txt:red fish
grep_file_5:yellow fish
$ egrep -r --exclude-from=grep_file_exclude 'fish|cute|dog3' *
grep_file_5:yellow fish
$ grep -s ' fish' *
grep_file_4.txt:red fish
grep_file_5:yellow fish
$ grep -s --include='*.txt' ' fish' *
grep_file_4.txt:red fish
$ grep -s --include=\*.txt ' fish' *
grep_file_4.txt:red fish
$ grep -s --include=*.txt ' fish' *
grep_file_4.txt:red fish
```

Patterns passed to `--include`, `--exclude`, and `--exclude-dir` are conventionally quoted or have their wildcard characters escaped to indicate that they are intended for grep rather than for shell pathname expansion.

`-f` and `--exclude-from` each accept a single file path and wildcards do not work. To read patterns from multiple files, the option must be repeated.

```bash
$ ls *.txt
grep_file_1.txt  grep_file_4.txt
$ grep -f=*.txt grep_file_5
grep: =*.txt: No such file or directory
```

There are three cases in which either grep or the shell performs the operation:

- **Search path:** `.` vs `*` (`grep -r pat .` vs `grep -r pat *`)
- **Recursive traversal:** `-r` vs `**` (`grep -r pat .` vs `grep pat **`)
- **File filtering:** `--include` vs globbing (`grep -r --include='*.txt' pat .` vs `grep pat **/*.txt`)

Allowing grep to perform these tasks is preferred over relying on shell expansion because the latter may lead to several issues:

- Hidden files and directories are ignored (unless `dotglob` is enabled).
- No match or empty directories cause an error.
- Filenames beginning with `-` may be interpreted as command-line options.
- Expanding a large number of pathnames may result in an `Argument list too long` error.
