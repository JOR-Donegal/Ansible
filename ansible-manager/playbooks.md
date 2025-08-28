# Playbooks

Playbooks are written as YAML files, you need to be careful of the spacing, indentation, etc. I save mine under the \~/Ansible directory. Getting privilege escalation is an issue and you will need to do research on any real application to see what is appropriate.

## Updates

I'm going to install updates on all my web servers.&#x20;

This playbook is an example from **jeffgerling.com**

```
---
- name: Updates
  hosts: all
  gather_facts: yes
  become: yes

  tasks:
    - name: Perform a dist-upgrade.
      ansible.builtin.apt:
        upgrade: dist
        update_cache: yes

    - name: Check if a reboot is required.
      ansible.builtin.stat:
        path: /var/run/reboot-required
        get_md5: no
      register: reboot_required_file

    - name: Reboot the server (if required).
      ansible.builtin.reboot:
      when: reboot_required_file.stat.exists == true

    - name: Remove dependencies that are no longer required.
      ansible.builtin.apt:
        autoremove: yes

```

I need to provide the sudo password for the target servers, my full command line is

```
ansible-playbook updates.yml --ask-become-pass
```

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

## Webservers

I want to install Apache on my web servers only. I use the following playbook.

```
- name: Install Apache
  hosts: webservers
  become: true
  
  tasks:
  - name: Install Apache
    apt:
      name: apache2
      state: present
  
  - name: Start Apache
    service:
      name: apache2
      state: started
      enabled: yes
```

I ran the command

```
ansible-playbook InstallApache.yml --ask-become-pass
```

Which gave me the result

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

I go to a web browser to check if the service is running.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## Databases

Finally, we need to install MariaDB on all our database servers.

```
- name: Install MariaDB
  hosts: mariadbservers
  become: true
  
  tasks:
  - name: Install MariaDB
    apt:
      name: mariadb-server
      state: present
  
  - name: Start MariaDB
    service:
      name: mariadb
      state: started
      enabled: yes
```

I execute the playbook with&#x20;

```
ansible-playbook InstallMariaDB.yml --ask-become-pass
```

And I get the result

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

And as always I test. From the SSH console of **ansibleserver2**

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Tested!
