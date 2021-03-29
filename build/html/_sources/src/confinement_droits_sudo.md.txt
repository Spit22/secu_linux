# Confinement des droits sudo

Un utilisateur ayant des droits sudo peut avoir un grand impact sur un système Linux, c'est pourquoi ces droits doivent être octroyé avec précautions.

## Classification

* Niveau ANSSI : intermédiaire
* Gestion utilisateurs
* Gestion des droits des utilisateurs

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

## Procédures

Ajoutons l'utilisateur _bastien_ au groupe _sudo_ :

![Ajout de Bastien dans sudo](img/Confinement_droits_sudo/1_ajout_bastien_dans_sudo.PNG)

Vérifions que _bastien_ a bien des droits sudo en tentant d'accéder au fichier _/etc/shadow_ :

![Vérification des droits](img/Confinement_droits_sudo/2_verification.PNG)

Nous pouvons également limiter les droits sudo à des applications spécifiques, exemple avec l'utilisateur stagiaire. On octroit des droits sudo à cet utilisateur seulement lors de l'utilisation de nmap :

![Ajout du stagiaire](img/Confinement_droits_sudo/3_ajout_stagiaire_pour_exec_nmap_en_root.PNG)

Le stagiaire a donc le droit d'exécuter nmap en tant qu'utilisateur root :

![Vérification stagiaire](img/Confinement_droits_sudo/4_verif_stagiaire.PNG)

Mais le stagiaire n'a pas d'autres droits que celui ci :

![Vérification stagiaire 2](img/Confinement_droits_sudo/5_verif_stagiaire.PNG)


## Commentaires