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

#### connexion à la machine  client1 et client2
```bash
cd cursus-devops/vagrant/ansible
vagrant ssh client1
vagrant ssh client2
```
