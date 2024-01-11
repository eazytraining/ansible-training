### Versions sur eazytraining/ansible
OS: Centos7
    ansible 2.9.25
      config file = /etc/ansible/ansible.cfg
      configured module search path = [u'/home/admin/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
      ansible python module location = /usr/lib/python2.7/site-packages/ansible
      executable location = /bin/ansible
      python version = 2.7.5 (default, Oct 14 2020, 14:45:30) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]

### Versions sur eazytraining/client
    OS: Centos7
    Python 2.7.5
    Docker version 20.10.8


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