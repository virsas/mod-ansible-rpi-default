# rpi-default

## Role installation

### create requirements file

``` bash
cd ANSIBLE_FOLDER
mkdir requirements
touch requirements/pi_default.yml
```

### file content

``` yaml
---

# ansible-galaxy install -p vss_galaxy_roles --force -r requirements/pi_default.yml
- src: "https://github.com/virsas/mod-ansible-rpi-default"
  scm: git
  version: v1.0.0
  name: rpi-default
  path: vss_galaxy_roles
```

## Install role

### Installation

``` bash
ansible-galaxy install -p galaxy_roles --force -r requirements/pi_default.yml
```

### Best practices

If you are using git for your playbooks and sites configuration, add vss_galaxy_roles to your .gitignore file. You are free to download the role directly to your roles directory, but it will be then pushed to your repo too.

## Role access

### Direct access

``` bash
$ cd vss_galaxy_roles
$ ls
rpi-default
$ cd rpi-default
$ ls -1
defaults
handlers
LICENSE
meta
README.md
tasks
templates
$
```

### Access from ansible (ansible.cfg)

``` bash
[defaults]
gathering = smart
roles_path=./roles:../roles:../../roles:./vss_galaxy_roles
log_path=./ansible_run.log
retry_files_enabled=False
[ssh_connection]
scp_if_ssh=True
host_key_checking=False
```

## Files

### Playbook (./playbooks/pi_all.yml)

``` yaml
---

- hosts: pi_all
  remote_user: pi
  become: yes
  roles:
    - rpi-default
```

### Inventory (./sites/NAME/inventory)

``` txt
[pi_all]
pi_media_01
```

### Host vars (./sites/NAME/host_vars/pi_media_01.yml)

``` yaml
---

ansible_ssh_host: 1.1.1.1
```

### Group vars (./sites/NAME/group_vars/pi_all.yml)

Please see the variables below to get the complete list of possible modifications. The YAML file below is just a basic example of a minimal configuration.

``` yaml
TIMEZONE: Europe/Prague
```

## Usage

``` bash
ansible-playbook -i sites/NAME/inventory playbooks/pi_all.yml --diff
```

## Variables

``` yml

TIMEZONE: Europe/Prague
LOCALE: en_US.UTF-8
LC_ALL: en_US.utf8

MAILNAME: MTA
MYNETWORKS: 127.0.0.1/32
MYORIGIN: localhost
```
