
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

