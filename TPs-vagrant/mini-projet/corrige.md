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

#### Installation du rôle

Avant de déployer le playbook, il est nécessaire de télécharger et d'installer le rôle ansible-role-containerized-apache depuis Galaxy.
Vous pouvez le faire avec la commande ansible-galaxy comme suit :

```bash
ansible-galaxy install -r roles/requirements.yml
```

#### Déploiement
Pour déployer le serveur Apache conteneurisé, exécutez le playbook apache.yml à l'aide de la commande suivante :

```bash
ansible-playbook playbooks/apache.yml
```
