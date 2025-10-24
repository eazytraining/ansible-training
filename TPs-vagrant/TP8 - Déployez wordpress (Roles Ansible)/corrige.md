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
