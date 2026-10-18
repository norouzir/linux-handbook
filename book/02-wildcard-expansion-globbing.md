# Wildcard Expansion (Globbing)

Globbing is the operation that expands a wildcard pattern into the list of pathnames matching the pattern.

| Character | Meaning | Example |
| --- | --- | --- |
| * | Any string, including empty | $ ls c*.txt<br>cake.txt  c2.txt |
| ? | Any single character | $ ls b??d<br>bard  bird |
| [...] | Multiple options for single character position | $ ls p[ui]t<br>pit  put<br>$ ls f[a-dt-v]ll<br>fall  full |
| [!...] | Negate | $ ls f[!a-dt-v]ll<br>fill  fell |

Globbing is performed by the shell, not by the command itself. The shell expands patterns into matching pathnames, then passes the resulting arguments to the command.

If no files or directories match a glob pattern, the shell does not expand it and passes the pattern unchanged to the command (this behavior can be changed).

Examples of glob patterns using `*` and `/`:

```bash
$ tree
.
├── d1A
│   ├── d2A
│   │   ├── d3
│   │   └── f3
│   ├── d2B
│   ├── f2A
│   └── f2B
├── d1B
└── f1B

6 directories, 4 files
$ printf '%s\n' *
d1A
d1B
f1B
$ printf '%s\n' */
d1A/
d1B/
$ printf '%s\n' */*
d1A/d2A
d1A/d2B
d1A/f2A
d1A/f2B
$ printf '%s\n' */*/
d1A/d2A/
d1A/d2B/
$ printf '%s\n' */*/*
d1A/d2A/d3
d1A/d2A/f3
$ printf '%s\n' */*/*/*   # no matches found, so the shell left the pattern unchanged
*/*/*/*
$ printf '%s\n' */*A
d1A/d2A
d1A/f2A
$ printf '%s\n' */* */*/*
d1A/d2A
d1A/d2B
d1A/f2A
d1A/f2B
d1A/d2A/d3
d1A/d2A/f3
```

Bash provides the following options, which can be enabled with the `shopt` command:

**globstar:** when enabled, the `**` pattern matches files and directories recursively at any depth. When disabled, the `**` pattern has no special meaning and is equivalent to `*`. It is usually disabled by default.

```bash
$ printf '%s\n' **
d1A
d1B
f1B
$ shopt -s globstar
$ printf '%s\n' **
d1A
d1A/d2A
d1A/d2A/d3
d1A/d2A/f3
d1A/d2B
d1A/f2A
d1A/f2B
d1B
f1B
$ printf '%s\n' **/*
d1A
d1A/d2A
d1A/d2A/d3
d1A/d2A/f3
d1A/d2B
d1A/f2A
d1A/f2B
d1B
f1B
$ printf '%s\n' **/f*   # equivalent to: find . -name 'f*'
d1A/d2A/f3
d1A/f2A
d1A/f2B
f1B
$ printf '%s\n' **f*
f1B
$ printf '%s\n' **/
d1A/
d1A/d2A/
d1A/d2A/d3/
d1A/d2B/
d1B/
$ printf '%s\n' */*/**
d1A/d2A
d1A/d2A/d3
d1A/d2A/f3
d1A/d2B
```

**extglob:** when enabled, extended pattern-matching operators become available. It is usually enabled by default.

| Operator | Description |
| --- | --- |
| ?(pattern-list) | Zero or one occurrence |
| *(pattern-list) | Zero or more occurrences |
| +(pattern-list) | One or more occurrences |
| @(pattern-list) | Exactly one of the patterns |
| !(pattern-list) | Anything except the patterns |

```bash
$ ls -F
image/  image2/  image22  image222  image2222  image22.png  image23  image3  image.jpg  image.png  video.mp4
$ ls image/
pic1.jpeg  pic2.jpeg
$ ls image2/
sc1.png  sc2.png  sc3.png
$ shopt -u extglob
$ printf '%s  ' image?(.png); echo
bash: syntax error near unexpected token `('
$ shopt -s extglob
$ printf '%s  ' image?(.png); echo   # equivalent to: printf '%s  ' image@(|.png); echo
image  image.png  
$ printf '%s  ' image?(2|.png); echo
image  image2  image.png  
$ printf '%s  ' image?(22).png; echo
image22.png  image.png  
$ printf '%s  ' image?(2?); echo
image  image22  image23  
$ printf '%s  ' image*(2); echo
image  image2  image22  image222  image2222  
$ printf '%s  ' image*(22); echo
image  image22  image2222  
$ printf '%s  ' image+(22); echo
image22  image2222  
$ printf '%s  ' image@(22); echo
image22  
$ printf '%s  ' image.@(png|jpg|jpeg); echo
image.jpg  image.png  
$ printf '%s  ' image.{png,jpg,jpeg}; echo
image.png  image.jpg  image.jpeg  
$ printf '%s  ' image?(2)/*.@(png|jpeg); echo 
image2/sc1.png  image2/sc2.png  image2/sc3.png  image/pic1.jpeg  image/pic2.jpeg  
$ printf '%s  ' **([0-9]).@(png|mp4); echo
image22.png  image.png  video.mp4  
$ printf '%s  ' **/**([0-9]).@(png|mp4); echo 
image22.png  image2/sc1.png  image2/sc2.png  image2/sc3.png  image.png  video.mp4  
$ printf '%s  ' *.*; echo
image22.png  image.jpg  image.png  video.mp4  
$ printf '%s  ' *.!(mp4); echo
image22.png  image.jpg  image.png  
$ rm -r image@(+(2)|3); ls
image  image22.png  image23  image.jpg  image.png  video.mp4
$ printf '%s  ' *!(.mp4); echo
image  image22.png  image23  image.jpg  image.png  video.mp4  
$ printf '%s  ' !(*.mp4); echo
image  image22.png  image23  image.jpg  image.png  
$ rm -r !(image|video.mp4); ls
image  video.mp4
```

In Bash extglob patterns, `/` does not behave like it does in normal pathname globs:

```bash
$ printf '%s  ' */; echo
image/  
$ printf '%s  ' !(*/); echo
image  video.mp4  
$ printf '%s  ' **/*.*; echo
image/sc1.png  image/sc2.png  image/sc3.png  video.mp4  
$ printf '%s  ' !(**/*.*); echo
image  video.mp4  
```

These operators can also be used in scripts:

```bash
$ cat .test_script.sh
name=foo
if [[ $name == @(foo|bar) ]]; then
    echo match
fi
```

**dotglob:** when enabled, filenames beginning with a `.` are included in pathname expansion. It is usually disabled by default.

```bash
$ ls -a
.  ..  pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg
$ printf '%s  ' *; echo
pic1.jpeg  pic2.jpeg  
$ printf '%s  ' .*; echo
.pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' .* *; echo
.pic3.jpeg  .pic4.jpeg  .pic5.jpeg  pic1.jpeg  pic2.jpeg  
$ printf '%s  ' ?(.)*; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' {,.}*; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' *.jpeg; echo
pic1.jpeg  pic2.jpeg  
$ shopt -s dotglob
$ printf '%s  ' *; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' .*; echo
.pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' .* *; echo
.pic3.jpeg  .pic4.jpeg  .pic5.jpeg  pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' ?(.)*; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' {,.}*; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' *.jpeg; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' pic*; echo
pic1.jpeg  pic2.jpeg  
$ printf '%s  ' .pic*; echo
.pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
```

**nullglob:** when enabled, patterns with no matches expand to nothing instead of remaining unchanged. It is usually disabled by default.

```bash
$ ls
pic1.jpeg  pic2.jpeg
$ printf '%s  ' *.webp; echo
*.webp  
$ shopt -s nullglob 
$ printf '%s\n' *.webp; echo

$ echo $?
0
```

**failglob:** when enabled, patterns with no matches cause an error instead of remaining unchanged. It is usually disabled by default.

```bash
$ ls
pic1.jpeg  pic2.jpeg
$ printf '%s  ' *.webp; echo
*.webp  
$ shopt -s failglob 
$ printf '%s  ' *.webp; echo
bash: no match: *.webp
$ echo $?
1
```

If both `nullglob` and `failglob` are enabled, `failglob` takes precedence.

```bash
$ shopt nullglob failglob 
nullglob        on
failglob        on
$ printf '%s  ' *.webp; echo
bash: no match: *.webp
```

**nocaseglob:** when enabled, pathname expansion ignores character case. It is usually disabled by default.

```bash
$ ls
pic1.jpeg  pic2.jpeg
$ printf '%s  ' *.JPEG; echo
bash: no match: *.JPEG
$ shopt -s nocaseglob
$ printf '%s  ' *.JPEG; echo
pic1.jpeg  pic2.jpeg  
```

**globskipdots:** when enabled, pathname expansion never matches `.` and `..`. It is usually enabled by default.

```bash
$ shopt -u globskipdots
$ shopt -s dotglob
$ ls -a
.  ..  pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg
$ printf '%s  ' *; echo
pic1.jpeg  pic2.jpeg  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
$ printf '%s  ' .*; echo
.  ..  .pic3.jpeg  .pic4.jpeg  .pic5.jpeg  
```
