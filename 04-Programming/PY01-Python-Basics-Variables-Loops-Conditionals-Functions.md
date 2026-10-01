# PY01: Python Basics I — Variables, Loops, Conditionals, Functions

Python is dynamically typed — variable types are inferred automatically from the assigned value, with no explicit type declaration needed. This chapter covers variables, lists, loops, conditionals, and functions — the core building blocks used in virtually every automation and recon script.

## Variables

A variable is a named reference to a stored value. Python determines the type automatically based on what's assigned.

```python
age = 25          # int
name = "Manikanta" # str
is_admin = False   # bool
```

**Note:** Wrapping a number in quotes makes it a string, not an integer — `port = "443"` is a `str`, not an `int`. Mixing types in arithmetic (e.g. `"443" + 1`) raises an error, since Python does not implicitly convert between string and numeric types.

## Lists

A list is an ordered container holding multiple values.

```python
ports = [22, 80, 443, 3389]
```

## Loops (`for`)

A `for` loop iterates over each item in a list, running the indented block once per item. The number of iterations equals the length of the list.

```python
for port in ports:
    print(port)
```

## Conditionals (`if` / `else`)

Conditionals branch execution based on whether a condition evaluates to `True` or `False`.

```python
if port < 1024:
    print("well-known port")
else:
    print("registered/dynamic port")
```

## Loops and Conditionals Combined

Combining a loop with a conditional is the basic skeleton behind many recon/automation tools — iterating over a set of values and evaluating each one.

```python
ports = [22, 80, 443, 3389]

for port in ports:
    if port < 1024:
        print(port, "- well-known port")
    else:
        print(port, "- registered/dynamic port")
```

## Functions

Functions package logic for reuse, defined using `def`. The `return` keyword sends a value back to the caller silently, which is distinct from `print()`, which displays output immediately.

```python
def check_port(port):
    if port < 1024:
        return "well-known port"
    else:
        return "registered/dynamic port"
```

**Note:** Calling `check_port(80)` alone produces no visible output — the returned value must be explicitly printed (`print(check_port(80))`) or stored in a variable (`result = check_port(80)`) to be seen.

**Note:** Since `return` exits the function immediately, an `else` is not strictly required if the `if` branch already returns a value — any code reached after the `if` implies the condition was false.
