
### Modification du fichier hosts en yaml

copiez le contenu du fichier hosts-vagrant.yml depuis ce repos dont le repertoire est:

```bash
    TP3 - Inventaire au format yaml/hosts.yaml
```

### Se rassurer de remplecer ip par l'ip du client ansible dans le fichier hosts-vagrant.yaml

#### Tester: Commande ping

ansible -i hosts.yaml all -m ping
 
#### Commande pour créer un fichier

ansible -i hosts.yaml all -m copy -a "dest=/home/vagrant/toto.txt content='bonjour eazytraining'"
