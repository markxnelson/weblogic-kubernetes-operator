---
title: "Secrets"
date: 2019-02-23T17:36:33-05:00
weight: 6
description: "Kubernetes secrets for the WebLogic Operator"
---

#### WebLogic Domain Credentials Secret

The credential for the WebLogic domain is kept in a kubernetes `secret` that
follows the pattern `<domainUID>-weblogic-credentials`, where `<domainUID>` is
the unique identifier of the domain, for example, `mydomain-weblogic-credentials`.

If the WebLogic domain will be started in `domain1-ns` and the `<domainUID>` is `domain1`,
an example of creating a kubernetes `generic secret` is as follows:

```bash
$ kubectl -n domain1-ns create secret generic domain1-weblogic-credentials \
  --from-file=username --from-file=password
$ kubectl -n domain1-ns label secret domain1-weblogic-credentials \
  weblogic.domainUID=domain1 weblogic.domainName=domain1
```

{{% notice tip %}}
Oracle recommends that you not include unencrypted passwords on command lines.
Passwords and other sensitive data can be prompted for or looked up by shell scripts and/or
tooling. For more information about creating Kubernetes secrets, see the Kubernetes
[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/#creating-your-own-secrets)
documentation.
{{% /notice %}}

The WebLogic operator's introspector job will expext the secret key names to be:

- `username`
- `password`

For example, here is what results when describing the kubernetes `secret`:
```bash
$ kubectl -n domain1-ns describe secret domain1-weblogic-credentials
Name:         domain1-weblogic-credentials
Namespace:    domain1-ns
Labels:       weblogic.domainName=domain1
              weblogic.domainUID=domain1
Annotations:  <none>

Type:  Opaque

Data
====
password:  8 bytes
username:  8 bytes
```
