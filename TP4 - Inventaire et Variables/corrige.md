#### Versions sur eazytraining/ansible
```bash
OS: Centos7
ansible 2.9.25
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/admin/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /bin/ansible
  python version = 2.7.5 (default, Oct 14 2020, 14:45:30) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]
```

#### Versions sur eazytraining/client
```bash
  OS: Centos7
  Python 2.7.5
```

#### Creation du fichier hosts.ini

vi hosts.ini

#### Inserez le contenu suivant

```bash
[all:vars]
ansible_user=admin
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[prod]
client ansible_host=10.0.0.4

[prod:vars]
ansible_password=admin
env=production
```

#### Commandes ad-hoc
ansible -i hosts.ini all -m ping
ansible -i hosts.ini prod -m ping
ansible -i hosts.ini client -m ping
ansible -i hosts.ini all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining {{ env }}'"

#### Creation du fichier hosts.yml
#### NB: attention à l’indentation au niveau du fichier yaml
```bash
vi hosts.yml
```

#### Inserez le contenu suivant

```bash
all:
  vars:
    ansible_user: admin
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
prod:
  hosts:
    client:
      ansible_host: 10.0.0.4
  vars:
    ansible_password: admin
    env: production
```

#### Ping
```bash
ansible -i hosts.yml all -m ping
ansible -i hosts.yml all -m copy -a "dest=/home/admin/toto.txt content=’bonjour eazytraining {{ env }}'"
```