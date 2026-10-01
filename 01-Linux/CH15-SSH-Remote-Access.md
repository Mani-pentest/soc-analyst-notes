# CH-15: SSH & Remote Access

## Overview

SSH (Secure Shell) is a protocol used to securely log into and control a remote machine over a network, encrypting all traffic between client and server.

**Note:** SSH is one of the most common services found open on target machines, and a frequent target for both legitimate access and misconfiguration abuse.

## Core Commands

| Command | Purpose |
|---|---|
| `ssh user@target_ip` | Connects to a remote machine as the specified user |
| `ssh -p 2222 user@target_ip` | Connects using a non-default port |
| `ssh -i keyfile user@target_ip` | Connects using a specific private key instead of a password |
| `scp file.txt user@target_ip:/path/` | Copies a file from local machine to a remote machine |
| `scp user@target_ip:/path/file.txt .` | Copies a file from a remote machine to the local machine |

## Key-Based Authentication

Instead of a password, SSH can authenticate using a matched pair of cryptographic keys — a PRIVATE key kept on your machine, and a PUBLIC key placed on the server.

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id user@target_ip
```

`ssh-keygen` generates the key pair (`-t rsa` sets the key type, `-b 4096` sets the strength). `ssh-copy-id` copies your public key to the remote server's authorized keys, enabling password-less login afterward.

## Common Misconfigurations to Check

| Issue | Why It Matters |
|---|---|
| Private key with weak permissions | SSH refuses to use a key that's readable by others (`chmod 600 keyfile` required) |
| Root login enabled (`PermitRootLogin yes`) | Allows direct root access over SSH — a high-value target |
| Password authentication with weak passwords | Makes brute-forcing viable |

## Summary

- `ssh user@ip` connects to a remote machine; `scp` transfers files over the same encrypted channel
- Key-based authentication uses a private/public key pair instead of a password
- Misconfigured permissions, enabled root login, or weak passwords are common SSH-related findings during a pentest
