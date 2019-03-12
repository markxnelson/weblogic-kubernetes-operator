---
title: "Service Accounts"
date: 2019-02-23T17:36:12-05:00
weight: 4
description: "Kubernetes service accounts for the WebLogic Operator"
---

#### Reference
* [Helm Service Account]({{<relref "userguide/managing-operators/_index.md#install-helm-and-tiller">}})
* [Operator Helm Chart Service Account Configuration]({{<relref "/userguide/managing-operators/using-the-operator/using-helm/_index.md#serviceaccount">}})

#### WebLogic Operator Service Account

When the WebLogic Operator is installed, the Helm chart property of `serviceAccount` can
be specified where the value contains the name of the Kubernetes `ServiceAccount`
in the namespace that the WebLogic Operator will be installed.
For more information about the Helm chart see the
[operator Helm configuration values]({{<relref "/userguide/managing-operators/using-the-operator/using-helm/_index.md#operator-helm-configuration-values">}}).

The WebLogic Operator will use this `ServiceAccount` when calling the Kubernetes API server
and the appropriate access controls will be created for this `ServiceAccount` by
the operator's Helm Chart.

{{% notice info %}}
For more information about access controls see [RBAC]({{<relref "/security/rbac.md">}}) under **Security**.
{{% /notice %}}

In order to display the `ServiceAccount` used by the WebLogic Operator
where the operator was installed using the Helm release name `weblogic-operator`,
look for the `serviceAccount` value using the Helm command:
```bash
$ helm get values --all weblogic-operator
```
