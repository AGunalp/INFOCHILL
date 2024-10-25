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
- L'installation de **plugins** est notamment nécessaire car ce sont des scripts exécutés localement (comme on l'installe ces plugins sur notre serveur nagios, alors on pourra executer ces scripts sur celui-ci).

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Déplacez les plugins dans le bon répertoire :**  

```
mv /usr/lib/nagios/plugins/* /usr/local/nagios/libexec/
```
**Transférez les droits à nagios :**
```
chown nagios:nagios /usr/local/nagios/libexec/*
```
<div style="border: 1px solid #007BFF; border-radius: 5px; padding: 10px; margin: 1em 0;">
    <strong>💡 À SAVOIR :</strong><br>
    - Le paquet <strong>nagios-plugins</strong> installe tous les plugins dans le répertoire 
 <code>/usr/lib/nagios/plugins/</code><br>
    - Mais l'endroit le plus courant où Nagios attend ces plugins est <code>/usr/local/nagios/libexec/</code>
</div>

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Modifiez le fichier de configuration Nagios :**  
Nous avons maintenant besoin d'indiquer à Nagios quel répertoire utiliser pour déterminer les éléments à superviser. Il est donc essentiel de préciser à Nagios le chemin du répertoire où il doit consulter nos fichiers de configuration.


**Editez ce fichier :**
```
vim /usr/local/nagios/etc/nagios.cfg
```

- Décommentez (rendre actif) la ligne suivante :

  ```
  cfg_dir=/usr/local/nagios/etc/servers
  ```
Nagios saura ainsi qu'il doit consulter ce répertoire pour récupérer les informations nécessaires à la supervision des machines.

En résumé, c'est dans ce répertoire (la ligne qu'on a rendu actif) que vous devez définir les machines à superviser, en créant des fichiers avec l'extension **.cfg**.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Créez le répertoire des serveurs (si nécessaire) :**

Si le répertoire `/usr/local/nagios/etc/servers` n'existe pas encore, créez-le manuellement.

```
mkdir -p /usr/local/nagios/etc/servers
```
- -p : Permet de créer notamment les répertoires parents si ils n'existent pas.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Changez les droits d'accès :**

Après avoir créé le répertoire, nous allons le transmettre à l'utilisateur et au groupe nagios : 

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