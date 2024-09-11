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


### Commandes ad-hoc
#####  Installation de ansible-lint
```bash
sudo pip3 install ansible-lint
```

### Stack vagrant

Bien se rassurer que le user et le password dans TP5 - Déployez un conteneur apache/webapp/group_vars/prod.yml soit vagrant

```bash

cat P5 - Déployez un conteneur apache/webapp/group_vars/prod.yml
---
ansible_user: vagrant
ansible_password: vagrant
```
Bien se rassurer de remplacer l'ip dans le fichier hosts.yml par celui du client ansible

### Stack eazylab

Bien se rassurer que le user et le password dans TP5 - Déployez un conteneur apache/webapp/group_vars/prod.yml soit admin

```bash

cat P5 - Déployez un conteneur apache/webapp/group_vars/prod.yml
---
ansible_user: admin
ansible_password: admin
```
Bien se rassurer de remplacer IP_client dans le fichier hosts.yml par celui du client ansible

#####  Check de l'indentation du playbook
```bash
ansible-lint deploy_v1.yml
ansible-lint deploy_v2.yml
```

#####  Lancement du playbook dans la stack vagrant

Bien se rassurer de changer la valeur de ansible_sudo_pass en vagrant dans deploy.ymldeploy_v1.yml: ansible_sudo_pass: vagrant

```bash
ansible-playbook -i hosts.yml -vvv deploy_v1.yml
ansible-playbook -i hosts.yml -vvv deploy_v2.yml
```

#####  Lancement du playbook dans eazylab

Bien se rassurer de changer la valeur de ansible_sudo_pass en vagrant dans deploy.ymldeploy_v1.yml: ansible_sudo_pass: admin

```bash
ansible-playbook -i hosts.yml -vvv deploy_v1.yml
ansible-playbook -i hosts.yml -vvv deploy_v2.yml
```
### Tester

#### Stack vagrant: Allez dans le navigateur puis entrer l'ip du client suivi du port 80: http://ip_client:80
#### Eazylab: Ouvrir le port 80 dans OPEN PORT de eazylab pour visualiser l'application

#####  Push du code sur github
```bash
git init
git add .
git config –global user.email “examplen@example.example”
git config –global user.name “votre nom”
git commit -m “webapp first version”
git add origin
git push origin master
```
