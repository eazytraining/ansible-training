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
sudo pip install ansible-lint
```

#####  Check de l'indentation du playbook
```bash
ansible-lint deploy.yml
```

#####  Lancement du playbook
```bash
ansible-playbook -i hosts.yml -vvv deploy.yml
```

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
