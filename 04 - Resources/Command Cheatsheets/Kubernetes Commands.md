---
type: cheatsheet
tags: [commands, kubernetes, kubectl, k8s, cheatsheet]
created: 2026-03-02
---

# Kubernetes Commands

---

## Cluster Info

```bash
kubectl cluster-info
```

```bash
kubectl version
```

```bash
kubectl config current-context
```

```bash
kubectl config get-contexts
```

```bash
kubectl config use-context context-name
```

---

## Namespaces

```bash
kubectl get namespaces
```

```bash
kubectl create namespace myns
```

```bash
kubectl delete namespace myns
```

```bash
kubectl config set-context --current --namespace=myns
```

---

## Pods

```bash
kubectl get pods
```

```bash
kubectl get pods -A
```
> All namespaces

```bash
kubectl get pods -o wide
```
> With node and IP info

```bash
kubectl get pods -w
```
> Watch for changes

```bash
kubectl describe pod pod-name
```

```bash
kubectl logs pod-name
```

```bash
kubectl logs -f pod-name
```
> Follow logs

```bash
kubectl logs pod-name -c container-name
```
> Specific container

```bash
kubectl logs pod-name --previous
```
> Previous container instance

```bash
kubectl exec -it pod-name -- bash
```

```bash
kubectl exec -it pod-name -- sh
```

```bash
kubectl delete pod pod-name
```

```bash
kubectl run test --image=nginx --restart=Never
```

```bash
kubectl port-forward pod-name 8080:80
```

---

## Deployments

```bash
kubectl get deployments
```

```bash
kubectl describe deployment deploy-name
```

```bash
kubectl create deployment myapp --image=nginx
```

```bash
kubectl scale deployment myapp --replicas=3
```

```bash
kubectl rollout status deployment myapp
```

```bash
kubectl rollout history deployment myapp
```

```bash
kubectl rollout undo deployment myapp
```

```bash
kubectl rollout restart deployment myapp
```

```bash
kubectl set image deployment/myapp container=image:tag
```

```bash
kubectl delete deployment myapp
```

---

## Services

```bash
kubectl get services
```

```bash
kubectl describe service svc-name
```

```bash
kubectl expose deployment myapp --port=80 --type=LoadBalancer
```

```bash
kubectl port-forward service/svc-name 8080:80
```

```bash
kubectl delete service svc-name
```

---

## ConfigMaps & Secrets

```bash
kubectl create configmap myconfig --from-literal=key=value
```

```bash
kubectl create configmap myconfig --from-file=config.txt
```

```bash
kubectl get configmaps
```

```bash
kubectl create secret generic mysecret --from-literal=password=mypass
```

```bash
kubectl get secrets
```

```bash
kubectl describe secret mysecret
```

---

## Apply & Delete

```bash
kubectl apply -f manifest.yaml
```

```bash
kubectl apply -f directory/
```

```bash
kubectl apply -f https://url/manifest.yaml
```

```bash
kubectl delete -f manifest.yaml
```

```bash
kubectl diff -f manifest.yaml
```
> Preview changes before applying

---

## Nodes

```bash
kubectl get nodes
```

```bash
kubectl describe node node-name
```

```bash
kubectl top nodes
```
> Resource usage

```bash
kubectl top pods
```

```bash
kubectl cordon node-name
```
> Mark unschedulable

```bash
kubectl drain node-name --ignore-daemonsets
```

```bash
kubectl uncordon node-name
```

---

## Debugging

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

```bash
kubectl get all
```

```bash
kubectl get all -A
```

```bash
kubectl api-resources
```

```bash
kubectl explain pod.spec.containers
```

---

*See also: [[Docker Commands]] | [[AWS CLI Commands]]*
