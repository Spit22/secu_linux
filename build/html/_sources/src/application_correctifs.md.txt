# Applications des correctifs

Nous allons mettre en place un mécanismes pour automatiser les mises à jour de sécurité sur notre système Linux. Pour cela, nous allons utiliser l'outil _unattended-upgrades_.

## Classification

* Niveau ANSSI : intermédiaire
* Mises à jour
* Patchs de sécurité
* Automatisation
* Veille et maintenance


## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

* [unattended-upgrades](https://www.kanjian.fr/automatiser-mises-a-jour-de-serveur-debian.html#.YGGZxa8zaUl)


## Procédures

Pour automatiser la gestion des mises à jour, nous allons utiliser deux paquets debian : apt-listchanges (pour afficher les changelogs) et unattended-upgrades (pour mettre à jour les paquets automatiquement).

### Installation
Les deux paquets peuvent être installés directement depuis apt :

![](img/application_correctif/installation.PNG)

### Configuration
Nous allons configurer unattended-upgrades de manière à ce qu'il sache comment réagir en cas de demande de reboot par une màj, et on va lui dire de n'effecter que les mises à jour qui ont le tag Security : 

![](img/application_correctif/maj_for_security_only.PNG)

![](img/application_correctif/disable_automatic_reboot.PNG)

### PoC
Pour vérifier que l'outil marche comme prévu, on peut le lancer à la main en mode verbose, et il devrait nous retourner les configurations qu'il applique :

![](img/application_correctif/maj_poc.PNG)


## Commentaires
