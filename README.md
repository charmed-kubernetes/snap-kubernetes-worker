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

### cacert

We also need to feed in a `ca.cert` to allow Kubernetes to get data from the kubelet.

Example:

```bash
snap set kubernetes-worker cacert="$(cat ~/ca.crt)"
```

## Commands

## ctr

Access the underlying containerd CLI.  Must be used with `sudo`.

Example:

```bash
sudo kubernetes-worker.ctr --namespace=k8s.io containers ls
```

## kubectl

Run the Kubernetes CLI within the context of the Snap.

Example:

```bash
kubernetes-worker.kubectl get all --all-namespaces
```

## Developer notes

When building with `snapcraft`'s default environment (thus using `multipass`),
the `kubernetes` part will fail due to running out of memory.  I cannot see
how to expand the `multipass` VM's memory via `snapcraft`'s CLI, so we must force
the build to use `lxd` instead.

```bash
snapcraft --use-lxd
```
