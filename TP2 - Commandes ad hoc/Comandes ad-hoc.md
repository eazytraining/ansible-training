#### Création du fichier d'inventaire principal
  C'est le fichier **hosts**

#### Ping ansible via le module ping:
```bash
  ansible -i hosts all -m ping
```  
#### Création de fichier à l'aide du module copy
```bash
  ansible -i hosts all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytraining'"
```  
#### Découverte des machines distances avec le module setup
```bash
  ansible -i hosts all -m setup
```  
  
  
