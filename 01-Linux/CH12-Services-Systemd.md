# CH-12: Services and systemd

## Definition
A service is a background program managed by systemd (e.g. a web server, SSH, or database). systemd is the Linux init system that starts, stops, and monitors these services.

## Command
```bash
systemctl list-unit --type=service
```
List all currently active services.

## Why It Matters
Identifying active or misconfigured services is a common enumeration entry point into a target system.

## systemctl status <service>
Shows whether a service is running, stopped, or failed.

```bash
sudo systemctl start <service>    # Starts a service immediately
sudo systemctl stop <service>     # Stops a service immediately
sudo systemctl restart <service>  # Restarts a service immediately
sudo systemctl enable <service>   # Enable a service to auto-start on boot
sudo systemctl disable <service>  # Disable a service from auto-starting
```

## Example
Just installed an SSH server, want it to start now and every reboot:
```bash
sudo systemctl start sshd     # turn it on now
sudo systemctl enable sshd    # start on every reboot
systemctl status sshd         # confirm it's active
```

## Summary
- `systemctl status` shows a service's current state.
- `start`/`stop`/`restart` manage a service by systemd.
- `enable`/`disable` control what can be done with autostarting.
- `systemctl list-units --type=service` shows everything currently running.
