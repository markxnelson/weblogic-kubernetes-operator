+++
title = "Manage Operators"
date = 2019-02-23T16:43:38-05:00
weight = 3
pre = "<b> </b>"
+++


## Overview

Helm is used to create and deploy necessary operator resources and to run the operator in a Kubernetes cluster. Helm is a framework that helps you manage Kubernetes applications, and Helm charts help you define and install Helm applications into a Kubernetes cluster. The operator's Helm chart is located in the `kubernetes/charts/weblogic-operator` directory.

{{% notice warning %}}
If you have an older version of the operator installed on your cluster, then you must remove it before installing this version. This includes the 2.0-rc1 version; it must be completely removed. You should remove the deployment (for example, `kubectl delete deploy weblogic-operator -n your-namespace`) and the custom
resource definition (for example, `kubectl delete crd domain`).  If you do not remove
the custom resource definition, then you might see errors like this:
```
Error from server (BadRequest): error when creating "/scratch/output/uidomain/weblogic-domains/uidomain/domain.yaml":
the API version in the data (weblogic.oracle/v2) does not match the expected API version (weblogic.oracle/v1
```
{{% /notice %}}      

{{% notice note %}}
You should be able to upgrade from version 2.0-rc2 to 2.0 because there are no backward incompatible changes between these two releases.
{{% /notice %}}
