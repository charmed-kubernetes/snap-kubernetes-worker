# Kubernetes worker Snap package

This is currently a work in progress.

## Installing

Install via `snap`.

```bash
sudo snap install kubernetes-worker
```

Then connect all the interfaces.

```bash
sudo snap connect kubernetes-worker:kernel-module-control
```

## Configuration

### kubeconfig

We must feed in a `kubeconfig` file to allow the worker to enrol with the 
cluster.

Example:

```bash
sudo snap set kubernetes-worker kubeconfig="$(cat ~/.kube/config)"
```

### cacert

We also need to feed in a `ca.cert` to allow Kubernetes to get data from the kubelet.

Example:

```bash
sudo snap set kubernetes-worker cacert="$(cat ~/ca.crt)"
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

When building with `snapcraft`, due to the snap using `core18`, the build will
take place within a `multipass` container.  We need to up the default memory
for this container, otherwise the build will fail.  We can do this with the
following command.

```bash
SNAPCRAFT_BUILD_ENVIRONMENT_MEMORY=8G snapcraft
```

We can also add cores to make the build faster.

```bash
SNAPCRAFT_BUILD_ENVIRONMENT_MEMORY=8G SNAPCRAFT_BUILD_ENVIRONMENT_CPU=8 snapcraft
```

Install the built snap.

```bash
sudo snap install ./kubernetes-worker_*.snap --dangerous
```

Connect all the interfaces.

```bash
for i in docker-privileged k8s-kubelet dot-kube docker-support firewall-control hardware-observe kernel-module-control mount-observe network-control process-control system-observe; do sudo snap connect kubernetes-worker:$i; done
```

## Cloud Providers

We are adding support for different cloud providers.  Supported providers are listed below.

### EKS

For instructions on deploying to EKS, see the [Using with EKS wiki page](https://github.com/charmed-kubernetes/snap-kubernetes-worker/wiki/Using-with-EKS).