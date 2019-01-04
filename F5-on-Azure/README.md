# F5 Big-IP on Azure


## Provisioning 

VNFM-F5-Prov-Azure-vm.yaml is responsible for creation BIG-IP Virtual Machine connected to 3 networks:
* Management,
* WAN,
* Public.

### Prerequesites

Before installation of VNFM-F5-Prov-Azure-vm.yaml, suitable resource group, networks and security group have to be created.
Install those using networks.yaml blueprint:

``cfy install networks.yaml -b base``

### BIG-IP provisioning

Resources created in Prerequesites subsection are fetched using existing_networks.yaml blueprint and VNFM-F5-Prov-Azure-vm.yaml is using it.
To provision BIG-IP:

``cfy install VNFM-F5-Prov-Azure-vm.yaml -b VNFM-F5-Prov-Azure``
