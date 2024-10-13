<link rel="stylesheet" type="text/css" href="/assets/css/blue-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore sur Debian](../../index.md) > [Superviser des machines avec le plugin NRPE](superviser-nrpe.md)

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
Après avoir terminé l'installation et vérifié que votre serveur Nagios est en "Daemon running" via votre navigateur, l'objectif est désormais de comprendre comment superviser efficacement vos machines et collecter des informations sur leur état. À savoir : Pour faire un lien Nagios entre notre serveur Nagios et un hôte (cible que nous allons remonter sur Nagios), il faut que dans les deux machines nous installions le plugin NRPE et que nous le configurions.  

## 🖥️ Depuis votre serveur Nagios
Depuis votre serveur Nagios, installez d’abord ces paquets :

```bash
apt install nagios-nrpe-server nagios-plugins
```

Copiez tous les fichiers de `/usr/lib/nagios/plugins/*` vers le répertoire `/usr/local/nagios/libexec/`. C'est ici que Nagios attend les plugins nécessaires pour effectuer les vérifications sur les machines distantes.

```bash
cp /usr/lib/nagios/plugins/* /usr/local/nagios/libexec/
```

Vous allez maintenant modifier le fichier `nagios.cfg` pour activer un paramètre nécessaire à la surveillance des machines distantes. Cela permettra à Nagios d'utiliser les plugins NRPE et d'assurer une communication efficace avec les clients NRPE.
```bash
vim /usr/local/nagios/etc/nagios.cfg
```

Décommentez (rendez actif) la ligne suivante :

```bash
cfg_dir=/usr/local/nagios/etc/servers
```

Si le répertoire `/usr/local/nagios/etc/servers` n'existe pas encore, créez-le manuellement. Cela permettra à Nagios de stocker les fichiers de configuration des hôtes et services.

```bash
mkdir -p /usr/local/nagios/etc/servers
```

Après avoir créé le répertoire, il est important de transmettre le répertoire à l'utilisateur et au groupe nagios : 

```bash
chown -R nagios:nagios /usr/local/nagios/etc/servers
```

## Sélectionnez le type de système à superviser :

<!-- Styles CSS intégrés pour les boutons -->
<style>
    .button {
        display: block; /* Affiche le bouton comme un bloc pour qu'il prenne toute la largeur */
        width: 100%; /* Prend toute la largeur de la page */
        border: 2px solid white; /* Contour blanc */
        border-radius: 5px; /* Arrondir les coins */
        padding: 15px; /* Espacement intérieur */
        text-align: center; /* Centrer le texte */
        cursor: pointer; /* Changer le curseur pour indiquer que c'est cliquable */
        margin: 10px 0; /* Espacement vertical entre les boutons */
        transition: background-color 0.3s; /* Animation de transition pour le survol */
        color: white; /* Couleur du texte */
        text-decoration: none; /* Pas de soulignement */
    }

    .button:hover {
        background-color: rgba(255, 255, 255, 0.1); /* Changement de couleur au survol */
    }

    /* Ajout de cette règle pour empêcher le changement de couleur par défaut des liens */
    .button:visited,
    .button:link {
        color: white; /* Couleur du texte pour les liens */
    }

    .button:focus {
        outline: none; /* Supprimer le contour par défaut au focus */
    }
</style>

<!-- Les boutons -->
<a class="button" href="linux-debian.md" target="_blank">Superviser un Système Linux (Debian)</a>

<a class="button" href="windows.md">Superviser un Système Windows</a>
<a class="button" href="switch-cisco.md">Superviser un Switch Cisco</a>
<a class="button" href="switch-hp.md">Superviser un Switch HP</a>
