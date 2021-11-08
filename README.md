# 📚 Homelab Playbooks
### Collection of Ansible Playbooks used with my homelab.

## 📕 Playbooks
Five main playbooks are defined:
### hostname-rename
This playbook renames all hosts according to a defined scheme in `/etc/names`. It also renames ESXi machines along the way.

A writeup of this can be found [here](https://serverspike.io/applying-a-naming-scheme-to-all-hosts-using-ansible/).

### vm-deploy
This playbook deploys a new ESXi VM with kickstart for a given ansible host.
It also sets a Static DHCP mapping for pfSense!

Check [this post](https://serverspike.io/automating-vm-creation-with-kickstart-and-ansible-without-using-a-pxe-server/) for more info.

### cockpit-deploy
cockpit-deploy installs Redhat Cockpit and fetches a wildcard ssl certificate from a host defined in the inventory, in my case a pfSense box.

### create-ansible-user
I wanted to make a new user for ansible to better guard privileged access. This playbook creates a new user on the host called `ansible` and sets up sudo permission and ssh authentication for it.

### join-ad-linux
This playbook automates the joining of my local Active Directory domain for RHEL and Ubuntu machines. It also enables login for AD users and sets up ssh pubkeys from AD.

## 🏷️ Roles
### Common
- defines variable `scheme_name`
- checks the hosts OS Family and runs the OS specific role if found

### Virtual
- depends on common
- defines vsphere credentials from env

### Active Directory
- defines vault protected admin credentials for AD

### Distro Specific (Ubuntu & RHEL)
- Define OS specific variables, such as location of binaries
- if Active Directory role is active, loads variables for required AD packages

## 📜 Templates
Jinja2 templates used by the plays.

### ADsudoers.j2
- `/etc/sudoers.d` template
- Defines sudo permissions for AD groups based on hostname 

### hosts.j2
- `/etc/hosts` template
- all hosts in play are added as hosts with `scheme_name`

### kickstart.cfg.j2
- kickstart template for `vm-deploy`
- should be customized for your needs

### krb5.j2
- Kerberos config file
- Configures the domain from variables defined in join-ad-linux.yml

### sshd\_config.j2
- Strict `/etc/ssh/sshd_config` config file
- Disables root login
- Disables password login
- Instructs to check authorized keys with `sss_ssh_authorizedkeys`
- Templating for sftp location based on OS

### sssd.conf.j2
- SSSD config file
- Configures the domain much same like the krb5.j2
- Instructs SSSD to get ssh pubkeys from altSecurityIdentities attribute

### staticmap.xml.j2
- pfSense DHCP Static Mapping Entry
- Used to add new DHCP Static Map to pfSense with ansible.builtin.xml
- Gets mac address from VMware ESXi

