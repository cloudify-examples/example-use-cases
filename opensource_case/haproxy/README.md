# HAProxy
## Provisioning
### Prerequisite
#### Network
If you don't have the networks or wish to deploy all the networks required for this
example automatically please deploy the network topolgy blueprints.
#### Cloudify manager
A working Cloudify manager and a configured management network and router.
#### Inputs
* network_deployment_name - Deployment name of the VNF network
* manager_network - Management network name.
* private_key_path - The content of the agent's private key.
* agent_user - The username of the agent running on the instance created from the image.
* image_name - An OpenStack Image ID.
* flavor - An OpenStack Flavor ID.
* key_pair_name - OpenStack key pair name.
#### Secrets
* keystone_username
* keystone_password
* keystone_tenant_name
* keystone_url
* keystone_region

### How to run?
* Upload to the Cloudify manager the following plugins:
 * OpenStack plugin
 * Cloudify utilities plugin
* Upload (if needed) the basic OpenStack vnf blueprint (generic_provision) with the blueprint id of "connected_host",
 this is needed for importing the blueprint in the different components.
* Upload the haproxy blueprint.
* Deploy the blueprint with the relevant inputs and secrets.

## Configuring
### Prerequisite
Upload a new HAProxy configuration file to a running HAProxy server, and restart the service.

#### Inputs
* host - The host name of the HAProxy server
* user - Username used when SHH-ing into the already running HAProxy server
* key_filename - Path to a private key that resides on the Cloudify Manager machine.

### How to run?
* Upload Fabric plugin to the Cloudify Manager. The plugin provides an agent-less method for running operations on destination hosts, in this case on the HAProxy server. Get it here https://cloudify.co/plugins.
* Create a modified HAProxy configuration file (`haproxy.cnf`) and make sure it is stored in the blueprint`s directory under `/resources`. Please note that the current configuration file will be replaced by the new one (and not updated).
The blueprint directory should look like this:
<blueprint_root_dir>/
	configure-haproxy-blueprint.yaml
	scripts/
		haproxy.py
	resources/
		haproxy.cfg
* Upload the blueprint to the Manager.
* Create a deployment and provide the following inputs: host_string, user, key_filename.
* Install the deployment.
