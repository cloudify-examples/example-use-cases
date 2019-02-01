# Network Service 

## Prerequisites:

* **BIG IP Provisioning & Configuration** - [Instructions](../bigip/README.md)
* **Fortigate Provisioning & Configuration** - [Instructions](../fortigate/README.md)
* **HTTPD Provisioning & Configuration** - [Instructions](../httpd/README.md)

## Service creation

### Inputs

* *f5_prov_deployment_name* - Name of BIG IP Provisioning deployment - default: VNFM-F5-Prov-Azure-vm
* *fg_conf_deployment_name* - Name of Fortigate Configuration deployment - default: VNFM-Fortigate-Conf
* *httpd_prov_deployment_name* - Name of HTTPD Provisioning deployment - default: VNFM-HTTPD-Prov-Azure-vm
* *lb_public_port* - Load balancer public network port on which service is exposed - default: 8080
* *wan_port* - Fortigate WAN port on which service is going to be exposed - default: '8080'

### Installation

``cfy install NS-LB-Firewall-F5-Fortigate-HTTPD.yaml -b NS-LB-Firewall-F5-Fortigate-HTTPD``

### Service validation

After service creation You should be able to display web server using *web_server* deployment output.
