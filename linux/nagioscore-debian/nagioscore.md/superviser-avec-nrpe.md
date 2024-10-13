<link rel="stylesheet" type="text/css" href="/assets/css/light-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../../index.md) > [NagiosCore sur Debian](../index.md) > [Superviser des machines avec le plugin NRPE](superviser-avec-nrpe.md)

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


<details>

 Superviser un Système Linux (Debian)





## 🖥️ Depuis un Système Linux (Debian)

  Pour superviser un système Linux (Debian) avec le plugin NRPE, suivez les étapes ci-dessous. Cela vous permettra de configurer efficacement la machine afin qu'elle soit surveillée par votre serveur Nagios.

  ### Étapes à Suivre

  1. **Mettre à jour le système :**
     Assurez-vous que votre système est à jour pour éviter les problèmes de compatibilité.

     ```bash
     apt update && apt upgrade
     ```

  2. **Installer les paquets nécessaires :**
     Installez le serveur NRPE et les plugins Nagios.

     ```bash
     apt install nagios-nrpe-server nagios-plugins
     ```

  3. **Modifier le fichier de configuration NRPE :**
     Ouvrez le fichier de configuration NRPE pour autoriser les connexions depuis votre serveur Nagios.

     ```bash
     vim /etc/nagios/nrpe.cfg
     ```

     - **Configurer les adresses IP autorisées :**
       Ajoutez l'adresse IP de votre serveur Nagios à la ligne suivante (par exemple, pour l'IP `192.168.13.2`):

       ```bash
       allowed_hosts=127.0.0.1,::1,192.168.13.2
       ```

  4. **Redémarrer le service NRPE :**
     Appliquez vos modifications en redémarrant le service NRPE.

     ```bash
     systemctl restart nagios-nrpe-server.service
     ```

  ## 🖥️ Retournez sur Nagios pour définir des Hôtes

  Après avoir configuré votre machine Debian pour NRPE, vous devez maintenant définir cet hôte sur votre serveur Nagios. Cela permettra à Nagios de commencer à surveiller la machine.

<div style="border: 1px solid #007BFF; border-radius: 5px; padding: 10px; margin: 1em 0;">
    <strong>📝 Méthodes de Configuration</strong>
    <p>Il existe deux approches pour gérer les fichiers de configuration des hôtes dans Nagios :</p>
    <ol>
        <li><strong>Un seul fichier .cfg :</strong> Regroupez toutes les machines dans un seul fichier. Cette méthode peut rendre la gestion plus complexe.</li>
        <li><strong>Fichiers séparés :</strong> Créez un fichier .cfg pour chaque machine. C'est la méthode recommandée car elle facilite la gestion et la maintenance.</li>
    </ol>
    <p>Dans ce guide, nous allons opter pour la méthode des <strong>fichiers séparés</strong>.</p>
</div>


  #### Création du Fichier de Configuration pour l'Hôte (SrvDeb)

  1. **Créer le fichier de configuration :**
     Accédez au répertoire approprié et créez le fichier pour votre machine (SrvDeb).

     ```bash
     touch /usr/local/nagios/etc/servers/SrvDeb.cfg
     ```

  2. **Éditer le fichier :**
     Ouvrez le fichier créé pour ajouter les informations nécessaires.

     ```bash
     vim /usr/local/nagios/etc/servers/SrvDeb.cfg
     ```

  3. **Ajouter les définitions de l'hôte :**
     Insérez le code suivant dans le fichier :

     ```plaintext
     define host {
         use                     linux-server          ; Modèle prédéfini pour les serveurs Linux
         host_name               SrvDeb                ; Nom de l'hôte
         alias                   Serveur de Test       ; Alias pour afficher dans Nagios
         address                 192.168.13.2          ; Adresse IP de la machine
         max_check_attempts      5                     ; Nombre de tentatives avant une alerte
         check_period            24x7                  ; Vérification continue
         notification_interval    30                   ; Intervalle de notification
         notification_period     24x7                  ; Période de notification
     }
     ```
#### Redémarrez vos services :

```bash
systemctl restart apache2
systemctl restart nagios
```
Cliquez sur l'onglet `host` à gauche, vous pouvez maintenant voir votre machine qui y est référenciée, pour mon cas j'ai remonté une machine debian ayant pour nom `AP4-GLPI` :

![alt text](/assets/images/host_debian_nagios.png)

---

### Récapitulatif des Étapes de Configuration de Nagios et NRPE

#### Sur le Serveur Nagios (étape précédente):

- Installation du plugin NRPE
- Copie des plugins dans le bon répertoire `/usr/local/nagios/libexec/`
- Activation et création du répertoire contenant les futurs emplacements pour définir les hôtes en modifiant le fichier `nagios.cfg`

#### Sur la Machine Cible (SrvDeb) :

- Installation du plugin NRPE
- Configuration du fichier de configuration NRPE pour autoriser l'adresse IP du serveur Nagios

#### Retour sur le Serveur Nagios :

- Définition de l'hôte dans un fichier de configuration dans le répertoire `/usr/local/nagios/etc/servers/`

</details>

<!-- -->
<!-- -->
<!-- -->

<details>
<summary style="background-color: #212529; color: white; padding: 10px; border-radius: 5px; cursor: pointer; border: 2px solid #c3e6cb;">
    <strong>Superviser un Système Windows</strong>
</summary>

<div style="background-color: #343A40; padding: 20px; border-radius: 5px; border: 1px solid #c3e6cb;">
<!-- Contenu de la section Windows ici -->
EN COURS
</div>
</details>


<!-- -->
<!-- -->
<!-- -->

<details>
<summary style="background-color: #212529; color: white; padding: 10px; border-radius: 5px; cursor: pointer; border: 2px solid #c3e6cb;">
    <strong>Superviser un Switch Cisco</strong>
</summary>

<div style="background-color: #343A40; padding: 20px; border-radius: 5px; border: 1px solid #c3e6cb;">
<!-- Contenu de la section Switch Cisco ici -->
EN COURS
</div>
</details>

<!-- -->
<!-- -->
<!-- -->

<details>
<summary style="background-color: #212529; color: white; padding: 10px; border-radius: 5px; cursor: pointer; border: 2px solid #c3e6cb;">
    <strong>Superviser un Switch HP</strong>
</summary>

<div style="background-color: #343A40; padding: 20px; border-radius: 5px; border: 1px solid #c3e6cb;">
<!-- Contenu de la section Switch HP ici -->
EN COURS
</div>
</details>

