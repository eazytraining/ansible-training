
#### Creation du fichier hosts.ini

vi hosts.ini

#### Inserez le contenu suivant tout en se rassurant de remplacer IP par l'ip du client

```bash
[all:vars]
ansible_user=vagrant
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[prod]
client ansible_host=IP

[prod:vars]
ansible_password=vagrant
env=production
```

#### Commandes ad-hoc

```bash
ansible -i hosts.ini all -m ping
ansible -i hosts.ini prod -m ping
ansible -i hosts.ini client -m ping
ansible -i hosts.ini all -m copy -a "dest=/home/vagrant/toto.txt content='bonjour eazytraining {{ env }}'"
```

#### Creation du fichier hosts.yml
#### NB: attention à l’indentation au niveau du fichier yaml
```bash
vi hosts.yml
```

#### Inserez le contenu suivant tout en se rassurant de remplacer IP par l'ip du client

```bash
all:
  vars:
    ansible_user: vagrant
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
prod:
  hosts:
    client:
      ansible_host: IP
  vars:
    ansible_password: vagrant
    env: production
```

#### Ping
```bash
ansible -i hosts.yml all -m ping
ansible -i hosts.yml all -m copy -a "dest=/home/vagrant/toto.txt content='bonjour eazytraining {{ env }}'"
```
