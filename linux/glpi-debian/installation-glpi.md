<link rel="stylesheet" type="text/css" href="/assets/css/blue-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [GLPI Debian](../glpi-debian/index.md) > [Installation GLPI Debian](installation-glpi.md)

# 📚 Installation et Configuration de GLPI sur Debian

Vous êtes actuellement dans le guide d'installation de **GLPI** sur Debian. Suivez les étapes ci-dessous pour installer et configurer le serveur GLPI avec MariaDB.

---

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  ⚠️ <strong>Important :</strong>
  <ul>
    <li>Ce guide part du principe que vous êtes connecté en tant que <code>root</code> (via <code>su -</code>).</li>
    <li>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande.</li>
  </ul>
</div>

---

**Installer les paquets nécessaires**

Pour commencer, installez les paquets requis pour faire de votre serveur une machine prête à héberger GLPI. Vous aurez besoin d'Apache2 pour le serveur web, et de PHP pour l'interprétation des scripts :

```
apt install apache2 && apt install php
```

---

**Télécharger GLPI**

Accédez à un répertoire temporaire pour télécharger l'archive de GLPI :

```
cd /tmp
```
```
wget https://github.com/glpi-project/glpi/releases/download/10.0.16/glpi-10.0.16.tgz
```

---

**Extraire le fichier téléchargé**

Une fois l'archive téléchargée, extrayez son contenu :

```
tar -xvzf glpi-10.0.16.tgz
```

---

**Déplacer GLPI dans le répertoire web**

Déplacez le dossier extrait vers le répertoire où Apache pourra rendre GLPI accessible via un navigateur :

```
mv /tmp/glpi /var/www/html/
```

---

**Ajuster les permissions**

Pour permettre à Apache2 d’accéder et de manipuler correctement les fichiers de GLPI, changez le propriétaire du dossier :

```
chown -R www-data:www-data /var/www/html/glpi/
```

---

**Installer les extensions PHP nécessaires**

GLPI nécessite plusieurs extensions PHP pour fonctionner correctement. Installez-les avec la commande suivante :

```
apt install php-mysqli php-curl php-gd php-intl php-xml
```

---

**Redémarrer les services**

Redémarrez Apache après avoir installé les extensions PHP :

```
systemctl restart apache2
```

---

**Configurer la base de données MariaDB**

Lorsque vous arrivez à l'étape de la configuration de la base de données dans l'interface GLPI, vous devez indiquer l'adresse IP de votre serveur MariaDB, ainsi que les informations d'utilisateur et de mot de passe associés.

[Voir comment installer et configurer une base de données MariaDB](../mariadb-configuration/index.md)

---

**Reprendre l'installation de GLPI**

Une fois la base de données et l'utilisateur créés, renseignez les informations dans l'interface de GLPI :

- **Serveur SQL MariaDB ou MySQL :** l'IP de votre serveur MariaDB.
- **Utilisateur GLPI :** le nom d'utilisateur créé pour la base de données.
- **Mot de passe :** le mot de passe associé.

---

**Sélectionner la base de données**

Sur la page suivante, sélectionnez votre base de données existante.

---

**Connexion à GLPI**

Une fois l'installation terminée, vous arrivez à la page de connexion. Le compte super-administrateur par défaut est `glpi` avec le mot de passe `glpi`. Utilisez ces identifiants pour accéder à toutes les fonctionnalités de gestion de GLPI.

![alt text](/assets/images/interface_glpi.png)
