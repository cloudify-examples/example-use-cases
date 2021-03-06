# Common resources

Before installation of any service, suitable resource group, networks and security group must be created.

The following resources are created in Azure using the ``VNFM-Networking-Prov-Azure-networks.yaml`` blueprint:
* **Management Network** - This network connects the Cloudify manager to all managed components.
* **LAN network** - This network connects the firewall to the web server.
* **WAN network** - This network connects the load balancer to the web server.
* **Public network** - This is the public network accessible to the user. BIG IP exposes the web server on the Public network interface.
* **Security group** - Security group for VNF NICs, defined by *network_security_group_rules* input,
* **Resource group** - Group that is required to create any other resource in Azure.

Those resources are fetched by all other provisioning deployments.

## Prerequisites:

Prior to installation you have to upload plugins and create secrets.

### Plugins

Upload:
* **cloudify-azure-plugin** - Tested for version 2.1.1
* **cloudify-utilities-plugin** - Tested for version 1.12.5

You can do this using Cloudify UI from *Cloudify Catalog* page with *Plugins Catalog* widget just by picking the plugin and clicking *Upload*.\
Using *cfy* you can upload those with following commands:\
``cfy plugins upload https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.12.5/cloudify_utilities_plugin-1.12.5-py27-none-linux_x86_64-centos-Core.wgn -y https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.12.5/plugin.yaml``

``cfy plugins upload https://github.com/cloudify-incubator/cloudify-azure-plugin/releases/download/2.1.1/cloudify_azure_plugin-2.1.1-py27-none-linux_x86_64-centos-Core.wgn -y https://github.com/cloudify-incubator/cloudify-azure-plugin/releases/download/2.1.1/plugin.yaml``

### Secrets

Create the below secrets in the secret store management:
* **Azure secrets:**
    * *azure_client_id* - Service Principal appId
    * *azure_client_secret* - Service Principal password
    * *azure_subscription_id* - Service Principal ID
    * *azure_tenant_id* - Service Principal tenant
    * *azure_location* - Specifies the supported Azure location for the resource

You can create those with the following cfy commands:\
``cfy secrets create azure_client_id -s <azure_client_id>``\
``cfy secrets create azure_client_secret -s <azure_client_secret>``\
``cfy secrets create azure_subscription_id -s <azure_subscription_id>``\
``cfy secrets create azure_tenant_id -s <azure_tenant_id>``\
``cfy secrets create azure_location -s <azure_location>``

## Environment creation

### Inputs
* *retry_after* - The number of seconds for each task retry interval (in the
          case of iteratively checking the status of an asynchronous operation) - default: 5
* *resource_prefix* - Prefix of every resource created at this deployment on Azure - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Azure - default: 0
* *mgmt_subnet_cidr* - Management subnet CIDR - default: 10.10.1.0/24
* *public_subnet_cidr* -Public subnet CIDR - default: 10.10.2.0/24
* *wan_subnet_cidr* - WAN subnet CIDR - default: 10.10.3.0/24
* *lan_subnet_cidr* - LAN subnet CIDR - default: 10.10.4.0/24
* *network_api_version* - API Version for Network - default: "2015-06-15"
* *network_security_group_rules* - Security group rules for VNF's NICs -
    default:
    ````
      - name: all_tcp
        properties:
          description: All TCP
          protocol: Tcp
          sourcePortRange: '*'
          destinationPortRange: '*'
          sourceAddressPrefix: '*'
          destinationAddressPrefix: '*'
          priority: 100
          access: Allow
          direction: Inbound
      - name: all_ucp
        properties:
          description: All UDP
          protocol: Udp
          sourcePortRange: '*'
          destinationPortRange: '*'
          sourceAddressPrefix: '*'
          destinationAddressPrefix: '*'
          priority: 101
          access: Allow
          direction: Inbound
    ````
### Installation

Install using VNFM-Networking-Prov-Azure-networks.yaml blueprint:

``cfy install  VNFM-Networking-Prov-Azure-networks.yaml -b  VNFM-Networking-Prov-Azure-networks``

**It should be installed only one time before start of provisioning services.**
It will be reused automatically by blueprints using the capabilities mechanism.

###Uninstalling

``cfy uninstall VNFM-Networking-Prov-Azure-networks``
