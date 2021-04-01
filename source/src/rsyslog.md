# Rsyslog


## Classification

* Niveau ANSSI : renforcé
* Journalisation

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [golinuxcloud.com](https://www.golinuxcloud.com/secure-remote-logging-rsyslog-tls-certificate/)


## Procédures

Nous allons nous intéresser ici aux étapes de configuration de la journalisation sécurisée avec rsyslog sur un serveur de journalisation distant à l'aide de certificats TLS. 
Ce document décrit donc un moyen sécurisé de configurer rsyslog (certificats TLS) pour transférer les journaux vers le serveur de journaux distant. 
Le simple chiffrement du canal de transmission n'est pas suffisant pour un environnement de journalisation sécurisé. Cependnat, voici quelques-uns des avantages de cette solution :
* les messages syslog sont chiffrés lors de leurs déplacements sur le fil
* l'expéditeur syslog s'authentifie auprès du serveur syslog; ainsi, ce dernier sait qui est l'expéditeur des logs.
* le serveur syslog s'authentifie auprès de l'expéditeur syslog; ainsi, l'expéditeur peut vérifier s'il envoie bien au destinataire attendu
* l'authentification mutuelle empêche en grande partie les attaques man-in-the-middle

### Génération des clés

Pour créer un certificat auto-signé afin d'obtenir une transmission sécurisée de syslog vers un serveur de journalisation distant, nous utiliserons certtool.

Générons les clés privés pour le client et le serveur :

![](img/rsyslog/key-generation.PNG)

![](img/rsyslog/generate_serv_key.PNG)

Cette clé a besoin des autorisations appropriées pour la rendre lisible par l'utilisateur root uniquement

![](img/rsyslog/generate_serv_key.PNG)

Générons ensuite une requête de certificat pour le serveur :

![](img/rsyslog/generate_serv_certificates.PNG)

On créer enfin un certificat autosigné pour le client :

![](img/rsyslog/generate_self-signed_certificates.PNG)

Et un certificat signé avec la clé du client pour le serveur :

![](img/rsyslog/generate_signed_serv1_certificates.PNG)


## Commentaires
Cette solution nécessitant l'utilisation de TCP et un chiffrement via TLS, son coût est beaucoup plus élevé qu'un simple envoie de logs via UDP. 
Par conséquent, si le volume de log est trop important, ce système ne pourra sans doute pas être appliqué dû à son débit fortement réduit comparé à d'autres moyens.
