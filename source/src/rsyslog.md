# Rsyslog


## Classification

* Niveau ANSSI : renforcé
* Journalisation

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [golinuxcloud.com](https://www.golinuxcloud.com/secure-remote-logging-rsyslog-tls-certificate/)


## Procédures

On veut que les flux syslog soient chiffrés :

### Génération des clés

Générons les clés privés pour le client et le serveur :

![](img/rsyslog/key-generation.PNG)

![](img/rsyslog/generate_serv_key.PNG)

Générons ensuite une requête de certificat pour le serveur :

![](img/rsyslog/generate_serv_certificates.PNG)

On créer enfin un certificat autosigné pour le client :

![](img/rsyslog/generate_self-signed_certificates.PNG)

Et un certificat signé avec la clé du client pour le serveur :

![](img/rsyslog/generate_signed_serv1_certificates.PNG)


## Commentaires