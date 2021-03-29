# Parefeu local

-classification (niveau ANSSI,type de mesure,  ...)
    -sources
    -procédures
    -commentaires (résultat, mesure écartée, conservée, adaptée,...)

## Classification

* Niveau ANSSI : intermédiaire
* Gestion des flux


## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

## Procédures

Voici une première version du script iptables que nous allons appliquer sur le serveur. Ses caractéristiques sont :

* Les flux entrants sont **bloqués** par défaut
* Les flux sortants sont **autorisés** par défaut
* Les flux entrants sur le port SSH sont **autorisés**
* Les nouvelles connexions SSH sont **journalisées**

![Script firewall](img/iptables_sortant/script_firewall.PNG)

On restreint les droits sur ce script :

![Droit firewall](img/iptables_sortant/droits_script_firewall.PNG)

On redirige les journaux généré par le parefeu vers le fichier _/var/log/iptables.log_ :

![Configuration Rsyslog](img/iptables_sortant/configuration_logging.PNG)

### Mise en place du parefeu local

Appliquons maintenant les politiques définies dans le script :

![Execution firewall](img/iptables_sortant/execution_firewall.PNG)

Faisons en sorte que ces politiques soient persitantes :

![Execution automatique du firewall](img/iptables_sortant/execution_auto_fw.PNG)

### Test du parefeu local

Testons nos politiques de parefeu :

![Test1](img/iptables_sortant/test_part1.PNG)

![Test2](img/iptables_sortant/test_part2.PNG)


## Commentaires