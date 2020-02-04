# Setup sample Guestbook Application on the EKS Cluster

- Based on https://github.com/kubernetes/examples/tree/master/guestbook

## Redis Master

deploy the master Redis pod and a _service_ on top of it:

```bash
kubectl apply -f redis-master.yaml
kubectl get pods
kubectl get services
```

## Redis Slaves

deploy the Redis slave pods and a _service_ on top of it:

```bash
kubectl apply -f redis-slaves.yaml
kubectl get pods
kubectl get services
```

## Frontend Application

deploy the PHP Frontend pods and a _service_ of type **NodePort** on top of it, to expose the service and later on use the ALB Controller to route traffic to the guestbook:

```bash
kubectl apply -f frontend.yaml
kubectl apply -f frontend-service.yaml
```

Some checks:

```bash
kubectl get pods
kubectl get pods -l app=guestbook
kubectl get pods -l app=guestbook -l tier=frontend
```

## Access from outside the cluster

With the ALB Controller up and running:

```bash
kubectl get pods -n kube-system
```

We can now route the ingress traffic to the guestbook:

```bash
kubectl apply -f ingress.yaml
```

After a while, check the status of the service:

```bash
kubectl get ingress/guestbook-ingress
```

and grab the public URL and check the URL in your browser.

## kubectl commands for scaling pods

scaling a deployment:

```bash
kubectl scale --replicas <number-of-replicas> deployment <name-of-deployment>
```

e.g. set no of replicas for _frontend_ service to _5_ :

```bash
kubectl scale --replicas 5 deployment frontend
```

### Commands to check the state

- get all pods incl additional info like e.g. k8s worker node the pod is running on

```bash
kubectl get pods -o wide
```

- state of service(s)

```bash
kubectl get services
```

- details of a particular service:

```bash
kubectl describe service <servicename>
```
