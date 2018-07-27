Kubernetes Usefull Notes
==========

Simple Application
----------

Create, via Deployment, a replica set of webserver exposed via service and ingress

    kubectl create -f webserver.yaml

or

    kubectl create deployments --image=nginx:alpine webserver
    kubectl scale deployments --replicas=3 webserver

Fragment of webserver.yaml

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

