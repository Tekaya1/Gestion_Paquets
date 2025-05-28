# Gestion des paquets RPM, DNF et Dépôts sous RedHat/CentOS

## Partie 1 : RPM

### Introduction

- **RPM (RedHat Package Manager)** est un gestionnaire de paquets créé par Red Hat en 1995, distribué sous licence GNU GPL.
- Utilisé par de nombreuses distributions (Fedora, Mandriva, SuSe, ...).
- Permet l’installation, la suppression, la mise à jour, la vérification et la gestion des dépendances des logiciels.

### Format RPM

- Un paquet RPM contient :
    - Une archive de fichiers.
    - Des métadonnées (scripts, attributs, informations sur le paquet).

### Manipulation des RPM

#### Installer, mettre à jour, désinstaller

```bash
# Installer un paquet
rpm -ivh coreutils-8.30-4.el8.x86_64.rpm

# Mettre à jour un paquet
rpm -Uhv coreutils-8.30-4.el8.x86_64.rpm

# Désinstaller un paquet
rpm -evh coreutils-8.30-4.el8.x86_64.rpm
```

#### Recherche sur les paquets

```bash
# Vérifier si un paquet est installé
rpm -q coreutils-8.30-4.el8.x86_64

# Lister tous les paquets installés
rpm -qa

# Rechercher un paquet par nom
rpm -qa | grep bind

# Trouver le paquet fournissant un fichier
rpm -q --whatprovides /usr/bin/netstat

# Trouver le paquet d'un fichier ou répertoire
rpm -qf /etc/yum.repos.d
```

---

## Partie 2 : DNF

### Introduction

- **dnf** est le gestionnaire de paquets par défaut sur RedHat, CentOS et Fedora (remplace yum depuis RHEL 8/Fedora 22).
- Gère l'installation, la désinstallation, la recherche, la mise à jour et les dépendances des paquets RPM.

### Manipulation des paquets avec dnf

```bash
# Rechercher un paquet
dnf search openssh

# Lister les paquets
dnf list openssh
dnf list openssh*
dnf list

# Obtenir des infos sur un paquet
dnf info httpd

# Installer un paquet et ses dépendances
dnf install httpd

# Mettre à jour les paquets
dnf update

# Supprimer un paquet
dnf remove openssh

# Supprimer un paquet et ses dépendances inutilisées
dnf autoremove openssh
```

### Manipulation des groupes de paquets

```bash
# Lister les groupes
dnf group list

# Infos sur un groupe
dnf group info "RPM Development Tools"

# Installer un groupe
dnf group install "RPM Development Tools"

# Supprimer un groupe
dnf group remove "RPM Development Tools"

# Mettre à jour un groupe
dnf group update "RPM Development Tools"
```

---

## Partie 3 : Dépôts

### Introduction

- Les dépôts sont des serveurs contenant des listes de logiciels disponibles.
- Les fichiers de configuration des dépôts se trouvent dans `/etc/yum.repos.d/`.

### Dépôts de RedHat 8

- **BaseOS** : Fournit les fonctionnalités de base du système.
- **AppStream** : Fournit des applications supplémentaires, langages, bases de données, etc.

### Modules

- Permettent d’installer une version spécifique d’une application.
- Commandes principales :

```bash
# Activer un stream spécifique
dnf module enable module:stream

# Installer un stream spécifique
dnf module install module:stream/profile

# Désactiver un module et supprimer ses paquets
dnf module remove module && dnf module disable module
```

#### Exemple : Installer PHP avec Apache

```bash
# Installer php et httpd
dnf install php httpd

# Démarrer et activer les services
systemctl start httpd
systemctl enable httpd
systemctl start php-fpm
systemctl enable php-fpm

# Ajouter le service http au pare-feu
firewall-cmd --add-service=http --permanent
firewall-cmd --reload

# Créer un fichier info.php dans /var/www/html/
echo "<?php phpinfo(); ?>" > /var/www/html/info.php
```

#### Gestion des versions de modules PHP

```bash
# Lister les modules php
dnf module list php

# Désactiver le module php (Stream 7.2)
dnf module disable php

# Activer le module php (Stream 7.4)
dnf module enable php:7.4

# Mettre à jour php
dnf upgrade php

# Rétrograder php vers 7.3
dnf module disable php
dnf module enable php:7.3
dnf downgrade php
```