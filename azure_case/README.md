# Commercial VNF Story

This tutorial presents simple network service consisting of a load balancer, a firewall and simple web service to allow for complete user experience.

![ns](https://user-images.githubusercontent.com/30900001/52050834-12889e00-2552-11e9-9a68-452e92cc7014.png)

In this example the whole service is based on Azure IaaS.  
The VNFs are:
* F5 BIG-IP VE (Load balancer)
* Fortigate (Firewall)
* Httpd (Web Server)

Creation of whole service consists of following steps:
1. Environment preparation\
Creation of networks, resource group and security group. 
For details check [common/README](common/README.md)
2. Provisioning of VNFs\
Creation of Virtual Machines on Azure and connecting those to proper networks.\
Each VNF is created by blueprint named VNFM-<VNF_NAME>-Prov-Azure-vm.yaml:
* bigip/VNFM-F5-Prov-Azure-vm.yaml - [BIG IP Provisioning instruction](bigip/README.md##Provisioning)
* fortigate/VNFM-Fortigate-Prov-Azure-vm.yaml - [Fortigate Provisioning instruction](fortigate/README.md##Provisioning)
* httpd/VNFM-HTTPD-Prov-Azure-vm.yaml - [HTTPD Provisioning instruction](httpd/README.md##Provisioning)
3. Configuration of VNFs\
Basic configuration of VNFs.\
VNFs are configured with blueprint named VNFM-<VNF_NAME>-Conf.yaml:
* bigip/VNFM-F5-Conf.yaml (licensing and VLAN configuration) - [BIG IP Configuration instruction](bigip/README.md##Configuration)
* fortigate/VNFM-Fortigate-Conf.yaml (Setting firewall rules and port forwarding) - [Fortigate Configuration instruction](fortigate/README.md##Configuration)
* httpd/VNFM-HTTPD-Conf.yaml (Creation of Web Server) - [HTTPD Configuration instruction](httpd/README.md##Configuration)
4. Service chaining\
The last step creates a service chain of connected network services (Load Balancer, Firewall and Web Server). 
In this case service chaining consists of port forwarding rule on fortigate and load balancing rule on BIG IP in order to pass traffic through.\
Use service/NS-LB-Firewall-F5-Fortigate-HTTPD.yaml - [Service creation instruction](service/README.md)

After all steps You should be able to display web server on public ip of load balancer (web_server output on Service deployment).