# Certified Kubernetes Administrator(CKA)

- [Certified Kubernetes Administrator(CKA)](#certified-kubernetes-administratorcka)
  - [Useful Links](#useful-links)
  - [Basic Kubernetes Architecture](#basic-kubernetes-architecture)
    - [Node Type and their Components (19%)](#node-type-and-their-components-19)
  - [API Premitives (or) Cluster Objects](#api-premitives-or-cluster-objects)
    - [Common Cluster Objects](#common-cluster-objects)
    - [Names and UIDs](#names-and-uids)
  - [Services & Network Primitives](#services--network-primitives)
  - [Designing a K8s Cluster](#designing-a-k8s-cluster)
    - [Local Deployment Options](#local-deployment-options)
    - [Cloud/COAS(Container Orchestrator as Service) Turnkey Solutions](#cloudcoascontainer-orchestrator-as-service-turnkey-solutions)
    - [Add-on Solutions](#add-on-solutions)
  - [Hardware and Underlying Infrastructure](#hardware-and-underlying-infrastructure)
  - [Securing Cluster Communications](#securing-cluster-communications)
    - [Securing API](#securing-api)
    - [Securing Kubelet](#securing-kubelet)
    - [Securing Network](#securing-network)
    - [Vulnerabilities](#vulnerabilities)
    - [Thrid Party Integrations](#thrid-party-integrations)
  - [High Availability(HA)](#high-availabilityha)
    - [Build Reliable Cluster](#build-reliable-cluster)
      - [Step One - `Kubelet`](#step-one---kubelet)
      - [Step Two - `Storage`](#step-two---storage)
      - [Step Three - `Replicated API Services`](#step-three---replicated-api-services)
      - [Step Four - Controller/Scheduler Daemons](#step-four---controllerscheduler-daemons)
    - [Installing Configuration Files](#installing-configuration-files)
  - [End-to-End Testing & Validation Nodes & the Cluster](#end-to-end-testing--validation-nodes--the-cluster)
  - [Labels & Selectors](#labels--selectors)
  - [Taints and Tolerations](#taints-and-tolerations)
  - [Manually Scheduling Pods](#manually-scheduling-pods)
  - [Monitoring Cluster and Application Components (5% of the Exam)](#monitoring-cluster-and-application-components-5-of-the-exam)
  - [Cluster Maintenance (11% of the Exam)](#cluster-maintenance-11-of-the-exam)
    - [Upgrading Kubernetes Components using kubeadm](#upgrading-kubernetes-components-using-kubeadm)
    - [Upgrading Worker Nodes](#upgrading-worker-nodes)
    - [Upgrading OS on Nodes](#upgrading-os-on-nodes)
  - [Networking (11%)](#networking-11)
    - [Node Networking Configuration](#node-networking-configuration)
    - [Service Networking](#service-networking)
      - [ServiceTypes](#servicetypes)
    - [Ingress](#ingress)
      - [Types of Ingress](#types-of-ingress)
    - [DNS for Service and Pods](#dns-for-service-and-pods)
    - [Network Policies](#network-policies)
  - [Storage(7%)](#storage7)
    - [Types of Volumens](#types-of-volumens)
    - [PresistentVolume](#presistentvolume)
    - [PresistentVolumeClaim](#presistentvolumeclaim)
  - [Secuirty](#secuirty)
    - [Secrets](#secrets)
  - [Liveness and Readiness Probes](#liveness-and-readiness-probes)
  - [Troubleshooting](#troubleshooting)
  
## Useful Links

[Top Reference](https://goo.gl/Rywkpd)

[Infrastructure for container projects - LXC, LXD & LXCFS](https://linuxcontainers.org/)
 further reading on what are [LXD](https://help.ubuntu.com/lts/serverguide/lxd.html.en) and [LXC](https://help.ubuntu.com/lts/serverguide/lxc.html) container runtime technologies is highly recommended

[Components](https://kubernetes.io/docs/concepts/overview/components/)

[API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)

[Master-Node-Communication](https://kubernetes.io/docs/concepts/architecture/master-node-communication/)

[Pod-Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

[Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)

[CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

[Storage](https://kubernetes.io/docs/concepts/storage/volumes/)

[Resources](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/)

[Running Locally (non Minikube)](https://kubernetes.io/docs/getting-started-guides/alternatives/)

[Monitoring Kubernetes with Prometheus](https://www.weave.works/technologies/monitoring-kubernetes-with-prometheus/)


## [Basic Kubernetes Architecture](https://kubernetes.io/docs/concepts/architecture/cloud-controller/)

![Architecture](https://lms.quickstart.com/custom/858487/images/Kubernetes%20Architecture.png)

### [Node Type and their Components](https://kubernetes.io/docs/concepts/overview/components/) (19%)

- `Master` - cluster’s control plane with HA setup (or) single node instance
  - `kube-apiserver` - answers api call
  - `etcd` - key/value store used by API Server for configuration and other persistent storage needs
  - `kube-scheduler` - Determins which nodes are responsable for `PODS` and their respective containers as they are up in the cluster
  - `Cloud Controler Manager` - split out in to several containers depends on cloud platform we are running - responsible for persistatnt storage, routes for networking
  - `kube-controller-manager` - make all of the above avaialble to cloud through this service (or) we can block this if not needed

- `Nodes` - workers/minions are work force of the cluster
  - `kublet` - takes orders from master to run `PODS`
  - `kube-proxy` - assist with `Container Network Interface(CNI)` with routing traffic around the cluster
  - `POD` - one (or) more containers run as part of `POD` which are considered disposablea and replacable

## API Premitives (or) Cluster Objects

Persistent entities and represent state of the cluster like,

- What applications are running
- Which node those applications are running on
- Policies around those applications

K8s Objects are `records of intent`

[Object Spec](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

- User provides desired state of an object
  
[Object Status](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

- Provided by K8s to describe the actual state of the object which is managed by control plane of K8s

### [Common Cluster Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

- [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) - which makes the cluster
- [PODS](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/) - single instance of application containers
- [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) - Controller for `PODS` and it ensures that resources are available, such as IP address, and storage and then deploys a `ReplicaSet`
- [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) - controller which deploys and restart `PODS` until requested number of `PODS`are running.
- [Services](https://kubernetes.io/docs/concepts/services-networking/service/) - Exposes deployments to external networks by means of 3rd party LoadBalancer (or) ingress rules
- [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) - KV pair that can dynamically plugged in to other objects as needed which allows to decouple configuration from individual `PODS` (or) `Deployments` which gives lot of flexibility

### [Names and UIDs](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/)

Names

- lower case alphanumeric with max len 253 characters and can be resued and provided by client but must be uniq. `BTW:` - and . are allowed.

UIDs

- generated by K8s

Namespaces

- multiple virtual clusters back by same physical cluster
- best for large deployments to isolate cluster resources and to define RBAC and quotas
- `kube-system` special namespace used to differenciate systems `PODS` from user `PODS`

[Cloud Controller Manager](https://kubernetes.io/docs/concepts/architecture/cloud-controller/)

- `Route Controller(GCE clusters only)`
- `Service Controller` - responsbile for listening to service create/update/delete events and update cloud load balancers like AWS-ELB, Google-LB,Azure-LB to reflect the state of services in K8s
- `Persistent Volume Labels Controller` - applies labels AWS-EBS,GCE-PD(persistent disk) volumes when those get created which allows to manually define by user. These lables are centeral to scheduling of `PODS` as those labels are constrain to work only in the region/zone they are in.

[Node Controller](https://kubernetes.io/docs/concepts/architecture/nodes/#node-controller)

- Assigns CIDR block to new nodes(if CIDR assignment is turned on).
- Keeps track of the nodes using health of the node and changes the status of `Not Ready` to `UNKNOWN`when node becomes unreahable with-in the heartbeat intervals(`40 sec timeout`). If node state is flagged as UNKNOWN and `5 min` after that it evicts `PODS`(rate of `0.1/sec`\[effected by cluster_size(default 50) configurable by `large_cluster_size_threshold`\] means it will not effect `PODS` from more than `1 node every 10 sec`) from unhealthy nodes. `BTW:` evictions are stopped if nodes in all availablity zones are flagged as `UNKNOWN` because there could be scenario where `master node` could be having problem connecting to `worker nodes`
- Checks the status of nodes every few sec which is configurable by setting `node_monitor_period`

## [Services & Network Primitives](https://kubernetes.io/docs/concepts/services-networking/service/)

- Servics refere to deployments and exposes particualr port via [kube-proxy](https://kubernetes.io/docs/concepts/overview/components/#kube-proxy) (or) using special IP Address for entire `POD` and prefernce of this setup strictly depends on [network policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/), security policies, [ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) rules to handle load balancing and port forwarding with-in each cluster deployment.

## Designing a K8s Cluster

### Local Deployment Options

- [Minikube](https://kubernetes.io/docs/setup/minikube/) - single node K8s on local workstation
- [Kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) - multi-nodes cluster on local workstation
- `Ubuntu on LXD` - provides nine-instance suppoted cluster on local workstation

### Cloud/COAS(Container Orchestrator as Service) Turnkey Solutions

- [conjure-up](https://docs.conjure-up.io/stable/en/) - deploy the Canonical Distribution of Kubernetes with Ubuntu on AWS, Azure, Google and Oracle Cloud
- AWS EKS, Azure AKS, Google GKE, Tectonic by CoreOS, Stackpoint.io, kube2go.io as well as madcore.ai

### Add-on Solutions

- Container Network Interface(CNI)
  - Calico - secured L3 networking and policy provider
  - Canal unites Flannel & Calico - provided networking and policies
  - Cilium - L3 network and policy plugin that can enforce HTTP/API/L7 policies transparently
  - CNI-Genie - enables K8s to seamlessly connect to CNI of any choice such as Calico, Canal, Flannel, Romana (or) Weave
  - NSX-T(NCP) provides integration between VMWare NSX-T and container orchestrator such as K8s as well as container-based platforms(CaaS,PaaS) like PCF and Openshift.
  - Nuage is a SDN platform that provides policy-based networking between K8s `PODS` and non-K8s environments with visibility and security monitoring
  - Weave Net provides networking and policy and doesn't require an external database.

## Hardware and Underlying Infrastructure

- Nodes including master can be physical/virtual running K8s components and a container runtime like Docker, rocket(rkt)
- nodes should be connected by a common network where `Flannel` is used as basic `PODS` networking application.

## Securing Cluster Communications

### Securing API

- Communication to API Server/Service, control-plane communications inside the cluster and even `POD-to-POD` communications.
- Default encryption communication is `TLS`
- Ensure any unsecure taffic/ports are `DISABLED`
- Anything that connects to API, including nodes, proxies, the scheduler, volume plugins and users should be `Authenticated`
- Certs for all internal communication channels are automatically creaded unless you chose to deploy [K8s cluster the hardway](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- Once Authenticated `Authorization checks` should be passed.
- `RBAC Control Component` used to match users/groups to permission sets that are organized in to roles

### Securing Kubelet

- By default exposes HTTPS endpoints which gives access to both data and actions on the nodes, to enable Kubelet to use RBAC policies by starting it with a flag `--anonymous-auth=false` and assign an appropriate x509 client certificate in its configuration.

### Securing Network

- Network policies restrict access to network for a particualr namespace
- Users can be assigned quotas or limit ranges to prevent over usage of node ports (or) Load Balance services

### Vulnerabilities

- etcd is default key/value to store for entire cluster to store configuration and secrets of the cluster so isolate its access behind FW so only it is accessed through API which should be well enforced using RBAC
- Audit logging and architect to store the audit file on to a secure server
- Rotate Infra Credentails frequently by setting short liftime and use any automated rotation methods where applicable

### Thrid Party Integrations

- Don't allow them in to kube-system namespace
- Always review integration before enabling

Join [K8s announcement group](https://groups.google.com/forum/#!forum/kubernetes-announce) for emails about security announcements.

## [High Availability(HA)](https://kubernetes.io/docs/setup/independent/high-availability/)

### Build Reliable Cluster

- Create reliable nodes that forms the cluster
- Setup redundant and reliable storage service with multinode deployment of [etcd](https://kubernetes.io/docs/setup/independent/setup-ha-etcd-with-kubeadm/)
- Start replicated and load balanced API Server/Service
- Setup a master-elected scheduler and controller manager daemons.

#### Step One - `Kubelet`

- Ensure cluster services automatically restart if they fail and `Kubelet`already does this, but what if `Kubelet` goes `DOWN`, so for that we need something like [monit](https://mmonit.com/monit/#slideshow) to watch the service/process.

#### Step Two - `Storage`

- Clustered `etcd` already replicates the storage to all master instances in the cluster
- Add additional reliability by increasing the cluster from 3 to 5 nodes and even more where applicable
- If using cloud provider then use any Block Device Persistent Storage offerings of that cloud provider to map/mount then on to your virtual machines in the cloud.
- If running on Baremetal - use options like NFS, Clustered Filesystem like Gluster (or) Ceph, Hardware level RAID

#### Step Three - `Replicated API Services`

- Refere to image in `images/k8s-ha-step3-1.png` and `images/k8s-ha-step3-2.png` files for detail steps

#### Step Four - Controller/Scheduler Daemons

- Allow our state to change, we use lease-lock on API to perform master election. So each scheduler and controller manager will be started using `--leader-elect` flag to ensure only 1 instance of scheduler and controller manager(CM) are running and making changes to cluster
- Scheduler and CM can be configured to talk to the API Server/Service that is on the same node (or) through Loadbalancd IP(preferred).

### Installing Configuration Files

- Create empty log files on each node, so that Docker will mount the files and not make new directhries,

  ```bash
  touch /var/log/kube-scheduler.log
  touch /var/log/kube-controller-manager.log
  ```

- Setup the description of the scheduler and CM `PODS`on each node by copying the `kube-scheduler.yaml` and `kube-controller-manager.yaml` in to the `/etc/kubernetes/manifests/` directory.

## End-to-End Testing & Validation Nodes & the Cluster

- `Kubetest Suite` - ideal of GCE and AWS users to,
  - Build
  - Stage
  - Extract
  - Bring up the Cluster
  - Test
  - Dump logs
  - Tear Down the cluster.

- manual validation
  - `kubectl get nodes`
  - `kubectl desribe node <node name>`
  - `kubectl get pods --all-namespaces -o wide`
  - `kubectl get pods -n kube-system -o wide`
  - `ps aux | grep [k]ube` - validate with kube processes are running.

## [Labels & Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

- Labels are key/value pairs that are attached to objects, such as pods
- Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users
- One use case of labels is to [constrain the set of nodes onto which a pod can schedule](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)
- The API currently supports two types of selectors: [equality-based](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#equality-based-requirement) and [set-based](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#set-based-requirement)
  - **equality-based**

  ```bash
  kubectl get pods -l environment=production,tier=frontend
  ```

  - **set-based**
    - Newer resources, such as Job, Deployment, Replica Set, and Daemon Set support set-based requirements as well.

  ```bash
  kubectl get pods -l 'environment in (production),tier in (frontend)'
  ```

## [Taints and Tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)

- Node affinity, described here, is a property of pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite – they allow a node to repel a set of pods.
- Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints. Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.
  
- [Example Use Caes](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#example-use-cases)
- [Taint based Evictions](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#taint-based-evictions)

- [Taint Nodes by Condition](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#taint-nodes-by-condition) beta in v1.12

## [Manually Scheduling Pods](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)

## Monitoring Cluster and Application Components (5% of the Exam)

- cAdvisor exposes simple UI for local containers on port 4194(default)
- [metric-server](https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/) (< K8s v1.8)
- [Logging](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
  - Good discussion on K8s issue in github  - [Kubernetes logging, journalD, fluentD, and Splunk, oh my!](https://github.com/kubernetes/kubernetes/issues/24677)
  - [Logging at the node level](https://kubernetes.io/docs/concepts/cluster-administration/logging/#logging-at-the-node-level)
    - Clustet setup using`kube-up.sh`configures [logrotate](https://linux.die.net/man/8/logrotate) tool to run each hour to rotate containers logs when log file exceeds `10MB (default`)
    - Container engine/runtime can also rotate logs for example: [Docker Logging Drivers](https://docs.docker.com/config/containers/logging/configure/)
  - [System Component Logs](https://kubernetes.io/docs/concepts/cluster-administration/logging/#system-component-logs)
    - Two types of system components: those that run in a container and those that do not run in a container. For example:
      - The Kubernetes scheduler and kube-proxy run in a container.
      - The kubelet and container runtime, for example Docker, do not run in containers.
    - On machines with `systemd`, the `kubelet` and `container runtime` write to `journald`.
    - If `systemd` is not present, they write to `.log` files in the `/var/log` directory.
    - System components inside containers always write to the `/var/log` directory,bypassing the default logging mechanism. They use the [glog](https://godoc.org/github.com/golang/glog) logging library.
      - Conventions for logging severity for these components can be found in the [development docs on logging](https://git.k8s.io/community/contributors/devel/logging.md)
    - Logrotations happens either daily or once the size exceeds `100MB (default)`.
  
  - [Cluster-Level-Logging](https://kubernetes.io/docs/concepts/cluster-administration/logging/#cluster-level-logging-architectures)
    - logs should have a separate storage and lifecycle independent of nodes, pods, or containers
    - Here are some options:
      - Use a `node-level logging agent` that runs on every node.
      - Include a `dedicated streaming sidecar container` (or) `dedicated sidecar container with logging agent`for logging in an application pod.
      - `Push logs directly` to a backend from within an application.

      [Node-level Logging Agent](https://kubernetes.io/docs/concepts/cluster-administration/logging/#using-a-node-logging-agent)

      ![node-logging-agent](https://d33wubrfki0l68.cloudfront.net/2585cf9757d316b9030cf36d6a4e6b8ea7eedf5a/1509f/images/docs/user-guide/logging/logging-with-node-agent.png)

      - Because the logging agent must run on every node, it's implemented as`DaemonSet replica`
      - Best suties only for applications emitting logs to `stdout` and `stderr`

      [Streaming sidecar container](https://kubernetes.io/docs/concepts/cluster-administration/logging/#using-a-sidecar-container-with-the-logging-agent)

      ![streaming-sidecar-container](https://d33wubrfki0l68.cloudfront.net/c51467e219320fdd46ab1acb40867b79a58d37af/b5414/images/docs/user-guide/logging/logging-with-streaming-sidecar.png)

      - The individual sidecar container streams application logs to its own `stdout` and `stderr`
      - The sidecar container runs a logging agent, which is configured to pick up logs from an application container.
      - The sidecar containers read logs from a file, a socket, or the journald.
      - writing logs to a file and then streaming them to stdout can double disk usage
      - If you have an application that writes to a single file, it’s generally better to set `/dev/stdout` as destination rather than implementing the streaming `sidecar container` approach.

      [Sidecar Container with Logging Agent](https://kubernetes.io/docs/concepts/cluster-administration/logging/#using-a-sidecar-container-with-the-logging-agent)

      ![sidecar-container-with-logging-agent](https://d33wubrfki0l68.cloudfront.net/d55c404912a21223392e7d1a5a1741bda283f3df/c0397/images/docs/user-guide/logging/logging-with-sidecar-agent.png)

      - [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) are used to configure the configuration for logging-agents like - [Fluend](https://kubernetes.io/docs/tasks/debug-application-cluster/logging-elasticsearch-kibana/), [Splunk](http://jasonpoon.ca/kubernetes-logging-with-splunk/) (or) [Splunkforwarder-with-DeploymentServer](https://medium.com/@swarupdonepudi/setup-splunk-on-kubernetes-94716dab0e9)

## Cluster Maintenance (11% of the Exam)

### Upgrading Kubernetes Components using [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/)

- We need to have a `kubeadm` Kubernetes cluster
- Swap must be disabled
- The cluster should use a static control plane and etcd pods.
- Make sure you read the release notes carefully.
- Make sure to back up any important components, such as app-level state stored in a database.
- Upgrade `kubeadm` pkg using the pkg manager specific to OS - `yum` (or) `apt`
- `kubeadm upgrade does not touch your workloads`, only components internal to Kubernetes, but `backups are always a best practice.
- `All containers are restarted after upgrade,` because the container spec hash value is changed.
- `You can upgrade only from one minor version to the next minor version.` That is, you cannot skip versions when you upgrade. For example, you can upgrade only from 1.10 to 1.11, not from 1.9 to 1.11.

### Upgrading Worker Nodes

- [Drain](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#drain) node in prepartion for maintenace by using `kubectl drain <node-name> --ignore-daemonsets`. The given node will be marked unschedulable to prevent new pods from arriving. `drain` evicts the pods if the APIServer supports [eviction](http://kubernetes.io/docs/admin/disruptions/). Otherwise, it will use normal DELETE to delete the pods.
- If there are `DaemonSet-managed pods`, drain will not proceed without `--ignore-daemonsets`, and regardless it will not delete any DaemonSet-managed pods, because those pods would be immediately replaced by the DaemonSet controller, which ignores unschedulable markings
- If there are any `PODS` that are neither mirror pods nor managed by ReplicationController, ReplicaSet, DaemonSet, StatefulSet or Job, then drain will not delete any pods unless you use `--force`.
- `drain` waits for graceful termination. You should not operate on the machine until the command completes.
- Upgrade `kubelet` pkg using the pkg manager specific to OS - `yum` (or) `apt`
- Verify using `systemctl status kubelet` to ensure its still active upgrade the upgrade
- Once Upgraded successfully finally [uncordon](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#uncordon) the node so its make as schedulable

### Upgrading OS on Nodes

- `drain` the node
- Ensure all pods are evicted and node status is flagged as `NotReady, SchedulingDisabled`
- As we are not going to bring same machine in to cluster we will delete the node from the know list of `kubeadm` using `kubectl delete node <node name>`
- To add new upgraded server as node to cluster, follow these steps to get cluster join command with valid [token](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-token/)
  - get available token list `kubeadm token list`
  - if no tokens founds (or) expired generate a new token using `kubeadm token generate`
  - create token to request the join command
  
    ```bash
    kubeadm token create <token-from-generate-cmd> --ttl <h> --print-join-command
    kubeadm join <master-ip>:6443 --token <token-provided> --discovery-token-ca-cert-hash sha256:<random-hash>
    ```
  
  (or)

  - Just use `kubeadm token create --print-join-command` to generate new token and print-join-command using one single command

## [Networking (11%)](https://kubernetes.io/docs/concepts/cluster-administration/networking/)

### Node Networking Configuration

- `Master Node`
  
| Ports(over TCP) | Service                 |
| --------------- | ----------------------- |
| 6443            | API Server              |
| 2379, 2380      | etcd server client API  |
| 10250           | Kublet API              |
| 10251           | kube-scheduler          |
| 10252           | kube-controller-manager |
| 10255           | read-only Kublet API    |

- `Worker Node`

| Ports(over TCP) | Service              |
| --------------- | -------------------- |
| 10250           | Kublet API           |
| 10255           | read-only Kublet API |
| 30000 - 32767   | NodePort Services    |

### [Service Networking](https://kubernetes.io/docs/concepts/services-networking/service/)

#### ServiceTypes

- Kubernetes ServiceTypes allow you to specify what kind of service you want [ The default is `ClusterIP`]
  
  Type values and their behaviors are:

  - `ClusterIP`: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster. This is the default ServiceType.
  - `NodePort`: Exposes the service on each Node’s IP at a static port (the NodePort). A ClusterIP service, to which the NodePort service will route, is automatically created. You’ll be able to contact the NodePort service, from outside the cluster, by requesting <NodeIP\>:<NodePort\>.
  - `LoadBalancer`: Exposes the service externally using a cloud provider’s load balancer. NodePort and ClusterIP services, to which the external load balancer will route, are automatically created.
  - `ExternalName`: Maps the service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up. This requires version 1.7 or higher of kube-dns.

- `LoadBalancer` - On cloud providers which support external load balancers, setting the type field to LoadBalancer will provision a load balancer for your Service. The actual creation of the load balancer happens asynchronously, and information about the provisioned balancer will be published in the Service’s .status.loadBalancer field. For example:

  ```yaml
  kind: Service
  apiVersion: v1
  metadata:
    name: my-service
  spec:
    selector:
      app: MyApp
    ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
    clusterIP: 10.0.171.239
    loadBalancerIP: 78.11.24.19
    type: LoadBalancer
  status: ## This information is published 
    loadBalancer:
      ingress:
      - ip: 146.148.47.155
  ```

  _Note: **Special notes for Azure:** To use user-specified public type loadBalancerIP, a static type public IP address resource needs to be created first, and it should be in the same resource group of the other automatically created resources of the cluster. For example, `MC_myResourceGroup_myAKSCluster_eastus`. Specify the assigned IP address as loadBalancerIP. Ensure that you have updated the securityGroupName in the cloud provider configuration file. For information about troubleshooting `CreatingLoadBalancerFailed` permission issues see, [Use a static IP address with the Azure Kubernetes Service (AKS) load balancer or CreatingLoadBalancerFailed on AKS cluster with advanced networking](https://docs.microsoft.com/en-us/azure/aks/static-ip)._

- [Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#servicespec-v1-core) for `Service` with Selector

  ```yaml
  kind: Service
  apiVersion: v1
  metadata:
    name: my-service
  spec:
    selector:
      app: MyApp
    ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
  ```

- [Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#servicespec-v1-core) for `Service` without Selector
  - Services generally abstract access to Kubernetes Pods, but they can also abstract other kinds of backends. For example:
    - You want to have an external database cluster in production, but in test you use your own databases.
  
    - You want to point your service to a service in another Namespace or on another cluster.

    - You are migrating your workload to Kubernetes and some of your backends run outside of Kubernetes.
  
  In any of these scenarios you can define a service without a selector:

  ```yaml
  kind: Service
  apiVersion: v1
  metadata:
    name: my-service
  spec:
    ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
  ```

  and because this service has no selector, the corresponding Endpoints object will not be created. You can manually map the service to your own specific endpoints:

  ```yaml
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: my-service
  subsets:
    - addresses:
        - ip: 1.2.3.4
      ports:
        - port: 9376
  ```

- Multi-Port Services

  ```yaml
  kind: Service
  apiVersion: v1
  metadata:
    name: my-service
  spec:
    selector:
      app: MyApp
    ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 9376
    - name: https
      protocol: TCP
      port: 443
      targetPort: 9377
  ```

  _Note: Port names must only contain lowercase alphanumeric characters and -, and must begin & end with an alphanumeric character. 123-abc and web are valid, but 123\_abc and -web are not valid names._

  - Load Balancer

### [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)

- Manages external access to the services in a cluster, typically HTTP.
- Provide load balancing, SSL termination and name-based virtual hosting.
- Collection of rules that allow inbound connections
- Exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the ingress resource.
- An ingress does not expose arbitrary ports or protocols. Exposing services other than `HTTP` and `HTTPS` to the internet typically uses a service of type `Service.Type=NodePort` or `Service.Type=LoadBalancer`.

  ```yaml
  apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    name: test-ingress
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /
  spec:
    rules:
    - http:
        paths:
        - path: /testpath
          backend:
            serviceName: test
            servicePort: 80

  ```

#### [Types of Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/#types-of-ingress)

- Single Service Ingress: 

### [DNS for Service and Pods](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)

### [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## [Storage(7%)](https://kubernetes.io/docs/concepts/storage/volumes/)

- Lifetime of volume is same as that of `POD`enclosing it
- Volumes outlives any containers that run with the `POD`
- To use a volume, a Pod specifies what volumes to provide for the Pod (the `.spec.volumes` field) and where to mount those into Containers (the `.spec.containers.volumeMounts` field).
- Volumes can not mount onto other volumes or have hard links to other volumes

### [Types of Volumens](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes)

- `emptyDir`
  - volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on that node.
  - When a `POD` [**not when container crashes**] is removed from a node for any reason, the data in the emptyDir is deleted forever.
  - Some uses for an emptyDir are:
    - scratch space, such as for a disk-based merge sort
    - checkpointing a long computation for recovery from crashes
    - holding files that a content-manager Container fetches while a webserver Container serves the data
  - `emptyDir.medium` field in `.spec.containers.volumes` can be set to "**Memory**" to tell Kubernetes to mount a `tmpfs` (RAM-backed filesystem) for you instead, but files you write will count against your Container’s memory limit. **Beware**:  unlike disks, tmpfs is cleared on node reboot

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: test-pd
    spec:
      containers:
      - image: k8s.gcr.io/test-webserver
        name: test-container
        volumeMounts:
        - mountPath: /cache
          name: cache-volume
      volumes:
      - name: cache-volume
        emptyDir: {}
    ```
- [azureDisk](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk/README.md)

  ```yaml
  ....
      volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
        - name: azure
          azureDisk:
            diskName: test.vhd
            diskURI: https://someaccount.blob.microsoft.net/vhds/test.vhd
    ...
    ```
- [azureFile](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_file/README.md)

- [downwardAPI](https://kubernetes.io/docs/tasks/inject-data-application/downward-api-volume-expose-pod-information/) -  is used to make `downward API` data available to applications. It mounts a directory and writes the requested data in plain text files.

### PresistentVolume

- Provisioned storage in the cluster
- Cluster resource
- Volume plugins has independent lifecycle from pods
- Volumes share the lifecycle of the pod, Persistent volument doesn't
- API object(YAML) details the implementation

### PresistentVolumeClaim

- Request for Storage part of cluster resource
- Pods consume node resources, PVCs consumes PV resources
- Pods can request specific CPU and memory:PV's can request specific size and access mode.

Lifecycle is PV's are Provisioned and then bound to PVC's and PV's are reclaimed once PVC's are released.

- Provisioning
  - Static
    - Administrator creates PV's in the K8s API and made availble for consumption
  - Dynamic
    - Used when one of the static PV's match the PVC and its strictly based on `StorageClasses`
    - PVC must request a created and configured storage class
    - Claims requesting for nameless classes disable Dynamic provisioning
  
  To enable dynamic storage provisioning, `DefaultStorageClass` addmission controller on the API server must be enabled.

## Secuirty

### [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

## [Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)

- The kubelet uses liveness probes to know when to restart a Container. For example, liveness probes could catch a deadlock, where an application is running, but unable to make progress. Restarting a Container in such a state can help to make the application more available despite bugs.

- The kubelet uses readiness probes to know when a Container is ready to start accepting traffic. A Pod is considered ready when all of its Containers are ready. One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.


## Troubleshooting
  
- Ensure `kubelet` process is up and running on nodes
- Ensure `CNI plugin, kube-proxy` pods are running across the cluster nodes
- Ensure if any `taints`and un-ignored `Tolerations`are cause of missing core service `PODS` on nodes, if so fix by updating necessary objects to unblock it
- Use `kubectl describe node/pod <node-name/pod-name>` to look at the events to understand more about the status
- Look at log files under `/var/log/containers`for core service `PODS`.
