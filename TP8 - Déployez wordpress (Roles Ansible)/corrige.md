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

## Stack vagrant

### Récupéreration des roles ansible
```bash
ansible-galaxy install -r roles/requirements.yml
```

### Se rassurer de remplecer IP_client par l'ip du client ansible dans le fichier hosts.yml

### Contenu du fichier TP8 - Déployez wordpress (Roles Ansible)/webapp/group_vars/prod.yml:

  ```bash
 ---
ansible_user: vagrant
# Commenter cette ligne pour empêcher Ansible d'utiliser le login par password et plutot utiliser la clés ssh
ansible_password: "{{ vault_ansible_password }}" 

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
### Se rassurer de remplecer IP_client par l'ip du client ansible dans le fichier hosts.yml

### Contenu du fichier TP8 - Déployez wordpress (Roles Ansible)/webapp/group_vars/prod.yml:

  ```bash
 ---
ansible_user: admin
# Commenter cette ligne pour empêcher Ansible d'utiliser le login par password et plutot utiliser la clés ssh
ansible_password: "{{ vault_ansible_password }}" 
  ```

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
