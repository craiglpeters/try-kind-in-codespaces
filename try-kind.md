# Learning a bit about Kubernetes through kind and kubectl (which are pre-installed on this environment)

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

Let's check the pod logs
```bash
kubectl logs $POD_NAME
```

Let's find out what environment vars are defined in the pod
```bash
kubectl exec $POD_NAME -- env
```

Let's start a bash interactive session in the pod
```bash
kubectl exec -ti $POD_NAME -- bash
```

Feel free to poke around in the pod (e.g. ls)
```bash
cat server.js
```

Let's see what the pod returns when we hit the exposed port
```bash
curl localhost:8080
```
If you like you can update the "Hello Kubernetes bootcamp!" text.
```bash
sed -i 's/Hello Kubernetes bootcamp/Hi contributing to Kubernetes course/g' server.js
```

Now exit out of the pod back to the codespace environment
```bash
exit
```

Let's now make the app accessible
Let's find out what IP and port our app's service listens on
```bash
kubectl get services
```

Now let's expose the service on port 8080
```bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

So that we can access the service from the outside world, let's forward the port
```bash
kubectl port-forward ${POD_NAME} 8080:8080
```

You'll notice that Codespaces has detected the forwarded port. Go ahead and open a brower to see what the pod returns - it should look familiar

Press CTRL+C in the terminal windows to end the port forwarding

If you've got some work in your environment that you want to save, go to github.com/codespaces, find this codespace, click the `...` and `Publish to repo`