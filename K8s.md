# Kubernetes Usefull Notes

## Simple Application

### Creation

Create, via Deployment, a replica set of webserver exposed via service and ingress

    kubectl create -f webserver.yaml

or

    kubectl create deployments --image=nginx:alpine webserver
    kubectl scale deployments --replicas=3 webserver

Exemple of webserver.yaml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: webserver
    labels:
        app: nginx
    spec:
    replicas: 3
    selector:
        matchLabels:
        app: nginx
    template:
        metadata:
        labels:
            app: nginx
        spec:
        containers:
        - name: nginx
            image: nginx:alpine
            ports:
            - containerPort: 80

So now we have 3 nginx running with the 80 port opened at container level only.

>**Note:** The deployments automatically create a replicaset that manage to mantain the desired replica number

### Exposing via Service

For expose the application we have to create e service (svc) that expose the POD (container) port to a (Casually extracted) Static NodePort

    kubectl create -f webserver-svc.yaml

or

    kubectl create service nodeport --tcp=80 webserver

Example of webserver-svc.yaml

    apiVersion: v1
    kind: Service
    metadata:
    name: web-service
    labels:
        run: web-service
    spec:
    type: NodePort
    ports:
    - port: 80
        protocol: TCP
    selector:
        app: nginx

### Create a Secret

Create a secret to pull image from a private docker registry

    kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>

## CheatSheet

### kubectl

```kubectl
kubectl drain <node-name> --ignore-daemonsets --grace-period 0 --force --delete-emptydir-data --ignore-errors

kubectl get pods --all-namespaces -o wide --field-selector spec.nodeName=<node-name>

kubectl config set-context --current --namespace=<namespace>
kubectl config use-context <context-name>
```

### eksctl

```
eksctl upgrade cluster --name <cluster-name>
eksctl upgrade cluster --name <cluster-name> --approve
```
Edit <cluster-name>.yaml:

* update version
* update nodeGroup name

```yaml
# ...
metadata:
  version: '1.20'
# ...
nodeGroups:
  - name: nodegroupName-v20
# ...
```

Create new nodegroup, cordon and drain the old, delete the old

```
eksctl create nodegroup --include=nodegroupName-v20 -f <cluster-name>.yaml
kubectl drain <node-name> --ignore-daemonsets --grace-period 0 --force --delete-emptydir-data --ignore-errors
```

```eksctl
eksctl scale nodegroup --name <nodegroup-name> --nodes-min 1 --nodes-max 3 --nodes 2 --cluster <cluster-name>
eksctl create nodegroup --include=<nodegroup-name> -f kubeflow-ai.yaml
```