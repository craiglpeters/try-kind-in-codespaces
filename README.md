# Learn Kubernetes with Kind and Kubectl in Codespaces

This repo allows you to quickly spin up an environment in [GitHub Codespaces](https://github.com/features/codespaces), and try out [Kubernetes](https://k8s.dev) using [kind](https://kind.sigs.k8s.io) and [kubectl](https://kubernetes.io/docs/reference/kubectl/).

To get started [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=551578719) and then open the `try-kind.md` file in the explorer.

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

## Let's now make the app accessible outside of our machine (and the cluster)

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

## Codespaces implementation

To take a look under the covers:
 - [Codespaces configuration docs](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/introduction-to-dev-containers)
 - open source [dev container specification](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/introduction-to-dev-containers)
 - Take a look at the `.devcontainer` folder of this repository

 > Note: [Codespaces port forwarding](https://docs.github.com/en/codespaces/developing-in-codespaces/forwarding-ports-in-your-codespace) has a known limitation, so `kubectl proxy` allows connecting to the kubernetes API, but not to services exposed by pods. The workaround is to use `kubectl port-forward`.