# Creating and Managing Deployments

Deployments abstract away the low level details of managing Pods. Pods are tied to the lifetime of the node they are created on. When the node goes away so does the Pod. ReplicaSets can be used to ensure one or more replicas of a Pods are always running, even when nodes fail.

Deployments sit on top of ReplicaSets and add the ability to define how updates to Pods should be rolled out.

## Create an nginx Deployment

By default (i.e. without `--restart=Never`) `kubectl run` will create a
deployment:

```
kubectl run --image=nginx nginx
```

* What pods are running?
* How many are there?

The Deployment will make sure there's always the right number of pods running.
It continuously looks for pods that match the labels in its `selector`. Look for
the selectors used to match pods in your new deployment and what labels are set
in the deployment's pod template:

```
kubectl get -o yaml deployment nginx
```

You may want to use `kubectl explain` to explore what each field represents, for
example:

```
kubectl explain deployment.spec.template
```

## Scaling Deployments

Behind the scenes Deployments manage ReplicaSets. Each deployment is mapped to one active ReplicaSet. Use the `kubectl get replicasets` command to view the current set of replicas.

```
kubectl get replicasets
```

ReplicaSets are scaled through the Deployment for each service and can be scaled independently. Use the `kubectl scale` command to scale the nginx deployment:

```
kubectl scale deployments nginx --replicas=3
```

```
kubectl describe deployments nginx
```

```
kubectl get pods
```

```
kubectl get replicasets
```

When updating a deployment, Kubernetes will create a new replicaset and scale it
up while scaling down the outdated replicaset. Switch to the alpine build of
nginx by changing `spec.template.spec.containers[0].image` from `nginx` to
`nginx:alpine` using `kubectl edit`:

```
kubectl edit deployment nginx
```

```
kubectl get replicasets
```

Examine the deployments event log:

```
kubectl describe deployment nginx
```
