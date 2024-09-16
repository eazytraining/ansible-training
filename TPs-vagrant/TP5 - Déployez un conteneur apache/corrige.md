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

#### Lancer le déploiement 

ansible-playbook -i hosts.yml -vvv deploy.yml

```bash
ansible-playbook -i hosts.yml -vvv deploy_v1.yml
ansible-playbook -i hosts.yml -vvv deploy_v2.yml
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
