---
title: "Channels"
date: 2019-03-08T19:07:36-05:00
weight: 2
description: "WebLogic Channels"
---

#### WebLogic T3 Channels

{{% notice warning %}}
Oracle recommends not to expose any admin, RMI, or T3 channels outisde the Kubernetes cluster
unless absolulety necessary. If exposing any of these channels, limit access using
controls like security lists or setup a Bastion to provide access.
{{% /notice %}}

When accesing T3/RMI based channels, the preferred approach is to `kubectl exec` into
the kubernetes pod and then run `wlst` or setup Bastion access and then run
`wlst` from the Bastion host to connect to the Kubernetes cluster.

Also, consider a private VPN if you need use cross-domain T3 access
between clouds, data centers, etc.
