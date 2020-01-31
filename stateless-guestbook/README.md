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

deploy the PHP Frontend pods and a _service_ of type **LoadBalancer** on top of it, to expose the load-balanced service to the public via ELB:

```bash
kubectl apply -f frontend.yaml
```

Some checks:

```bash
kubectl get pods
kubectl get pods -l app=guestbook
kubectl get pods -l app=guestbook -l tier=frontend
```

Check AWS management console for the ELB which has been created

## Access from outside the cluster

grab the public DNS of the frontend service LoadBalancer (ELB):

```bash
kubectl describe service frontend
```

copy the name and paste it into your browser

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
