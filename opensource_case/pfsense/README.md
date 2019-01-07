# pfSense
## Prerequisite
### Network
If you don't have the networks or wish to deploy all the networks required for this
example automatically please deploy the network topolgy blueprints.
### Inputs
* network_deployment_name - Deployment name of the VNF network
* manager_network - Management network name.
* private_key_path - The content of the agent's private key.
* agent_user - The username of the agent running on the instance created from the image.
* image_name - An Openstack Image ID.
* flavor - An Openstack Flavor ID.
* key_pair_name - Openstack key pair name.
### Secrets
* keystone_username
* keystone_password
* keystone_tenant_name
* keystone_url
* get_secret: region

## How to run?
* Upload to the Cloudify manager the following plugins:
** OpenStack plugin
* Upload the basic openstack vm blueprint (generic_provision) and PFSense blueprint.
* Deploy the blueprint with the relevant inputs.
