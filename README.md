# Kubernetes worker Snap package

This is currently a work in progress.

## Developer notes

When building with `snapcraft`'s default environment (thus using `multipass`),
the `kubernetes` part will fail due to running out of memory.  I cannot see
how to expand the `multipass` VM's memory via `snapcraft`'s CLI, so we must force
the build to use `lxd` instead.

```bash
SNAPCRAFT_BUILD_ENVIRONMENT=lxd snapcraft
```
