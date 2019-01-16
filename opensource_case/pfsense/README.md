# pfSense
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
* get_secret: region

### How to run?
* Upload to the Cloudify manager the following plugins:
 * OpenStack plugin
 * Cloudify utilities plugin
* Upload (if needed) the basic OpenStack vnf blueprint (generic_provision) with the blueprint id of "connected_host",
 this is needed for importing the blueprint in the different components.
* Upload the pfSense blueprint.
* Deploy the blueprint with the relevant inputs and secrets.

## Configuring
Update the pfSense configuration file on a running pfSense server using a
REST call to FauxAPI (a REST api for pfSense).
### Prerequisite
#### Inputs
* FauxAPI REST endpoint
* FauxAPI api_key
* FauxAPI api_secret

### How to run?
* Upload Cloudify Utilities plugin to the Cloudify Manager. The plugin provides a generic type in a blueprint in order to integrate with REST based systems. Get it here https://cloudify.co/plugins.
* Create a .json file that contains the pfSense configuration changes and store it under the blueprint’s resources.. Please note that the this blueprint uses the `patch` command - it will only update the existing configuration and not replace it).
* The blueprint directory should look like this:
<blueprint_root_dir>/
	pfsense-configuration.yaml
	scripts/
		create_faux_auth_token.sh
	resources/
		config.json
templates/
		config_patch_template.yaml
* The config_patch_template.yaml contains a template of the REST call that will be performed, it can be modified.
* Upload the blueprint to the Manager.
* Create a deployment and provide the following inputs: rest_endpoint, api_key, api_secret.
* Install the deployment.
