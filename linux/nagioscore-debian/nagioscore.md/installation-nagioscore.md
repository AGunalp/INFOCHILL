<link rel="stylesheet" type="text/css" href="/assets/css/light-theme.css">

# Installation de Nagios Core sur Debian

Pour commencer, assurez-vous que votre système est à jour. Exécutez la commande suivante : 

```bash
sudo apt update && sudo apt upgrade
```

Lancez l'installation des paquets nécessaires pour Nagios :

```bash
sudo apt install openssh-server
sudo apt install autoconf gcc libc6 make wget unzip apache2 apache2-utils php libgd-dev
sudo apt install openssl libssl-dev
```

Accédez à un répertoire temporaire

```bash
cd /tmp
```

Téléchargez le fichier pour installer Nagios Core depuis Internet :

```bash
sudo wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.6.tar.gz
```

**Info importante :** Dans l'exemple ci-dessus, nous installons la version la plus récente au moment où nous rédigeons ce document. Si vous voulez installer la dernière version, rendez-vous sur [Nagios Core Downloads](https://www.nagios.org/downloads/nagios-core/).

Une fois le téléchargement terminé, extrayez le fichier compressé : 

```bash
sudo tar -xzvf nagios-4.5.6.tar.gz
```

Naviguez dans le répertoire que vous venez d'extraire : 

```bash
cd nagios-4.5.6
```

Exécutez le script de configuration de Nagios Core : 

```bash
sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
```

Compilez les fichiers avec la commande suivante : 

```bash
sudo make all
```

Créez le groupe et l'utilisateur Nagios sur le système : 

```bash
sudo make install-groups-users
```

Installez les fichiers principaux : 

```bash
sudo make install
```

Pour installer Nagios Core et le configurer correctement, vous devez exécuter plusieurs commandes. Cela inclut l'installation des fichiers principaux, la configuration du démarrage automatique de Nagios en tant que service, et l'installation des fichiers de configuration nécessaires pour l'interface web. Voici les commandes à exécuter : 

```bash
sudo make install
sudo make install-daemoninit
sudo make install-commandmode
sudo make install-config
sudo make install-webconf
```

Activez les modules nécessaires pour Apache : 

```bash
sudo a2enmod rewrite
sudo a2enmod cgi
```

L'installation est terminée, mais vous devez encore créer des logins pour obtenir des permissions admin (pour accéder à l'interface de gestion): 

```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Ici, le `-c` indique de créer un nouveau fichier pour stocker les identifiants (il n'est pas nécessaire de réutiliser cette option si vous créez d'autres utilisateurs). Le `nagiosadmin` est le nom du login.

Terminez par le redémarrage des services : 

```bash
sudo systemctl restart apache2
sudo systemctl restart nagios
```

Vous pouvez maintenant tenter d'accéder à votre interface web depuis le navigateur (pour mon cas) : 

```bash
http://192.168.13.2/nagios
```