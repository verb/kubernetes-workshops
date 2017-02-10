# Creating and Managing Services

Services provide stable endpoints for Pods based on a set of labels.

In this lab you will create the `nginx` service and "expose" the `secure-nginx` Pod externally. You will learn how to:

* Create a service
* Use label selectors to expose a limited set of Pods externally

## Tutorial: Create a Service

As with deployments & pods, you would normally use a configuration file and
`kubectl create -f` to create a service. `kubectl expose` will create one for
you on-the-fly:

```
kubectl expose deployment nginx --port=80
```

Examine the newly created service using `kubectl get` and `kubectl describe`.
Notice that it doesn't have an external IP address. By default services are
internal to the cluster. Edit the service and change `spec.type` from
`ClusterIP` to `LoadBalancer`:

```
kubectl edit service nginx
```

Google Container Engine integrates with Google load balancing, so we have to
wait for an IP address to be assigned. Many things in Kubernetes are eventually
consistent, so it's frequently useful to watch for changes:

```
kubectl get services --watch
```

Eventually you should see nginx updated with an IP address.

```
curl -i http://$EXTERNAL_IP
```

*Dragons! Kubernetes automatically added a GCE firewall rule that allows the
entire world to connect to this port.*

## Tutorial: Add Labels to Pods

Services match pods based on labels, which are arbitrary key/value strings that
can be set on any Kubernetes object. Since services route traffic to pods based
on their labels, what would happen if no pods had labels matching those
specified in a service's `selector`?

Edit `spec.selector` and add a `secure: enabled` label inline with the current
`run: nginx` one:

```
kubectl edit service nginx
```

```
curl -i http://$EXTERNAL_IP  # HINT: don't wait too long...
```

Now the `nginx` service does not have any endpoints. One way to troubleshoot an
issue like this is to use the `kubectl get pods` command with a label query.

```
kubectl get pods -l "run=nginx"
```

```
kubectl get pods -l "run=nginx,secure=enabled"
```

> Notice this label query does not print any results

Use the `kubectl label` command to add the missing `secure=enabled` label to an
arbitrary nginx pod.

```
kubectl label pods <POD-NAME> 'secure=enabled'
```

View the list of endpoints on the `nginx` service:

```
kubectl describe services nginx
```

```
curl -i http://$EXTERNAL_IP
```
