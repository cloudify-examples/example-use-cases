# End-to-End Network Service - Commercial VNFs

This Blueprint describes the entire network service, by embedding all sub-blueprints describing the different components, and executing the provisioning, configurations, and chaining steps in the proper order.

For more information about particular steps, please go to the [openstack_commercial_case/README.md](https://github.com/cloudify-examples/example-use-cases/blob/master/openstack_commercial_case/README.md)

## Prerequisites:

Prior to installation please upload the below plugins and create the mentioned secrets.

### Plugins

Upload:
* **cloudify-openstack-plugin** - Tested for version 2.14.7
* **cloudify-utilities-plugin** - Tested for version 1.12.5

This can be applied through the Cloudify manager user interface or using the CLI.
* To upload plugins using the Cloudify manager:
    * Browse to the *Cloudify Catalog* page and scroll to the *Plugins Catalog* widget. Select the relevant plugins and click *Upload*.
* To upload plugins using the CLI, run the following commands:
``cfy plugins upload https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.12.5/cloudify_utilities_plugin-1.12.5-py27-none-linux_x86_64-centos-Core.wgn -y https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.12.5/plugin.yaml``

``cfy plugins upload https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases/download/2.14.7/cloudify_openstack_plugin-2.14.7-py27-none-linux_x86_64-centos-Core.wgn -y https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases/download/2.14.7/plugin.yaml``

### Secrets

Create the below secrets in the secret store management:
* **Openstack secrets:**
    * *keystone_username* - Keystone username
    * *keystone_password* - Keystone password
    * *keystone_tenant_name* - Keystone tenant name
    * *keystone_url* - Keystone URL
    * *keystone_region* - Keystone region
    * *bigip_license_key* - License key for BIG IP VE, it is being applied during configuration
    * *fortigate_license* - Content of license file, its used during provisioning to license Fortigate
    * *httpd_website* - Content of website file for HTTPD VM, it is set during provisioning and served after configuration

You can create those with the following cfy commands:\
``cfy secrets create keystone_username -s <keystone_username>``\
``cfy secrets create keystone_password -s <keystone_password>``\
``cfy secrets create keystone_tenant_name -s <keystone_tenant_name>``\
``cfy secrets create keystone_url -s <keystone_url>``\
``cfy secrets create keystone_region -s <keystone_region>``\
``cfy secrets create bigip_license_key -s <bigip_license_key>``\
``cfy secrets create fortigate_license -f <path to a fortigate license>``\
``cfy secrets create httpd_website -f <path to a httpd website>``

### Inputs

* *common_prov_name* - The name of the Common resources provisioning deployment - default: VNFM-Networking-Prov-Openstack-networks
* *f5_prov_name* - The name of the BIG IP Provisioning deployment - default: VNFM-F5-Prov-Openstack-vm
* *f5_conf_name* - The name of the BIG IP Configuration deployment - default: VNFM-F5-Conf
* *fg_prov_name* - The name of the Fortigate Provisioning deployment - default: VNFM-Fortigate-Prov-Openstack-vm
* *fg_conf_name* - The name of the Fortigate Configuration deployment - default: VNFM-Fortigate-Conf
* *httpd_prov_name* - The name of the HTTPD Provisioning deployment - default: VNFM-HTTPD-Prov-Openstack-vm
* *httpd_conf_name* - The name of the HTTPD Configuration deployment - default: VNFM-HTTPD-Conf
* *service_prov_name* - The name of the Common resources provisioning deployment - default: NS-LB-Firewall-F5-Fortigate-HTTPD


### Installation

To apply the service configuration execute:

``cfy install VNFM-E2E-F5-Fortigate-HTTPD.yaml -b VNFM-E2E-F5-Fortigate-HTTPD``

### Service validation

Once all steps had been performed, you should be able to access the web page displayed by the web service by accessing the ip of the load balancer (This IP will be the output of the service deployment flow, and will be titled *web_server*).

### Uninstalling

To tear down the service configuration execute:

``cfy uninstall VNFM-E2E-F5-Fortigate-HTTPD``
