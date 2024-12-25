<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [Nagios Core](../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Installer Nagios Core</a>


<div style="background-color: #333; color: #fff; border-left: 5px solid #ff9900; border-right: 5px solid #ff9900; padding: 20px 25px; margin-bottom: 20px; text-align: center;">
  <strong style="font-size: 24px; color: #ff9900;">📚 INSTALLATION DE NAGIOS CORE SUR DEBIAN</strong>
</div>

<!-- Alerte importante concernant la distribution et les droits d'utilisateur -->
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
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS VOTRE SERVEUR NAGIOS :</strong>
</div>


### Pré-requis :

<br>

**Mettez à jour votre système**  
Avant chaque installation, il est important de s'assurer que le système est à jour.
```
apt update && apt upgrade
```
<br>

**Installez les paquets nécessaires**  
Avant d'installer Nagios, il est essentiel d'installer les paquets nécessaires au bon fonctionnement de Nagios et à son environnement.
```
apt install unzip autoconf gcc libc6 make wget apache2 apache2-utils php libgd-dev openssl libssl-dev
```
---

### Téléchargez Nagios Core :  
<br>

**Placez vous dans un répertoire temporaire :**
```
cd /tmp
```
<br>

**Téléchargez nagios :**
```
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.9.tar.gz
```

**Information :** Dans l'exemple ci-dessus, nous installons la version la plus récente au moment où nous rédigeons cette documentation. Si vous voulez connaitre la dernière version, rendez-vous sur [Nagios Core Downloads](https://www.nagios.org/downloads/nagios-core/).

<br>

**Extraire le dossier téléchargé :**

Une fois le téléchargement terminé, extrayez le fichier compressé : 

```
tar -xzvf nagios-4.5.9.tar.gz
```


---

### Configuration et installation de Nagios
<br>

**Placez vous dans le répertoire extrait :**
```
cd nagios-4.5.9
```
<br>

**Exécutez le script de configuration :**
```
./configure --with-httpd-conf=/etc/apache2/sites-enabled
```
<br>

**Compilez les fichiers :**

```
make all
```

<br>

**Créez le groupe et l'utilisateur Nagios sur le système :**
```
make install-groups-users
```
<br>

**Installez les fichiers de configuration et démarrez Nagios :**

Pour installer Nagios Core et le configurer correctement, vous devez exécuter plusieurs commandes. Cela inclut l'installation des fichiers principaux, la configuration du démarrage automatique de Nagios en tant que service, et l'installation des fichiers de configuration nécessaires pour l'interface web. Voici les commandes à exécuter : 

```
make install
make install-daemoninit
make install-commandmode
make install-config
make install-webconf
```
<br>

**Activez les modules nécessaires pour Apache :**

```
a2enmod rewrite
a2enmod cgi
```
<br>

**Créez un compte administrateur :**

Une fois l'installation terminée, vous devez créer un compte administrateur pour accéder à l'interface web de Nagios. Utilisez la commande suivante pour configurer les identifiants :

```
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Ici, le `-c` indique de créer un nouveau fichier pour stocker les identifiants (il n'est pas nécessaire de réutiliser cette option si vous créez d'autres utilisateurs). Le `nagiosadmin` est le nom du login.
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

<br>

**Redémarrez les services :**

Ensuite, redémarrez les services Apache et Nagios pour appliquer les modifications :

```
systemctl restart apache2
systemctl restart nagios
```
<br>

**Accédez à l'interface Nagios :**

Ouvrez un navigateur web et accédez à votre interface Nagios :

```
http://[adresse_IP]/nagios
```



























































### ETAT ACTUEL :

![alt text](/assets/images/interface_nagios.png)

![alt text](/assets/images/nagioshosts.png)  
  
![alt text](/assets/images/nagiosservice.png)

⚠️ Origine du problème : Nous avons installé et configuré Nagios, mais rien au sujet des plugins.

--- 

<div style="background-color: #333; color: #fff; border-left: 5px solid #ff9900; border-right: 5px solid #ff9900; padding: 20px 25px; margin-bottom: 20px; text-align: center;">
  <strong style="font-size: 24px; color: #ff9900;">📚 INSTALLATION DE PLUGINS EN LOCAL</strong>
</div>

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


**Visualiser les onglets hosts/services :**

![alt text](/assets/images/nagioshostsv.png)  

![alt text](/assets/images/nagiosservicesv.png)  




<div style="background-color: #f8d7da; color: #721c24; border-left: 5px solid #f5c6cb; padding: 15px; margin-top: 20px; text-align: center;">
  <strong>⚠️ IMPORTANT :</strong> Vérifiez les permissions des plugins pour vous assurer que Nagios dispose des permissions nécessaires pour les exécuter sur le serveur.
</div>


### **[↩️ Retour](../../linux/nagioscore-debian/index.md)**