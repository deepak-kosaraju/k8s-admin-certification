# [CKA Quick Reference Topics](https://github.com/cncf/curriculum/blob/master/certified_kubernetes_administrator_exam_v1.11.0.pdf)

From exam point use `create` with `--dry-run -o yaml` to create the config and modify necessary objects using vim (or) nano editor, for example:

Create a deployment with image nginx:1.7.8, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

Ref: [dgkanatsios/CKAD-exercises/](https://github.com/dgkanatsios/CKAD-exercises/blob/master/c.pod_design.md#create-a-deployment-with-image-nginx178-called-nginx-having-2-replicas-defining-port-80-as-the-port-that-this-container-exposes-dont-create-a-service-for-this-deployment)
```
kubectl create deployment nginx  --image=nginx:1.7.8  --dry-run -o yaml | sed 's/replicas: 1/replicas: 2/g'  | sed 's/image: nginx:1.7.8/image: nginx:1.7.8\n        ports:\n        - containerPort: 80/g' | kubectl apply -f -
```

## Bash alias and TMUX shortcuts 

- `.bashrc` - Aliases.

```bash
k = 'kubectl'
kgp = 'kubectl get pods'
kgs = 'kubectl get svc'
kgc = 'kubectl get componentstatus'
```

- Autocomplete kubectl

```bash
source <(kubectl completion bash)
```

- tmux - shortcuts

```bash
# start a new session with the name mysession and window mywindow
$ tmux new -s mysession -n mywindow
```

|           Keys           |                     Description                      |
| :----------------------: | :--------------------------------------------------: |
|       `Ctrl + b c`       |                  Create new window                   |
|       `Ctrl + b ,`       |                Rename current window                 |
|       `Ctrl + b &`       |                 Close Current Window                 |
|       `Ctrl + b p`       |                   Previous Window                    |
|       `Ctrl + b n`       |                     Next Window                      |
|     `Ctrl + b 0..9`      |            Switch/Select Window by number            |
| `:swap-window -s 2 -t 1` | Reorder window, swap window number 2(src) and 1(dst) |
|   `:swap-window -t -1`   |   Move current window to the left by one position    |

## Application Lifecycle Management 8%

- Understand deployments and how to perform updates and rollback
- various ways to configure applications
- How does information get from CLI (or) yaml in to the container and ultimately to application
- manually scale-up/scale-down a deployment
- What does self-healing even mean in realtion to K8s and how does k8s enable self-healing.
- Understand the primitives necessary to create a self-healing application

## Installation, Configuration & Validation 12%

- Things to run K8s Cluster
- Technology to make sure cluster communications are secure
- Components of K8s to have HA setup - `etcd, api`
- Where to look and grab K8s Binaries - `kubernetes.io`

## Core Concepts 19%

- K8s API primitives
- Diff between POD, Deployment and a Service
- Diff between POD and a Node
- What is NetworkPolicy?
- Which Piece accepts request on behalf of the cluster
- Which piece directs traffic around the cluster
- which piece orchestrates container on worker nodes
- which piece provide DNS service (or) name resolutions for PODS.
- K8s Cluster Architecture
- Services and other network primitives

## [Networking (11%)](https://kubernetes.io/docs/concepts/cluster-administration/networking/)

- K8s networking model and how its implemented
- How many IP's does a POD get and does the answer change based on how many containers are in the POD? - `One IP address per POD`
- How to create a service and its types
- What are Ingress Rules
- What's required for Ingress Rules to be enforced - `Ingress Controller`
- How is name resolution is handled inside the POD - `using cluster DNS`
- What is CNI and its principles
- `kind:` [Service](https://kubernetes.io/docs/concepts/services-networking/ingress/#types-of-ingress) `type:` [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) and [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/#types-of-ingress) rules on how route/divert traffic from outside world in to the cluster
- ### [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## Scheduling 5%

- How do you label and Pod (or) Node
- How do you schedule a specific POD to a specfic node
- DaemonSet
- How resource limits can effect POD scheduling
- Run multiple scheduler and how to configure pod to use specific scheduler
- Can a POD be scheduled without a scheduler, if so How?
- Look at events logs of a scheduler
- how to configure default K8s Scheduler and where does it run

## Security 12%

- Authentication & Authorization
- What is default security state of POD networking and how it can be changed
- Know how to NetworkPolicies and applying them to PODS
- Create and manage TLS certificates for cluster components
- Cluster Image Sources
- What is Security Context and how can we define one.
- Secureing persistent key/value store
- RBAC

## Cluster Maintenance 11%

- Detail understanding of K8s Upgrade Process
- Evict all PODS from a node to prepare for both Operatings System(S/W) (or) H/W Maintenance
- Put node back in to service
- Backup and restore methodologies

## Logging / Monitoring 5%

- monitoring cluster components
- monitor applications
- capture the output of STDOUT from a particular POD
- logs written to individual containers how to you
- cluster-level-logging strategies

## Storage 7%

- create and allocate persistent volumes
- What is the storageClass and why are they important
- What are 3 mains access modes for persistent volumes
- Diff between persistent volumen claim and persistent volume and their lifecycle and how are they linked
- configure application with persistent storage in a  POD

## Troubleshooting 10%

## [Configuration Tips](https://kubernetes.io/docs/concepts/configuration/overview/#general-configuration-tips)

- components to check if application in a deployment wasn't working correctly
- what would you check and do if API wasn't responding
- What if node isn't behaving as it should what are some ways to check it out
- Troubleshoot networking issues