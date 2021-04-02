.. Sécurité Linux documentation master file, created by
   sphinx-quickstart on Thu Apr  1 16:53:59 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

=============================================

.. toctree::
   :maxdepth: 2
   :caption: Niveau minimal ANSSI

   src/installation
   src/hardening_PAM
   src/application_correctifs


=============================================

.. toctree::
   :maxdepth: 2
   :caption: Niveau intermédiaire ANSSI

   src/parefeu_local
   src/hardening_openssh
   src/confinement_droits_sudo
   src/hardening_grub
   src/desactivation_magickeys
   src/rsyslog


=============================================

.. toctree::
   :maxdepth: 2
   :caption: Niveau renforcé ANSSI

   src/auditd
   src/fail2ban


=============================================

.. toctree::
   :maxdepth: 2
   :caption: Niveau élevé ANSSI

   src/apparmor


=============================================

.. toctree::
   :maxdepth: 2
   :caption: Audits de sécurité

   src/audit_lynis
