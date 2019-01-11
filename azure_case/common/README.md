# Common resources

Before installation of bluerints, suitable resource group, networks and security group have to be created.

Install those using VNFM-Networking-Prov-Azure-networks.yaml blueprint:

``cfy install  VNFM-Networking-Prov-Azure-networks.yaml -b  VNFM-Networking-Prov-Azure-networks``

It should be installed only one time before start of provisioning services.
It will be reused automatically by blueprints using capabilities mechanism.
