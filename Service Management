# Service Management Guide

Service management involves controlling system services - starting, stopping, enabling, disabling, and monitoring them. On modern Linux systems, this is primarily done using `systemctl` with systemd.

## What is Service Management?

Service management means controlling background processes (services/daemons) that run on your system. These services provide functionality like web servers, databases, network services, etc.

**Example:** `systemctl start nginx` starts the nginx web server service.

## Basic Commands

```bash
systemctl start servicename     # Start service
systemctl stop servicename      # Stop service
systemctl restart servicename   # Restart service
systemctl status servicename    # Check service status
systemctl enable servicename    # Enable at boot
systemctl disable servicename   # Disable at boot
```

## Common Examples

```bash
# Web servers
systemctl start nginx
systemctl status apache2

# Databases
systemctl start mysql
systemctl restart postgresql

# SSH
systemctl enable ssh
systemctl status sshd
```

## Useful Commands

```bash
systemctl list-units --type=service        # List all services
systemctl list-units --failed              # List failed services
journalctl -u servicename                  # View service logs
systemctl daemon-reload                    # Reload systemd config
```

## Service States

- **active (running)** - Service is running
- **inactive (dead)** - Service is stopped
- **failed** - Service failed to start

## Quick Reference

| Command | Purpose |
|---------|---------|
| `systemctl start service` | Start a service |
| `systemctl stop service` | Stop a service |
| `systemctl restart service` | Restart a service |
| `systemctl status service` | Check service status |
| `systemctl enable service` | Enable service at boot |
| `systemctl disable service` | Disable service at boot |
| `systemctl is-active service` | Check if service is running |
| `systemctl is-enabled service` | Check if service is enabled |
| `journalctl -u service` | View service logs |
| `systemctl list-units --type=service` | List all services |


## Example Workflow

Here's a typical workflow for managing a web server:

```bash
# Install and configure nginx
sudo apt install nginx                 # Install nginx
sudo systemctl start nginx            # Start the service
sudo systemctl status nginx           # Verify it's running
sudo systemctl enable nginx           # Enable at boot
sudo systemctl reload nginx           # Reload after config changes
```

## Best Practices

1. **Always check status** after starting/stopping services
2. **Use reload instead of restart** when possible to avoid downtime
3. **Enable important services** to start at boot
4. **Monitor service logs** regularly for issues
5. **Use systemctl daemon-reload** after modifying service files
6. **Test services** in development before deploying to production
