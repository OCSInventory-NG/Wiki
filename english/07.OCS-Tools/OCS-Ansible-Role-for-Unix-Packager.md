## Summary

- [Summary](#summary)
- [Description](#description)
- [Current compatibility](#current-compatibility)
- [Configure Playbooks](#configure-playbooks)
- [Run Playbooks](#run-playbooks)
    - [Deploy OCS Agent package on Discovery Machines](#deploy-ocs-agent-package-on-discovery-machines)
    - [Run IP Discovery](#run-ip-discovery)
    - [Run SSH Discovery](#run-ssh-discovery)
    - [Run RDP Discovery](#run-rdp-discovery)
    - [Deploy OCS Agent package on all SSH discovered servers](#deploy-ocs-agent-package-on-all-ssh-discovered-servers)
    - [Run OCS Agent command on all discovered servers](#run-ocs-agent-command-on-all-discovered-servers)
- [Encrypted Passwords File](#encrypted-passwords-file)

## Description

This package aim to deploy the Packager for unix generated archive to system depending on the target OS.

The ansible can deploy the OCS Agent as a "one shot" and only inventory once but you can also change configuration to create crontab and automate the inventory process

## Current compatibility
- Debian
- Ubuntu
- Redhat

## Configure Playbooks

Inventory file is "inventory". You can group servers and give them a name. The name of the group should be then enter in the playbook files (the yml files).

group_vars directory contains directoies that depicts the configuration you can encounter during deployment:

* debian_hosts/config.yml: connect with a standard user then become root with "su" command by providing root password
* redhat_hosts/config.yml: connect with root user and root password
* ubuntu_hosts/config.yml: connect with standard user then become root with "sudo" command by providing standard user password

If the case you need to support more operating system you can edit the roles/ocs_deploy/vars

You can mix and adapt configuration to fit your needs

## Run Playbooks

### Deploy OCS Agent package on Discovery Machines

1. Identify a server per subnet
2. Enter the identified IP addresses in nmap_hosts section of ocs.inventory file
3. Uplaod the OCS Agent packages built thanks to the Unix packager, onto the server you will run the Ansible playbook from
4. Set the OCS Agent packages directory into ocs_deploy.yml (ocs_agent_package_dir variable)
5. Set "nmap_hosts" in "hosts" parameter in ocs_deploy.yml
6. Run the playbook:

```shell
ansible-playbook ocs_deploy.yml -i ocs.inventory
```

### Run IP Discovery

For each discovering server:

1. Enter discovering server IP address in namp.inventory file
2. Enter the network IP and netmask on nmap_run.yml
3. Run the playbook:

```shell
ansible-playbook nmap_ipdiscovery.yml -i nmap.inventory
```

The output is stored into nmap_results directory. Output file name is tagged witht he network adress and netmask: nmap_ipdiscovery_network_netmask.output

### Run SSH Discovery

This role will identify the SSH listenning servers (to install and run OCS Agent with Ansible on Linux servers)

For each dicovering server:

1. Enter discovering server IP address in namp.inventory file
2. Enter the network IP, netmask and SSH port in nmap_sshdiscovery.yml
3. Run the playbook:

```shell
ansible-playbook nmap_sshdiscovery.yml -i nmap.inventory
```

The output is stored into nmap_results directory. Output file name is tagged witht he network adress and netmask: nmap_sshdiscovery_network_netmask.output


### Run RDP Discovery

This role will identify the RDP listenning servers (to install and run OCS Agent the OCS deployer tool for Windows)

For each dicovering server:

1. Enter discovering server IP address in namp.inventory file
2. Enter the network IP, netmask and RDP prt in nmap_rdpdiscovery.yml
3. Run the playbook:

```shell
ansible-playbook nmap_rdpdiscovery.yml -i nmap.inventory
```

The output is stored into nmap_results directory. Output file name is tagged witht he network adress and netmask: nmap_rdpdiscovery_network_netmask.output

### Deploy OCS Agent package on all SSH discovered servers

1. Fill in the inventory file with the SSH discovered list and group the servers to fit your needs.
2. Run the playbook

```shell
ansible-playbook ocs_deploy.yml -i ocs.inventory
```

### Run OCS Agent command on all discovered servers

1. Set the OCS server port and protocol in ocs_run.yml
2. Run the playbook

```shell
ansible-playbook ocs_run.yml -i ocs.inventory
```

## Encrypted Passwords File

Generate encrypted file with ansible-vault

```shell
ansible-vault encrypt password.yml
[enter pasword]
[confirm password]
```

Run a playbook with vault password asked

```shell
ansible-playbook playbook.yml --ask-vault-pass
[enter pasword]
```

Run a playbook with vault password in text file

```shell
ansible-playbook playbook.yml --vault-password-file ~/.vault_pass.txt
```

Decrypt vaulted file

```shell
ansible-vault decrypt password.yml
[enter pasword]
```
