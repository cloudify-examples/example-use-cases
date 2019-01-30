# VNF Network
## Prerequisite
### Network
The management network and the central router needs to be existing.
### Cloudify Manager
A working Cloudify manager and a configured management network and router.
### Inputs
* external_network_name - OpenStack tenant external network name.
* nameservers - IP addresses of DNS nameservers.
* public_subnet_cidr
* public_subnet_application_pool
* private_subnet_cidr
* private_subnet_application_pool
* mgmt_network_id - Existing management network name
* router_id - Existing network router.
### Secrets
* keystone_username
* keystone_password
* keystone_tenant_name
* keystone_url
* keystone_region

## How to run?
* Upload to the Cloudify manager all the required blueprints (see relevant guides):
 * HAProxy
 * httpd
 * PFSense
* Upload service_chaining blueprint.
* Deploy the blueprint with the relevant inputs and secrets.

## How to implement?
* Every VNF has a common provisioning step, loading the existing image on to the node.
So each VNF inherits from a basic node type, which loads an image from OpenStack.
This node type is defined in the 'connected_host' blueprint, so the different VNFs
could be placed in different clouds (due to separate namespaced inputs). But that's
not an issue, this could be changed to a regular yaml import.
* The VNFs network is provisioned in a pre-step to the VNFs creation, so the networks
can be constructed and tested out beforehand.
* To connect each VNF to it's desired network connectivity, there is a need to
connect to the running network deployment, this is achieved by using DeploymentProxy
(which can be used by importing cloudify-utilities plugin). With that, the different
network specifications can be set across the different VNFs, which is exposed via
the DeploymentProxy node in the blueprints (also you can set in that node definition
it will create the network if needed and create deployment chaining).
