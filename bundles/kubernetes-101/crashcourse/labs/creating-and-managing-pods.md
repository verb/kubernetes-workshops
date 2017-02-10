# Creating and managing pods

At the core of Kubernetes is the Pod. Pods represent a logical application and hold a collection of one or more containers and volumes. In this lab you will learn how to:

* Create and inspect Pods
* Modify Pod configuration
* Interact with Pods remotely using kubectl

## Tutorial: Creating Pods

Run a new pod with arguments specified on the command line:

```
kubectl run --image=nginx --restart=Never nginx
```

## Exercise: View Pod Details

Use the `kubectl get` and `kubectl describe` commands to view details for the
`nginx` pod.

### Quiz

* What is the IP address of the `nginx` Pod?
* What node is the `nginx` Pod running on?
* What containers are running in the `nginx` Pod?

## Exercise: View Pod Configuration

Normally pods are created and configured using a YAML or JSON configuration
description. One was automatically created by the `kubectl run` command. View
your pod's configuration:

```
kubectl get -o yaml pod nginx
```

## Exercise: Interact with a Pod remotely

Pods are allocated a private IP address by default and cannot be reached outside of the cluster. Use the `kubectl port-forward` command to map a local port to a port inside the `nginx` pod. 

Use two terminals. One to run the `kubectl port-forward` command, and the other to issue `curl` commands.

```
kubectl port-forward nginx 8080:80
```

```
curl http://localhost:8080
```

You can also use Cloud Shell's Web Preview feature to view the page in your web
browser.

## Exercise: View the logs of a Pod

Use the `kubectl logs` command to view the logs for the `nginx` Pod:

```
kubectl logs nginx
```

> Use the -f flag and observe what happens.

## Exercise: Run an interactive shell inside a Pod

Use the `kubectl exec` command to run an interactive shell inside the `nginx` Pod:

```
kubectl exec nginx --stdin --tty sh
```

## Cleanup

Delete the pod you've created:

```
kubectl delete pod nginx
```
