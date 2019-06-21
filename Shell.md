* Special symbols

> -: dash
> 
> /: slash
> 
> \\: backslash
> 
> (): parentheses
> 
> \[]: brackets
> 
> {}: curly braces
> 
> ~: tilde
> 
> `: back tick
> 
> *: asterisk
> 
> \#: pound


* ls

List directory contents:
```
ls [options] [file/dir]
```

List in reverse order:
```
ls -r
```
List recursively directory tree:
```
ls -R
```

List all files:
```
ls -a
```

* mkdir

Create new direcotries:
```
mkdir [option]
mkdir my\ cool\ folder
mkdir 'my cool folder'
```

* cp, mv, rm

> Hidden files' names start with .

Copy files:

```
cp *.jpg
```

Copy a file recursively:
```
cp -r abc
```
Rename or move:
```
mv
```
Add prefix by batch:
```
for filename in *.jpg; do mv "$filename" "prefix_$filename"; done;
```

rename by batch:
```
for filename in *.jpg; do echo mv \"$filename\" \"${filename//_namechangefrom_/_namechangeinto_}\"; done > remane.txt
for filename in *.jpg; do echo mv \"$filename\" \"${filename//_namechangefrom_/_namechangeinto_}\"; done | /bin/bash
for filename in *.jpg; do mv "$filename" "${filename//_namechangefrom_/_namechangeinto_}"; done
```

Remove:
```
rm
```

Remove directory:
```
rm -r
```

Remove directory with no prompt (be careful):
```
rm -rf
```

* cat, less, head, tail

Display contents of a file or files:
```
cat [option] [file]

cat a.cpp
cat a.cpp b.cpp
cat \*.cpp
```

Create a file:
```
cat c.cpp
```
Display contents of a file:
```
less [option] [file]
```

To the beginning of a file:
```
less -g
```
To the end of a file:
```
less -G
```
Word search:
```
/
```
Quit out of less and go back to shell:
```
q
```
*Less is a file reading program, and Cat is a string manipulation program.*

Display the first k(10) lines:
```
head (-k) [file]
```
Display the last k(10) lines:
```
tail (-k) [file]
```
* grep

Global search regular expression and print out the line:
```
grep match_pattern [file]
grep "match_pattern" [file]
grep match_pattern file_1 file_2 file_3
grep -E "[1-9]+"
```

Print the searched content only:

```
grep -o -E "[a-z]+\."
```
Count the match times:
```
grep -c "text" file_name
```
Print the line number:
```
grep "text" -n file_name
cat file_name | grep "text" -n
```

Print the match file:
```
grep -l "text" file1 file2 file3...
```
Search recursively:
```
grep "text" . -r -n
```
Ignore uppercase/lowercase difference:
```
grep -i "HELLO"
```
Print target line and previous k lines:
```
grep "text" -A k
```
Print target line and following k lines:
```
grep "text" -B k
```
Print both:

```
grep "text" -C 3
```
* Count files

Count files:

```
ls -l | grep "^-" | wc -l
```
Count files recursively:

```
ls -lR| grep "^-" | wc -l
```
Count diretroies:

```
ls -lR | grep "^d" | wc -l
```

File sizes:

```
ls -sh [file/dir]
```

Directory size totally:
```
du -hs [directory]
```

* Vim

![vim](resources/vim.png)