### Versions sur eazytraining/ansible
```bash
    OS: Centos7
    ansible 2.9.25
      config file = /etc/ansible/ansible.cfg
      configured module search path = [u'/home/admin/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
      ansible python module location = /usr/lib/python2.7/site-packages/ansible
      executable location = /bin/ansible
      python version = 2.7.5 (default, Oct 14 2020, 14:45:30) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]
```
  
### Versions sur eazytraining/client
```bash
    OS: Centos7
    Python 2.7.5
```    

### Commandes ah-hoc  
##### Création du fichier d'inventaire principal
  C'est le fichier **hosts**

##### Ping ansible via le module ping:
```bash
  ansible -i hosts all -m ping
```  
##### Création de fichier à l'aide du module copy
```bash
  ansible -i hosts all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining'"
```  
##### Découverte des machines distances avec le module setup
```bash
  ansible -i hosts all -m setup
```