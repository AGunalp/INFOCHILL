<link rel="stylesheet" type="text/css" href="/assets/css/blue-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../../index.md) > [NagiosCore sur Debian](../index.md) > [Installation de Nagios Core sur Debian](installation-nagioscore.md)

# 📚 Installation de Nagios Core sur Debian

Vous êtes actuellement dans le guide d'installation de **Nagios Core** sur Debian. Suivez les étapes ci-dessous pour compléter l'installation.

---

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  ⚠️ <strong>Important :</strong>
  <ul>
    <li>Ce guide part du principe que vous êtes connecté en tant que <code>root</code> (via <code>su -</code>).</li>
    <li>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande.</li>
  </ul>
</div>

---

Pour commencer, assurez-vous que votre système est à jour. Exécutez la commande suivante : 

```bash
apt update && apt upgrade
```

Lancez l'installation des paquets nécessaires pour Nagios :

```bash
apt install openssh-server
apt install autoconf gcc libc6 make wget unzip apache2 apache2-utils php libgd-dev
apt install openssl libssl-dev
```

Accédez à un répertoire temporaire

```bash
cd /tmp
```

Téléchargez le fichier pour installer Nagios Core depuis Internet :

```bash
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.6.tar.gz
```

**Info importante :** Dans l'exemple ci-dessus, nous installons la version la plus récente au moment où nous rédigeons ce document. Si vous voulez installer la dernière version, rendez-vous sur [Nagios Core Downloads](https://www.nagios.org/downloads/nagios-core/).

Une fois le téléchargement terminé, extrayez le fichier compressé : 

```bash
tar -xzvf nagios-4.5.6.tar.gz
```

Naviguez dans le répertoire que vous venez d'extraire : 

```bash
cd nagios-4.5.6
```

Exécutez le script de configuration de Nagios Core : 

```bash
./configure --with-httpd-conf=/etc/apache2/sites-enabled
```

Compilez les fichiers avec la commande suivante : 

```bash
make all
```

Créez le groupe et l'utilisateur Nagios sur le système : 

```bash
make install-groups-users
```

Installez les fichiers principaux : 

```bash
make install
```

Pour installer Nagios Core et le configurer correctement, vous devez exécuter plusieurs commandes. Cela inclut l'installation des fichiers principaux, la configuration du démarrage automatique de Nagios en tant que service, et l'installation des fichiers de configuration nécessaires pour l'interface web. Voici les commandes à exécuter : 

```bash
make install
make install-daemoninit
make install-commandmode
make install-config
make install-webconf
```

Activez les modules nécessaires pour Apache : 

```bash
a2enmod rewrite
a2enmod cgi
```

Une fois l'installation terminée, vous devez créer un compte administrateur pour accéder à l'interface web de Nagios. Utilisez la commande suivante pour configurer les identifiants :

```bash
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Ici, le `-c` indique de créer un nouveau fichier pour stocker les identifiants (il n'est pas nécessaire de réutiliser cette option si vous créez d'autres utilisateurs). Le `nagiosadmin` est le nom du login.

Ensuite, redémarrez les services Apache et Nagios pour appliquer les modifications :

```bash
systemctl restart apache2
systemctl restart nagios
```

Ouvrez un navigateur web et accédez à votre interface Nagios à l'adresse suivante (en remplaçant par l'IP de votre serveur) :
```bash
http://[votre-serveur]/nagios
```
Par exemple (pour mon cas) :
```bash
http://192.168.13.2/nagios
```
Si tout s'est bien passé, vous devriez voir l'interface de gestion de Nagios, comme illustré ci-dessous :

![alt text](/assets/images/interface_nagios.png)