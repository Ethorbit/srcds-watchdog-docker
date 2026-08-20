# SRCDS Watchdog
A small Docker watchdog that monitors [SRCDS containers](https://github.com/Ethorbit/steamcmd-server-docker/tree/main) through RCON and automatically restarts them if they are unresponsive.

## How It Works

* Discovers SRCDS containers using **Docker labels**.
* Periodically checks **RCON** with `status`.
* Retries failed checks.
* Restarts containers after repeated failures.
* Runs independently from the monitored SRCDS containers.

## Labels
Each monitored container specifies its RCON settings through Docker labels:

```yaml
labels:
  - srcds.watchdog.enabled=true
  - srcds.watchdog.rcon.port=27015
  - srcds.watchdog.rcon.password_file=/run/rconpass
```

The watchdog uses the container's Docker name/network address automatically.

## Security

The watchdog requires access to the **Docker daemon**. This is **privileged access** and should be treated as equivalent to host-level access.

RCON passwords are supplied through **mounted password files**. These files should be stored securely and mounted into the watchdog container read-only.

Only deploy the watchdog in a trusted environment and restrict access accordingly.
