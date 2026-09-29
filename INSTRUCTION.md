# Kubernetes Scheduling

## 1. Start cluster

```bash
chmod +x bootstrap.sh
./bootstrap.sh
```

## 2. Check nodes, labels and taints

```bash
kubectl get nodes --show-labels
kubectl get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
```

MySQL nodes should have:

```text
app=mysql
app=mysql:NoSchedule
```

TodoApp nodes should have:

```text
app=todoapp
```

## 3. Check MySQL

```bash
kubectl get pods -n mysql -o wide
kubectl get statefulset -n mysql
```

MySQL Pods must run on `app=mysql` nodes and on different nodes.

## 4. Check TodoApp

```bash
kubectl get pods -n todoapp -o wide
kubectl get deployment -n todoapp
```

TodoApp Pods should prefer `app=todoapp` nodes and should not run on the same node.

## 5. Final check

```bash
kubectl get pods -A -o wide
```

All required Pods should be in `Running` status.
