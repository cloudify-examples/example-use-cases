# VNF network
## Prerequisite
### Network
The management network and the central router needs to be exisiting.
### Inputs
* external_network_name - Openstack tenant external network name.
* nameservers - IP addresses of DNS nameservers.
* public_subnet_cidr
* public_subnet_application_pool
* private_subnet_cidr
* private_subnet_application_pool
* mgmt_network_id - Exisiting management network name
* router_id - Exisiting network router.
### Secrets
* keystone_username
* keystone_password
* keystone_tenant_name
* keystone_url
* keystone_region

## How to run?
* Upload to the Cloudify manager the following plugins:
** OpenStack plugin
* Upload network topolgy blueprint.
* Deploy the blueprint with the relevant inputs.
