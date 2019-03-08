---
title: "Domain Security"
date: 2019-02-27T11:26:21-05:00
draft: false
weight: 2
description: "WebLogic Domain security and the WebLogic Operator"
---

#### WebLogic T3 Channels

{{% notice info %}}
Oracle recommends not to expose any T3 channels outisde the Kubernetes cluster
unless absolulety necessary. If exposing a T3 channel, limit access using
controls like security lists or setup a Bastion to provide access.
{{% /notice %}}

When accesing T3 channels, the preferred approach is to `kubectl exec` into
the kubernetes pod and then run `wlst` or setup Bastion access and then run
`wlst` from the Bastion host to connect to the Kubernetes cluster.

Also, consider a private VPN if you need use cross-domain T3 access
between clouds, data centers, etc.
