# F5 Big-IP on Azure

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create the below secrets in the secret store management:
* **bigip_license_key** - License key for BIG IP VE, it is being applied during configuration.

You can create those with the following cfy commands:\
``cfy secrets create bigip_license_key -s <bigip_license_key>``

## Provisioning

VNFM-F5-Prov-Openstack-vm.yaml is responsible for creation BIG-IP Virtual Machine connected to 3 networks:
* Management,
* WAN,
* Public.

Network's NICs are connected to security group created in network deployment.
Networks and security group names are fetched from network deployment using `get_capability` intrinsic function.

### Inputs
* *flavor_id* - ID of the flavor in OpenStack - default: f7cfaaa8-e2db-4f9b-a65b-6a407f340960
* *vnf_vm_name* - Name of Virtual Machine - default: bigip
* *image_id* - ID of the image in OpenStack - default: 6d8ff903-f35b-43df-b7c2-e219929924b9
* *resource_prefix* - Prefix of every resource created at this deployment on Openstack - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Openstack - default: 0
* *openstack_network_deployment_name* - Name of the deployment responsible for router, security group and networks creation -
    default: VNFM-Networking-Prov-Openstack-networks

### Installation

To provision BIG-IP execute:

``cfy install VNFM-F5-Prov-Openstack-vm.yaml -b VNFM-F5-Prov-Openstack-vm``

### Uninstalling

To delete BIG IP execute:

``cfy uninstall VNFM-F5-Prov-Openstack-vm``

## Configuration

The configuration requires the IP addresses of the VM created during provisioning, therefore the provisioning deployment name
is required as an input. Exposed IP addresses are fetched using *get_capability* function, ie:\
``{ get_capability: [ {get_input: prov_deployment_name}, wan_ip ] }``

VNFM-F5-Conf.yaml is responsible for licensing BIG IP with the provided registration key and applying VLAN configuration necessary for further LTM configuration.
It consists of 2 nodes:
1. *license* - Applies license using [install_license.txt](Resources/templates/install_license.txt) file and revokes it using [revoke_license.txt](Resources/templates/revoke_license.txt).
2. *vlan_configuration* - Creates VLAN configuration on WAN and Public interfaces - using [vlan_config.txt](Resources/templates/vlan_config.txt) to apply it during install and [vlan_config_delete.txt](Resources/templates/vlan_config_delete.txt) to tear it down during uninstall.


### Inputs

* *prov_deployment_name* - Name of BIG IP Provisioning deployment created in previous section

### Install

``cfy install VNFM-F5-Conf.yaml -b VNFM-F5-Conf``

### Uninstall
During uninstall the license is revoked so it can be used on different BIG IP VE or on the same one again.
Also VLAN configuration is deleted.

``cfy uninstall VNFM-F5-Conf``
