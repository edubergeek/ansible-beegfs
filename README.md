# ansible-beegfs
Ansible playbooks for containerized beegfs deployment

## Configuring parameters
Edit beegfs-cluster.yaml which defines the inventory (i.e. use -i beegfs-cluster.yaml)
Currently two kinds of storage targets are supported: RAID and ZFS.
The RAID mode assumes hardware RAID is already configured and will create an xfs filesystem and automount it to use as a storage target.
The ZFS mode creates a script to create zpools and mount them for usa as storage targets.

There are two kinds of meta targets: MIRROR and SYNC.
The MIRROR mode assumes two drives are available to configure as a ZFS raidz mirror while the SYNC mode is for a single metadata disk that relies on a cron job to sync the metadata content to a different host as a backup.

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
To bring up the beegfs storage cluster, first edit the group_vars/beegfs as needed.
Next, edit the beegfs-cluster.yaml file to configure your nodes for various node specific values mostly controlling logical drives and physical partitions.
Finally, run the plays below in the order given.

### Prepare each node
```
ansible-playbook -i beegfs-cluster.yaml beegfs-prep.yaml 
```

### Prepare the management node
```
ansible-playbook -i beegfs-cluster.yaml beegfs-mgmt.yaml 
```

### Prepare each meta node
```
ansible-playbook -i beegfs-cluster.yaml beegfs-meta.yaml 
```

### Prepare each storage node
```
ansible-playbook -i beegfs-cluster.yaml beegfs-storage.yaml 
```

### Start Docker Containers
```
ansible-playbook -i beegfs-cluster.yaml beegfs-up.yaml 
```
### Stop Docker Containers
Be sure to stop all beegfs-clients before bringing down the core services.
```
ansible-playbook -i beegfs-cluster.yaml beegfs-down.yaml 
```

