+++
title = "Manage Domains"
date = 2019-02-23T16:43:45-05:00
weight = 4
pre = "<b> </b>"
+++

#### Important considerations for WebLogic domains in Kubernetes

Please be aware of the following important considerations for WebLogic domains running in Kubernetes.

* _Domain Home Location:_ The WebLogic domain home location is determined by the domain resource `domainHome` if set; otherwise, a default location is determined by the `domainHomeInImage` setting. If a domain resource `domainHome` field is not set
  and `domainHomeInImage` is `true` (the default), then the operator will
  assume that the domain home is a directory under `/u01/oracle/user_projects/domains/` and report an error if no domain is found
  or more than one domain is found.  If a domain resource `domainHome` field is not set and `domainHomeInImage` is `false`, then the operator will
  assume that the domain home is `/shared/domains/DOMAIN_UID`.

* _Log File Locations:_ The operator can automatically override WebLogic domain and server log locations using situational
  configuration overrides.  This occurs if the domain resource `logHomeEnabled` field is explicitly set to `true`, or if `logHomeEnabled` isn't set
  and `domainHomeInImage` is explicitly set to `false`.   When overriding, the log location will be the location specified by the `logHome` setting.

* _Listen Address Overrides:_  The operator will automatically override all WebLogic domain default,
  SSL, admin, or custom channel listen addresses (using situational configuration overrides).  These will become `domainUID` followed by a
  hyphen and then the server name, all lower case, and underscores converted to hyphens.  For example, if `domainUID=domain1` and
  the WebLogic server name is `Admin_Server`, then its listen address becomes `domain1-admin-server`.

* _Domain, Cluster, Server, and Network-Access-Point Names:_ WebLogic domain, cluster, server, and network-access-point (channel)
  names must contain only the characters `A-Z`, `a-z`, `0-9`, `-`, or `_`.  This ensures that they can be converted to
  meet Kubernetes resource and DNS1123 naming requirements.  (When generating pod and service names, the operator will convert
  configured names to lower case and substitute a hyphen (`-`) for each underscore (`_`).)

* _Node Ports:_ If you choose to expose any WebLogic channels outside the Kubernetes cluster via a `NodePort`, for example, the
  administration port or a T3 channel to allow WLST access, you need to ensure that you allocate each channel a
  unique port number across the entire Kubernetes cluster.  If you expose the administration port in each WebLogic domain in
  the Kubernetes cluster, then each one must have a different port.  This is required because `NodePorts` are used to
  expose channels outside the Kubernetes cluster.  

{{% notice warning %}}
Exposing admin, RMI, or T3 capable channels via a node port
can create an insecure configuration; in general only HTTP protocols should be made available externally and this exposure
is usually accomplished by setting up an external load balancer that can access internal (non-NodePort) services.
{{% /notice %}}

* _Host Path Persistent Volumes:_ If using a `hostPath` persistent volume, then it must be available on all worker nodes in the cluster and have read/write/many permissions for all container/pods in the WebLogic Server deployment.  Be aware
  that many cloud provider's volume providers may not support volumes across availability zones.  You may want to use NFS or a clustered file system to work around this limitation.

* _Security Note:_ The `USER_MEM_ARGS` environment variable defaults to `-Djava.security.egd=file:/dev/./urandom` in all WebLogic Server pods and the WebLogic introspection job. It can be explicitly set to another value in your domain resource YAML file using the `env` attribute under the `serverPod` configuration.

The following features are not certified or supported in this release:

* Whole Server Migration
* Consensus Leasing
* Node Manager (although it is used internally for the liveness probe and to start WebLogic Server instances)
* Multicast
* Multitenancy
* Production redeployment

Please consult My Oracle Support Doc ID 2349228.1 for up-to-date information about the features of WebLogic Server that are supported in Kubernetes environments.


### Creating and managing WebLogic domains

You can locate a WebLogic domain either in a persistent volume (PV) or in a Docker image.
For examples of each, see the [WebLogic operator samples](../kubernetes/samples/README.md).

If you want to create your own Docker images, for example, to choose a specific set of patches or to create a domain
with a specific configuration and/or applications deployed, then you can create the domain custom resource
manually to deploy your domain.  This process is documented in [this
sample](../kubernetes/samples/scripts/create-weblogic-domain/manually-create-domain/README.md).

### Modifying domain configurations

You can modify the WebLogic domain configuration for both the "domain in persistent volume" and the "domain in image" options before deploying a domain resource:

* When the domain is in a persistent volume, you can use WLST or WDT to change the configuration.
* For either case, you can use [configuration overrides](config-overrides.md).   

Configuration overrides allow changing a configuration without modifying its original `config.xml` or system resource XML files, and also support parameterizing overrides so that you can inject values into them from Kubernetes secrets.   For example, you can inject database user names, passwords, and URLs that are stored in a secret.

### About the domain resource

For information about the domain resource, see [Domain resource](domain-resource.md).

### Managing life cycle operations

You can perform life cycle operations on WebLogic servers, clusters, or domains.
See [Starting, stopping, and restarting servers](server-lifecycle.md) and [Restarting WebLogic servers](restart.md).

### Scaling clusters

The operator let's you initiate scaling of clusters in various ways:

* [Using kubectl to edit the domain resource](scaling.md#on-demand-updating-the-domain-resource-directly)
* [Using the operator's REST APIs](scaling.md#calling-the-operators-rest-scale-api)
* [Using WLDF policies](scaling.md#using-a-wldf-policy-rule-and-script-action-to-call-the-operators-rest-scale-api)
* [Using a Prometheus action](scaling.md#using-a-prometheus-alert-action-to-call-the-operators-rest-scale-api)
