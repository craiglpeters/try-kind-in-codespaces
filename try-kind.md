# Learning a bit about kind

## Starting and connecting with the cluster

Start with creating a cluster
```bash
kind create cluster
```

Inspect the basic info about the cluster
```bash
kubectl cluster-info
```

Find out about the node that makes up the cluster
```bash
kubectl get nodes
```

## Now let's deploy a very simple app

```bash
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

```bash
kubectl get deployments
```

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
```

```bash
echo Name of the Pod: $POD_NAME
```

## Let's learn about pods

```bash
kubectl get pods
```

```bash
kubectl describe pods
```

If you've got some work here you want to save, go to github.com/codespaces, find this codespace, click the `...` and `Publish to repo`