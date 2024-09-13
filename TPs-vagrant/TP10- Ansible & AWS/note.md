# Module 10: Ansible et AWS

L'objectif de ce tp est de vous montrer comment est ce que nous pouvons
provisionner des machines sur aws à l'aide de Ansible 

Pour realiser ce tp il vous faudra realiser un certains nombres d'etapes:

Prerequis:
- Creation d'un compte AWS (NB: Bien vouloir creer un second compte , l'utilisation du compte root est proscrite!!!!!)
- Creation de Secret_key et Access pour votre compte
- Creation d'une paire de cles avec un nom personnaliser, dans notre cas: devops.pem (NB: .pem est l'extension de votre fichier )
- Recuperer l'id d'un subnet sur votre compte 


Corriger:

- Recuperer le code sur votre serveur a l'aide des commandes git
- Recuperer votre paire de cles , et changer les autorisations du fichier: sudo chmod 400 devops.pem
- Se rentre ou se trouve le code 
- Realiser un export de vos access_key et secret a l'aide des commandes ci-dessous:
    export AWS_ACCESS_KEY_ID='PUT YOUR OWN'
    export AWS_SECRET_ACCESS_KEY='PUT YOUR OWN'
- Realiser l'installation des roles de geerlinggur
    ansible-galaxy install -r roles/requirements.yml

- modifier le fichier all.yaml qui se trouve dans le repertoire group_vars en fonction de votre configuration
    dans notre cas nous nous trouvons dans la region "us-east-1 et notre subnet se trouve dans la zone de disponibilite 1a (us-east-1a)

- lancer votre playbook: ansible-playbook deploy.yml

N'hesiter pas a activer la versobité pour un meilleur Deboggage  (Pour l'activer rajouter juste cette option "-vvv" à la suite de votre commande)

Patienter quelques instant que le processus s'acheve puis recuperer l'ip qui s'affiche a l'ecran que rentrer le dans un navigateur 

Thanks 