# FortiGate NGFW Single VM on Azure

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create the below secrets in the secret store management:
* **fortigate_username** - Username for Fortigate VM, it is set during provisioning and used during configuration,
* **fortigate_password** - Password for Fortigate VM, it is set during provisioning and used during configuration.
* **fortigate_license** - Content of license file, its used during provisioning to license Fortigate

You can create those with the following cfy commands:\
``cfy secrets create fortigate_username -s <fortigate_username>``\
``cfy secrets create fortigate_password -s <fortigate_password>``\
``cfy secrets create fortigate_license -f <path to a fortigate license>``

## Provisioning

``VNFM-Fortigate-Prov-Azure-vm.yaml`` is responsible for FortiGate NGFW Single VM provisioning. VM is connected to 3 networks:
* Management,
* WAN,
* LAN.

Network's NICs are connected to the security group created in the network deployment.
Networks and security group names are fetched from network deployment using `get_capability` intrinsic function.

### Inputs

* *retry_after* - The number of seconds for each task retry interval (in the
          case of iteratively checking the status of an asynchronous operation) - default: 5
* *resource_prefix* - Prefix of every resource created at this deployment on Azure - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Azure - default: 0
* *azure_network_deployment_name* - Name of deployment responsible for creation resource group, security group and networks -
    default: VNFM-Networking-Prov-Azure-networks
* *vm_size* - Name of Virtual Machine Size in Azure - default: Standard_B2s
* *vm_os_family* - default: linux
* *vm_image_publisher* - Name of the organization that created the image - default: fortinet
* *vm_image_offer* - The name of a group of related images created by a publisher - default: fortinet_fortigate-vm_v5
* *vm_image_sku* - An instance of an offer, such as a major release of a distribution - fortinet_fg-vm
* *vm_image_version* - Version of the image - default: 6.0.3
* *vnf_vm_name* - Name of VM - default: fortigate
* *fortigate_license_filename* - Name of the Fortigate license file (It will be uploaded to Fortigate VM with this name). It should have .lic file extension. - default: FGVM02TM19000054.lic

### Installation

Resources created in Prerequesites subsection are fetched using capabilities exposed by *azure-networks* deployment and ``VNFM-Fortigate-Prov-Azure-vm.yaml`` is using it.
To provision FortiGate NGFW Single VM:

``cfy install VNFM-Fortigate-Prov-Azure-vm.yaml -b VNFM-Fortigate-Prov-Azure-vm``

### Uninstalling
To delete Fortigate execute:

``cfy uninstall VNFM-Fortigate-Prov-Azure-vm``

## Configuration

The configuration requires the IP addresses of the VM created during provisioning, therefore the provisioning deployment name
is required as an input. Exposed IP addresses are fetched using *get_capability* function, ie:\
``{ get_capability: [ { get_input: fortigate_vm_deployment_name }, vm_public_ip_address] }``

``VNFM-Fortigate-Conf.yaml`` is responsible for applying base configuration for the newly created FortiGate VM. It configures all of the interfaces.
It consists of one node:
1. *fortigate_vnf_config* - Applies base configuration for Fortigate (VNF name change and basic configuration to interfaces) using [fortigate-baseline.txt](Resources/templates/fortigate-baseline.txt) file.


### Inputs

* *fortigate_vm_deployment_name* - Name of Fortigate Provisioning deployment - default: VNFM-Fortigate-Prov-Azure-vm

### Install

``cfy install VNFM-Fortigate-Conf.yaml -b VNFM-Fortigate-Conf``

### Uninstall

``cfy uninstall VNFM-Fortigate-Conf``
