# HTTPD Webserver

This blueprint installs HTTPD webserver on an Openstack VM.

### Prerequisites

First make sure that you have satisfied the global requirements in the [main README](../README.md).

* These additional secrets should exist on your manager:
  * `httpd_website`: Content of website file for HTTPD VM, it is set during provisioning and served after configuration. Exemplary website can be found under `Resources/website/index.html`.

## Provisioning

* Blueprint: The `infrastructure.yaml` blueprint is responsible for the creation of an Ubuntu VM. It is connected to 2 networks:
  * Management
  * LAN

The networks' NICs are connected to the security group created in the network deployment. The networks and security group names are fetched from network deployment using `get_capability` intrinsic function.

* Inputs:
  * *vnf_vm_name* - Name of the VM - default: httpd
  * *flavor_id* - ID of the flavor in OpenStack - default: 6e2d4276-0390-4a24-b6ab-40f388edcc87
  * *image_id* - ID of the image in OpenStack - default: ee6a6582-1351-4f8b-b132-a90b7db88171
  * *openstack_network_deployment_name* - Name of deployment responsible for router, security group and networks creation -
      default: VNFM-Networking-Prov-Openstack-networks

### Installation

Upload the blueprint, create the deployment and execute install workflow in one command using the CLI:

```bash
cfy install  infrastructure.yaml -b  \
    VNFM-HTTPD-Prov-Openstack-vm
```

###Uninstalling

Uninstall the **VNFM-HTTPD-Prov-Openstack-vm** deployment:

```
cfy uninstall VNFM-HTTPD-Prov-Openstack-vm
```

## Configuration

* Blueprint: The `application.yaml` blueprint is responsible for starting HTTPD process on the target VM, `web_server` node is responsible for creating such server using the following command: `screen -dmS -X python3 -m http.server 8080`. The IP address of the target VM is fetched from VNFM-HTTPD-Prov-Openstack-vm deployment using capabilities.

* Inputs:
  * `httpd_vm_deployment_name`: Name of HTTPD Provisioning deployment. Default: `VNFM-HTTPD-Prov-Openstack-vm`.

### Install

`cfy install application.yaml -b VNFM-HTTPD-Conf`

### Uninstall

`cfy uninstall VNFM-HTTPD-Conf`
