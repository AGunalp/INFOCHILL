<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Superviser avec NRPE</a>


# 📚 Superviser des machines avec l'agent NRPE

<!-- Alerte importante concernant les droits d'utilisateur -->
<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  ⚠️ <strong>Important :</strong>
  <ul>
    <li>Ce guide part du principe que vous êtes connecté en tant que <code>root</code> (via <code>su -</code>).</li>
    <li>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande.</li>
  </ul>
</div>

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## Objectif

Après avoir installé et vérifié que votre serveur Nagios est en cours d'exécution ("Daemon running") via votre navigateur, l'objectif est de superviser efficacement vos machines et de collecter des informations sur leur état.

Pour cela, vous devez installer et configurer l'agent NRPE. Cet agent fait le lien entre votre serveur Nagios et les hôtes que vous voulez superviser. Il permet au serveur Nagios d'envoyer des commandes aux systèmes supervisés, qui à leur tour communiquent les résultats au serveur.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

# 🖥️ DEPUIS VOTRE SERVEUR NAGIOS

**Installez d’abord les paquets nécessaires :**  

```
apt install nagios-nrpe-server nagios-plugins
```
- L'installation de **l'agent NRPE** est indispensable, car c'est cet agent, (présent notament sur les machines qu'on veut superviser), qui permet l'échange des informations entre le serveur Nagios et les machines supervisées.
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
chown -R nagios:nagios /usr/local/nagios/libexec
```
On met **Nagios** comme propriétaire et groupe de ce répertoire, ainsi que de tous les fichiers qu’il contient, pour garantir que le service Nagios ait les permissions nécessaires pour exécuter les plugins correctement.


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Modifiez le fichier de configuration Nagios :**  

Nous devons maintenant indiquer à Nagios le chemin exact du répertoire où sont stockés les fichiers de configuration des éléments à superviser.
```
vim /usr/local/nagios/etc/nagios.cfg
```

- Décommentez (rendre actif) la ligne suivante :

  ```
  cfg_dir=/usr/local/nagios/etc/servers
  ```
Une fois activée, cette ligne précise à Nagios d’utiliser ce répertoire pour consulter les fichiers **.cfg** qui définissent les machines à superviser.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Créez le répertoire (si nécessaire) :**

Si le répertoire activé n'existe pas encore, créez-le manuellement avec la commande suivante :

```
mkdir -p /usr/local/nagios/etc/servers
```
- -p : Permet de créer notamment les répertoires parents si ils n'existent pas.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Changez les droits d'accès :**

Attribuez le répertoire nouvellement créé à l’utilisateur et au groupe Nagios pour qu’il puisse y accéder sans restriction :

```
chown nagios:nagios /usr/local/nagios/etc/servers
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## Choisissez le système à superviser :

<a href="https://infochill.com/linux/nagioscore-debian/supervision/machine-debian.html" class="button">
    <span class="icon">➤</span> Superviser un Système Linux (Debian)
</a>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/machine-windows.html" class="button">
    <span class="icon">➤</span> Superviser un Système Windows
</a>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/switch-cisco.html" class="button">
    <span class="icon">➤</span> Superviser un Switch Cisco
</a>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/switch-hp.html" class="button">
    <span class="icon">➤</span> Superviser un Switch HP
</a>


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### **[↩️ Retour](../../linux/nagioscore-debian/index.md)**