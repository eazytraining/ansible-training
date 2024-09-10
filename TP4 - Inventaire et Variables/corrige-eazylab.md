#### Versions sur eazytraining/ansible
```bash
      OS: Centos7
      ansible [core 2.11.12]
      config file = None
      configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
      ansible python module location = /usr/local/lib/python3.6/site-packages/ansible
      ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
      executable location = /usr/local/bin/ansible
      python version = 3.6.8 (default, Nov 14 2023, 16:29:52) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]
      jinja version = 3.0.3
      libyaml = True
```
  
#### Versions sur eazytraining/client
```bash
    OS: Centos7
    Python 3.6.8
```

#### Creation du fichier hosts.ini

vi hosts.ini

#### Inserez le contenu suivant tout en se rassurant de remplacer IP par l'ip du client

```bash
[all:vars]
ansible_user=admin
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[prod]
client ansible_host=IP

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

#### Inserez le contenu suivant tout en se rassurant de remplacer IP par l'ip du client

```bash
all:
  vars:
    ansible_user: admin
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
prod:
  hosts:
    client:
      ansible_host: IP
  vars:
    ansible_password: admin
    env: production
```

#### Ping
```bash
ansible -i hosts.yml all -m ping
ansible -i hosts.yml all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining {{ env }}'"
```
