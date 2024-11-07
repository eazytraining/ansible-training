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

### NB: Se rassurer qu'aucun conteneur ne tourne chez le client ansible sur le port 80

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

    - name: Installer Docker via le script officiel
      ansible.builtin.shell: |
        curl -fsSL https://get.docker.com -o get-docker.sh
        sh get-docker.sh
      args:
        creates: /usr/bin/docker
  
    - name: Assurez-vous que Docker est démarré et activé au démarrage
      ansible.builtin.service:
        name: docker
        state: started
        enabled: true

    - name: Télécharger Docker Compose
      ansible.builtin.get_url:
        url: "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-{{ ansible_system }}-{{ ansible_architecture }}"
        dest: /usr/local/bin/docker-compose
        mode: '0755'

    - name: Ajouter les permissions d'exécution à Docker Compose
      ansible.builtin.file:
        path: /usr/local/bin/docker-compose
        mode: '0755'
        state: file
  roles:
    - { role: ansible-role-containerized-wordpress}
```

### Lancez votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml --ask-vault-pass wordpress.yml
```
