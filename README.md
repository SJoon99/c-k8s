# c-k8s

c cluster preset repository for the ScaleX Repo POC.

This repo is a mobilex-style cluster values repo. It does not carry the full app
catalog; it is consumed with the SmartX-style base repo:

```text
eecs-k8s   # base/framework/app catalog
c-k8s     # concrete child-cluster values and app patches
```

## Active feature set

Cilium/CoreDNS has been verified, so this child cluster now enables Ceph-backed
CSI as the next storage bootstrap layer:

```yaml
features:
  - org.ulagbulag.io/cni
  - org.ulagbulag.io/cni/lb-ipam
  - org.ulagbulag.io/csi
  - org.ulagbulag.io/distributed-storage-cluster/ceph
  - org.ulagbulag.io/distributed-storage-cluster/ceph/provisioning
  - org.ulagbulag.io/omniverse/nucleus
  - org.ulagbulag.io/workload-namespace
```

The POC nodes are labeled `ControlPlane`/`Compute`, so
`patches/rook-ceph-cluster/values.yaml` allows OSD placement on those roles.

For this POC, `rookCeph.useReplicas: false` is set because the available data disks are not present on three independent OSD failure domains yet.

The Cilium patch declares the `10.33.143.0/24` LoadBalancer pool. The Ceph
provisioning patch keeps the current one-replica post-config explicit.

`patches/workload-namespace/values.yaml` declares
`Namespace/scalex-rgw-analysis-web` as C Infra state. Federation uses that
pre-existing namespace for C workloads and does not let Karmada's default
namespace controller own its lifecycle.

## Child API endpoint

```text
https://10.33.201.181:6443
```
