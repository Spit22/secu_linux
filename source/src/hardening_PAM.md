# Hardening PAM

PAM est un élément central de notre système Linux, gérant les authentifications des utilisateurs. Il est donc primordial de le durcir.

## Classification

* Niveau ANSSI : minimal
* Authentification
* Gestion des comptes
* Double authentification

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [2FA](https://www.vultr.com/docs/how-to-setup-two-factor-authentication-2fa-for-ssh-on-debian-9-using-google-authenticator)

## Procédures

Commençons par installer la librairie _libpam_ :

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

![Mot de passe root](img/PAM/disable_root_wheel_2.PNG)

Pour que ce mécanisme fonctionne, il est impératif d'ajouter les superutilisateurs (membres du groupe _sudo_) au groupe _wheel_.

Désactivons enfin le shell de l'utilisateur root :

![Mot de passe root](img/PAM/disable_root.PNG)

### Contres mesures bruteforce

Limitons le nombre de tentative d'authentification :

![Mot de passe root](img/PAM/bruteforce_pass_sshd.PNG)

![Mot de passe root](img/PAM/bruteforce_pass_login.PNG)

### Politiques de password

Définissons une politique de password pour forcer les utilisateurs à respecter les bonnes pratiques de sécurité

![Mot de passe root](img/PAM/password_policy.PNG)

Testons cette politique :

![Mot de passe root](img/PAM/password_policy_poc.PNG)

Définissons enfin une politique de stockage des mots de passe forte :

![Stockage password](img/PAM/common-password.PNG)

### Double authentification

Utilisons Google authentificator pour implémenter de la double authentification dans le PAM de notre système Linux :

![Installation google-auth](img/PAM_2fa/1_install_google_auth.PNG)

![Configure google auth](img/PAM_2fa/2_configure_google_auth.PNG)

Configurons PAM et sshd pour qu'il utilise Google authentificator :

![Configure PAM](img/PAM_2fa/3_configure_pam.PNG)

![Configure sshd](img/PAM_2fa/3_configure_sshd.PNG)

Vérifions que le mécanisme est fonctionel :

![Verification](img/PAM_2fa/4_verification.PNG)

## Commentaires