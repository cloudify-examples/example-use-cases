# HTTPD on Azure

## Prerequesites:

Prior to any deployment installation You have to upload plugins and create secrets.

### Plugins 

Upload:
* **cloudify-azure-plugin** - Tested for 2.1.0 version,
* **cloudify-utilities-plugin** - Tested for version 1.12.0

### Secrets

Create below secrets on secrets store management:
* **HTTPD secrets:**
    * *rsa_key_private* - RSA private key from a keypair used to log in to HTTPD hosting VM
    * *rsa_key_public* - RSA public key from a keypair used to log in to HTTPD hosting VM
* **Azure secrets:**
    * *azure_client_id*
    * *azure_client_secret*
    * *azure_subscription_id*
    * *azure_tenant_id*



## Provisioning 

VNFM-HTTPD-Prov-Azure-vm.yaml is responsible for creation BIG-IP Virtual Machine connected to 2 networks:
* Management,
* LAN,

### Prerequesites

Before installation of VNFM-HTTPD-Prov-Azure-vm.yaml, suitable resource group, networks and security group have to be created.
Install those using networks.yaml blueprint:

``cfy install  VNFM-Networking-Prov-Azure-networks.yaml -b  VNFM-Networking-Prov-Azure-networks``

### HTTPD provisioning

Resources created in Prerequesites subsection are fetched using capabilities mechnism.
To provision HTTPD:

``cfy install VNFM-HTTPD-Prov-Azure-vm.yaml -b VNFM-HTTPD-Prov-Azure``


## Configuration

### Install
VNFM-HTTPD-Conf.yaml is responsible for starting HTTPD process on the target VM
The IP address of the target VM is fetched form VNFM-HTTPD-Prov-Azure-vm deployment using capabilities

``cfy install VNFM-HTTPD-Conf.yaml -b VNFM-HTTPD-Conf``

### Uninstall
During uninstall the HTTPD service is stopped resources are reclaimed.
