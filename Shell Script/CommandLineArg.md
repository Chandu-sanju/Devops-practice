# Bash Command Line Arguments (CLA)

## What Are Command Line Arguments?

Command Line Arguments (CLA) are values passed to a script when it is executed.

### Syntax

```bash
./script.sh arg1 arg2 arg3 arg4
```

Example:

```bash
./script.sh apple banana cat dog worldcup
```

---

# Special Positional Parameters

## Sample Script

```bash
#!/bin/bash

# Checking how command line arguments work

echo "\$0 will print the name of the command itself = $0"
echo "\$1 will print the first argument = $1"
echo "\$2 will print the second argument = $2"
echo "\$3 will print the third argument = $3"
echo "\$4 will print the fourth argument = $4"

echo "\$# will print the total number of arguments passed = $#"

echo "\$@ will print all arguments = $@"
echo "\$* will print all arguments = $*"

echo "\$$ will print the process ID of the script = $$"

echo "\$? will print previous command exit status = $?"

echoram

echo $?

echo "Testing \$?"
echo $?
```

---

## Sample Output

```bash
[root@practice script]# ./script.sh apple banana cat dog worldcup

$0 will print the name of the command itself = ./script.sh
$1 will print the first argument = apple
$2 will print the second argument = banana
$3 will print the third argument = cat
$4 will print the fourth argument = dog

$# will print the total number of arguments passed = 5

$@ will print all arguments = apple banana cat dog worldcup

$* will print all arguments = apple banana cat dog worldcup

$$ will print the process ID of the script = 3596

$? will print previous command exit status = 0

./script.sh: line 16: echoram: command not found

127

Testing $?
0
```

---

# Understanding Special Variables

| Variable | Description                     |
| -------- | ------------------------------- |
| `$0`     | Script name                     |
| `$1`     | First argument                  |
| `$2`     | Second argument                 |
| `$n`     | nth argument                    |
| `$#`     | Total number of arguments       |
| `$@`     | All arguments (separate words)  |
| `$*`     | All arguments (single string)   |
| `$$`     | Current process ID (PID)        |
| `$?`     | Exit status of previous command |

---

# Difference Between `$@` and `$*`

This is one of the most important Bash concepts.

## Example Script

```bash
#!/bin/bash

echo "--> Creating files: $@"

touch "$@"

rm -v "$@"
```

Execute in debug mode:

```bash
sh -x test2.sh aasg vcse ssba
```

---

## Using `"$@"`

```bash
+ touch aasg
+ touch vcse
+ touch ssba

+ rm -v aasg vcse ssba

removed 'aasg'
removed 'vcse'
removed 'ssba'
```

### What Happened?

`"$@"` preserves each argument separately.

Equivalent to:

```bash
touch "aasg" "vcse" "ssba"
```

---

## Using `"$*"`

```bash
+ rm -v 'aasg vcse ssba'

rm: cannot remove 'aasg vcse ssba': No such file or directory
```

### What Happened?

`"$*"` combines all arguments into a single string.

Equivalent to:

```bash
rm "aasg vcse ssba"
```

Bash tries to remove one file named:

```text
aasg vcse ssba
```

which doesn't exist.

---

# When to Use `$@` vs `$*`

## Use `"$@"` (99% of the time)

Best for:

* Processing arguments
* Loops
* Passing arguments to another command
* Handling filenames containing spaces

Example:

```bash
./script.sh "chandra sekhar" banana
```

`"$@"` preserves:

```text
chandra sekhar
```

as a single argument.

---

## Use `"$*"`

Best for:

* Logging
* Printing
* Creating a sentence from all arguments

Example:

```bash
echo "$*"
```

Output:

```text
apple banana cat dog
```

---

## Rule of Thumb

```bash
Always use "$@"
```

Use `"$*"` only when you intentionally want all arguments merged into one string.

---

# Double-Digit Arguments

For arguments greater than 9:

❌ Incorrect

```bash
$10
```

Bash interprets it as:

```bash
$1 + 0
```

Example:

```bash
$1 = apple

$10 => apple0
```

---

## Correct Syntax

```bash
${10}
${11}
${12}
```

Example:

```bash
echo ${10}
echo ${11}
```

---

# Shift Command

`shift` is a Bash built-in command.

Think of command line arguments as a conveyor belt.

When `shift` is executed:

```text
$2 becomes $1
$3 becomes $2
$1 is discarded
```

---

## Example

```bash
#!/bin/bash

echo "\$1 ==> $1"
echo "\$2 ==> $2"
echo "\$3 ==> $3"

shift

echo "\$1 ==> $1"
echo "\$2 ==> $2"
```

Execute:

```bash
sh -x test3.sh chandu sanju 007
```

Output:

```bash
$1 ==> chandu
$2 ==> sanju
$3 ==> 007

shift

$1 ==> sanju
$2 ==> 007
```

---

## Processing All Arguments

A common pattern:

```bash
while [ $# -ne 0 ]
do
    echo "$1"
    shift
done
```

Output:

```text
chandu
sanju
007
```

---

## Shift Multiple Positions

```bash
shift 2
```

Removes first two arguments.

Example:

```text
Before:
$1 = a
$2 = b
$3 = c
$4 = d

After shift 2:
$1 = c
$2 = d
```

---

# Set Command

`set` is often considered the opposite of `shift`.

It can assign values to positional parameters.

Example:

```bash
set -- apple chandra banana
```

Now:

```bash
$1 = apple
$2 = chandra
$3 = banana
```

---

## Important Note

`set` only affects the current shell session.

Every script starts in its own process.

Therefore:

```bash
set -- apple banana
```

inside one script will not affect another script.

---

# Handling Default Values

Good scripts should not fail when users forget to provide arguments.

Use parameter expansion.

## Example

```bash
NAME=${1:-"Guest"}

echo "Hello, $NAME!"
```

---

### Execution

```bash
./script.sh
```

Output:

```text
Hello, Guest!
```

---

### Execution with Argument

```bash
./script.sh Chandra
```

Output:

```text
Hello, Chandra!
```

---

# Double Dash (`--`)

The double dash is called an **option terminator**.

It tells a command:

> "Stop processing options. Everything after this is data."

---

## Problem Example

Suppose the user passes:

```bash
./script.sh -chandu
```

Script:

```bash
touch $1
```

Bash executes:

```bash
touch -chandu
```

`touch` thinks `-chandu` is an option, not a filename.

Result:

```bash
touch: invalid option
```

---

## Solution

Use:

```bash
touch -- "$1"
```

Now Bash interprets everything after `--` as an argument.

Example:

```bash
touch -- -chandu
```

Creates a file named:

```text
-chandu
```

---

## Demonstration

```bash
[root@practice script]# sh -x test3.sh -chandu aanchal

+ touch -chandu

touch: invalid option -- 'n'

+ touch aanchal
```

Correct version:

```bash
touch -- "$1"
```

---

# Best Practices Summary

✅ Use `"$@"` instead of `$@`

✅ Quote variables whenever possible

```bash
"$1"
"$2"
"$@"
```

✅ Use `${10}`, `${11}` for double-digit arguments

✅ Use `shift` for argument processing loops

✅ Use `${VAR:-default}` for fallback values

✅ Use `--` before user-supplied filenames

```bash
rm -- "$file"
touch -- "$file"
cp -- "$src" "$dst"
```

✅ Check exit status using:

```bash
$?
```

---

# Quick Cheat Sheet

| Symbol            | Meaning                      |
| ----------------- | ---------------------------- |
| `$0`              | Script name                  |
| `$1-$9`           | Positional arguments         |
| `${10}`           | Tenth argument               |
| `$#`              | Number of arguments          |
| `$@`              | All arguments separately     |
| `$*`              | All arguments as one string  |
| `$$`              | Process ID                   |
| `$?`              | Previous command exit status |
| `shift`           | Remove first argument        |
| `shift n`         | Remove first n arguments     |
| `set --`          | Set positional parameters    |
| `${var:-default}` | Default value                |
| `--`              | End of command options       |

```
```
