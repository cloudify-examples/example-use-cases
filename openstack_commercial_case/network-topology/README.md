# Network Topology

This blueprint installs the common infrastructure for the commercial VNF use case.

The following resources will be created:

* **Management Network** - This network connects the Cloudify manager to all managed components.
* **LAN network** - This network connects the firewall to the web server.
* **WAN network** - This network connects the load balancer to the web server.
* **Public network** - This is the public network accessible to the user. BIG IP exposes the web server on the Public network interface.
* **Security group** - Security group for VNF NICs, defined by *network_security_group_rules* input,
* **Router** - Router, which allows to reach the management and public networks from the external network.

### Installation

**Note: Only install this deployment once.** Other blueprints will reuse the existing deployment.

Upload the blueprint, create the deployment and execute install workflow in one command using the CLI:

```bash
cfy install  infrastructure.yaml -b  \
    VNFM-Networking-Prov-Openstack-networks
```

###Uninstalling

Uninstall the **VNFM-Networking-Prov-Openstack-networks** deployment:

```
cfy uninstall VNFM-Networking-Prov-Openstack-networks
```
