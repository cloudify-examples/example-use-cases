# FortiGate Firewall

This blueprint installs the NGFW Single VM on Openstack.

### Prerequisites

First make sure that you have satisfied the global requirements in the [main README](../README.md).

* These additional secrets should exist on your manager:
  * `fortigate_license`: Content of license file, its used during provisioning to license Fortigate. You can set this up via the CLI: `cfy secrets create fortigate_license -s [secret value]`.

## Provisioning

* Blueprint: The `infrastructure.yaml` blueprint is responsible for FortiGate NGFW Single VM provisioning. This VM is connected to 3 networks:
  * Management
  * WAN
  * LAN

The networks' NICs are connected to the security group created in the network deployment. The networks and security group names are fetched from network deployment using `get_capability` intrinsic function.

* Inputs:
  * *openstack_network_deployment_name* - Name of deployment responsible for router, security group and networks creation -
      default: VNFM-Networking-Prov-Openstack-networks
  * *flavor_id* - ID of the flavor in OpenStack - default: 5aaa5054-f7a4-4bbe-8b47-69da2308ecb2
  * *image_id* - ID of the image in OpenStack - default: 20acb407-2a20-405e-9e19-360c0a705368
  * *vnf_vm_name* - Name of VM - default: fortigate
  * *fortigate_license_filename* - Name of the Fortigate license file (It will be uploaded to Fortigate VM with this name). It should have .lic file extension. - default: FGVM02TM19000054.lic

### Installation

Upload the blueprint, create the deployment and execute install workflow in one command using the CLI:

```bash
cfy install  infrastructure.yaml -b  \
    VNFM-Fortigate-Prov-Openstack-vm
```

###Uninstalling

Uninstall the **VNFM-Fortigate-Prov-Openstack-vm** deployment:

```
cfy uninstall VNFM-Fortigate-Prov-Openstack-vm
```

## Configuration

The configuration requires the IP addresses of the VM created during provisioning, therefore the provisioning deployment name is required as an input. Exposed IP addresses are fetched using `get_capability` function: `{ get_capability: [ {get_input: prov_deployment_name}, wan_ip ] }`.

* Blueprint: The `application.yaml` blueprint is responsible for applying base configuration for the newly created FortiGate VM. It configures all of the interfaces. It consists of one node:
  * `fortigate_vnf_config`: Applies base configuration for Fortigate (VNF name change and basic configuration to interfaces) using [fortigate-baseline.txt](Resources/templates/fortigate-baseline.txt) file.

* Inputs:
  * `fortigate_vm_deployment_name`: Name of Fortigate Provisioning deployment. Default: `VNFM-Fortigate-Prov-Openstack-vm`.

### Install

`cfy install application.yaml -b VNFM-Fortigate-Conf`

### Uninstall

`cfy uninstall VNFM-Fortigate-Conf`
