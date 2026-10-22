# RegEx

A regular expression (regex) is a sequence of characters that defines a search pattern.

Unlike glob patterns, a regex pattern matches anywhere in the string by default. Using `^` anchors it to the start and `$` anchors it to the end.
For example, the regex pattern equivalent for `*log` glob pattern, is `log$`.

There are three types of regular expressions:

| Name | grep Switch | Power | Speed | Standard |
| --- | --- | --- | --- | --- |
| BREs (Basic Regular Expressions) | `-G` (default) | Low | High | POSIX |
| EREs (Extended Regular Expressions) | `-E` | Mid | Mid | POSIX |
| PCREs (Perl Compatible Regular Expressions) | `-P` | High | Low | PCRE |

ERE special characters and special character classes:

<table dir="ltr" style="border-collapse: collapse; width: 100%;">
<tr>
<td style="vertical-align: top; width: 50%; padding: 8px;">
<table dir="ltr" style="border-collapse: collapse; width: 100%;">
<thead>
<tr>
<th style="border: 1px solid #999; padding: 4px 8px; text-align: left;">Character</th>
<th style="border: 1px solid #999; padding: 4px 8px; text-align: left;">Description</th>
</tr>
</thead>
<tbody>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>.</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Any character except newline character</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>*</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Zero or more times of the preceding character</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Character class</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>^</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Start of the line / Negate</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>$</code></td><td style="border: 1px solid #999; padding: 4px 8px;">End of the line</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>{}</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Interval of preceding character</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>+</code></td><td style="border: 1px solid #999; padding: 4px 8px;">One or more times of the preceding character</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>?</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Zero or one time of the preceding character</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>|</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Or</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>()</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Group two or more characters as one</td></tr>
</tbody>
</table>
</td>
<td style="vertical-align: top; width: 50%; padding: 8px;">
<table dir="ltr" style="border-collapse: collapse; width: 100%;">
<thead>
<tr>
<th style="border: 1px solid #999; padding: 4px 8px; text-align: left;">Class</th>
<th style="border: 1px solid #999; padding: 4px 8px; text-align: left;">Description</th>
</tr>
</thead>
<tbody>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:alpha:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Equivalent to [a-zA-Z]</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:alnum:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Equivalent to [a-zA-Z0-9]</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:digit:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Equivalent to [0-9]</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:lower:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Equivalent to [a-z]</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:upper:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Equivalent to [A-Z]</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:punct:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">A punctuation character, such as !, #, $, and @</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:blank:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">A space or Tab character</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:space:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Any whitespace character: space, Tab, NL (newline), FF (formfeed), VT (vertical tab), CR (carriage return)</td></tr>
<tr><td style="border: 1px solid #999; padding: 4px 8px;"><code>[[:print:]]</code></td><td style="border: 1px solid #999; padding: 4px 8px;">Any printable character</td></tr>
</tbody>
</table>
</td>
</tr>
</table>

In BREs to use:

- `.`, `*`, `[]`, `^` and `$` as normal text characters, you must escape them by using `\,` and enclose the pattern in quotation marks. Also, if you position `^` anywhere other than at the beginning of the pattern, it acts like a normal character. The opposite is true for `$`.

- `{}`, `+`, `?`, `|`, and `()` as special characters, you must escape them (`\`) and enclose the pattern in quotation marks.

In EREs to use:

- special characters as normal text characters, you must escape them (`\`), and enclose the pattern in quotation marks.

- `$`, `{m,n}`, `|`,  and `()` as regex special characters, you must enclose the pattern in quotation marks, otherwise, errors happen.

Also, by using the `-F` switch in `grep`, the pattern will be interpreted as a literal (non-regex) string. See the [grep](06-grep.md) section for examples.

A few examples of BREs:

```bash
$ echo 'why' | grep w.y
why
$ echo 'why' | grep 'w.y'
why
$ echo 'why' | grep w\.y
why
$ echo 'why' | grep 'w\.y'
$ echo 'why' | grep w'\.'y
$ echo 'floor' | sed -n /o{2}r/p   # sed utilizes BRE engine
$ echo 'flo{2}r' | sed -n /o{2}r/p
flo{2}r
$ echo 'floor' | sed -n /o\{2\}r/p
$ echo 'floor' | sed -n '/o\{2\}r/p'
floor
$ echo 'floor' | sed -n /'o\{2\}r'/p
floor
$ echo 'floor' | sed -n /o'\{2\}'r/p
floor
$ echo b^n | grep b^
b^n
$ echo '$4.50' | grep '$4'
$4.50
```

A few examples of EREs:

```bash
$ echo 'bee' | grep -E e{2}
bee
$ echo 'bee' | grep -E e{1,3}
grep: e3: No such file or directory
$ echo 'bee' | grep -E 'e{1,3}'
bee
$ echo 'beeee' | grep -E 'e{1,3}'
beeee
$ echo 'beeee' | grep -E '(^|[^e])e{1,3}($|[^e])'   # exactly 1 to 3 e's
$ echo 'beee' | grep -E '(^|[^e])e{1,3}($|[^e])'
beee
$ echo 'be' | grep -E '(^|[^e])e{1,3}($|[^e])'
be
$ echo 'b' | grep -E '(^|[^e])e{1,3}($|[^e])'
$ echo 'eeb' | grep -E '(^|[^e])e{1,3}($|[^e])'
eeb
```

More examples:

```bash
$ cat regex_file.txt
fleet
flint
flirt
float
flout
flt
flet
break out
outbreak
$ grep fli.t regex_file.txt
flint
flirt
$ grep fl..t regex_file.txt | sort
fleet
flint
flirt
float
flout
$ grep fl[oe].t regex_file.txt
fleet
float
flout
$ grep fl.[m-y]. regex_file.txt
flint
flirt
flout
$ grep br regex_file.txt
break out
outbreak
$ grep ^br regex_file.txt
break out
$ grep ut$ regex_file.txt
flout
break out
$ grep b.*t regex_file.txt
break out
$ grep fle*t regex_file.txt
fleet
flt
flet
$ grep -E 'fle+t' regex_file.txt
fleet
flet
$ grep -E 'fle?t' regex_file.txt
flt
flet
$ grep -E 'fle{2}t' regex_file.txt   # equivalent to: grep -E 'fleet' regex_file.txt
fleet
$ echo 'fleeeet' >> regex_file.txt
$ grep -E 'fle{1,3}t' regex_file.txt   # includes 3; equivalent to: grep -E 'fl(e|ee|eee)t' regex_file.txt
fleet
flet
$ grep -E 'fle{1,}t' regex_file.txt
fleet
flet
fleeeet
$ grep -E 'flw{,2}t' regex_file.txt   # includes 0
flt
$ echo floated >> regex_file.txt
$ grep -E fl[^e][^ru] regex_file.txt
flint
float
floated
$ grep -E 'fl[^e][^ru].$' regex_file.txt
flint
float
$ echo floooaat >> regex_file.txt
$ grep -E fl[ao]+t regex_file.txt
float
floated
floooaat
$ grep -E 'fl[ao]{1,3}t$' regex_file.txt
float
$ grep -E 'int|oat' regex_file.txt
flint
float
floated
$ grep -E '^[a-zA-Z]{4}$|^[[:alpha:]]{7}$|^.{3}$' regex_file.txt
flt
flet
fleeeet
floated
$ echo seeded >> regex_file.txt
$ grep -E '(ed)+' regex_file.txt
floated
seeded
$ grep -E '(ed){2}' regex_file.txt
seeded
$ grep -E '[^e](e{1,3}|[nr])t' regex_file.txt   # equivalent to: '[^e](e{1,3}|n|r)t'
fleet
flint
flirt
flet
$ diff -s <(grep -E '^([a-zA-Z]{4}|[[:alpha:]]{7}|.{3})$' regex_file.txt | tee /dev/stderr) \
> <(grep -E '^[a-zA-Z]{4}$|^[[:alpha:]]{7}$|^.{3}$' regex_file.txt)
flt
flet
fleeeet
floated
Files /dev/fd/63 and /dev/fd/62 are identical
$ echo -e "line1\n\nline3" | grep -E '.'   # removes empty lines
line1
line3
```

For more information:

```bash
$ man 7 regex
```
