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

### Se rassurer de remplacer IP_client dans hosts.yml par l'ip du client ansible

### Se rassurer de remplacer que le user ansible dans group_vars soit:

```bash
cat group_vars/prod.yml
---
ansible_user: vagrant
ansible_password: "{{ vault_ansible_password }}"

```

### Vous allez modifiez le playbook deploy.yml afin de lui demander de charger le fichier vaulté en tant que vars_files, rajouter ce qui suit:
```bash
---
- name: "Apache installation using Docker on CentOS 7"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3.6
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
ansible-playbook -i hosts.yml --ask-vault-pass deploy.yml
```

## Eazylabs

### Créez un répertoire files qui va lui-même contenir un dossier secrets qui lui-même va contenir un fichier credentials.vault

```bash
files/secrets/credentials.vault
```
### Déplacez la variable contenant la variable du mot de passe admin dans ce fichier
```bash
ansible_password: admin
```

### Ensuite vous allez encryptez ce fichier à l’aide de la commande ansible-vault encrypt
```bash
ansible-vault encrypt files/secrets/credentials.vault
```


### Retirer la variable ansible_password dans le group_vars/prod.yml

### Se rassurer de remplacer IP_client dans hosts.yml par l'ip du client ansible

### Se rassurer de remplacer que le user ansible dans group_vars soit:

```bash
cat group_vars/prod.yml
---
ansible_user: admin
ansible_password: "{{ vault_ansible_password }}"

```

### Vous allez modifiez le playbook deploy.yml afin de lui demander de charger le fichier vaulté en tant que vars_files, rajouter ce qui suit:
```bash
---
- name: "Apache installation using Docker on CentOS 7"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3.6
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
        dest: /home/admin/index.html
    - name: Create Apache container
      docker_container:
        name: webapp
        image: httpd
        ports:
          - "80:80"
        volumes: 
         - /home/admin/index.html:/usr/local/apache2/htdocs/index.html
```

### Lancez votre playbook en rajoutant le paramètre qui vous permettra de fournir la clé vault
```bash
ansible-playbook -i hosts.yml --ask-vault-pass deploy.yml
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

### contenu du fichier deploy.yml

```bash

---
- name: "Apache installation using Docker on CentOS 7"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3.6
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
ansible-playbook -i hosts.yml deploy.yml
```

## Eazylabs

### Générez une paire de clé en laissant tous les paramètres par defaut
```bash
ssh-keygen -t rsa
```

### Copiez le contenu de la clé publique (id_rsa.pub) dans le fichier /home/admin/.ssh/authorized_host du client
```bash
ssh-copy-id admin@IP_client
```

### Verifier que le cop s'est bien passe en se connectant chez le client via
```bash
ssh admin@IP_client
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

### contenu du fichier deploy.yml

```bash
---
- name: "Apache installation using Docker on CentOS 7"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3.6
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
        dest: /home/admin/index.html
    - name: Create Apache container
      docker_container:
        name: webapp
        image: httpd
        ports:
          - "80:80"
        volumes: 
         - /home/admin/index.html:/usr/local/apache2/htdocs/index.html
```

### Lancez à nouveau votre playbook et verifier que tout se passe bien
```bash
ansible-playbook -i hosts.yml deploy.yml
```
