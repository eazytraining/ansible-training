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
### Mini-projet Ansible

##### Methode 1

#### Déploiement

#### Install necessary roles

```bash
ansible-galaxy install geerlingguy.pip
ansible-galaxy install geerlingguy.docker
```

Se rassurer de remplacer IP_Client par l'ip du client ansible dans le fichier host_vars/client1.yml et host_vars/client2.yml

Pour déployer le serveur Apache conteneurisé, exécutez le playbook apache.yml à l'aide de la commande suivante:

```bash

ansible-playbook deploy-app.yml
```

##### Methode 2

#### Installation du rôle

Vous pouvez découplez votre rôle au cycle de vie du playbook à cet effet creez un repos git où vous mettrez uniquement le contenu du dossier rôle.

Avant de déployer le playbook, il est nécessaire de télécharger et d'installer le rôle dont le nom est basic-apache-container depuis votre role sur github.

on aura donc dans requirements.yml:

```bash
cat roles/requirements.yml
---
# Install a role for apache
- name: basic-apache-container 
  src: "https://repos_name"
- geerlingguy.pip
- geerlingguy.docker
```

Vous pouvez le faire avec la commande ansible-galaxy comme suit pour installer votre rôle :

```bash
ansible-galaxy install -r roles/requirements.yml
```

#### Déploiement
Pour déployer le serveur Apache conteneurisé, exécutez le playbook apache.yml à l'aide de la commande suivante :

```bash
ansible-playbook deploy-app.yml
```
#### Tester

Aller sur le navigateur taper: 
pour prod donc client1: http://ip_client1:80
pour staging donc client2: http://ip_client2:80 pour ce cas faudra modifier dans le playbook deploy-app.yml cd dernier hosts: staging
