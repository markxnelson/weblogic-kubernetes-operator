---
title: "Encryption"
date: 2019-02-23T17:36:29-05:00
weight: 3
description: "WebLogic Domain encryption and the WebLogic Operator"
---

#### Reference
* [Encryption of values for WebLogic configuration overrides]({{<relref "/userguide/configoverrides/config-overrides.md#override-template-macros">}})

#### Encryption of Kubernetes Secrets

{{% notice tip %}}
To protect your Kubernetes secret with encryption, see the Kubernetes documenation about
[encryption at rest for secret data](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
and [using a KMS provider for data encryption](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/).
{{% /notice %}}

#### WebLogic Operator Introspector

The credential for the WebLogic domain is kept in a kubernetes `secret` that
follows the pattern `<domainUID>-weblogic-credentials`, where `<domainUID>` is
the unique identifier of the domain, for example, `domain1-weblogic-credentials`.

The introspector will read this secret and generate `boot.properties`.
