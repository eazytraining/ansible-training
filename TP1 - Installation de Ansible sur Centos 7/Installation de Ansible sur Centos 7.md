#### Mise à jour du système
```bash
  sudo yum update -y
```  
#### installer les release epel
```bash
  sudo yum install epel-release -y
```  
#### Installer Ansible
```bash
  sudo yum install ansible -y 
```  
#### Vérifier la version de ansible
```bash
  ansible --version
```

#### Si vous voulez la dernière version et non celle sur epel
```bash
  pip install ansible
```

#### Avec python3, installation de pip3
```bash
  curl -sS https://bootstrap.pypa.io/get-pip.py | sudo python3
  pip3 install ansible
```