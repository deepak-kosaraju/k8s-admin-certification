# [CKA Quick Reference Topics](https://github.com/cncf/curriculum/blob/master/certified_kubernetes_administrator_exam_v1.11.0.pdf)

From exam point use `create` with `--dry-run -o yaml` to create the config and modify necessary objects using vim (or) nano editor, for example:

Create a deployment with image nginx:1.7.8, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

Ref: [dgkanatsios/CKAD-exercises/](https://github.com/dgkanatsios/CKAD-exercises/blob/master/c.pod_design.md#create-a-deployment-with-image-nginx178-called-nginx-having-2-replicas-defining-port-80-as-the-port-that-this-container-exposes-dont-create-a-service-for-this-deployment)
```
kubectl create deployment nginx  --image=nginx:1.7.8  --dry-run -o yaml | sed 's/replicas: 1/replicas: 2/g'  | sed 's/image: nginx:1.7.8/image: nginx:1.7.8\n        ports:\n        - containerPort: 80/g' | kubectl apply -f -
```

## Application Lifecycle Management 8%

## Installation, Configuration & Validation 12%

## Core Concepts 19%

## [Networking (11%)](https://kubernetes.io/docs/concepts/cluster-administration/networking/)

- `kind:` [Service](https://kubernetes.io/docs/concepts/services-networking/ingress/#types-of-ingress) `type:` [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) and [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/#types-of-ingress) rules on how route/divert traffic from outside world in to the cluster
- ### [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## Scheduling 5%
- How do you label and Pod (or) Node
- How do you schedule a pod on a specific node

## Security 12%

## Cluster Maintenance 11%

## Logging / Monitoring 5%

## Storage 7%

## Troubleshooting 10%

## [Configuration Tips](https://kubernetes.io/docs/concepts/configuration/overview/#general-configuration-tips)
