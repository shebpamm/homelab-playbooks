# 📚 Homelab Playbooks
### Collection of Ansible Playbooks used with my homelab.

## 📕 Playbooks
Two main playbooks are defined:
### hostname-rename
This playbook renames all hosts according to a defined scheme in `/etc/names`. It also renames ESXi machines along the way.

A writeup of this can be found [here](https://serverspike.io/applying-a-naming-scheme-to-all-hosts-using-ansible/).

### vm-deploy
This playbook deploys a new ESXi VM with kickstart for a given ansible host.

Check [this post](https://serverspike.io/automating-vm-creation-with-kickstart-and-ansible-without-using-a-pxe-server/) for more info.

## 🏷️ Roles
### Common
- defines variable `scheme_name`

### Virtual
- depends on common
- defines vsphere credentials from env

## 📜 Templates
Jinja2 templates used by the plays.

### hosts.j2
- `/etc/hosts` template
- all hosts in play are added as hosts with `scheme_name`

### kickstart.cfg.j2
- kickstart template for `vm-deploy`
- should be customized for your needs
