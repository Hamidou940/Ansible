# Ansible

Hamidou MARY

Cours Ansible


Tp préliminaire : https://www.katacoda.com/oliverveits/scenarios/ansible-bootstrap#
Connecter en ssh vm azure : ssh -i Ansible_key.pem azureuser@104.214.65.2
ansible all -i 'target,' -m ping => all : pour tous les machine après le target
Documentation vim : https://doc.ubuntu-fr.org/vim 
Dans vim => :vsplit /etc/ansible/hosts pour scinder le vim en 2 ecran
Ansible-playbook playbooks/main.yml –diff (à l’ancien depuis l’extérieur du dossier bonne pratice)
(main.yml)
 
Ansible.cfg placé sur la racine
 
[defaults]
Interpreter_python = /usr/bin/python3

	Dev.host => fichier dans lequel on met des groupes de host pour les développeurs par ex
	Ansible-playbook  -i dev.host playbooks/main.yml –diff
 

Labs :
1.	Machines virtuelles > créer > créer un groupe de ressources appelé "Ansible", nom de la machine: ansible-controller, Ubuntu 18.04 LTS - Génération 1 , taille de machine Standard_D4s_v3 , Clé SSH , utiliser une clé existante et la coller puis le reste par défaut

Sur windows: générer une paire de clé

2.	Connecter en ssh sur notre machine azure avec : ssh -i Ansible_key.pem azureuser@104.214.65.

3.	Installer Ansible
a.	apt-add-repository ppa:ansible/ansible
b.	apt update && apt upgrade
c.	apt install ansible
d.	ansible –version
i.	resultat : Ansible 2.9.20

4.	Installer Docker
a.	https://www.digitalocean.com/community/tutorials/comment-installer-et-utiliser-docker-sur-ubuntu-18-04-fr
b.	Lien ci-dessus pour installer docker

5.	Créer un repository sur GitLab
a.	Créer une clé ssh pour se connecter sur notre gitlab depuis notre VM
b.	Créer un repository « Ansible » sur GitLab en initialisant le README
c.	Git config –global user.name « hamidou.mary »
d.	Git config –global user.email « hamidou.mary@supdevinci-edu.fr »
e.	Faire un git clone du repository
f.	Cd ansible
g.	Git init
h.	Git remote (seulement si ce n’est pas fait)

6.	Créer un Dockerfile 
FROM  ubuntu:18.04
 
RUN apt-get update 
RUN apt-get upgrade -y
RUN apt-get install -y net-tools 
RUN apt-get install -y ssh 
 
RUN echo 'root:root' |chpasswd
 
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN service ssh start
EXPOSE 22
ENTRYPOINT service ssh restart && bash

7.	Construire l’image ubuntu1804-ansible-cours
a.	sudo docker build -t ubuntu1804-ansible-cours .

8.	Construire conteneur host1
a.	sudo docker run -dti --name=host1 ubuntu1804-ansible-cours

9.	Construire conteneur host1
a.	sudo docker run -dti --name=host2 ubuntu1804-ansible-cours

10.	Récupérer l’ip avec : 
a.	sudo docker inspect host1

11.	Mettre dans /etc/ansible/hosts
a.	[host1]
172.17.0.2
b.	[host2]
172.17.0.3

12.	Mettre dans /etc/hosts
a.	172.17.0.2 host1
b.	172.17.0.2 host2

13.	Se connecter en root : Important car la suite va en dépendre
a.	Sudo su

14.	Générer une clé ssh :
a.	ssh-keygen -t rsa
b.	Laisser les informations par défaut

15.	Copier la clé ssh dans les conteneurs host1 et host2
a.	ssh-copy-id -f -i /root/.ssh/id_rsa.pub root@host1
b.	ssh-copy-id -f -i /root/.ssh/id_rsa.pub root@host2

