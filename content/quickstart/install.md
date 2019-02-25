---
title: "Install Operator and Load Balancer"
date: 2019-02-22T15:44:42-05:00
draft: true
weight: 5
---

### 2. Grant the Helm service account the `cluster-admin` role.

```bash
$ cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: helm-user-cluster-admin-role
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: default
  namespace: kube-system
EOF
```

### 3. Create a Traefik (Ingress-based) load balancer.

Use `helm` to install the [Traefik](../kubernetes/samples/charts/traefik/README.md) load balancer. Use the [values.yaml](../kubernetes/samples/charts/traefik/values.yaml) in the sample but set `kubernetes.namespaces` specifically.

```bash
$ helm install stable/traefik \
  --name traefik-operator \
  --namespace traefik \
  --values kubernetes/samples/charts/traefik/values.yaml  \
  --set "kubernetes.namespaces={traefik}" \
  --wait
```

### 4. Install the operator.

a.  Create a namespace for the operator:

```bash
$ kubectl create namespace sample-weblogic-operator-ns
```

b.	Create a service account for the operator in the operator's namespace:

```bash
$ kubectl create serviceaccount -n sample-weblogic-operator-ns sample-weblogic-operator-sa
```

c.  Use `helm` to install and start the operator from the directory you just cloned:	 

```bash
$ helm install kubernetes/charts/weblogic-operator \
  --name sample-weblogic-operator \
  --namespace sample-weblogic-operator-ns \
  --set image=oracle/weblogic-kubernetes-operator:2.0-rc2 \
  --set serviceAccount=sample-weblogic-operator-sa \
  --set "domainNamespaces={}" \
  --wait
```

d. Verify that the operator's pod is running, by listing the pods in the operator's namespace. You should see one for the operator.

```bash
$ kubectl get pods -n sample-weblogic-operator-ns
```

e.  Verify that the operator is up and running by viewing the operator pod's log:

```bash
$ kubectl logs -n sample-weblogic-operator-ns -c weblogic-operator deployments/weblogic-operator
```
