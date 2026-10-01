# PROG-BASH01: Bash Reinforcement — Host Availability Checker

**Definition:** This chapter reinforces earlier bash scripting fundamentals (CH09-10) by building a practical host availability checker — applying loops, conditionals, and exit code checking together in one working script.

---

## Loop Syntax (Bash vs Python)

**Definition:** Bash uses `do` and `done` keywords to mark the start and end of a loop body, in place of Python's colon-and-indentation style.

```bash
for ip in 192.168.1.1 192.168.1.2 192.168.1.3
do
    echo $ip
done
```

| Language | Loop body markers |
|---|---|
| Python | `:` + indentation |
| Bash | `do` ... `done` |

---

## Exit Codes

**Definition:** Every command returns an exit code upon completion — `0` indicates success, any non-zero value indicates failure. The special variable `$?` holds the exit code of the most recently executed command.

```bash
ping -c 1 192.168.1.1
echo $?
```

---

## Conditional on Command Success

**Definition:** Bash's `if` statement can test a command's success or failure directly, rather than only evaluating a stored value.

```bash
if ping -c 1 192.168.1.1 &> /dev/null
then
    echo "192.168.1.1 is UP"
else
    echo "192.168.1.1 is DOWN"
fi
```

| Part | Definition |
|---|---|
| `&> /dev/null` | Suppresses the command's standard output and error messages, keeping the terminal clean |
| `fi` | Closes the `if` block — bash spells "if" backwards, the same convention as `do`/`done` closing a loop |

---

## Full Script — Host Availability Checker

```bash
for ip in 192.168.1.1 192.168.1.2 192.168.1.3
do
    if ping -c 1 "$ip" &> /dev/null
    then
        echo "$ip is UP"
    else
        echo "$ip is DOWN"
    fi
done
```

- Quoting the variable (`"$ip"`) is good practice, preventing errors if a value ever contains spaces or special characters.

---

## Why It Matters

This is a real, usable "which hosts are alive" recon script — the same underlying logic (loop + conditional) as the Python `requests`-based site checker from PY02, just written in bash syntax. It demonstrates that the recon pattern (loop over targets, check status, report result) is language-agnostic.

## Summary

- Bash loops use `do`/`done` instead of Python's colon-and-indentation
- Every command returns an exit code — `0` for success, non-zero for failure — accessible via `$?`
- Bash `if` can test a command's success/failure directly, not just a stored value
- `&> /dev/null` suppresses output for a clean terminal; `fi` closes an `if` block just as `done` closes a loop
- Quoting variables (`"$ip"`) protects against bugs from spaces or special characters
- This script mirrors PY02's site-checker logic — same loop-and-conditional recon pattern, different language
