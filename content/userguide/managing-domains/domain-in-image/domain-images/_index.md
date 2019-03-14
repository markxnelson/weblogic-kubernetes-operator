+++
title = "Domain images"
date = 2019-02-23T16:45:55-05:00
weight = 3
pre = "<b> </b>"
+++

{{% notice warning %}}
Oracle strongly recommends storing a domain image as private in the registry.
A Docker image that contains a WebLogic domain home has sensitive information
including keys and credentials that are used to access external resources
(for example, data source password). For more information, see
[domain home in image protection]({{<relref "/security/domain-security/image-protection.md#weblogic-domain-in-docker-image-protection">}})
in the ***Security*** section.
{{% /notice %}}
