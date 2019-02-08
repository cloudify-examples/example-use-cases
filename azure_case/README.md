# Network Service - Commercial VNFs

In this examples we demonstrate how to construct a simple network service consisting of a load balancer and a firewall. To make it a tad more interesting we will be deploying a simple web service to allow for complete user experience:

![ns](https://user-images.githubusercontent.com/30900001/52050834-12889e00-2552-11e9-9a68-452e92cc7014.png)

If we break it down to how we would typically build such a service the basic steps would probably be:

1. Provision a firewall and configure it’s network interfaces and the network settings

2. Provision a load balancer and setup basic settings

3. Provision a web server instance, configure it, and setup basic web content

4. Compose the service flow by setting the load balancer to accept traffic on a certain port, direct it to the firewall, configure the firewall to allow web traffic to the web server, etc.

This example contains blueprints implementing each of these steps. These can be easily modified for other VNFs or different infrastructure.

Upon completion of this example we will have a complete running network service.

Note!    The infrastructure used in this example is Microsoft Azure, and the demonstrated VNFs are:
* F5 BIG-IP VE (Load balancer)
* Fortigate (Firewall)
* Httpd (Web Server)

## Cloudify Manager

Before we get started, please make sure you have a Cloudify manager deployed.

The cloudify manager is available in multiple formats ranging from Docker to RPM. In this tutorial we will be using the docker option and assume that it is deployed on your local computer. It can be of course deployed using any other method and on any given platform.

To learn more about Cloudify manager deployment go to: [Cloudify-Getting-Started](https://cloudify.co/download/)

## Example overview

Creation of the whole service consists of the following steps. Each step is available as a blueprint (yaml file) in this example folder.

1. *Environment preparation*
Create networks, a resource group, and a security group. For more details check [common/README](common/README.md)
2. *Provisioning of the VNFs*
Create the virtual machines in Azure and connect those to the proper networks.
Each VNF is created using a blueprint named ``VNFM-<VNF_NAME>-Prov-Azure-vm.yaml``:
* **bigip/VNFM-F5-Prov-Azure-vm.yaml** - [BIG IP Provisioning instruction](bigip/README.md##Provisioning)
* **fortigate/VNFM-Fortigate-Prov-Azure-vm.yaml** - [Fortigate Provisioning instruction](fortigate/README.md##Provisioning)
* **httpd/VNFM-HTTPD-Prov-Azure-vm.yaml** - [HTTPD Provisioning instruction](httpd/README.md##Provisioning)
3. *Configure the VNFs*
Apply basic configuration of the VNFs. This is done using blueprints named ``VNFM-<VNF_NAME>-Conf.yaml``:
* **bigip/VNFM-F5-Conf.yaml** (licensing and VLAN configuration) - [BIG IP Configuration instruction](bigip/README.md##Configuration)
* **fortigate/VNFM-Fortigate-Conf.yaml** (Setting firewall rules and port forwarding) - [Fortigate Configuration instruction](fortigate/README.md##Configuration)
* **httpd/VNFM-HTTPD-Conf.yaml** (Creation of Web Server) - [HTTPD Configuration instruction](httpd/README.md##Configuration)
4. *Service chaining*
The last step creates a service chain of connected network services (Load Balancer, Firewall and Web Server). In this case service chaining consists of port forwarding rule on Fortigate and load balancing rule on BIG IP in order to pass traffic through.
Use the ``service/NS-LB-Firewall-F5-Fortigate-HTTPD.yaml`` - [Service creation instruction](service/README.md)

Once all steps had been performed, you should be able to access the web page displayed by the web service by accessing the ip of the load balancer (This IP will be the output of the service deployment flow, and will be titled web_server).
