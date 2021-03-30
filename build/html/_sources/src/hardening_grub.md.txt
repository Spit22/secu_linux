# Hardening de Grub

Le service OpenSSH est le point d'entrée principal de notre système Linux. Il est donc primordial de durcir ce service.

## Classification

* Niveau ANSSI : intermédiaire
* Bootloader
* Authentification

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [](https://www.computersecuritystudent.com/UNIX/UBUNTU/1204/lesson3/index.html)

## Procédures

### Verrouillage de GRUB

Faisons en sorte qu'un mot de passe soit requis pour accéder au bootloader.

Générons d'abord le mot de passe (protégé par [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2)) :

![Generer grub password](img/hardening_grub/1_generate_password.png)

Modifions ensuite la configuration de grub (dans _/etc/grub.d/00-header_)

![Configurer grub](img/hardening_grub/2_grub_conf_header.png)

Effectuons un update de grub :

![Update grub](img/hardening_grub/3_grub_update.png)

Vérifions qu'une authentification est bien requise pour accéder au bootloader :

![Vérification](img/hardening_grub/4_verif.png)

## Commentaires