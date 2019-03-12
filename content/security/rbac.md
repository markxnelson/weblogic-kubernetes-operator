---
title: "RBAC"
date: 2019-02-23T17:15:36-05:00
weight: 5
description: "Role based authorization for the WebLogic Operator"
---

The operator assumes that certain Kubernetes roles are created in the
Kubernetes cluster.  The operator Helm chart creates the required cluster roles,
cluster role bindings, roles and role bindings for the `ServiceAccount` that
is used by the operator. The operator will also attempt to verify that
the RBAC settings are correct when the Operator starts running.

{{% notice info %}}
For more information about Kubernetes roles, please see the Kubernetes
[RBAC](https://kubernetes.io/docs/admin/authorization/rbac/) documentation.
{{% /notice %}}

In order to display the Kubernetes roles and related bindings used by
the WebLogic Operator where the operator was installed using the
Helm release name `weblogic-operator`, look for:

- `Role`
- `RoleBinding`
- `ClusterRole`
- `ClusterRoleBinding`

when using the Helm command:
```bash
$ helm status weblogic-operator
```

{{% notice info %}}
For more information about the Kubernetes `ServiceAccount` used by the operator, please see
[Service Accounts]({{<relref "/security/service-accounts.md#weblogic-operator-service-account">}})
under **Security**.
{{% /notice %}}


The general design goal is to provide the operator with the minimum amount of
permissions that the operator requires and to favor built-in roles over custom roles
where it make sense to use the Kubernetes built-in roles.

#### Kubernetes role definitions

| Cluster role | Resources | Verbs | Notes |
| --- | --- | --- | --- |
| `NAMESPACE-weblogic-operator-clusterrole-general` |	namespaces, persistentvolumes	| get, list, watch | 1 |
| |	customresourcedefinitions in API group apiextensions.k8s.io	| get, list, watch, create, update, patch, delete, deletecollection	| |
| |	domains in API group weblogic.oracle	| get, list, watch, update, patch	| |
| |	Ingresses in API group extensions	| get, list, watch, create, update, patch, delete, deletecollection	| |
| `NAMESPACE-weblogic-operator-clusterrole-nonresource`	| nonResourceURLs: ["/version/*"]	| get |	1 |
|`NAMESPACE-weblogic-operator-clusterrole-namespace`	| secrets, persistentvolumeclaims	| get, list, watch	| 2 |
| |	services, pods, networkpolicies	| get, list, watch, create, update, patch, delete, deletecollection | |
| `NAMESPACE-weblogic-operator-clusterrolebinding-discovery`	| system:discovery in API group rbac.authorization.k8s.io | |		1 |
| `NAMESPACE-weblogic-operator-clusterrolebinding-auth-delegator`	| system:auth-delegator in API group rbac.authorization.k8s.io	| |	1 |

**Notes**:

1. This cluster role is assigned to the operator’s service account in the operator’s namespace.  The uppercase text `NAMESPACE` in the cluster role name is replaced with the operator’s namespace.
2. This cluster role is assigned to the operator’s service account in each of the “target namespaces”; that is, each namespace that the operator is configured to manage.
