# Parefeu local

-classification (niveau ANSSI,type de mesure,  ...)
    -sources
    -procédures
    -commentaires (résultat, mesure écartée, conservée, adaptée,...)

## Classification

* Niveau ANSSI : intermédiaire
* Gestion des flux
* Minimisation


## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

## Procédures

Voici une première version du script iptables que nous allons appliquer sur le serveur. Ses caractéristiques sont :

* Les flux entrants sont **bloqués** par défaut
* Les flux sortants sont **bloqués** par défaut
* Les flux entrants sur le port SSH sont **autorisés**
* Les nouvelles connexions SSH sont **journalisées**

![](img/iptables_entrant/script_firewall.PNG)

On restreint les droits sur ce script :

![](img/iptables_entrant/droits_script_firewall.PNG)

On redirige les journaux généré par le parefeu vers le fichier _/var/log/iptables.log_ :

![](img/iptables_entrant/configuration_logging.PNG)

### Mise en place du parefeu local

Appliquons maintenant les politiques définies dans le script :

![](img/iptables_entrant/execution_firewall.PNG)

Faisons en sorte que ces politiques soient persitantes :

![](img/iptables_entrant/execution_auto_fw.PNG)

### Test du parefeu local

Testons nos politiques de parefeu. On ouvre un port sur un port qui n'est pas explicitement autorisé par le firewall :

![](img/iptables_entrant/test_part1.PNG)

En se connectant à une autre machine, on remarque que le port est bien bloqué par le firewall, tandis que le port SSH apparaît bien comme ouvert :

![](img/iptables_entrant/test_part2.PNG)

### Alternative : nftable

nftable s'impose que la suite logique d'iptable. On proposera donc ici une procédure qui montre comment convertir un firewall iptable existant en un firewall nftable.

Sauvergardons nos règles iptables appliquées jusqu'à maintenant :

![](img/convert_iptable2nftable/1_save_current_iptables_rules.PNG)

On convertie nos règles iptable en règle nftable :

![](img/convert_iptable2nftable/2_convert_iptable_to_nftable.PNG)

On exécute ce nouveau script de firewall avec nftable :

![](img/convert_iptable2nftable/3_execute_nftable_rules.PNG)

Voici un aperçu du script firewall en version nftable :

![](img/convert_iptable2nftable/4_save_as_firewall.nft.PNG)

### Amélioration du parefeu local

Ajoutons des règles de filtrage sur le traffic sortant :

![](img/iptables_output/1_output_rules.PNG)

Application des policies par défaut :

![](img/iptables_output/2_output_rules.PNG)

## Commentaires
