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

#### Commandes ah-hoc  

#### Creation du fichier hosts et definissions des inventaires

Ouvrir le noeud

Taper 
```bash
  sudo su admin
  cd 
  vi hosts
```

#### Inserer ce qui suit dans le fichier hosts
```bash
web1 ansible_host=10.0.45.5 ansible_user=admin ansible_password=admin ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

#### Ping sur les clients web1
```bash
  ansible -i hosts web1 -m ping
  ansible -i hosts all -m ping
```

#### Creation d'un fichier avec le module copy
```bash
  ansible -i hosts all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining'"
```

**doc: https://docs.ansible.com/archive/ansible/2.3/copy_module.html**

#### Vérification du fichier
```bash
  ansible -i hosts all -m shell -a "ls -l /home/admin/toto.txt"
  ansible -i hosts all -m shell -a "cat /home/admin/toto.txt"
```

**doc: https://docs.ansible.com/ansible/2.8/modules/shell_module.html**

#### Ajout d'un client nommé web2
```bash
web2 ansible_host=node3 ansible_user=admin ansible_password=admin ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

#### Tentative de ping des deux clients
```bash
  ansible -i hosts all -m ping
```

#### Creation du fichier sur les deux clients
```bash
  ansible -i hosts all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining'"
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

