# ansible-beegfs
Ansible playbooks for containerized beegfs deployment

## Configuring parameters
Edit beegfs-cluster.yaml which defines the inventory (i.e. use -i beegfs-cluster.yaml)
Currently two kinds of storage targets are supported: RAID and ZFS.
The RAID mode assumes hardware RAID is already configured and will create an xfs filesystem and automount it to use as a storage target.
The ZFS mode creates a script to create zpools and mount them for usa as storage targets.

## Validation the configuration
### Verify connectivity to each host
```
ansible all -i beegfs-cluster.yaml -m ping
```

### Show host OS facts
```
ansible all -m setup -a "filter=ansible_distribution*" -i beegfs-cluster.yaml
```

## Running the playbooks

### Docker
```
ansible-playbook -i beegfs-cluster.yaml beegfs-docker.yaml 
```

### Beegfs
```
ansible-playbook -i beegfs-cluster.yaml beegfs-storage.yaml 
```
