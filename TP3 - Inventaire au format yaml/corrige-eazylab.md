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

### Modification du fichier hosts en yaml

copiez le contenu du fichier hosts-eazylab.yml depuis ce repos dont le repertoire est:

```bash
    TP3 - Inventaire au format yaml/hosts-eazylab.yml
```

### Se rassurer de remplecer IP par l'ip du client ansible dans le fichier hosts-eazylab.yml

#### Tester: Commande ping

ansible -i hosts.yml all -m ping
 
#### Commande pour créer un fichier

ansible -i hosts.yml all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining'"
