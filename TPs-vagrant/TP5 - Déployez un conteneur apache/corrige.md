
### Commandes ad-hoc

####  Installer docker sur la machine client via le playbook install_docker.yml

#### Pour le faire on va installé les rôles suivant sur le controleur ansible

#### Taper ces commande sur le controleur ansible

```bash
ansible-galaxy install geerlingguy.pip
ansible-galaxy install geerlingguy.docker
```

#### Créer les fichiers hosts.ini et install_docker.yml

```bash
 cat hosts.ini

      client ansible_host=IP_client ansible_user=vagrant ansible_password=vagrant ansible_ssh_common_args='-o StrictHostKeyChecking=no' ansible_python_interpreter=/usr/bin/python3
```
### NB: Remplacer IP_client par l'ip du client ansible dans le fichier hosts.ini

```bash
cat install_docker.yml
- hosts: all
  become: yes
  vars:
    pip_install_packages:
      - name: docker
    docker_users:
      - vagrant
  roles:
    - geerlingguy.pip
    - geerlingguy.docker

```

#### Lancer le playbook pour installer docker: 
```bash
ansible-playbook -i hosts.ini install_docker.yml
```

### Installation de ansible-lint
```bash
sudo pip3 install ansible-lint
```

Bien se rassurer que le user et le password dans webapp/group_vars/prod.yml soit vagrant

```bash

cat webapp/group_vars/prod.yml
---
ansible_user: vagrant
ansible_password: vagrant
```
Bien se rassurer de remplacer l'ip dans le fichier hosts.yml par celui du client ansible

#### Lancer le déploiement sur centos

```bash
ansible-playbook -i hosts.yml -vvv deploy_v1_centos.yml
ansible-playbook -i hosts.yml -vvv deploy_v2_centos.yml
```

#### Lancer le déploiement sur ubuntu

```bash
ansible-playbook -i hosts.yml -vvv deploy_v1_ubuntu.yml
ansible-playbook -i hosts.yml -vvv deploy_v2_ubuntu.yml
```

#### Tester

Stack vagrant: Allez dans le navigateur puis entrer l'ip du client suivi du port 80: http://ip_client:80

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
