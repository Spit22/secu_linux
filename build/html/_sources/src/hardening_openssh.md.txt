# Hardening de OpenSSH

Le service OpenSSH est le point d'entrée principal de notre système Linux. Il est donc primordial de durcir ce service.

## Classification

* Niveau ANSSI : intermédiaire
* Authentification
* Accès à distance
* Défense en profondeur

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [LeCrabeInfo](https://lecrabeinfo.net/se-connecter-en-ssh-par-echange-de-cles-ssh.html)

## Procédures

### Authentification avec clé SSH

Nous allons chercher à n'autoriser que les authentifications utilisant des clés SSH.
On commence par générer une clé SSH, avec :
* utilisation du chiffrement rsa
* utilisation d'une clé de longueuyr 4096 (comme recommandé par l'ANSSI pour ce genre de clés)
* pour une clé servant à effectuer des tâches d'administration sensibles, une longueure de 8 128 est recommandée 
![](img/hardening_ssh/1_generate_ssh_key_for_auth.PNG)

On envoie la clé de l'utilisateur sur notre serveur distant :

![](img/hardening_ssh/2_deploy_key_on_remote_server.PNG)

Et on peut alors se connectant en utilisant la clé :

![](img/hardening_ssh/3_test_key_is_accepted.PNG)

### Configuration de sshd

La configuration du service ssh est également durcie :

* le port d'écoute du service a été modifié (le parefeu local a été modifié en conséquence)
* l'authentification en tant qu'utilisateur root a été interdite
* l'authentification par mot de passe a été interdite
* les mécanismes de chiffrement utilisées sont robustes

![](img/hardening_ssh/4_configuration_sshd_config_part1.PNG)

![](img/hardening_ssh/4_configuration_sshd_config_part2.PNG)


## Commentaires

De nombreuses autres améliorations auraient pu être ajoutées, notamment dans la configuration d'openSSH. Cependant, nous avons souhaité que la configuration soit applicable pour un usage simple, et donc nous avons évité d'avoir des règles trop restrictives, comme par exemple des whitelist d'ip.
