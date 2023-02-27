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
```

#### Commandes ad-hoc depuis le serveur ansible
```bash
ansible -i hosts.ini client -m ping
ansible -i hosts.ini prod -m ping
ansible -i hosts.ini all -m copy -a "dest=/home/admin/toto.txt content='bonjour eazytrining {{ env }} '"
```