<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Superviser avec NRPE</a>


# 📚 Superviser des machines avec le plugin NRPE

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

Après avoir terminé l'installation et vérifié que votre serveur Nagios est en "Daemon running" via votre navigateur, l'objectif est désormais de comprendre comment superviser efficacement vos machines et collecter des informations sur leur état. À savoir : Pour faire un lien Nagios entre notre serveur Nagios et un hôte (cible que nous allons remonter sur Nagios), il faut que dans les deux machines nous installions le plugin NRPE et que nous le configurions. 

---

## 🖥️ Depuis votre serveur Nagios

Depuis votre serveur Nagios, installez d’abord ces paquets :

```
apt install nagios-nrpe-server nagios-plugins
```



**Copier les plugins nécessaires**

Copiez tous les fichiers de `/usr/lib/nagios/plugins/*` vers le répertoire `/usr/local/nagios/libexec/`. C'est ici que Nagios attend les plugins nécessaires pour effectuer les vérifications sur les machines distantes.

```
cp /usr/lib/nagios/plugins/* /usr/local/nagios/libexec/
```


**Modifier le fichier de configuration Nagios**

Vous allez maintenant modifier le fichier `nagios.cfg` pour activer un paramètre nécessaire à la surveillance des machines distantes. Cela permettra à Nagios d'utiliser les plugins NRPE et d'assurer une communication efficace avec les clients NRPE.

```
vim /usr/local/nagios/etc/nagios.cfg
```

- Décommentez (rendez actif) la ligne suivante :

  ```
  cfg_dir=/usr/local/nagios/etc/servers
  ```


**Créer le répertoire des serveurs (si nécessaire)**

Si le répertoire `/usr/local/nagios/etc/servers` n'existe pas encore, créez-le manuellement. Cela permettra à Nagios de stocker les fichiers de configuration des hôtes et services.

```
mkdir -p /usr/local/nagios/etc/servers
```



**Changer les droits d'accès**

Après avoir créé le répertoire, il est important de transmettre le répertoire à l'utilisateur et au groupe nagios : 

```
chown -R nagios:nagios /usr/local/nagios/etc/servers
```



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