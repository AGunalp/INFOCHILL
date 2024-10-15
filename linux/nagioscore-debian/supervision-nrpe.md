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

<style>
    body {
        font-family: 'Arial', sans-serif; /* Police de base */
        background-color: #1a1a1a; /* Couleur de fond pour un contraste */
    }

    .button {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%; /* Prendre toute la largeur du conteneur */
        height: 48px; /* Hauteur du bouton */
        background-color: transparent; /* Pas de couleur de fond */
        color: white; /* Couleur du texte */
        text-decoration: none; /* Pas de soulignement */
        border: 2px solid #ffffff; /* Contour blanc */
        border-radius: 7px; /* Coins arrondis */
        margin: 5px 0; /* Marges verticales */
        transition: background-color 0.3s, color 0.3s, transform 0.3s; /* Transition de couleur et transform */
        font-size: 17px; /* Taille de police plus grande */
        font-weight: 500; /* Poids de police moyen */
        text-transform: uppercase; /* Texte en majuscules pour un look moderne */
        letter-spacing: 1px; /* Espacement des lettres */
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2), inset 0 1px 5px rgba(255, 255, 255, 0.5); /* Ombre externe et interne */
        background-clip: padding-box; /* Pour le dégradé de bordure */
    }

    .button:hover {
        background-color: rgba(255, 255, 255, 0.2); /* Couleur au survol (semi-transparent) */
        color: white; /* Garder la couleur blanche au survol */
        text-decoration: none; /* Pas de soulignement au survol */
        transform: scale(1.01); /* Légère augmentation de taille au survol */
    }

    .icon {
        margin-right: 10px; /* Espace entre l'icône et le texte */
        font-size: 15px; /* Taille de l'icône légèrement plus grande */
    }
</style>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/linux-debian.html" class="button">
    <span class="icon">➤</span> Superviser un Système Linux (Debian)
</a>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/windows.html" class="button">
    <span class="icon">➤</span> Superviser un Système Windows
</a>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/switch-cisco.html" class="button">
    <span class="icon">➤</span> Superviser un Switch Cisco
</a>

<a href="https://infochill.com/linux/nagioscore-debian/supervision/switch-hp.html" class="button">
    <span class="icon">➤</span> Superviser un Switch HP
</a>


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > [Superviser avec NRPE](supervision-nrpe.md)

<p style="text-align: right; margin: 20px 0;">
    <a href="https://infochill.com" style="display: inline-block; padding: 8px 12px; background-color: #003366; color: white; text-decoration: none; border: 2px solid white; border-radius: 4px; font-weight: bold;">
        Retour à l'Accueil
    </a>
</p>
