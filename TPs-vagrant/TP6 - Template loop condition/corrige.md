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

- Bien se rassurer que volume dans deploy.yml soit

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
#### Lancement du playbook

```bash
ansible-playbook -i hosts.yml -vvv deploy.yml
```

### Tester

#### Stack vagrant: Allez dans le navigateur puis entrer l'ip du client suivi du port 80: http://ip_client:80
