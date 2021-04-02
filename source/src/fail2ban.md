# Fail2ban

L'outil __fail2ban__ va nous permettre de bannir certaines machines qui essayerais de bruteforce l'authentification sur notre système Linux.

## Classification

* Niveau ANSSI : intermédiaire
* Principes : Défense en profondeur
* Authentification

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)
* [Doc Ubuntu](https://doc.ubuntu-fr.org/fail2ban)

## Procédures

Commençons par installer Fail2ban :

    root@host:~# apt install fail2ban

Faisons en sorte que fail2ban se lance au démarrage de la machine :

    root@host:~# systemctl enable fail2ban

Nous pouvons maitenant configurer fail2ban. Sa configuration se trouve dans le répertoire `/etc/fail2ban`.

### Exemple avec ssh

Si par exemple je veux que fail2ban fonctionne derrière mon service ssh :

Je crée un fichier `hard_ssh.conf` dans le répertoire `/etc/fail2ban/jail.d/` avec dedans la configuration pour défaut :

    [DEFAULT]
    ignoreip = 127.0.0.1
    findtime = 10m
    bantime = 24h
    maxretry = 3

et la configuration spécifique au service ssh :

    [sshd]
    enabled = true
    port = 22
    logpath = /var/log/auth.log
    maxretry = 5

### PoC

Testons fail2ban avec la configuration suivante dans `/etc/fail2ban/jail.d/hard_ssh.conf` :

    [DEFAULT]
    ignoreip = 127.0.0.1
    findtime = 10m
    bantime = 1m
    maxretry = 3
    
    [sshd]
    enabled = true
    port = 22
    logpath = /var/log/auth.log
    maxretry = 5

Voici le résultat :

    root@host:~$ ssh toto@172.20.10.3
    toto@172.20.10.3's password: 
    Permission denied, please try again.
    toto@172.20.10.3's password: 
    Permission denied, please try again.
    toto@172.20.10.3's password:
    Permission denied, please try again.
    toto@172.20.10.3's password:
    Permission denied, please try again.
    toto@172.20.10.3's password:
    

Au bout de 5 tentatives d'authentification échouées, nous sommes bloqué :

    root@host:~$ ssh spit@172.20.10.3
    ssh: connect to host 172.20.10.3 port 22: Connection refused



## Commentaires