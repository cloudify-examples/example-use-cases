# FortiGate NGFW Single VM on Openstack

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create the below secrets in the secret store management:
* **fortigate_license** - Content of license file, its used during provisioning to license Fortigate

You can create those with the following cfy commands:\
``cfy secrets create fortigate_license -f <path to a fortigate license>``

## Provisioning

``VNFM-Fortigate-Prov-Openstack-vm.yaml`` is responsible for FortiGate NGFW Single VM provisioning. VM is connected to 3 networks:
* Management,
* WAN,
* LAN.

Network's NICs are connected to the security group created in the network deployment.
Networks and security group names are fetched from network deployment using `get_capability` intrinsic function.

### Inputs

* *resource_prefix* - Prefix of every resource created at this deployment on Openstack - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Openstack - default: 0
* *openstack_network_deployment_name* - Name of deployment responsible for router, security group and networks creation -
    default: VNFM-Networking-Prov-Openstack-networks
* *flavor_id* - ID of the flavor in OpenStack - default: 5aaa5054-f7a4-4bbe-8b47-69da2308ecb2
* *image_id* - ID of the image in OpenStack - default: 20acb407-2a20-405e-9e19-360c0a705368
* *vnf_vm_name* - Name of VM - default: fortigate
* *fortigate_license_filename* - Name of the Fortigate license file (It will be uploaded to Fortigate VM with this name). It should have .lic file extension. - default: FGVM02TM19000054.lic

### Installation

Resources created in Prerequesites subsection are fetched using capabilities exposed by *common* deployment and ``VNFM-Fortigate-Prov-Openstack-vm.yaml`` is using it.
To provision FortiGate NGFW Single VM:

``cfy install VNFM-Fortigate-Prov-Openstack-vm.yaml -b VNFM-Fortigate-Prov-Openstack-vm``

### Uninstalling
To delete Fortigate execute:

``cfy uninstall VNFM-Fortigate-Prov-Openstack-vm``

## Configuration

The configuration requires the IP addresses of the VM created during provisioning, therefore the provisioning deployment name
is required as an input. Exposed IP addresses are fetched using *get_capability* function, ie:\
``{ get_capability: [ { get_input: fortigate_vm_deployment_name }, vm_public_ip_address] }``

``VNFM-Fortigate-Conf.yaml`` is responsible for applying base configuration for the newly created FortiGate VM. It configures all of the interfaces.
It consists of one node:
1. *fortigate_vnf_config* - Applies base configuration for Fortigate (VNF name change and basic configuration to interfaces) using [fortigate-baseline.txt](Resources/templates/fortigate-baseline.txt) file.


### Inputs

* *fortigate_vm_deployment_name* - Name of Fortigate Provisioning deployment - default: VNFM-Fortigate-Prov-Openstack-vm

### Install

``cfy install VNFM-Fortigate-Conf.yaml -b VNFM-Fortigate-Conf``

### Uninstall

``cfy uninstall VNFM-Fortigate-Conf``
