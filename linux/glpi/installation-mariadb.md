<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [GLPI Debian](../glpi/index.md) > <a href="" style="color: #ff9900; text-decoration: underline;">Installation MariaDB</a>


# 📚 Installation de MariaDB

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  ⚠️ <strong>Important :</strong>
  <ul>
    <li>Ce guide part du principe que vous êtes connecté en tant que <code>root</code> (via <code>su -</code>).</li>
    <li>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande.</li>
  </ul>
</div>

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Étapes à Suivre

**Mettre à jour le système**  
Assurez-vous que votre système est à jour pour éviter les problèmes de compatibilité.

```
apt update && apt upgrade
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Installer MariaDB**  
Installez le serveur MariaDB ainsi que le client.

```
apt install mariadb-server mariadb-client
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Vérifier l'installation**  
Vérifiez que MariaDB est bien installé.

```
mariadb --version
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Démarrer et activer le service**  
Démarrez le service MariaDB et activez-le pour qu'il se lance au démarrage.

```
systemctl enable mariadb
systemctl start mariadb
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Vérifier le statut du service**  
Assurez-vous que le service MariaDB fonctionne correctement.

```
systemctl status mariadb
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Configurer l'accès distant (facultatif)**  
Si vous souhaitez permettre des connexions à distance à votre serveur MariaDB, vous devez modifier le fichier de configuration de MariaDB.

1. Ouvrez le fichier de configuration `my.cnf` :

   ```
   vim /etc/mysql/mariadb.conf.d/50-server.cnf
   ```

2. Recherchez la ligne contenant `bind-address`. Par défaut, elle est souvent définie sur `127.0.0.1` (localhost). Changez-la pour permettre des connexions externes :

   ```
   bind-address = 0.0.0.0
   ```
3. On oublie pas de restart : 
   ```
   systemctl restart mariadb
   ```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Se connecter à MariaDB**  
Connectez-vous à la base de données avec l'utilisateur root.

```
mysql -u root -p
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Créer une base de données pour GLPI**  
Créez une base de données nommée `glpi`.

```
CREATE DATABASE glpi;
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Créer un utilisateur et lui accorder des permissions**  
Créez l'utilisateur 'glpiuser' et définissez son mot de passe.

```
CREATE USER 'glpiuser'@'%' IDENTIFIED BY 'votremotdepasse';
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Accorder les droits nécessaires**  
Accordez à l'utilisateur tous les privilèges nécessaires sur la base de données `glpi`.

```
GRANT ALL PRIVILEGES ON glpi.* TO 'glpiuser'@'%';
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Appliquer les modifications**  
Rechargez les privilèges pour appliquer les changements.

```
FLUSH PRIVILEGES;
```

**Déconnexion de la base de données**
Vous pouvez maintenant vous déconnecter de la base de données.
```
exit
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

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

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### **[↩️ Retour](../glpi/index.md)**
