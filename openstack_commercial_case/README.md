# Commercial VNF Use Case

Upon completion of this example we will have a complete running network service.

![ns](https://user-images.githubusercontent.com/30900001/52050834-12889e00-2552-11e9-9a68-452e92cc7014.png)

This series of blueprints demonstrates how to install a simple network service consisting of a load balancer and a firewall. To make it a tad more interesting we will be deploying a simple web service to allow for complete user experience. All of the examples are currently implemented only for Azure.

**Note!**
The infrastructure used in this example is Microsoft Azure, and the demonstrated VNFs are:
  * F5 BIG-IP VE (Load balancer)
  * Fortigate (Firewall)
  * Httpd (Web Server)

## Common Prerequisites:

* Cloudify Manager 4.5.5, for more info: [Cloudify-Getting-Started](https://cloudify.co/download/).

* These plugins should exist on your manager. (E.g. You can just run `cfy plugins bundle-upload`, which will satisfy all plugin requirements.):
  * [cloudify-openstack-plugin](https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases), version 2.14.7 or higher.
  * [cloudify-utilities-plugin](https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases), version 1.12.5 or higher.

* These secrets should exist on your manager:
* **Openstack secrets:**
    * *keystone_username* - Username used for authentication in Keystone service
    * *keystone_password* - Password used for authentication in Keystone service
    * *keystone_tenant_name* - Name of the tenant in OpenStack
    * *keystone_url* - URL used for authentication in Keystone service
    * *keystone_region* - Name of the region in OpenStack

You can create those with the following cfy commands:\
``cfy secrets create keystone_username -s <keystone_username>``\
``cfy secrets create keystone_password -s <keystone_password>``\
``cfy secrets create keystone_tenant_name -s <keystone_tenant_name>``\
``cfy secrets create keystone_url -s <keystone_url>``\
``cfy secrets create keystone_region -s <keystone_region>``

## Installation

The installation is broken into a few basic steps. Go to the relevant README and progress through these steps in the correct order.

1. [Prepare the environment](network-topology/README.md##Installation): Create networks, subnets, router and a security group.
1. Provisioning of the VNFs:
  1. [Provision the load balancer](bigip/README.md##Provisioning) and setup basic settings.
  1. [Provision the firewall](fortigate/README.md##Provisioning) and configure its network interfaces and the network settings.
  1. [Provision the web server](httpd/README.md##Provisioning) instance, configure it, and setup basic web content.
1. Compose the service flow by:
  1. [Configuration the load balancer](bigip/README.md##Configuration) and setup basic settings.
  1. [Configuration the firewall](fortigate/README.md##Configuration) and configure its network interfaces and the network settings.
  1. [Configuration the web server](httpd/README.md##Configuration) instance, configure it, and setup basic web content.
1. [Create service](service/README.md) The last step creates a service chain of connected network services (Load Balancer, Firewall and Web Server). In this case service chaining consists of port forwarding rule on Fortigate and load balancing rule on BIG IP in order to pass traffic through.
