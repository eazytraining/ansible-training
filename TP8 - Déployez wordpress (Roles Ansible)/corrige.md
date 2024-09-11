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
docker-compose version 1.21.0
```
## Stack vagrant

### Récupéreration des roles ansible
```bash
ansible-galaxy install -r roles/requirements.yml
```

### Se rassurer de remplecer IP_client par l'ip du client ansible

### le contenu de wordpress.yml

```bash

---
- hosts: prod
  become: true
  vars:
    system_user: vagrant
    compose_binary_dir: /bin
  vars_files:
    - files/secrets/credentials.vault
  pre_tasks:
    - name: create www data
      user: 
        name:  www-data
        state: present
  roles:
    - { role: ansible-role-containerized-wordpress}

```

### Lancez votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml --ask-vault-pass wordpress.yml
```

## Eazylabs

### Récupéreration des roles ansible
```bash
ansible-galaxy install -r roles/requirements.yml
```

### Si vous avez ce bug lors de l'execution de la task Run docker-compose up ![Erreur Run docker-compose up](./bugs_potentiels/error_task_docker_compose.png) effectuez les taches suivantes: 

  ### reference de correction : https://github.com/docker/compose/issues/3927
  
  ### Augmenter le time out de docker sur le client si vous etes sur le client eazytraining/client
  ```bash
  ssh admin@10.0.52.5
  ```
  ### password: admin

  ```bash
  export DOCKER_CLIENT_TIMEOUT=120
  export COMPOSE_HTTP_TIMEOUT=120
  ```
  ```bash
  sudo systemctl stop docker
  ```
  ```bash
  sudo systemctl start docker
  ```
### Se rassurer de remplecer IP_client par l'ip du client ansible

### le contenu de wordpress.yml

```bash

---
- hosts: prod
  become: true
  vars:
    system_user: admin
    compose_binary_dir: /bin
  vars_files:
    - files/secrets/credentials.vault
  pre_tasks:
    - name: create www data
      user: 
        name:  www-data
        state: present
  roles:
    - { role: ansible-role-containerized-wordpress}

```

### Lancez votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml --ask-vault-pass wordpress.yml
```
