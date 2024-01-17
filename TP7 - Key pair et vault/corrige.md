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
Docker version 20.10.8
```    

### Créez un cluster (1 ansible et 1 client) et récupérez votre code du TP-6 depuis votre github

########################### VAULT ###########################

### Créez un répertoire files qui va lui-même contenir un dossier secrets qui lui-même va contenir un fichier credentials.vault

```bash
files/secrets/credentials.vault
```

### Déplacez la variable contenant la variable du mot de passe admin dans ce fichier
```bash
ansible_password: admin
```

### Ensuite vous allez encryptez ce fichier à l’aide de la commande ansible-vault encrypt
```bash
ansible-vault encrypt files/secrets/credentials.vault
```

### Verifier l'encryption est bien passé
```bash
cat files/secrets/credentials.vault
```

### Retirer la variable ansible_password dans le group_vars/prod.yml

### Vous allez modifiez le playbook deploy.yml afin de lui demander de charger le fichier vaulté en tant que vars_files, rajouter ce qui suit:
```bash
become: true
vars_files:
    - files/secrets/credentials.vault
```

### Lancez votre playbook en rajoutant le paramètre qui vous permettra de fournir la clé vault
```bash
ansible-playbook -i hosts.yml --ask-vault-pass deploy.yml
```

########################### KEYPAIR ###########################

### Générez une paire de clé en laissant tous les paramètres par defaut
```bash
ssh-keygen -t rsa
```

### Copiez le contenu de la clé publique (id_rsa.pub) dans le fichier /home/admin/.ssh/authorized_host du client
```bash
ssh-copy-id admin@10.0.0.6
```

### Verifier que le cop s'est bien passe en se connectant chez le client via
```bash
ssh admin@10.0.0.6
cat .ssh/authorized_keys
```

### Modifiez le fichier d’inventaire afin de rajouter cette variable à tous les hôtes (all) ansible_ssh_common_args='-o StrictHostKeyChecking=no
```bash
vi hosts.yml
```

### Rajoutez le contenu qui suit:
```bash
all:
vars:
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
```

### Lancez à nouveau votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml deploy.yml
```