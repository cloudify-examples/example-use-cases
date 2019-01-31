# HTTPd
## Prerequisite
### Network
If you don't have the networks or wish to deploy all the networks required for this
example automatically please deploy the network topolgy blueprints.
### Cloudify Manager
A working Cloudify manager and a configured management network and router.
### Inputs
* network_deployment_name - Deployment name of the VNF network
* image_name - An OpenStack Image ID.
* flavor - An OpenStack Flavor ID.
* private_key_name - OpenStack key pair name.
### Secrets
* keystone_username
* keystone_password
* keystone_tenant_name
* keystone_url
* keystone_region

## How to run?
* Upload to the Cloudify manager the following plugins:
 * OpenStack plugin.
 * Cloudify utilities plugin.
* Upload (if needed) the basic OpenStack vnf blueprint (generic_provision) with the blueprint id of "connected_host",
this is needed for importing the blueprint in the different components.
* Upload the httpd blueprint.
* Deploy the blueprint with the relevant inputs and secrets.
