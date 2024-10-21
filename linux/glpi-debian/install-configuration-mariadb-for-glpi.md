<link rel="stylesheet" type="text/css" href="/assets/css/blue-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [GLPI Debian](../glpi-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Installation MariaDB</a>


# 📚 Installation de MariaDB

Vous êtes actuellement dans le guide d'installation de **MariaDB** sur Debian. Suivez les étapes ci-dessous pour installer MariaDB (pour GLPI).

---

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  ⚠️ <strong>Important :</strong>
  <ul>
    <li>Ce guide part du principe que vous êtes connecté en tant que <code>root</code> (via <code>su -</code>).</li>
    <li>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande.</li>
  </ul>
</div>

---

## Installation de MariaDB

Vous trouverez ci-dessous les étapes nécessaires pour installer MariaDB sur votre système, afin de préparer l'installation de GLPI.

---

### Étapes à Suivre

**Mettre à jour le système**  
Assurez-vous que votre système est à jour pour éviter les problèmes de compatibilité.

```bash
apt update && apt upgrade
```

---

**Installer MariaDB**  
Installez le serveur MariaDB ainsi que le client.

```bash
apt install mariadb-server mariadb-client
```

---

**Vérifier l'installation**  
Vérifiez que MariaDB est bien installé.

```bash
mariadb --version
```

---

**Démarrer et activer le service**  
Démarrez le service MariaDB et activez-le pour qu'il se lance au démarrage.

```bash
systemctl enable mariadb
systemctl start mariadb
```

---

**Vérifier le statut du service**  
Assurez-vous que le service MariaDB fonctionne correctement.

```bash
systemctl status mariadb
```
---

**Configurer l'accès distant (facultatif)**  
Si vous souhaitez permettre des connexions à distance à votre serveur MariaDB, vous devez modifier le fichier de configuration de MariaDB.

1. Ouvrez le fichier de configuration `my.cnf` :

   ```bash
   vim /etc/mysql/mariadb.conf.d/50-server.cnf
   ```

2. Recherchez la ligne contenant `bind-address`. Par défaut, elle est souvent définie sur `127.0.0.1` (localhost). Changez-la pour permettre des connexions externes :

   ```plaintext
   bind-address = 0.0.0.0
   ```
3. On oublie pas de restart : 
   ```plaintext
   systemctl restart mariadb
   ```
---

**Se connecter à MariaDB**  
Connectez-vous à la base de données avec l'utilisateur root.

```bash
mysql -u root -p
```

---

**Créer une base de données pour GLPI**  
Créez une base de données nommée `glpi`.

```
CREATE DATABASE glpi;
```

---

**Créer un utilisateur et lui accorder des permissions**  
Créez l'utilisateur 'glpiuser' et définissez son mot de passe.

```
CREATE USER 'glpiuser'@'%' IDENTIFIED BY 'votremotdepasse';
```

---

**Accorder les droits nécessaires**  
Accordez à l'utilisateur tous les privilèges nécessaires sur la base de données `glpi`.

```
GRANT ALL PRIVILEGES ON glpi.* TO 'glpiuser'@'localhost';
```

---

**Appliquer les modifications**  
Rechargez les privilèges pour appliquer les changements.

```bash
FLUSH PRIVILEGES;
```

**Déconnexion de la base de données**
Vous pouvez maintenant vous déconnecter de la base de données.
```bash
exit
```

---

### Autres Commandes Essentielles à Connaitre

Voici quelques commandes supplémentaires qui pourraient vous être utiles :

- **Afficher les bases de données :**
  ```bash
  SHOW DATABASES;
  ```

- **Supprimer une base de données :**
  ```
  DROP DATABASE nom_dune_base;
  ```

- **Sélectionner une base de données :**
  ```
  USE nom_dune_base;
  ```

- **Afficher les tables d'une BDD : (à executer que si on a selectionné une BDD)**
  ```
  SHOW TABLES;
  ```
- **Vérifier la configuration d'un utilisateur :**
  ```
  SHOW GRANTS FOR 'nom_utilisateur'@'localhost';
  ```

---

<p style="text-align: right; margin: 20px 0;">
    <a href="javascript:history.back()" style="display: inline-block; padding: 8px 12px; background-color: #ff9900; color: white; text-decoration: none; border: 2px solid white; border-radius: 4px; font-weight: bold; margin-right: 10px;">
        Retour en arrière
    </a>
</p>
