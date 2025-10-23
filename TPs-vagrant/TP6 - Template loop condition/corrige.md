### Stack vagrant

Bien se rassurer que le user et le password dans webapp/group_vars/prod.yml soit vagrant

```bash

cat webapp/group_vars/prod.yml
---
ansible_user: vagrant
ansible_password: vagrant
```
Bien se rassurer de remplacer l'ip dans le fichier hosts.yml par celui du client ansible dans le fichier hosts.yml

#####  Lancement du playbook dans la stack vagrant

- Bien se rassurer que volume dans deploy_ubuntu.yml soit

```bash

---
- name: "Apache installation using Docker on Ubuntu"
  hosts: prod
  become: true
  vars:
    ansible_python_interpreter: /usr/bin/python3

  pre_tasks:
    - name: Update APT cache
      apt:
        update_cache: yes

    - name: Install required packages for Docker and tools
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - git
        - wget

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
#### Lancement du playbook sur centos

```bash
ansible-playbook -i hosts.yml -vvv deploy_ubuntu.yml
```


- Bien se rassurer que volume dans deploy_centos.yml soit

```bash

---
- name: "Apache installation using Docker"
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
#### Lancement du playbook sur centos

```bash
ansible-playbook -i hosts.yml -vvv deploy_centos.yml
```

### Tester

#### Stack vagrant: Allez dans le navigateur puis entrer l'ip du client suivi du port 80: http://ip_client:80
