# Common resources

Before installation of any service, suitable networks, subnets, router and security group must be created.

The following resources are created in OpenStack using the ``VNFM-Networking-Prov-Openstack-networks.yaml`` blueprint:
* **Management Network** - This network connects the Cloudify manager to all managed components.
* **LAN network** - This network connects the firewall to the web server.
* **WAN network** - This network connects the load balancer to the web server.
* **Public network** - This is the public network accessible to the user. BIG IP exposes the web server on the Public network interface.
* **Security group** - Security group for VNF NICs, defined by *network_security_group_rules* input,
* **Router** - Router, which allows to reach the management and public networks from the external network.

Those resources are fetched by all other provisioning deployments.

## Prerequisites:

Prior to installation you have to upload plugins and create secrets.

### Plugins

Upload:
* **cloudify-openstack-plugin** - Tested for version 2.14.7
* **cloudify-utilities-plugin** - Tested for version 1.12.5

This can be applied through the Cloudify manager user interface or using the CLI.
* To upload plugins using the Cloudify manager:
    * Browse to the *Cloudify Catalog* page and scroll to the *Plugins Catalog* widget. Select the relevant plugins and click *Upload*.
* To upload plugins using the CLI, run the following commands:
``cfy plugins upload https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.12.5/cloudify_utilities_plugin-1.12.5-py27-none-linux_x86_64-centos-Core.wgn -y https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.12.5/plugin.yaml``

``cfy plugins upload https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases/download/2.14.7/cloudify_openstack_plugin-2.14.7-py27-none-linux_x86_64-centos-Core.wgn -y https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases/download/2.14.7/plugin.yaml``

### Secrets

Create the below secrets in the secret store management:
* **Openstack secrets:**
    * *keystone_username* - Keystone username
    * *keystone_password* - Keystone password
    * *keystone_tenant_name* - Keystone tenant name
    * *keystone_url* - Keystone URL
    * *keystone_region* - Keystone region

You can create those with the following cfy commands:\
``cfy secrets create keystone_username -s <keystone_username>``\
``cfy secrets create keystone_password -s <keystone_password>``\
``cfy secrets create keystone_tenant_name -s <keystone_tenant_name>``\
``cfy secrets create keystone_url -s <keystone_url>``\
``cfy secrets create keystone_region -s <keystone_region>``

## Environment creation

### Inputs
* *resource_prefix* - Prefix of every resource created at this deployment on Openstack - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Openstack - default: 0
* *external_network_name* - Name of the external network - default: ext-net
* *nameservers* - List of DNS nameservers - default: [8.8.4.4, 8.8.8.8]
* *mgmt_subnet_cidr* - Management subnet CIDR - default: 10.10.1.0/24
* *mgmt_subnet_allocation_pools* - Management subnet allocation pools -
    default:
    ````
    - start: 10.10.1.2
      end: 10.10.1.254
    ````
* *public_subnet_cidr* -Public subnet CIDR - default: 10.10.2.0/24
* *public_subnet_allocation_pools* - Public subnet allocation pools -
    default:
    ````
    - start: 10.10.2.2
      end: 10.10.2.254
    ````
* *wan_subnet_cidr* - WAN subnet CIDR - default: 10.10.3.0/24
* *wan_subnet_allocation_pools* - WAN subnet allocation pools -
    default:
    ````
    - start: 10.10.3.2
      end: 10.10.3.254
    ````
* *lan_subnet_cidr* - LAN subnet CIDR - default: 10.10.4.0/24
* *lan_subnet_allocation_pools* - LAN subnet allocation pools -
    default:
    ````
    - start: 10.10.4.2
      end: 10.10.4.254
    ````
* *network_security_group_rules* - Security group rules for VNF's NICs -
    default:
    ````
      - port_range_min: 1
        port_range_max: 65535
        protocol: tcp
      - port_range_min: 1
        port_range_max: 65535
        protocol: udp
    ````
### Installation

Install using VNFM-Networking-Prov-Openstack-networks.yaml blueprint:

``cfy install  VNFM-Networking-Prov-Openstack-networks.yaml -b  VNFM-Networking-Prov-Openstack-networks``

**It should be installed only one time before start of provisioning services.**
It will be reused automatically by blueprints using the capabilities mechanism.

### Uninstalling

``cfy uninstall VNFM-Networking-Prov-Openstack-networks``
