---
title: "Encryption"
date: 2019-02-23T17:36:29-05:00
weight: 3
description: "WebLogic Domain encryption and the WebLogic Operator"
---

#### Reference
* [Encryption of values for WebLogic configuration overrides]({{<relref "/userguide/configoverrides/config-overrides.md#override-template-macros">}})
* [WebLogic Operator Introspector Encryption](#weblogic-operator-introspector-encryption")
* [Encryption of Kubernetes Secrets](#encryption-of-kubernetes-secrets")

#### WebLogic Operator Introspector Encryption

The WebLogic Operator has an introspection job that handles the WebLogic Domain encryption.
The intropsection also addresses use of kubernetes secrets for use with configuration overrides.
For additional information on the configuration handling, see the
[configuration overrides]({{<relref "/userguide/configoverrides/config-overrides.md">}})
documenation.

The intropection also creates a `boot.properties` file that is made available
to the pods in the WebLogic Domain. The credential used for the
WebLogic Domain is kept in a Kubernetes `Secret` which follows the naming pattern
`<domainUID>-weblogic-credentials`, where `<domainUID>` is
the unique identifier of the domain, for example, `mydomain-weblogic-credentials`.

{{% notice info %}}
For more information about the WebLogic credentials secret, please see [Secrets]({{<relref "/security/secrets.md#reference">}})
under **Securty**.
{{% /notice %}}

#### Encryption of Kubernetes Secrets

{{% notice tip %}}
To better protect your credentials and private keys, the Kubernetes cluster should be setup with encryption.
Please see the Kubernetes documenation about
[encryption at rest for secret data](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
and [using a KMS provider for data encryption](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/).
{{% /notice %}}
