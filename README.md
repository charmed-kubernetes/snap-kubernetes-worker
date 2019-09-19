# Kubernetes worker Snap package

This is currently a work in progress.

## Installing

Install via `snap`.

```bash
snap install kubernetes-worker
```

Then connect all the interfaces.

```bash
for i in docker-privileged k8s-kubelet dot-kube docker-support firewall-control hardware-observe kernel-module-control mount-observe network-control process-control system-observe; do snap connect kubernetes-worker:$i; done
```

## Configuration

### kubeconfig

We must feed in a `kubeconfig` file to allow the worker to enrol with the 
cluster.

Example:

```bash
snap set kubernetes-worker kubeconfig="$(cat ~/.kube/config)"
```

## Developer notes

When building with `snapcraft`'s default environment (thus using `multipass`),
the `kubernetes` part will fail due to running out of memory.  I cannot see
how to expand the `multipass` VM's memory via `snapcraft`'s CLI, so we must force
the build to use `lxd` instead.

```bash
SNAPCRAFT_BUILD_ENVIRONMENT=lxd snapcraft
```
