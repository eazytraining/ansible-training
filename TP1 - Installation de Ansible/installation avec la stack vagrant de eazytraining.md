#### Installation ansible et client
```bash
git clone https://github.com/diranetafen/cursus-devops.git 
cd cursus-devops/vagrant/ansible
vagrant up
```

#### connexion à la machine  ansible
```bash
cd cursus-devops/vagrant/ansible
vagrant ssh ansible
ansible --version
```

#### connexion à la machine  client1
```bash
cd cursus-devops/vagrant/ansible
vagrant ssh client1
```

####  Installer docker sur la machine client
```bash
ansible-galaxy install geerlingguy.pip
ansible-galaxy install geerlingguy.docker
```

#### Créer les fichiers hosts.ini et install_docker.yml
#### Lancer le déploiement: 
```bash
ansible-playbook -i hosts.ini install_docker.yml
```
#### Test
```bash
 ansible-playbook -i hosts.ini nginx.yml
```
