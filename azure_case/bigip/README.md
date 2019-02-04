# F5 Big-IP on Azure

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create below secrets on secrets store management:
* **bigip_username** - Username for BIG IP VE, it is set during provisioning and used during configuration, "admin" is not allowed
* **bigip_password** - Password for BIG IP VE, it is set during provisioning and used during configuration. The supplied password must be between 6-72 characters long and must satisfy at least 3 of password complexity requirements from the following: Contains an uppercase character, Contains a lowercase character, Contains a numeric digit, Contains a special characterr. Control characters are not allowed

You can create those with following cfy commands:\
``cfy secrets create bigip_username -s <bigip_username>``\
``cfy secrets create bigip_password -s <bigip_password>``

## Provisioning 

VNFM-F5-Prov-Azure-vm.yaml is responsible for creation BIG-IP Virtual Machine connected to 3 networks:
* Management,
* WAN,
* Public.

Networks names are fetched from network deployment using `get_capability` intrinsic function.

### Inputs
* *virtual_machine_size* - Name of Virtual Machine Size in Azure - default: Standard_A7
* *vm_name* - Name of Virtual Machine - default: BIGIP
* *virtual_machine_image_sku* - An instance of an offer, such as a major release of a distribution - default: 'f5-big-all-1slot-byol'
* *virtual_machine_image_publisher* - Name of the organization that created the image - default: 'f5-networks'
* *virtual_machine_image_offer* - The name of a group of related images created by a publisher - default: 'f5-big-ip-byol'
* *retry_after* - The number of seconds for each task retry interval (in the
          case of iteratively checking the status of an asynchronous operation) - default: 5
* *resource_prefix* - Prefix of every resource created at this deployment on Azure - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Azure - default: 0
* *network_api_version* - API Version for Network - default: "2015-06-15"
* *azure_network_deployment_name* - Name of deployment responsible for creation resource group, security group and networks -
    default: VNFM-Networking-Prov-Azure-networks

### Installation

Resources created in Prerequisites subsection are fetched in existing_networks.yaml blueprint file using capabilities and VNFM-F5-Prov-Azure-vm.yaml is using it.

To provision BIG-IP execute:

``cfy install VNFM-F5-Prov-Azure-vm.yaml -b VNFM-F5-Prov-Azure-vm``

### Uninstalling

To delete BIG IP execute:

``cfy uninstall VNFM-F5-Prov-Azure-vm``

## Configuration

Configuration requires IP addresses of VM created during provisioning, therefore Provisioning deployment name 
is required input so exposed IP addresses are fetched using *get_capability* function, ie:\
``{ get_capability: [ {get_input: prov_deployment_name}, wan_ip ] }``

### Inputs

* *bigip_license_key* - License key for BIG IP VE
* *prov_deployment_name* - Name of BIG IP Provisioning deployment created in previous section

### Install
VNFM-F5-Conf.yaml is responsible for licensing BIG IP with provided registration key and applying VLAN configuration necessary for further LTM configuration.

``cfy install VNFM-F5-Conf.yaml -b VNFM-F5-Conf``

### Uninstall
During uninstall the license is revoked so it can be used on different BIG IP VE or on the same one again.
Also VLAN configuration is deleted.

``cfy uninstall VNFM-F5-Conf``
