# Commercial VNF Story

This tutorial presents simple network service consisting of a load balancer and a firewall and simple web service to allow for complete user experience.

![ns](https://user-images.githubusercontent.com/30900001/52050834-12889e00-2552-11e9-9a68-452e92cc7014.png)

In this example the whole service is based on Azure IaaS.  
The VNFs are:
* F5 BIG-IP VE (Load balancer)
* Fortigate (Firewall)
* Httpd (Web Server)

Creation of whole service consists of following steps:
1. Environment preparation\
Creation of networks, resource group and security group. 
For details check [common README](common/README.md)
2. Provisioning of VNFs\
Creation of Virtual Machines on Azure and connecting those to proper networks.\
Each VNF is created by blueprint named VNFM-<VNF_NAME>-Prov-Azure-vm.yaml:
* bigip/VNFM-F5-Prov-Azure-vm.yaml
* fortigate/VNFM-Fortigate-Prov-Azure-vm.yaml
* httpd/VNFM-HTTPD-Prov-Azure-vm.yaml
3. Configuration of VNFs\
Basic configuration of VNFs.\
VNFs are configured with blueprint named VNFM-<VNF_NAME>-Conf.yaml:
* bigip/VNFM-F5-Conf.yaml (licensing and VLAN configuration)
* fortigate/VNFM-Fortigate-Conf.yaml (Setting firewall rules and port forwarding)
* httpd/VNFM-HTTPD-Conf.yaml (Creation of Web Server)
4. Service creation\
The last step is to create a service on top of configured VNFs. 
Use service/NS-LB-Firewall-F5-Fortigate-HTTPD.yaml

After all steps You should be able to display web server on public ip of load balancer (web_server output on Service deployment).