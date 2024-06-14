# OCP Disconnected Setup Resources Playbook
A simple playbook written to automate the process of preparing necessary resources for an OCP UPI disconnected environment installation.

# Variables
Modify the following files according to your needs:
- `inventory`
- `vars.yaml`

## Define hosts (inventory)
- For NTP: Define NTP server **IP adress** or **FQDN**.
- For all other fields: Define **ONLY THE HOSTNAME** (not FQDN) of the nodes.
- If mirror registry is hosted in bastion, leave the **mirror_reg** field as default value.

## Define variables (vars.yaml)
### Directories
- `dir_files`: Directory where all the files will be generated to

### OCP configs
- `ocp_version`: Full OCP version you're going for (e.g. 4.14.17)
- `cluster_name`: Cluster name
- `base_domain`: Cluster base domain (e.g. example.com)
- `cluster_network_cidr`: Cluster network CIDR (default: 10.128.0.0/14)
- `cluster_network_hostprefix`: Cluster network host prefix (default: 23)
- `network_type`: Cluster network type (default: OVNKubernetes)
- `service_network_cidr`: Service network CIDR (defaut: 172.30.0.0/16)
- `platform`: none (values: none/vsphere)

### Keepalived configs
- `network_inf`: Network interface device name (default: ens192 for VMWare)
- `master_lb`: Master LB IP address
- `backup_lb`: Backup LB IP address
- `vip_lb`: LB VIP address

### vSphere configs (ignore if platform: none)
- `vcenter_server`: your.vcenter.server
- `vcenter_user`: username
- `vcentre_password`: password
- `vcentre_datacenter`: datacenter
- `defaultDatastore`: datastore
- `diskType`: thin

### Contents to mirror configs
- `platform_images`: Indicate any platform images to mirror (default: y, values: y/n)
- `redhat_operators`: Indicate any Red Hat operators to mirror (default: y, values: y/n)
- `certified_operators`: Indicate any certified operators to mirror (default: y, values: y/n)
- `community_operators`: Indicate any community operators to mirror (default: y, values: y/n)
- `additional_images`: Indicate any additional images to mirror (default: y, values: y/n) 

### Operators List configs
- `redhat_operators_list`: List of Red Hat operators to mirror
- `certified_operators_list`: List of certified operators to mirror
- `community_operators_list`: List of community operators to mirror
- `additional_images_list`: List of additional images to mirror

## Running the playbook
Install pre-requisites.
```
$ sudo dnf install ansible-core
$ ansible-galaxy collection install community.general
```

To run the playbook.
```
$ ansible-playbook -i inventory OCP-disconnected-setup-resources.yaml
```

The end result should be a generated directory in the specified `dir_files` directory.  
You can then bring this folder into the disconnected environment, and proceed with the deployment.
```
$ tree

├── binaries
│   ├── butane
│   ├── kubectl
│   ├── oc
│   ├── oc-mirror
│   └── openshift-install
├── configs
│   ├── 99-master-chrony-conf.bu
│   ├── 99-worker-chrony-conf.bu
│   ├── config.json
│   ├── haproxy.cfg
│   ├── keepalived.conf_BACKUP
│   ├── keepalived.conf_MASTER
│   ├── local.repo
│   └── mcp-infra.yaml
├── installer
│   └── install-config.yaml
└── mirror-registry
    ├── execution-environment.tar
    ├── image-archive.tar
    ├── imageset-config.yaml
    └── mirror-registry
```