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
