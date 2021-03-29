# Applications des correctifs

Nous allons mettre en place un mécanismes pour automatiser les mises à jour de sécurité sur notre système Linux. Pour cela, nous allons utiliser l'outil _unattended-upgrades_.

## Classification

* Niveau ANSSI : intermédiaire
* Mises à jour
* Patchs de sécurité
* Automatisation


## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

* [unattended-upgrades](https://www.kanjian.fr/automatiser-mises-a-jour-de-serveur-debian.html#.YGGZxa8zaUl)


## Procédures

### Installation

![Installation](img/application_correctif/installation.PNG)

### Configuration

![Configuration security only](img/application_correctif/maj_for_security_only.PNG)

![Configuration automatic reboot](img/application_correctif/disable_automatic_reboot.PNG)

### PoC

![Installation](img/application_correctif/maj_poc.PNG)


## Commentaires