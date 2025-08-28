# Ansible Manager

I add Ansible's PPA to the list of sources and update them.

```
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
```

I install Ansible.

```
sudo apt install ansible -y
```

I edit Ansible's inventory file **/etc/ansible/hosts**

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

I can test this by typing

```
ansible-inventory --list -y
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Now test you can access both servers.

```
ansible all -m ping -u root
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

I can now run arbitrary commands on any group of servers. The **df** command reports on file system disk space usage.

```
ansible all -a "df -h" -u johnoraw
```

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
