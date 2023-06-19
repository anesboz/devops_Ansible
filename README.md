## TP 3 - Ansible session

### Document playbook.yml

Fichier de configuration qui définit les rôles à exécuter sur un ensemble d'hôtes.

- `hosts: all` : Spécifie que tous les hôtes sont concernés par cette configuration.

- `gather_facts: false` : Désactive la collecte automatique des informations spécifiques à chaque hôte.

- `become: true` : Utilisation en mode `sudo`

- `roles:` : Déclare une liste de rôles à exécuter séquentiellement sur les hôtes.

  - `docker` : Tâche pour installer et configurer Docker sur les hôtes.

  - `create-network` : créer un réseau spécifique dans Docker.

  - `database` : configurer et démarrer la base de données.

  - `app` : Déploit le backend simple-api

  - `httpd` : installer et configurer un serveur HTTP

### Document your inventory

Définir un groupe d'hôtes avec des variables spécifiques. Voici une explication de chaque élément :

- `all:` : tous les hôtes.

- `vars:` : Déclare les variables qui seront appliquées à tous les hôtes.

  - `ansible_user: centos` : utilisateur remote "centos".

  - `ansible_ssh_private_key_file: /mnt/c/D/CURSUS/M2/DevOps/3 Ansible/tp3/id_rsa` : le chemin d'accès au fichier de la clé privée SSH permettant d'établir la connexion SSH aux hôtes.

  - `ansible_python_interpreter: /bin/python3` : Vu avec le prof, permet de forcer l'utilisation de python3

- `children:` : Déclare des groupes d'hôtes

  - `prod:` : Un groupe appelé "prod".

    - `hosts: anesbouzouaoui.takima.cloud` : Définit l'hôte

---

## Commandes utilisées

## run all

sudo ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml

## Test your inventory with the ping command:

sudo ansible all -i ansible/inventories/setup.yml -m ping

## get ansible destribution from remote server

sudo ansible all -i ansible/inventories/setup.yml -m setup -a "filter=ansible_distribution\*"

## desinstaller apache

ansible all -i ansible/inventories/setup.yml -m yum -a "name=httpd state=absent" --become

## lancer le play book

sudo ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml

## lancer le play book

sudo ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml --syntax-check

## install docker

sudo ansible-playbook -i ansible/inventories/setup.yml ansible/install-docker.yml

---

# TD

host
centos@anesbouzouaoui.takima.cloud

mettre en place un server apache

```sh
sudo ansible all -m yum -a "name=httpd state=present" --private-key=id_rsa -u centos --become
```

setup Apache service:

```sh
sudo ansible all -m shell -a 'echo "<html><h1>Hello World</h1></html>" >> /var/www/html/index.html' --private-key=id_rsa -u centos --become
```

Now start your Apache service:

```sh
sudo ansible all -m service -a "name=httpd state=started" --private-key=id_rsa -u centos --become

```
