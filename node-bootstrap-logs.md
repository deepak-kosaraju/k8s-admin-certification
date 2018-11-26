# Bootstrap Event Logs

## Adding node node to the cluster

```bash
sudo kubeadm join <master-api-server-ip>:6443 --token ahasi5.aytigy5460jxp8ea --discovery-token-ca-cert-hash sha256:ebb7d9bc5b874d26b5b5ad5a1ab15f9fb25b9ba79c96c0fb4685733b6959c1fa
[preflight] running pre-flight checks
  [WARNING RequiredIPVSKernelModulesAvailable]: the IPVS proxier will not be used, because the following required kernel modules are not loaded: [ip_vs_sh ip_vs ip_vs_rr ip_vs_wrr] or no builtin kernel ipvs support: map[ip_vs:{} ip_vs_rr:{} ip_vs_wrr:{} ip_vs_sh:{} nf_conntrack_ipv4:{}]
you can solve this problem with following methods:
 1. Run 'modprobe -- ' to load missing kernel modules;
2. Provide the missing builtin kernel ipvs support

[discovery] Trying to connect to API Server "<master-api-server-ip>:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://<master-api-server-ip>:6443"
[discovery] Requesting info from "https://<master-api-server-ip>:6443" again to validate TLS against the pinned public key
[discovery] Cluster info signature and contents are valid and TLS certificate validates against pinned roots, will use API Server "<master-api-server-ip>:6443"
[discovery] Successfully established connection with API Server "<master-api-server-ip>:6443"
[kubelet] Downloading configuration for the kubelet from the "kubelet-config-1.12" ConfigMap in the kube-system namespace
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[preflight] Activating the kubelet service
[tlsbootstrap] Waiting for the kubelet to perform the TLS Bootstrap...
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "deepakk4c.mylabserver.com" as an annotation

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the master to see this node join the cluster.
```