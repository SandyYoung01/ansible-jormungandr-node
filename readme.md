# Cardano Jormungandr Node

!!! THIS PROJECT IS WORK IN PROGRESS !!!

[Ansible](https://duckduckgo.com/?q=ansible) installation and configuration of cardano jormungandr node.

## Getting started

#### Requirements

Both the host running jormungandr and your localhost are required to run [debian or a derivative](https://www.debian.org/derivatives/) of it (e.g. Ubuntu, MX Linux).

#### Step 1
Push your ssh public key to the remote node for passwordless authentication.
```shell
ssh-copy-id root@REMOTE_IP
```

#### Step 2
Install `ansible` and `python` on both your local machine and the jormungandr host.
```shell
apt-get update && apt-get install -y ansible python
```

#### Step 3
Clone this project into a directory on your local host.
```shell
git clone https://github.com/SandyYoung01/ansible-jormungandr-node.git ansible-jormungandr-node
```

#### Step 4
Create configuration file.
```shell
cd ansible-jormungandr-node/group_vars
touch production.yaml
vim production.yaml
```

Paste following content into the previously created `production.yaml` and change variables as needed.
```yaml
---
common_timezone:              UTC

common_swap_file:             /swapfile
common_swap_size:             5000 # in megabytes

common_ssmtp_mailhub:         SMTP_MAILHUB
common_ssmtp_auth_user:       SMTP_USER
common_ssmtp_auth_pass:       SMTP_PASSWORD
common_ssmtp_tls:             YES
common_ssmtp_rewrite_domain:  ~

jormungandr_node_user:                  jormungandr
jormungandr_node_group:                 jormungandr

jormungandr_node_dir_home:              /home/jormungandr
jormungandr_node_dir_base:              /home/jormungandr/node
jormungandr_node_dir_repo:              /home/jormungandr/node/jormungandr-repo
jormungandr_node_dir_storage:           /home/jormungandr/node/storage
jormungandr_node_dir_certificates:      /home/jormungandr/node/certificates
jormungandr_node_dir_logs:              /home/jormungandr/node/logs
jormungandr_node_dir_scripts:           /home/jormungandr/node/scripts

jormungandr_node_version:               v0.8.0-rc9                                                       # see https://github.com/input-output-hk/jormungandr/releases
jormungandr_node_rest_port:             3100
jormungandr_node_public_ip:             ~                                                                # Define which public IP address to set
jormungandr_node_public_ip_auto_get:    1                                                                # Tries to fetch public IP automatically if set to 1
jormungandr_node_public_port:           3000
jormungandr_node_genesis_block_hash:    20dbeb9e3ed3063efa7391f55ea23e0638b5cb8ca4f6260e724b6c0c87b1e1ee # see https://hydra.iohk.io/job/Cardano/jormungandr/jormungandrConfigs.nightly/latest
```

#### Step 5
Create configuration inventory file.
```shell
cd ansible-jormungandr-node/inventories
touch production.yaml
vim production.yaml
```

Paste following content into the previously created `production.yaml` and change as needed.
```yaml
---
production:
  children:
    nodes:
      hosts:
        jormungandr-node.example.com:
          ansible_host:          IP_ADDRESS_OF_REMOTE_HOST    # ipv4 address
          jormungandr_public_id: RANDOM_UNIQUE_ID             # use `openssl rand -hex 24` to generate
```
#### Step 6
Deploy jormungandr node (or a thousand nodes) with just one command! Hurray!
```shell
ansible-playbook -i inventories/production.yaml playbook.yaml
```