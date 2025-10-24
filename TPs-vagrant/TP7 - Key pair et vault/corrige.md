### Créez un cluster (1 ansible et 1 client) et récupérez votre code du TP-6 depuis votre github

########################### VAULT ###########################

## Stack vagrant

### Créez un répertoire files qui va lui-même contenir un dossier secrets qui lui-même va contenir un fichier credentials.vault

```bash
files/secrets/credentials.vault
```
### Déplacez la variable contenant la variable du mot de passe admin dans ce fichier
```bash
ansible_password: vagrant
```

### Ensuite vous allez encryptez ce fichier à l’aide de la commande ansible-vault encrypt
```bash
ansible-vault encrypt files/secrets/credentials.vault
```


### Retirer la variable ansible_password dans le group_vars/prod.yml

### Se rassurer de remplacer IP_client dans hosts.yml par l'ip du client ansible dans le fichier hosts.yml

### Se rassurer de remplacer que le user ansible dans group_vars soit:

```bash
cat group_vars/prod.yml
---
ansible_user: vagrant
ansible_password: "{{ vault_ansible_password }}"

```

### Vous allez modifiez le playbook deploy_ubuntu.yml afin de lui demander de charger le fichier vaulté en tant que vars_files, rajouter ce qui suit:
```bash
---
- name: "Apache installation using Docker on Ubuntu"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3
  vars_files:
    - files/secrets/credentials.vault

  pre_tasks:
    - name: Update APT cache
      apt:
        update_cache: yes

    - name: Install required packages
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - git
        - wget
        - python3
        - python3-pip

    - name: Install Docker SDK for Python
      pip:
        name: docker
        executable: pip3

  tasks:
    - name: Copy website file template
      template:
        src: index.html.j2
        dest: /home/vagrant/index.html
        owner: vagrant
        group: vagrant
        mode: '0644'

    - name: Create Apache container
      community.docker.docker_container:
        name: webapp
        image: httpd
        ports:
          - "80:80"
        volumes: 
          - /home/vagrant/index.html:/usr/local/apache2/htdocs/index.html
```

### Lancez votre playbook en rajoutant le paramètre qui vous permettra de fournir la clé vault
```bash
ansible-playbook -i hosts.yml --ask-vault-pass deploy_ubuntu.yml
```


### Vous allez modifiez le playbook deploy_centos.yml afin de lui demander de charger le fichier vaulté en tant que vars_files, rajouter ce qui suit:
```bash
---
- name: "Apache installation using Docker on CentOS 7"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3
  vars_files:
      - files/secrets/credentials.vault
  pre_tasks:
    - name: Install EPEL repo
      package: name={{ item }} state=present
      when: ansible_distribution == "CentOS"
      loop:
        - epel-release
        - git
        - wget

    - name: Install Python 3 and pip3
      package:
        name:
          - python3
          - python3-pip
        state: present

    - name: Install Docker module for Python
      pip:
        name: docker
        executable: pip3
        
  tasks:
    - name: Copy website file template
      template:
        src: index.html.j2
        dest: /home/vagrant/index.html
    - name: Create Apache container
      docker_container:
        name: webapp
        image: httpd
        ports:
          - "80:80"
        volumes: 
         - /home/vagrant/index.html:/usr/local/apache2/htdocs/index.html
```

### Lancez votre playbook en rajoutant le paramètre qui vous permettra de fournir la clé vault
```bash
ansible-playbook -i hosts.yml --ask-vault-pass deploy_centos.yml
```

########################### KEYPAIR ###########################

## Stack vagrant

### Générez une paire de clé en laissant tous les paramètres par defaut
```bash
ssh-keygen -t rsa
```

### Copiez le contenu de la clé publique (id_rsa.pub) dans le fichier /home/admin/.ssh/authorized_host du client
```bash
ssh-copy-id vagrant@IP_client
```

### Verifier que le cop s'est bien passe en se connectant chez le client via
```bash
ssh vagrant@IP_client
cat .ssh/authorized_keys
```

### Modifiez le fichier d’inventaire afin de rajouter cette variable à tous les hôtes (all) ansible_ssh_common_args='-o StrictHostKeyChecking=no
```bash
vi hosts.yml
```

### Rajoutez le contenu qui suit:
```bash
all:
vars:
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
```
### Se rassurer de remplacer IP_client dans hosts.yml par l'ip du client ansible

### contenu du fichier deploy_ubuntu.yml

```bash

---
- name: "Apache installation using Docker on Ubuntu"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3
  vars_files:
    - files/secrets/credentials.vault

  pre_tasks:
    - name: Update APT cache
      apt:
        update_cache: yes

    - name: Install required packages
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - git
        - wget
        - python3
        - python3-pip

    - name: Install Docker SDK for Python
      pip:
        name: docker
        executable: pip3

  tasks:
    - name: Copy website file template
      template:
        src: index.html.j2
        dest: /home/vagrant/index.html
        owner: vagrant
        group: vagrant
        mode: '0644'

    - name: Create Apache container
      community.docker.docker_container:
        name: webapp
        image: httpd
        ports:
          - "80:80"
        volumes: 
          - /home/vagrant/index.html:/usr/local/apache2/htdocs/index.html
```

### Lancez à nouveau votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml deploy_ubuntu.yml
```

### contenu du fichier deploy_centos.yml

```bash

---
- name: "Apache installation using Docker on CentOS 7"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3
  pre_tasks:
    - name: Install EPEL repo
      package: name={{ item }} state=present
      when: ansible_distribution == "CentOS"
      loop:
        - epel-release
        - git
        - wget

    - name: Install Python 3 and pip3
      package:
        name:
          - python3
          - python3-pip
        state: present

    - name: Install Docker module for Python
      pip:
        name: docker
        executable: pip3
        
  tasks:
    - name: Copy website file template
      template:
        src: index.html.j2
        dest: /home/vagrant/index.html
    - name: Create Apache container
      docker_container:
        name: webapp
        image: httpd
        ports:
          - "80:80"
        volumes: 
         - /home/vagrant/index.html:/usr/local/apache2/htdocs/index.html

```

### Lancez à nouveau votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml deploy_centos.yml
```
