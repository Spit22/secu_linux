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

![Script firewall](img/iptables_entrant/script_firewall.PNG)

On restreint les droits sur ce script :

![Droit firewall](img/iptables_entrant/droits_script_firewall.PNG)

On redirige les journaux généré par le parefeu vers le fichier _/var/log/iptables.log_ :

![Configuration Rsyslog](img/iptables_entrant/configuration_logging.PNG)

### Mise en place du parefeu local

Appliquons maintenant les politiques définies dans le script :

![Execution firewall](img/iptables_entrant/execution_firewall.PNG)

Faisons en sorte que ces politiques soient persitantes :

![Execution automatique du firewall](img/iptables_entrant/execution_auto_fw.PNG)

### Test du parefeu local

Testons nos politiques de parefeu :

![Test1](img/iptables_entrant/test_part1.PNG)

![Test2](img/iptables_entrant/test_part2.PNG)

### Alternative : nftable

Sauvergardons nos règles iptables appliquées jusqu'à maintenant :

![Sauvergarde iptables](img/convert_iptable2nftable/1_save_current_iptables_rules.PNG)



![Convertion](img/convert_iptable2nftable/2_convert_iptable_to_nftable.PNG)



![Sauvergarde iptables](img/convert_iptable2nftable/3_execute_nftable_rules.PNG)



![Sauvergarde iptables](img/convert_iptable2nftable/4_save_as_firewall.nft.PNG)

### Amélioration du parefeu local

Ajoutons des règles de filtrage sur le traffic sortant :

![Règles sortant](img/iptables_output/1_output_rules.PNG)

![Règles sortant 2](img/iptables_output/2_output_rules.PNG)



## Commentaires