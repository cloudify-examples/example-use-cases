# FortiGate NGFW Single VM on Azure

## Prerequesites:

Prior to any deployment installation You have to upload plugins and create secrets.

### Plugins

Upload:
* **cloudify-azure-plugin** - Tested for 2.1.0 version,
* **cloudify-utilities-plugin** - Tested for version 1.12.0

### Secrets

Create below secrets on secrets store management:
* **Azure secrets:**
    * *azure_client_id*
    * *azure_client_secret*
    * *azure_subscription_id*
    * *azure_tenant_id*
    * *azure_location*

## Provisioning

``VNFM-Fortigate-Prov-Azure-vm.yaml`` is responsible for FortiGate NGFW Single VM provisioning. VM is connected to 3 networks:
* Management,
* WAN,
* LAN.

### Prerequesites

Before installation of VNFM-Fortigate-Prov-Azure-vm.yaml, suitable resource group, networks and security group have to be created.
Go to *common* directory and install blueprint as described in readme file.
It should be installed only one time before start of provisioning services.

### FortiGate NGFW Single VM provisioning

Resources created in Prerequesites subsection are fetched using capabilities exposed by *azure-networks* deployment and ``VNFM-Fortigate-Prov-Azure-vm.yaml`` is using it.
To provision FortiGate NGFW Single VM:

``cfy install VNFM-Fortigate-Prov-Azure-vm.yaml -b VNFM-Fortigate-Prov-Azure-vm``

## Configuration

**IMPORTANT**: The version of the image, which is being used for FortiGate VM instantiation, is of BYOL type. That means, before
installing the configuration blueprint, the license file has to be manually applied to the newly provisioned FortiGate VM.

To do it, please use the public IP address of the VM (you can find it in deployment's capabilities) to log into the FortiGate GUI
and follow the instructions to apply the license.

### Install
``VNFM-Fortigate-Conf.yaml`` is responsible for applying the configuration for newly created FortiGate VM. It configures all of the interfaces and prepares NAT rules and policies, which are required to perform the service chain.

``cfy install VNFM-Fortigate-Conf.yaml -b VNFM-Fortigate-Conf``

### Uninstall
During uninstall, all of the NAT rules and policies are being deleted.
