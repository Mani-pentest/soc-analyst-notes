# CH-10: Bash Scripting Basics

## Overview
A bash script is a text file containing a sequence of shell commands, executed in order, used to automate repetitive tasks.

## Structure
```bash
#!/bin/bash
```
The shebang line always sits at the very top of the script, specifying which shell should run the script.

## Creating and Running

```bash
nano myscript.sh    # Create/edit a script
chmod +x myscript.sh #  give it execute permission first
./myscript.sh # Runs the script 
```

## Variables
```bash
name="pentester"
echo "Hello, $name"
```
A labeled box for storing values.

## Conditionals
```bash
if ["$1" == "root"]; then
  echo "You are root"
else
  echo "You are not root"
fi
```
`if/else` making decisions.

## Loops

### for loop — Repeating actions
```bash
for ip in 10.10.10.1 10.10.10.2 10.10.10.3; do
  ping -c 1 $ip
done
```
Repeat an action for each IP in a list.

### while loop — Repeat action until a condition becomes false
```bash
count=1
while [$count -le 5]; do
  echo "Attempt $count"
  count=$((count+1))
done
```

## Arguments
```bash
#!/bin/bash
echo "Target IP: $1"
```
`$1, $2 = ...etc.` — refer to arguments passed.

**Example script:**
```bash
./myscript.sh 10.10.10.10
# Target IP: 10.10.10.10
```

## Argument Handling
- Input typed when running the script.
- `#!/bin/bash` — automatically saved in sequence commands, own arguments; `$1`, `$2` capture arguments passed to `./myscript.sh`.
- Requires `#!/bin/bash` and `chmod +x` to be executed.

## Summary
- A script = saved terminal commands, run automatically in sequence.
- `$1, $2` capture arguments; `if/else` handle conditions; `for/while` handle repetition.
