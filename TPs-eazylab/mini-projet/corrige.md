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
### Mini-projet Ansible

#### Déploiement

Se rassurer de remplacer IP_Client par l'ip du client ansible dans le fichier host_vars/client1.yml et host_vars/client2.yml

Pour déployer le serveur Apache conteneurisé, exécutez le playbook apache.yml à l'aide de la commande suivante:

```bash

ansible-playbook deploy-app.yml
```
#### Tester: ouvrir le port 80 au niveau de OPEN PORT de l'environnement eazylab
