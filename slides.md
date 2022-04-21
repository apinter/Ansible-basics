---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
drawings:
  persist: false
---

# An introduction to Ansible

A configuration management tool for maintaing your sanity
<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: image-right
image: adathor.png

---
# Whoami

- Attila Pinter aka Adathor 
- Father, husband, DevOps eng. 
- FOSS advocate
- openSUSE Board Member
- openSUSE Mods Team
- openSUSE Docs Team
- openSUSE Telegram Admin
- Tumbleweed user
- CTO and founder at OpenStorage.io

---
layout: image-right
image: ansible.png
---
# What is Ansible?

_Ansible_ is a modular configuration management tool for IT automation, and _DevOps_. Can be run on _Linux_, _Windows_, and _Mac_, integrated with CI platforms

- _Python based_ 
- `YAML`
- Idempotent
- Hundreds of plugins, and roles
<br>
<br>

[Ansible](https://www.ansible.com/)   
[Ansible Blog](https://www.ansible.com/blog)   
[Official docs](https://docs.ansible.com)

---
layout: default
---
### Installing Ansible

_Ansible_ can be installed a few different ways, but the upstream recommended way is to use _pip_ and avoid using the distribution packaged versions. Using _pip_ will also allow for using the latest version of _Ansible_.

`pip3 install ansible`

Another method is to run _Ansible_ in a _toolbox_ type container, this way the system doesn't get cluttered by _python_ libs.

---
layout: default
---

### Requirements

_Ansible_ is agentless and can connect a number of different ways to hosts. By default _Ansible_ will connect to a host over _ssh_. It is preferred to use keypairs to connect to hosts which will make it easy to run _playbooks_, _roles_ in a CI pipeline.
With that being said _Ansible_ does require a version of _Python_ to be installed on the target host.

---
layout:  center
---

# Ansible Concepts

- _Ansible modules_
- _Ansible ad-hoc commands_
- _Inventory_
- _Ansible playbooks_
- _Ansible roles_
- _Ansible collections_
- _Ansbile facts_
---
layout: center
---
# Inventory Example

```ini
[example]
myserver
14.15.17.2
example_host ansible_host=35.234.72.202

[example:vars]
ansible_ssh_private_key_file=~/.ssh/aws_privkey
ansible_user=attila.pinter
```
---
layout: default
---
# Ansible ad-hoc commands

The ad-hoc commands can be used by not writing a task or a playbook, but running a command with _ansible_ from the cli. This can be done by using the `-a ` flag which will default to the `command` module and executes the desired command on the targeted host(s).

Example:
```
$ ansible -i inventory all -m ping ## Check if every host is accessible by _Ansible_ using the ping module

$ ansible -i inventory all -a "free-h" ## Show memory usage from all hosts in the inventory 

$ ansible -i inventory www -a "df -h" ## Show disk usage from the www group hosts only

```

---
layout: center
---
### Tasks
Tasks are configured modules to run on a host

__Important:__ _Ansible_ is item potent therefore if a task is running, but the host already has the task impelemented changes in existence it will not run it again and give back an __OK__ message.

Example task:

```
- name: Install ranger on openSUSE
  community.general.zypper:
    name: ranger
    state: latest
```
---
layout: center
---
# Playbook Example
```yaml
---
  - name: Playbook
    hosts: example
    become: true
    tasks:
      - name: Install nmap
        community.general.zypper:
          name: nmap
          state: present
      - name: ensure apache is running
         ansible.builtin.systemd:
           name: httpd
           state: started
```
---
layout: default
---
# Variables
_Ansible_ is using the _jinja2_ variable mechanism. In yaml this gets tricky from time to time and must be used to following way:

- If the variable if the first value to a key it must be double quoted "" and within the quotes must use a double curly bracket {{ }}.
- If the varaible if not the first value then quotes can be left out. 
---
layout: default
---
# Variables in playbooks, files

- `vars`
- `vars_files`
- `cli supplied` (`-e`)
---
layout: center
---
# Variables in playbooks

```yaml
- hosts: webservers
  vars:
    http_port: 80
    admin_password: megaLargEsecrEt
```
---
layout: two-cols
---
# Variables in files

```yaml
- hosts: webservers
  vars:
    http_port: 80
    admin_password: megaLargEsecrEt
  vars_files:
    - vars/external_vars.yml
```

::right::

# vars/external_vars.yml

```yaml
---
user_password: chikenNoodleSoup
ro_user_password: withAsodaOnTheSide
```
---
layout: center
---
# This is the end of part one, thank you!

---
layout: end
---