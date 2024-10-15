<link rel="stylesheet" type="text/css" href="/assets/css/blue-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > [Superviser avec NRPE](supervision-nrpe.md)

# 📚 Superviser des machines avec le plugin NRPE

Bienvenue dans la section dédiée à l'installation et à la configuration du plugin **NRPE**. Vous êtes actuellement dans le guide d'installation du plugin NRPE pour Nagios.

---

<!-- Alerte importante concernant les droits d'utilisateur -->
<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  ⚠️ <strong>Important :</strong>
  <ul>
    <li>Ce guide part du principe que vous êtes connecté en tant que <code>root</code> (via <code>su -</code>).</li>
    <li>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande.</li>
  </ul>
</div>

---

## Objectif

Après avoir terminé l'installation et vérifié que votre serveur Nagios est en "Daemon running" via votre navigateur, l'objectif est désormais de comprendre comment superviser efficacement vos machines et collecter des informations sur leur état. À savoir : Pour faire un lien Nagios entre notre serveur Nagios et un hôte (cible que nous allons remonter sur Nagios), il faut que dans les deux machines nous installions le plugin NRPE et que nous le configurions. 

---

## 🖥️ Depuis votre serveur Nagios

Depuis votre serveur Nagios, installez d’abord ces paquets :

```bash
apt install nagios-nrpe-server nagios-plugins
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Copier les plugins nécessaires**

Copiez tous les fichiers de `/usr/lib/nagios/plugins/*` vers le répertoire `/usr/local/nagios/libexec/`. C'est ici que Nagios attend les plugins nécessaires pour effectuer les vérifications sur les machines distantes.

```bash
cp /usr/lib/nagios/plugins/* /usr/local/nagios/libexec/
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Modifier le fichier de configuration Nagios**

Vous allez maintenant modifier le fichier `nagios.cfg` pour activer un paramètre nécessaire à la surveillance des machines distantes. Cela permettra à Nagios d'utiliser les plugins NRPE et d'assurer une communication efficace avec les clients NRPE.

```bash
vim /usr/local/nagios/etc/nagios.cfg
```

- Décommentez (rendez actif) la ligne suivante :

  ```bash
  cfg_dir=/usr/local/nagios/etc/servers
  ```
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Créer le répertoire des serveurs (si nécessaire)**

Si le répertoire `/usr/local/nagios/etc/servers` n'existe pas encore, créez-le manuellement. Cela permettra à Nagios de stocker les fichiers de configuration des hôtes et services.

```bash
mkdir -p /usr/local/nagios/etc/servers
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Changer les droits d'accès**

Après avoir créé le répertoire, il est important de transmettre le répertoire à l'utilisateur et au groupe nagios : 

```bash
chown -R nagios:nagios /usr/local/nagios/etc/servers
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## Choisissez le système à superviser :

- [**Système Linux (Debian)**](./supervision/linux-debian.md)  
  Pour surveiller un serveur ou une machine Debian (Linux).

- [**Système Windows**](./supervision/windows.md)  
  Pour surveiller des postes ou serveurs sous Windows.

- [**Switch Cisco**](./supervision/switch-cisco.md)  
  Pour superviser des switches réseau de marque Cisco.

- [**Switch HP**](./supervision/switch-hp.md)  
  Pour superviser des switches réseau de marque HP.
