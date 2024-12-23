<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Installer Nagios Core</a>


# 📚 Installer Nagios Core

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
</div>

**Mettre à jour le système**  
Avant chaque installation, il est important d'avoir un système à jour :
```
apt update && apt upgrade
```
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Installer les paquets nécessaires**  
Lancez l'installation des paquets nécessaires pour Nagios :
```
apt install unzip autoconf gcc libc6 make wget apache2 apache2-utils php libgd-dev openssl libssl-dev
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">



**Accéder à un répertoire temporaire**  
Placez vous dans le répertoire `tmp` :
```
cd /tmp
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Télécharger Nagios Core :**  
Téléchargez le paquet Nagios Core depuis Internet : 


```
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.9.tar.gz
```

**Info :** Dans l'exemple ci-dessus, nous installons la version la plus récente au moment où nous rédigeons cette documentation. Si vous voulez connaitre la dernière version, rendez-vous sur [Nagios Core Downloads](https://www.nagios.org/downloads/nagios-core/).

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Extrayez le dossier téléchargé :**

Une fois le téléchargement terminé, extrayez le fichier compressé : 

```
tar -xzvf nagios-4.5.9.tar.gz
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


**Placez vous dans le répertoire que vous venez d'extraire :**

```
cd nagios-4.5.9
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

Aller sur l'onglet hosts :  
![alt text](/assets/images/nagioshosts.png)

Aller sur l'onglet services : 
![alt text](/assets/images/nagiosservice.png)

Origine du problème : Nous avons installé et configuré Nagios, mais rien au sujet des plugins.

**Installez d’abord les paquets nécessaires :**  

```
apt install nagios-plugins
```
- L'installation des **plugins** est nécessaire, car ils contiennent les scripts exécutés localement qui fournissent les informations de supervision demandées.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Déplacez les plugins dans le bon répertoire :**  

```
mv /usr/lib/nagios/plugins/* /usr/local/nagios/libexec/
```
- Le paquet **nagios-plugins** installe tous les plugins dans le répertoire `/usr/lib/nagios/plugins/`
- Mais l'endroit le plus courant où Nagios attend ces plugins est `/usr/local/nagios/libexec/`  

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Transférez les droits à nagios :**
```
##### chown -R nagios:nagios /usr/local/nagios/libexec
```
On met **Nagios** comme propriétaire et groupe de ce répertoire, ainsi que de tous les fichiers qu’il contient, pour garantir que le service Nagios ait les permissions nécessaires pour exécuter les plugins correctement.

**Visualiser les onglets hosts/services :**



<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">



### **[↩️ Retour](../../linux/nagioscore-debian/index.md)**