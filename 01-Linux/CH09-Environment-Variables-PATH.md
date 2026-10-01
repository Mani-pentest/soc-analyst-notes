# CH-09: Environment Variables & PATH

## Environment Variables
**Definition** — Named values shared by the shell that programs can reference.

**Example:** Your home directory, username.

```bash
echo $HOME    # Print the value of HOME
echo $USER    # Print the current username
echo $PATH    # Print the list of folders searched when a command is typed
env           # List every environment variable when a command is immediately typed
```

## $PATH
**Definition** — A colon-separated list of directories the shell searches, in order, to locate a command's executable file when you type a name.

**Example:** `$HOME`, `$USER`, `$PATH` are common environment variables.

## Modifying $PATH
```bash
export PATH=$PATH:/opt/mytool
```
- `export PATH=$PATH:/opt/mytool` → adds a folder, `/opt/mytool`, to the current session only
- `echo $PATH` → Print the current PATH value
- `source ~/.bashrc` → reloads it immediately
- `# adds it permanently` — put the `export` line in `~/.bashrc` to make it permanent

## $PATH determines which folders are searched for commands, by name.

**If a program isn't in a folder listed in `$PATH`, typing it not found**, even if it temporarily exists — its name must be added to `~/.bashrc` (or `export PATH=$PATH:/folder`) to make it permanent.

**Example:**
```bash
# /usr/local/bin:/usr/bin:/bin:/usr/sbin
```
`$PATH` is searched for commands by name.

## Summary
- `$HOME`, `$USER`, `$PATH` are common environment variables.
- `$PATH` determines which folders are searched when you run a command.
- `export PATH=$PATH:/folder` adds a folder to `$PATH` temporarily; adding it to `~/.bashrc` makes it permanent.
