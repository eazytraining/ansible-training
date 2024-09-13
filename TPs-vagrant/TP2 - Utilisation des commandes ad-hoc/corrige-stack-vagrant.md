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

## Vagrant: 

se connecter à la vm ansible de la stack vagrant

Taper 
```bash
  vagrant ssh ansible
  vi hosts
```

#### Inserer ce qui suit dans le fichier hosts tout en se rassurant de remplacer ip par l'ip de client1
```bash
web1 ansible_host=ip ansible_user=vagrant ansible_password=vagrant ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

#### Ping sur les clients web1
```bash
  ansible -i hosts web1 -m ping
  ansible -i hosts all -m ping
```

#### Creation d'un fichier avec le module copy
```bash
  ansible -i hosts all -m copy -a "dest=/home/vagrant/toto.txt content='bonjour eazytraining'"
```

**doc: https://docs.ansible.com/archive/ansible/2.3/copy_module.html**

#### Vérification du fichier
```bash
  ansible -i hosts all -m shell -a "ls -l /home/vagrant/toto.txt"
  ansible -i hosts all -m shell -a "cat /home/vagrant/toto.txt"
```

**doc: https://docs.ansible.com/ansible/2.8/modules/shell_module.html**

#### Ajout d'un client nommé web2 tout en se rassurant de remplacer ip2 par l'ip de client2
```bash
web2 ansible_host=ip2 ansible_user=vagrant ansible_password=vagrant ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

#### Tentative de ping des deux clients
```bash
  ansible -i hosts all -m ping
```

#### Creation du fichier sur les deux clients
```bash
  ansible -i hosts all -m copy -a "dest=/home/vagrant/toto.txt content='bonjour eazytraining'"
```

**constat**: ansible est stateful

#### Output de commande ad hoc sur une seule ligne par machine
```bash
ansible -i hosts all -m ping -o
```

#### Test du module setup
```bash
  ansible -i hosts all -m setup
``` 

#### Récupération de la RAM
```bash
  ansible -i hosts all -m setup -a 'filter=ansible_memtotal_mb'
``` 

