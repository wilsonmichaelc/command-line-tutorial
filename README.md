# Introduction to the Command Line

## Finding your terminal

- Windows: Start -> Accessories -> Windows PowerShell -> Windows PowerShell
- Linux: Alt+F2  -- If Linux Desktop
- Mac OS: Applications -> Utilities -> Terminal.app

## Basic Commands

#### Listing Files and Directories

*`Linux / Mac OS`*

```bash
ls          List contents of current directory. Default behavior is to use columns.
ls /path    List contents of specific directory.
ls -1       List content one item per line.
ls -la      List content one item per line, include attributes.
ls -R       List contents of directory and all subdirectories (recursively)
```

*`Windows`*

Note: On Windows, `ls` is an alias for Get-ChildItem.
```bash
ls
ls \path
ls -r
ls -r -inc *foo*
```

#### Cancel Command | Clear Screen

*`Linux / Mac OS`*

```bash
ctrl+c
ctrl+l
```

*`Windows`*

```bash
ctrl+c
cls
```

#### Getting your current directory

*`Linux / Mac OS / Windows`*
```bash
pwd
```

#### Changing Directories

*`Linux / Mac OS / Windows`*

Note: On Windows, `cd` is an alias for Set-Location.

```bash
cd directoryName
cd /path/to/directory       <--- Linux/Unix
cd \path\to\diretory        <--- Windows
```

#### Copy files/directories

*`Linux / Mac OS / Windows`*

```bash
cp foo.md /new/path/                <--- Copy file
cp foo.md /new/path/newName.md      <--- Copy and rename

cp -r barDir /new/path/newNameDir   <--- Copy directory and rename
```

#### Move files/directories

*`Linux / Mac OS / Windows`*

```bash
# Files
mv foo.md /new/path/                <--- Move file
mv foo.md /new/path/newName.md      <--- Move and rename

# Directories
mv barDir /new/path/newNameDir      <--- Move directory and rename

# Only want to rename the file/directory??? Use the move command
mv originalName.md newName.md
mv originalDir newDir
```

#### Creating Files

*`Linux / Mac OS`*
```bash
touch fileName.md
echo '' >> fileName.md
echo '## Hello World!' > hello.md    <--- Write
echo '## Hello World!' >> hello.md   <--- Append
```

*`Windows`*

```bash
New-Item -ItemType File fileName.md
echo '' >> fileName.md
echo '## Hello World!' > hello.md     <--- Write
echo '## Hello World!' >> hello.md    <--- Append
```

#### Creating Directories

*`Linux / Mac OS / Windows`*

Note: On Windows, `mkdir` is an alias for New-Item.

```bash
mkdir directoryName
```


#### Removing Files/Directories

*`Linux / Mac OS / Windows`*

Note: On Windows, `rm` is an alias for Remove-Item.
```bash
rm fileName.md          <--- remove file
rm -r directoryName     <--- remove directory
```

#### Showing a file's type

*`Linux / Mac OS`*

Node: No true windows alternative.
```bash
file fileName.md
```

#### Printing the contents of a file

*`Linux / Mac OS / Windows`*

Note: Make sure its a text file before you do this.
```bash
less fileName.md      <--- Linux/Unix Only
more fileName.md
cat fileName.md
```

#### Print lines from file that match a pattern

*`Linux / Mac OS`*

Note: The pipe sends the output of the first command to the input of the second.
```bash
cat fileName.md | grep "pattern"
```

#### List running processes

*`Linux / Mac OS`*

```bash
top
top -o <key>
```
Keys: pid, cpu, time, threads, ports, mem, uid, state, user

*`Windows`*

```bash
while(1) {ps | sort -des cpu | select -f 15 | ft -a; sleep 1; cls}
```

#### List snapshot of running processes

*`Linux / Mac OS`*

```bash
ps
ps aux | grep "pattern"
```

#### Find Files

*`Linux / Mac OS`*

```bash
find . -name "*.md"
find . -name "fileName.*"
find . -name "foo" -print | less
```

*`Windows`*

```bash
dir -r -filter "*.md"
dir -r -filter "foo" | more
```

#### Zip

*`Linux and Mac OS Only`*

```bash
zip -r foo.zip foo
zip -r foo foo

unzip -l foo.zip
unzip foo.zip
```

#### Tar

*`Linux and Mac OS Only`*

```bash
tar -zcvf file.tgz directory
tar -zxvf file.tgz
```

## Advanced Commands

*`Linux and Mac OS Only`*

#### Advanced Find

```bash
# Find all mod files and print the filename/dataset
find ./test -maxdepth 1 -name "*.mod" -exec grep -H '$DATA' {} \; | awk -F '[: ]' '{print $1, $3}'

# Show the dataset used by each mod file in the current directory
grep -H -R "$DATA" ./*.mod | grep "csv" | awk -F '[: ]' '{print $1, $3}'

# Find all mod files using THEOPP dataset
find ./ -maxdepth 4 -name "*.mod" -exec grep -H 'THEOPP.csv' {} \;
```

#### Regular Expressions

"...a regular expression is a sequence of characters that define a search pattern..." -Wikipedia

Interactive Regular Expression playground with a great Cheatsheet [RegExr<sub>v2.1</sub>](http://www.regexr.com/)

```bash
# Simple search
/hello/g

# Pattern Search - US Phone Number
/\d{3}(\-|\.)\d{3}(\-|\.)\d{4}/g

# Pattern Search - Email Address
/[a-zA-Z0-9.]+\@[a-zA-Z0-9.]+/g
```

#### sed

```bash
# Substitute (s/) globally (/g) IN (-i) this file
sed -i s/Dose/DOSE/g foo.mod
# Convert CSV to tab delimited
sed -i s/,/\\t/g file.csv
# Create a new file with the replacements
sed s/Dose/DOSE/g foo.mod > newFoo.mod

# Print everything between $PK and first blank line
sed -n '/\$PK/,/\$/{/^$/d; p}' foo.mod
# Print everything between $PK and the next $
sed -n '/\$PK/,/\$/{/\$/d; p}' foo.mod
# Print just the $THETA section not including $THETA or the keyword after it.
sed -n '/\$THEAT/,/\$/{/\$/d; p}' foo.mod
```

