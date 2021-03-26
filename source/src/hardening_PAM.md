# Hardening PAM

-classification (niveau ANSSI,type de mesure,  ...)
    -sources
    -procédures
    -commentaires (résultat, mesure écartée, conservée, adaptée,...)

## Classification

* Niveau ANSSI : minimal
* Authentification
* Gestion des comptes


## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

## Procédures

![Mot de passe root](img/PAM/install_libpam.PNG)

### Désactivation du compte root

Le but de cette étapes est de bloquer l'accès au compte root. Pour cela, nous allons utiliser deux mécanismes :

* interdiction au groupe _wheel_ (groupe que nous allons créer pour les superutilisateurs) de se connecter en tant que root
* désactivation du shell de l'utilisateur root

Le groupe _wheel_ est l'équivalent du groupe _sudo_ chez RedHat.

Commençons par créer le groupe _wheel_ :

![Mot de passe root](img/PAM/group_wheel.PNG)

Bloquons ensuite l'accès au compte root aux membres du groupe _wheel_ :

![Mot de passe root](img/PAM/disable_root_wheel.PNG)

![Mot de passe root](img/PAM/disable_root_wheel2.PNG)

Pour que ce mécanisme fonctionne, il est impératif d'ajouter les superutilisateurs (membres du groupe _sudo_) au groupe _wheel_.

Désactivons enfin le shell de l'utilisateur root :

![Mot de passe root](img/PAM/disable_root.PNG)



### Contre mesures bruteforce

![Mot de passe root](img/PAM/bruteforce_pass_sshd.PNG)

![Mot de passe root](img/PAM/bruteforce_pass_login.PNG)

### Politiques de password

![Mot de passe root](img/PAM/password_policy.PNG)

![Mot de passe root](img/PAM/password_policy_poc.PNG)



## Commentaires