# HTTPD on Azure

## Prerequisites:

### Common resource creation
Prior to any deployment You have to upload plugins, create secrets and create common environment - [instructions](../common/README.md)

### Secrets

Create the below secrets in the secret store management:
* **httpd_username** - Username for HTTPD VM, it is set during provisioning and used during configuration,
* **httpd_password** - Password for HTTPD VM, it is set during provisioning and used during configuration,
* **httpd_website** - Content of website file for HTTPD VM, it is set during provisioning and served after configuration. Exemplary website can be found under ``Resources/website/index.html``.

You can create those with the following cfy commands:\
``cfy secrets create httpd_username -s <httpd_username>``\
``cfy secrets create httpd_password -s <httpd_password>``\
``cfy secrets create httpd_website -s <httpd_website>``

## Provisioning

VNFM-HTTPD-Prov-Azure-vm.yaml is responsible for the creation of an Ubuntu VM connected to 2 networks:
* Management,
* LAN.

For each network NIC, special security group is created (lan_security_group and mgmt_security_group).
Network names are fetched from network deployment using `get_capability` intrinsic function.

### Inputs
* *image* - Image information - default:   
      publisher: Canonical \
      offer: UbuntuServer \
      sku: 18.04-LTS \
      version: latest
* *size* - Name of Virtual Machine Size in Azure - default: Basic_A0
* *retry_after* - The number of seconds for each task retry interval (in the
          case of iteratively checking the status of an asynchronous operation) - default: 5
* *resource_prefix* - Prefix of every resource created at this deployment on Azure - default: cfy
* *resource_suffix* - Suffix of every resource created at this deployment on Azure - default: 0
* *network_deployment_name* - Name of deployment responsible for creation resource group, security group and networks -
    default: VNFM-Networking-Prov-Azure-networks

### Installation

Resources created in Prerequesites subsection are fetched using the capabilities mechanism.
To provision HTTPD:

``cfy install VNFM-HTTPD-Prov-Azure-vm.yaml -b VNFM-HTTPD-Prov-Azure-vm``

### Uninstalling

To delete VM execute:

``cfy uninstall VNFM-HTTPD-Prov-Azure-vm``

## Configuration

VNFM-HTTPD-Conf.yaml is responsible for starting HTTPD process on the target VM,
*web_server* node is responsible for creating such server using the following command:\
``screen -dmS -X python3 -m http.server 8080``\
The IP address of the target VM is fetched from VNFM-HTTPD-Prov-Azure-vm deployment using capabilities.

### Inputs

* *httpd_vm_deployment_name* - Name of HTTPD Provisioning deployment

### Install

``cfy install VNFM-HTTPD-Conf.yaml -b VNFM-HTTPD-Conf``

### Uninstall

``cfy uninstall VNFM-HTTPD-Conf``
