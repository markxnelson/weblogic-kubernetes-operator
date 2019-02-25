---
title: "Cleaning up"
date: 2019-02-22T15:44:42-05:00
draft: false
weight: 10
---


### 7. Remove the domain.

a.	Remove the domain's Ingress by using `helm`:

```bash
$ helm delete --purge sample-domain1-ingress
```
b.	Remove the domain resources by using the sample [`delete-weblogic-domain-resources`](../kubernetes/samples/scripts/delete-domain/delete-weblogic-domain-resources.sh) script.

```bash
$ kubernetes/samples/scripts/delete-domain/delete-weblogic-domain-resources.sh -d sample-domain1
```

c.	Use `kubectl` to confirm that the server pods and domain resource are gone:

```bash
$ kubectl get pods -n sample-domain1-ns
$ kubectl get domains -n sample-domain1-ns
```

### 8. Remove the domain namespace.
a.	Configure the Traefik load balancer to stop managing the Ingresses in the domain namespace:

```bash
$ helm upgrade \
  --reuse-values \
  --set "kubernetes.namespaces={traefik}" \
  --wait \
  traefik-operator \
  stable/traefik
```

b.	Configure the operator to stop managing the domain:

```bash
$ helm upgrade \
  --reuse-values \
  --set "domainNamespaces={}" \
  --wait \
  sample-weblogic-operator \
  kubernetes/charts/weblogic-operator
```
c.	Delete the domain namespace:

```bash
$ kubectl delete namespace sample-domain1-ns
```


### 9. Remove the operator.

a.	Remove the operator:

```bash
$ helm delete --purge sample-weblogic-operator
```

b.	Remove the operator's namespace:

```bash
$ kubectl delete namespace sample-weblogic-operator-ns
```

### 10. Remove the load balancer.

a.	Remove the Traefik load balancer:

```bash
$ helm delete --purge traefik-operator
```

b.	Remove the Traefik namespace:

```bash
$ kubectl delete namespace traefik
```
