# Kubernetes Nginx Node Affinity Example

This repository contains Kubernetes configuration files for deploying Nginx pods with specific node affinity and anti-affinity rules across a multi-node Kind cluster.

- `kind-config.yaml`: Configuration file for creating a multi-node Kubernetes cluster with Kind.
- `nginx-deployment-f.yaml`: Nginx deployment with affinity for `node-a`.
- `nginx-deployment-j.yaml`: Nginx deployment with anti-affinity for `node-c`.
- `nginx-deployment-t.yaml`: Nginx deployment with affinity for `node-b` and `node-c`.

Setup Instructions

1. Create the Kind Cluster

To create the Kubernetes cluster using Kind, run:
```bash
kind create cluster --config=kind-config.yaml
```
2. Apply Node Labels
Label the worker nodes to ensure proper scheduling of the Nginx pods:
```bash
  kubectl label node kind-worker kubernetes.io/hostname=node-a --overwrite
  kubectl label node kind-worker2 kubernetes.io/hostname=node-b --overwrite
  kubectl label node kind-worker3 kubernetes.io/hostname=node-c --overwrite
```
4. Deploy Nginx Pods
Apply the Kubernetes deployment files to schedule the Nginx pods:
```bash
  kubectl apply -f nginx-deployment-f.yaml
  kubectl apply -f nginx-deployment-j.yaml
  kubectl apply -f nginx-deployment-t.yaml
```
6. Verify Pod Placement
Check the status and node placement of the deployed pods:
```bash
  kubectl get pods -o wide
```
You should see:
```bash

nginx-deployment-f: All 3 pods running on node-a.
nginx-deployment-j: The 3 pods distributed between node-a and node-b, with none on node-c.
nginx-deployment-t: The 3 pods distributed between node-b and node-c.
```
