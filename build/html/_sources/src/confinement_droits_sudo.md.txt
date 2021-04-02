# Confinement des droits sudo

Un utilisateur ayant des droits sudo peut avoir un grand impact sur un système Linux, c'est pourquoi ces droits doivent être octroyé avec précautions.

## Classification

* Niveau ANSSI : intermédiaire
* Gestion utilisateurs
* Gestion des droits des utilisateurs
* Moindre privilège
* Défense en profondeur

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

## Procédures

On suppose que l'utilisateur bastien est administrateur système de la machine. Il doit donc pouvoir effectuer des actions root sur l'ensemble des commandes de la machine.

Ajoutons l'utilisateur _bastien_ au groupe _sudo_, tel que ce dernier puisse avoir un rôle d'administrateur de la machine :

![](img/Confinement_droits_sudo/1_ajout_bastien_dans_sudo.PNG)

Vérifions que _bastien_ a bien des droits sudo en tentant d'accéder au fichier _/etc/shadow_ :

![](img/Confinement_droits_sudo/2_verification.PNG)

On suppose désormais qu'un utilisateur _stagiaire_ est besoin d'exécuter nmap en root pour effectuer des scans UDP. 
Nous pouvons également limiter les droits sudo à des applications spécifiques. On octroit des droits sudo à cet utilisateur seulement lors de l'utilisation de nmap :

![](img/Confinement_droits_sudo/3_ajout_stagiaire_pour_exec_nmap_en_root.PNG)

Le stagiaire a donc le droit d'exécuter nmap en tant qu'utilisateur root :

![](img/Confinement_droits_sudo/4_verif_stagiaire.PNG)

Mais le stagiaire n'a pas d'autres droits que celui ci :

![](img/Confinement_droits_sudo/5_verif_stagiaire.PNG)

## Commentaires

Il faut garder à l'esprit que la meilleure bonne pratique consiste à éviter l'usage du ALL:ALL lors de la définition des droits sudo, et de privilégier l'usage de sudo pour des commandes spécifiques. Cependant, il est courant qu'un utilisateur ait besoin d'avoir l'équivalent d'un accès root (s'il est administrateur système par exemple), et donc l'usage du ALL:ALL est nécessaire.

/!\ Certaines applications permettent d'exécuter des shells -> donner un droit sudo sur ces commandes peut résulter en l'obtention d'un shell root ! (exemple : vim, cat, etc.)
