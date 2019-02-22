# HTTPD on Openstack

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create the below secrets in the secret store management:
* **httpd_website** - Content of website file for HTTPD VM, it is set during provisioning and served after configuration. Exemplary website can be found under ``Resources/website/index.html``.

You can create those with the following cfy commands:\
``cfy secrets create httpd_website -f <path to a file containing website>``

## Provisioning

VNFM-HTTPD-Prov-Openstack-vm.yaml is responsible for the creation of an Ubuntu VM connected to 2 networks:
* Management,
* LAN.

Network names are fetched from network deployment using `get_capability` intrinsic function.

### Inputs
* *vnf_vm_name* - Name of VM - default: httpd
* *flavor_id* - ID of the flavor in OpenStack - default: 6e2d4276-0390-4a24-b6ab-40f388edcc87
* *image_id* - ID of the image in OpenStack - default: ee6a6582-1351-4f8b-b132-a90b7db88171
* *resource_prefix* - Prefix of every resource created at this deployment on Openstack - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Openstack - default: 0
* *openstack_network_deployment_name* - Name of deployment responsible for router, security group and networks creation -
    default: VNFM-Networking-Prov-Openstack-networks

### Installation

Resources created in Prerequesites subsection are fetched using the capabilities mechanism.
To provision HTTPD:

``cfy install VNFM-HTTPD-Prov-Openstack-vm.yaml -b VNFM-HTTPD-Prov-Openstack-vm``

### Uninstalling

To delete VM execute:

``cfy uninstall VNFM-HTTPD-Prov-Openstack-vm``

## Configuration

VNFM-HTTPD-Conf.yaml is responsible for starting HTTPD process on the target VM,
*web_server* node is responsible for creating such server using the following command:\
``screen -dmS -X python3 -m http.server 8080``\
The IP address of the target VM is fetched from VNFM-HTTPD-Prov-Openstack-vm deployment using capabilities.

### Inputs

* *httpd_vm_deployment_name* - Name of HTTPD Provisioning deployment

### Install

``cfy install VNFM-HTTPD-Conf.yaml -b VNFM-HTTPD-Conf``

### Uninstall

``cfy uninstall VNFM-HTTPD-Conf``
