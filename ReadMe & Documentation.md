ReadMe & Documentation

Nginx Web Server Deployment on K3s Cluster & Kubernetes Dashboard Setup
This document outlines the steps taken to successfully deploy an Nginx web server on a K3s (Lightweight Kubernetes) cluster running on Raspberry Pis, synchronize code using Git, and set up the Kubernetes Dashboard for basic monitoring.

Nginx Application Deployment
This section details the deployment of a simple Nginx web server using Kubernetes manifests. The deployment is configured to run on worker nodes within the K3s cluster.

1.1. Kubernetes Manifests
Two primary YAML files define the Nginx application:

nginx-deployment.yaml: Defines the Nginx Deployment, specifying 2 replicas and a nodeAffinity rule to prefer worker nodes.

nginx-service.yaml: Defines a NodePort Service to expose the Nginx Deployment, making it accessible from outside the cluster via a high-numbered port on any node.

1.2. Deployment Steps (on Raspberry Pi Master Node)
Navigate to your my-k3s-apps repository: cd /path/to/my-k3s-apps

Apply the Kubernetes manifests: kubectl apply -f nginx-app/

1.3. Verification
Check Deployment Status: kubectl get deployment nginx-deployment

Output: 

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           <age>

2. Check Pods and Node Assignment: kubectl get pods -o wide

Output: 

NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE         NOMINATED NODE   READINESS GATES
nginx-deployment-<hash>-<hash>      1/1     Running   0          <age> <pod-ip-1>   worker-node  <none>           <none>
nginx-deployment-<hash>-<hash>      1/1     Running   0          <age> <pod-ip-2>   worker-node  <none>           <none>

3. Check Service Status and NodePort: kubectl get service nginx-service

Expected Output: 

NAME            TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   <cluster-ip>  <none>        80:3XXXX/TCP   <age>

Welcome to Nginx (jpg.image)

Kubernetes Dashboard Access
The Kubernetes Dashboard provides a web-based UI for managing and monitoring the K3s cluster.

Deploy the Kubernetes Dashboard: kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

Deploy Metrics Server:
The Dashboard relies on metrics-server for CPU/Memory graphs.

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

Create Service Account and Role Binding for Dashboard Access:
Create dashboard-adminuser.yaml:

Apply Manifest: kubectl apply -f dashboard-adminuser.yaml

Get the Authentication Token: kubectl -n kubernetes-dashboard create token admin-user

Accessed Dashboard by utilizing kubectl proxy 

(Dashboard.jpg)

Current Nginx Health Status
Both nginx-deployment pods are Running and Ready.

Memory usage is stable at approximately 10 MiB per pod.

No immediate abnormal events or high resource consumption observed.


Conclusion & Next Steps
We have successfully deployed an Nginx web server on K3s, synchronized the codebase via Git, and established remote management and basic monitoring using the Kubernetes Dashboard.

The next steps will involve actively practicing SRE principles by:

Simulating failures to observe how the cluster and Dashboard react.

Responding to simulated incidents to understand recovery processes.

Exploring more advanced monitoring and alerting solutions as the cluster grows.