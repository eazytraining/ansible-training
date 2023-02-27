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
Docker version 20.10.8
```    

### Commandes ad-hoc
##### Encrypter un fichier, utiliser "toto" comme mot de passe de chiffrement
```bash
ansible-vault encrypt files/secrets/credentials.yml
```

##### Décrypter un fichier
```bash
ansible-vault decrypt files/secrets/credentials.yml
```

##### Lancer ansible en lui passant le mot de passe de décryptage
```bash
ansible-playbook -i hosts.yml --ask-vault-pass deploy.yml
```

##### Créer un couple ssh de clé publique/privées
```bash
ssh-keygen -t rsa
```

##### Copier la clé publique sur le serveur distant
```bash
ssh-copy-id admin@10.0.36.4
```