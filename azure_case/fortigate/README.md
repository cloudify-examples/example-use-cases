# FortiGate NGFW Single VM on Azure

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create below secrets on secrets store management:
* **fortigate_username** - Username for HTTPD VM, it is set during provisioning and used during configuration,
* **fortigate_password** - Password for HTTPD VM, it is set during provisioning and used during configuration. 


## Provisioning

``VNFM-Fortigate-Prov-Azure-vm.yaml`` is responsible for FortiGate NGFW Single VM provisioning. VM is connected to 3 networks:
* Management,
* WAN,
* LAN.

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

### Installation

Resources created in Prerequesites subsection are fetched using capabilities exposed by *azure-networks* deployment and ``VNFM-Fortigate-Prov-Azure-vm.yaml`` is using it.
To provision FortiGate NGFW Single VM:

``cfy install VNFM-Fortigate-Prov-Azure-vm.yaml -b VNFM-Fortigate-Prov-Azure-vm``

### Uninstalling
To delete Fortigate execute:

``cfy uninstall VNFM-Fortigate-Prov-Azure-vm``

## Configuration

**IMPORTANT**: The version of the image, which is being used for FortiGate VM instantiation, is of BYOL type. That means, before
installing the configuration blueprint, the license file has to be manually applied to the newly provisioned FortiGate VM.

To apply license follow below steps:
1. Acquire fortigate license file (in .lic extension) 
2. Get public IP address of the VM (*vm_public_ip_address* deployment capability) to log into the FortiGate GUI
3. Log in using *fortigate_username* and *fortigate_password* secrets
4. Upload license file and wait till machine finish reboot
5. Refresh the fortigate GUI to see if the license was properly applied

### Inputs

* *fortigate_vm_deployment_name* - Name of Fortigate Provisioning deployment - default: VNFM-Fortigate-Prov-Azure-vm


### Install
``VNFM-Fortigate-Conf.yaml`` is responsible for applying the configuration for newly created FortiGate VM. It configures all of the interfaces and prepares NAT rules and policies, which are required to perform the service chain.

``cfy install VNFM-Fortigate-Conf.yaml -b VNFM-Fortigate-Conf``

### Uninstall
During uninstall, all of the NAT rules and policies are being deleted.

``cfy uninstall VNFM-Fortigate-Conf``
