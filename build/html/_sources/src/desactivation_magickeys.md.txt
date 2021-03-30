# Désactivation des Magic keys

Les _Magics keys_ peuvent avoir un impact énorme sur un système Linux (redémarrage, kill de processus, affichage de la mémoire dans la console...), il est donc intéressant de les désactiver.

## Classification

* Niveau ANSSI : intermédiaire
* Sécurité physique

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [Debian](https://www.debian.org/doc/manuals/securing-debian-manual/restrict-sysrq.en.html)

## Procédures

Vérifions si les _magic keys_ sont activées ou non :

![](img/desactivation_magickeys/1_is_activated.PNG)

Si elles sont activées, désactivons les :

![](img/desactivation_magickeys/2_disable_in_sysctl_conf.PNG)

## Commentaires