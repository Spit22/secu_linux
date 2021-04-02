# Mise en place de AppArmor

Nous allons mettre en place un mécanisme de contrôle d'accès obligatoire. En quelques mots, AppArmor permet d'autoriser ou non des appels au noyau pour un applicatif donné. On pourra ainsi par exemple autoriser l'exécution du _mount_ sur un répertoire spécifique, et interdire son exécution sur le reste du système.

## Classification

* Niveau ANSSI : élevé
* Défense en profondeur
* Confinement des applicatifs
* Mandatory Access Control

## Sources

* [ANSSI](https://www.ssi.gouv.fr/uploads/2016/01/linux_configuration-fr-v1.2.pdf)

* [Debian Handbook](https://debian-handbook.info/browse/fr-FR/stable/sect.apparmor.html)


## Procédures

L'objectif de ces procédure est de montrer comment ajouter de la sécurité à notre système à l'aide d'AppArmor. Il est important de noter que toute application exposée au réseau devrait avoir un profil AppArmor qui lui est propre.

### Installation de l'outil

AppArmor vient par défaut sur un set de distributions unix. On peut alors vérifier que ce dernier est bien exécuté sur notre machine :
```
bastien@debiansecu:~$ cat /sys/module/apparmor/parameters/enabled
Y
```
Malgré le fait que AppArmor soit installé par défaut, les outils de gestion de ce dernier ne sont pas inclus dans le même package. Il faut donc installer des outils d'administration d'AppArmor supplémentaires :
```
bastien@debiansecu:~$ sudo apt install apparmor-utils
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait
Le paquet suivant a été installé automatiquement et n'est plus nécessaire :
  linux-image-4.19.0-6-amd64
Veuillez utiliser « sudo apt autoremove » pour le supprimer.
Les paquets supplémentaires suivants seront installés :
  python3-apparmor python3-libapparmor
Paquets suggérés :
  vim-addon-manager
Les NOUVEAUX paquets suivants seront installés :
  apparmor-utils python3-apparmor python3-libapparmor
0 mis à jour, 3 nouvellement installés, 0 à enlever et 0 non mis à jour.
Il est nécessaire de prendre 344 ko dans les archives.
Après cette opération, 971 ko d'espace disque supplémentaires seront utilisés.
Souhaitez-vous continuer ? [O/n]
```

### Installation des profils développés par la communauté
Peut d'applicatifs intègrent par défaut un profil AppArmor.
La communauté ayant déjà développé un certain nombre de profils, nous allons les installer :

```
bastien@debiansecu:~$ sudo apt install apparmor-profiles-extra
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait

Les NOUVEAUX paquets suivants seront installés :
  apparmor-profiles-extra
0 mis à jour, 1 nouvellement installés, 0 à enlever et 0 non mis à jour.
Il est nécessaire de prendre 9 828 o dans les archives.
Après cette opération, 43,0 ko d'espace disque supplémentaires seront utilisés.
Réception de :1 http://deb.debian.org/debian buster/main amd64 apparmor-profiles-extra all 1.26 [9 828 B]
````
On peut ensuite lister les profils acitfs sur la machine :

```
bastien@debiansecu:~$ sudo aa-status
apparmor module is loaded.
13 profiles are loaded.
12 profiles are in enforce mode.
   /usr/bin/man
   /usr/bin/pidgin
   /usr/bin/pidgin//sanitized_helper
   /usr/bin/totem
   /usr/bin/totem-audio-preview
   /usr/bin/totem-video-thumbnailer
   /usr/bin/totem//sanitized_helper
   /usr/sbin/apt-cacher-ng
   man_filter
   man_groff
   nvidia_modprobe
   nvidia_modprobe//kmod
1 profiles are in complain mode.
   /usr/bin/irssi
0 processes have profiles defined.
0 processes are in enforce mode.
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.
```

### Création d'un profil pour une application
En exécutant la commande suivante, on peut lister les applications ayant accès une interface réseau mais qui ne sont pas confinées dans AppArmor. 
```
bastien@debiansecu:~$ sudo aa-unconfined --paranoid
1 /usr/lib/systemd/systemd (/lib/systemd/systemd) not confined
377 /usr/sbin/cron not confined
379 /usr/bin/dbus-daemon not confined
383 /usr/lib/systemd/systemd-logind (deleted) (/lib/systemd/systemd-logind) not confined
446 /usr/bin/login (/bin/login) not confined
452 /usr/lib/systemd/systemd (deleted) (/lib/systemd/systemd) not confined
453 /usr/lib/systemd/systemd (deleted) not confined
458 /usr/bin/bash (-bash) not confined
960 /usr/sbin/rsyslogd not confined
8828 /usr/bin/python3.7 (deleted) (/usr/bin/python3) not confined
9210 /usr/lib/systemd/systemd-udevd (/lib/systemd/systemd-udevd) not confined
14104 /usr/lib/systemd/systemd-timesyncd (/lib/systemd/systemd-timesyncd) not confined
14107 /usr/lib/systemd/systemd-journald (/lib/systemd/systemd-journald) not confined
20973 /usr/sbin/sshd not confined
24277 /usr/sbin/auditd (/sbin/auditd) not confined
26216 /usr/sbin/sshd not confined
26223 /usr/lib/systemd/systemd (/lib/systemd/systemd) not confined
26224 /usr/lib/systemd/systemd not confined
26232 /usr/sbin/sshd (sshd: bastien@pts/0) not confined
26233 /usr/bin/bash (-bash) not confined
26504 /usr/bin/sudo not confined
26505 /usr/bin/python3.7 (/usr/bin/python3) not confined
```
Dans la mesure où le seul service présent en écoute sur un port sur notre machine est SSH, nous allons créer un profil pour ce dernier :

```
bastien@debiansecu:~$ cat /etc/apparmor.d/usr.sbin.sshd
#include <tunables/global>

/usr/sbin/sshd {
  #include <abstractions/authentication>
  #include <abstractions/base>
  #include <abstractions/consoles>
  #include <abstractions/nameservice>
  #include <abstractions/wutmp>

  # Set authorized linux capabilities
  capability audit_control,
  capability chown,
  capability dac_override,
  capability fowner,
  capability fsetid,
  capability kill,
  capability net_bind_service,
  capability setgid,
  capability setuid,
  capability sys_chroot,
  capability sys_resource,
  capability sys_tty_config,

  # Set DAC on some files 
  /bin/ash rUx,
  /bin/bash rUx,
  /bin/bash2 rUx,
  /bin/bsh rUx,
  /bin/csh rUx,
  /bin/ksh rUx,
  /bin/sh rUx,
  /bin/tcsh rUx,
  /bin/zsh rUx,
  /dev/ptmx rw,
  /dev/pts/[0-9]* rw,
  /dev/urandom r,
  /etc/** r,
  /proc/*/oom_adj rw,
  /proc/*/oom_score_adj rw,
  /sbin/nologin rUx,
  /tmp/ssh-*/agent.[0-9]* rwl,
  /tmp/ssh-*[0-9]*/ w,
  /usr/sbin/sshd mrix,
  /var/log/* rw,
  /{,var/}run w,
  /{,var/}run/sshd{,.init}.pid wl,
  @{HOME}/.ssh/authorized_keys{,2} r,
  @{PROC}/[0-9]*/fd/ r,
  @{PROC}/[0-9]*/loginuid w,
  @{PROC}/[0-9]*/mounts r,

  # When trying ssh to authenticate to the host
  ^AUTHENTICATED {
    #include <abstractions/authentication>
    #include <abstractions/consoles>
    #include <abstractions/nameservice>
    #include <abstractions/wutmp>

    capability setgid,
    capability setuid,
    capability sys_tty_config,


    /dev/log w,
    /dev/ptmx rw,
    /etc/default/passwd r,
    /etc/localtime r,
    /etc/login.defs r,
    /etc/motd r,
    /tmp/ssh-*/agent.[0-9]* rwl,
    /tmp/ssh-*[0-9]*/ w,

  }

  # When ssh is attempting to execute a command
  ^EXEC {
    #include <abstractions/base>


    /bin/ash Ux,
    /bin/bash Ux,
    /bin/bash2 Ux,
    /bin/bsh Ux,
    /bin/csh Ux,
    /bin/ksh Ux,
    /bin/sh Ux,
    /bin/tcsh Ux,
    /bin/zsh Ux,
    /sbin/nologin Ux,

  }
  
  # Privilege separation process
  ^PRIVSEP {
    #include <abstractions/base>
    #include <abstractions/nameservice>

    capability setgid,
    capability setuid,
    capability sys_chroot,



  }
  # Privilege separation monitoring process
  ^PRIVSEP_MONITOR {
    #include <abstractions/authentication>
    #include <abstractions/base>
    #include <abstractions/nameservice>
    #include <abstractions/wutmp>

    capability chown,
    capability setgid,
    capability setuid,


    /dev/ptmx rw,
    /dev/pts/[0-9]* rw,
    /dev/urandom r,
    /etc/hosts.allow r,
    /etc/hosts.deny r,
    /etc/ssh/moduli r,
    @{HOME}/.ssh/authorized_keys{,2} r,
    @{PROC}/[0-9]*/mounts r,

  }
}
```
On peut ensuite activer ce profile en mode enforce avec la commande suivante :
```
bastien@debiansecu:~$ sudo aa-enforce sshd
Setting /usr/sbin/sshd to enforce mode.
```
Et chaque fois qu'on effectue une modification sur le fichier, il suffit de lancer apparmor pour qu'il reload la rule :
```
bastien@debiansecu:~$ sudo apparmor_parser -r /etc/apparmor.d/usr.sbin.sshd
```

## Notes importantes
Pour chaque nouvelle règle, il est important de vérifier que le service ciblé fonctionne toujours comme prévu. En effet, apparmor agissant sur une couche très basse (niveau noyau), il est parfois compliqué de gégner des profils apparmor qui prennent en compte tous les cas d'utilisation.
Dans certains cas, il est possible de se simplifier la vie en utilisant la commande suivante :
```
$ aa-genproof /usr/sbin/sshd
```
En effet, cette commande sert à générer un profil pour un binaire donné, en adaptant ses règles aux logs générés par l'application. On peut donc dire à apparmor d'analyser le service, et si en parrallèle on utilise ce même service, apparmor devrait prendre en compte un nombre important de cas d'usage.
