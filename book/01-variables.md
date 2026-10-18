# Variables

There may be various classifications for variables, but generally, variables are categorized as follows:

- local environment variables (also called environment variables)
- global environment variables (also called shell variables)
- special parameters

Unlike shell variables, environment variables are also available inside the subshells (and not the other shells). What makes it important is that a subshell (sometimes called a child shell or a child process) is created when certain tasks are performed, such as running a shell script or particular commands.

If the `SHLVL` value is higher than `1`, it is a subshell.

To create or change a shell variable:

```bash
$ v1=v1_content
$ v2='v2 content'
$ readonly VERSION="2.1.0"
$ arr=(a 2)
$ echo ${arr[1]}
2
$ echo $arr
a
$ echo ${arr[*]}   # equivalent to: echo ${arr[@]}
a 2
```

Or using `declare` to define the type or behavior of the variable:

```bash
$ declare -r APP_NAME="MyApp"   # read-only
$ APP_NAME="NewName"
bash: APP_NAME: readonly variable
$ declare -i count=10   # integer
$ count="hello"   # variable (count) value changes to 0 (no error)
$ count=5+3   # variable value changes to 8
$ normal_var=5+3   # variable value is "5+3"
$ declare -a fruits=(apple banana orange)   # indexed array
$ declare -A capitals=([Iran]=Tehran [France]=Paris)   # associative array (similar to python dict)
$ echo ${capitals[1]}; 

$ echo $capitals

$ echo ${capitals[Iran]}
Tehran
$ echo ${capitals[@]}
Tehran Paris
$ declare -l small="HELLO WORLD"   # lowercase (-u for uppercase) - variable value is "hello world"
$ small="MIXED Case"   # variable value changes to "mixed case"
$ real_var="original"
$ declare -n ref=real_var   # nameref
$ echo $ref
original
$ ref="changed"
$ echo $real_var
changed
$ declare -lr CONFIG_PATH="/etc/myapp/config"
```

Or using `export` or `declare -x` for environment variables:

```bash
$ export v3="v3 content"
$ export v1
$ declare -x v4="v4 content"
$ declare -x v2
```

`unset` deletes a variable and is not equivalent to `var=` (which sets an empty value). It is not possible to unset a read-only variable, even with `sudo`.

To make a variable permanent, it must be in `~/.bashrc`.

`env` or `printenv` to see environment variables, and `set` or `declare -p` to see both.

| Variable | Description |
| --- | --- |
| EDITOR | Default editor used by some shell commands |
| VISUAL | Default screen-based editor used by some shell commands |
| GROUPS | User account's group memberships |
| IFS | A list of characters that separate fields used by the shell to split text strings |
| MAIL | The filename for the current user’s mailbox (the Bash shell checks this file for new mail) |
| MAILPATH | A colon-separated list of multiple filenames for the current user’s mailbox (the Bash shell checks each file in this list for new mail) |
| LD_LIBRARY_PATH | A Colon-separated list of library directories to search prior to looking through the standard library directories |
| LANG | Locale category for the shell |
| TZ | User's time zone, if different from system's time zone |
| _ | The last argument of the last command |

Some lines from `printenv` command output:

```bash
SHELL=/bin/bash
DESKTOP_SESSION=Lubuntu
PWD=/home/slow
XDG_SESSION_TYPE=x11
HOME=/home/slow
LANG=en_US.UTF-8
XDG_CURRENT_DESKTOP=LXQt
TERM=xterm-256color
USER=slow
DISPLAY=:0
SHLVL=1
BROWSER=firefox
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
_=/usr/bin/printenv
OLDPWD=/home/slow
```

Some lines from `set` command output:

```bash
BASH=/bin/bash
BASH_VERSION='5.2.21(1)-release'
EUID=1000
HISTCONTROL=ignoreboth
HISTFILE=/home/slow/.bash_history
HISTFILESIZE=2000
HISTSIZE=1000
HOSTNAME=slub
HOSTTYPE=x86_64
IFS=$' \t\n'
GROUPS=()
LANGUAGE=
LOGNAME=slow
MACHTYPE=x86_64-pc-linux-gnu
MAILCHECK=60
OSTYPE=linux-gnu
PIPESTATUS=([0]="0" [1]="0")
PPID=1400
PS1='\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]
\$ '
PS2='> '
PS4='+ '
SSH_AGENT_PID=1178
UID=1000
```

Special parameters:

- There is no command to display all of them at once.
- They are unmodifiable (`?=0` does not work).
- The only way to access them is `$`.

| Variable | Content | Output Example |
| --- | --- | --- |
| ? | Last command exit status | 0 (success) |
| $ = BASHPID | Subshell PID | 12345 |
| ! | PID of last background process | 12346 |
| _ | Last argument of the last command | hello |
| 0 | Name of the current script or shell | bash or ./script.sh |
| 1, 2, ... | Arguments of the script | first, second |
| # | Number of the arguments | 3 |
| * | All the arguments as a string | "arg1 arg2 arg3" |
| @ | All the arguments as an array | "arg1" "arg2" "arg3" |
| - | Bash active options | himBHs |
| RANDOM | A random number (0-32767) | 624 |
| SECONDS | Seconds since the shell start | 54 |
| LINENO | Line number in script | 42 |
| FUNCNAME | Array of the names of the running functions | myfunc main |
