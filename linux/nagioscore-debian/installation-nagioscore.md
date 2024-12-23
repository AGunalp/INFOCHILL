<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Installation Nagios Core</a>


# 📚 Installer Nagios Core sur Linux

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">

  <p>Ce guide suppose les éléments suivants :</p>
  <ul>
    <li><strong>Distribution :</strong> Vous utilisez la distribution <strong>Debian</strong>.</li>
    <li><strong>Accès administrateur :</strong> Vous êtes connecté en tant que <code>root</code> (via la commande <code>su -</code>).</li>
  </ul>
  <p>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande pour l'exécuter avec les privilèges administratifs.</p>
</div>


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 20px 25px; margin-bottom: 20px;">
  <strong style="font-size: 22px; color: #00bcd4;">🖥️ DEPUIS VOTRE SERVEUR NAGIOS :</strong>
  <p style="font-size: 16px; color: #ddd;">Suivez les étapes ci-dessous pour configurer et installer Nagios directement sur votre serveur. Ces instructions vous guideront à travers le processus d'installation avec les commandes nécessaires.</p>
</div>

**Mettez à jour votre système :**  
Pour commencer, assurez-vous que votre système est à jour. Exécutez la commande suivante :  
```
apt update && apt upgrade
```
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Exemple d'info-bulle :**

Voici un exemple de texte avec une info-bulle qui apparaît lorsque vous survolez le texte :

<span style="position: relative; cursor: pointer; text-decoration: underline;">
  Survolez ce texte
  <span style="visibility: hidden; width: 200px; background-color: #333; color: #fff; text-align: center; border-radius: 5px; padding: 8px; position: absolute; z-index: 1; bottom: 125%; left: 50%; margin-left: -100px; opacity: 0; transition: opacity 0.3s ease-in-out;" class="tooltip-text">
    Ceci est une info-bulle.
  </span>
</span>

<style>
  span[style*="cursor: pointer"] {
    position: relative;
  }

  span[style*="cursor: pointer"]:hover .tooltip-text {
    visibility: visible;
    opacity: 1;
  }
</style>


**Installez les paquets nécessaires :**  
Lancez l'installation des paquets nécessaires pour Nagios : 

```
apt install openssh-server
apt install autoconf gcc libc6 make wget unzip apache2 apache2-utils php libgd-dev
apt install openssl libssl-dev
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">



**Accédez dans le répertoire temporaire :**

Placez vous dans le répertoire `tmp` :

```
cd /tmp
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Téléchargez Nagios Core :**

Téléchargez le paquet Nagios Core depuis Internet : 

```
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.6.tar.gz
```

**Info :** Dans l'exemple ci-dessus, nous installons la version la plus récente au moment où nous rédigeons ce document. Si vous voulez connaitre la dernière version, rendez-vous sur [Nagios Core Downloads](https://www.nagios.org/downloads/nagios-core/).

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Extrayez le dossier téléchargé :**

Une fois le téléchargement terminé, extrayez le fichier compressé : 

```
tar -xzvf nagios-4.5.6.tar.gz
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Placez vous dans le répertoire que vous venez d'extraire :**

```
cd nagios-4.5.6
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Exécutez le script de configuration :**
```
./configure --with-httpd-conf=/etc/apache2/sites-enabled
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Compilez les fichiers :**

```
make all
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Créez le groupe et l'utilisateur Nagios sur le système :**
```
make install-groups-users
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Installez les fichiers de configuration et démarrez Nagios :**

Pour installer Nagios Core et le configurer correctement, vous devez exécuter plusieurs commandes. Cela inclut l'installation des fichiers principaux, la configuration du démarrage automatique de Nagios en tant que service, et l'installation des fichiers de configuration nécessaires pour l'interface web. Voici les commandes à exécuter : 

```
make install
make install-daemoninit
make install-commandmode
make install-config
make install-webconf
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Activez les modules nécessaires pour Apache :**

```
a2enmod rewrite
a2enmod cgi
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Créez un compte administrateur :**

Une fois l'installation terminée, vous devez créer un compte administrateur pour accéder à l'interface web de Nagios. Utilisez la commande suivante pour configurer les identifiants :

```
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Ici, le `-c` indique de créer un nouveau fichier pour stocker les identifiants (il n'est pas nécessaire de réutiliser cette option si vous créez d'autres utilisateurs). Le `nagiosadmin` est le nom du login.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Redémarrez les services :**

Ensuite, redémarrez les services Apache et Nagios pour appliquer les modifications :

```
systemctl restart apache2
systemctl restart nagios
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Accédez à l'interface Nagios :**

Ouvrez un navigateur web et accédez à votre interface Nagios à l'adresse suivante (en remplaçant par l'IP de votre serveur) :

```
http://[votre-serveur]/nagios
```

Par exemple (pour mon cas) :

```
http://192.168.1.200/nagios
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


Si tout s'est bien passé, vous devriez voir l'interface de gestion de Nagios, comme illustré ci-dessous :

![alt text](/assets/images/interface_nagios.png)

---


### **[↩️ Retour](../../linux/nagioscore-debian/index.md)**