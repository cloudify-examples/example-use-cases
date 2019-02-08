# Network Service

Creates a service chain by creating forwarding rules on the VNFs (Fortigate and BIG IP).

## Prerequisites:

* **BIG IP Provisioning & Configuration** - [Instructions](../bigip/README.md)
* **Fortigate Provisioning & Configuration** - [Instructions](../fortigate/README.md)
* **HTTPD Provisioning & Configuration** - [Instructions](../httpd/README.md)

## Service creation

NS-LB-Firewall-F5-Fortigate-HTTPD.yaml consists of 2 nodes:
1. *fg_port_forwarding* - prepares NAT rules and policies, which are required to perform the service chain. [fortigate-portforward-start.txt](Resources/templates/fortigate-portforward-start.txt) file is used to apply configuration during installation and [fortigate-portforward-stop.txt](Resources/templates/fortigate-portforward-stop.txt) to delete it during uninstall.
2. *ltm_config* - creates load balancing rule responsible for passing traffic from app (exposed on WAN fortigate interface)
to BIG-IP Public interface using [ltm_config.txt](Resources/templates/ltm_config.txt) file to apply configuration and [ltm_config_stop.txt](Resources/templates/ltm_config_stop.txt) to delete it during uninstall

IP addresses are fetched using *get_capability* function.

### Inputs

* *f5_prov_deployment_name* - The name of the BIG IP Provisioning deployment, used to get management and Public IPs from BIG IP VE - default: VNFM-F5-Prov-Azure-vm
* *fg_prov_deployment_name* - The name of the Fortigate Provisioning deployment, used to get management and WAN IPs from Fortigate VM - default: VNFM-Fortigate-Conf
* *httpd_prov_deployment_name* - The name of the HTTPD Provisioning deployment, used to fetch HTTPD LAN interface IP - default: VNFM-HTTPD-Prov-Azure-vm
* *lb_public_port* - Load balancer public network port on which the service is exposed - default: 8080
* *wan_port* - Fortigate WAN port on which the service is going to be exposed - default: '8080'

### Installation

To apply service configuration execute:

``cfy install NS-LB-Firewall-F5-Fortigate-HTTPD.yaml -b NS-LB-Firewall-F5-Fortigate-HTTPD``

### Service validation

After service creation You should be able to display web server exposed on Public interface of BIG-IP.
The URL is available on *web_server* deployment output.

### Uninstalling

To tear down service configuration execute:

``cfy uninstall NS-LB-Firewall-F5-Fortigate-HTTP``
