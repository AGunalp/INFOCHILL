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

- [**<font color="red">Système Windows</font>**](./supervision/windows.md)  
  Pour surveiller des postes ou serveurs sous Windows.

- [**<font color="red">Switch Cisco</font>**](./supervision/switch-cisco.md)  
  Pour superviser des switches réseau de marque Cisco.

- [**<font color="red">Switch HP</font>**](./supervision/switch-hp.md)  
  Pour superviser des switches réseau de marque HP.

---
---

### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > [Superviser avec NRPE](supervision-nrpe.md)

<p style="text-align: right; margin: 20px 0;">
    <a href="https://infochill.com" style="display: inline-block; padding: 8px 12px; background-color: #003366; color: white; text-decoration: none; border: 2px solid white; border-radius: 4px; font-weight: bold;">
        Retour à l'Accueil
    </a>
</p>

# Superviser un système Linux (Debian) avec le plugin NRPE

<!-- Bouton -->
<div style="text-align: center; margin: 20px;">
    <button style="width: 80px; height: 40px; background-color: #007BFF; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold;" onclick="toggleContent()">Voir plus</button>
</div>

<!-- Contenu caché -->
<div id="content" style="display: none; margin-top: 20px; padding: 15px; border: 1px solid #ccc; border-radius: 5px;">
    ## Objectif
    L'objectif de ce guide est de comprendre comment superviser efficacement vos machines et collecter des informations sur leur état. Pour établir un lien entre le serveur Nagios et un hôte cible, nous devons installer et configurer le plugin NRPE sur les deux machines.

    ---

    ## Installation et Configuration de NRPE
    ### 🖥️ Depuis un Système Linux (Debian)
    Pour superviser un système Linux (Debian) avec le plugin NRPE, suivez les étapes ci-dessous. Cela vous permettra de configurer efficacement la machine afin qu'elle soit surveillée par votre serveur Nagios.

    ---

    ### Étapes à Suivre
    
    **Mettre à jour le système :**  
    Assurez-vous que votre système est à jour pour éviter les problèmes de compatibilité.
    ```bash
    apt update && apt upgrade
    ```

    ---

    **Installer les paquets nécessaires :**  
    Installez le serveur NRPE et les plugins Nagios.
    ```bash
    apt install nagios-nrpe-server nagios-plugins
    ```

    ---

    **Modifier le fichier de configuration NRPE :**  
    Ouvrez le fichier de configuration NRPE pour autoriser les connexions depuis votre serveur Nagios.
    ```bash
    vim /etc/nagios/nrpe.cfg
    ```

    - **Configurer les adresses IP autorisées :**  
    Ajoutez l'adresse IP de votre serveur Nagios à la ligne suivante (par exemple, pour l'IP `192.168.13.2`):
    ```bash
    allowed_hosts=127.0.0.1,::1,192.168.13.2
    ```

    ---

    **Redémarrer le service NRPE :**  
    Appliquez vos modifications en redémarrant le service NRPE.
    ```bash
    systemctl restart nagios-nrpe-server.service
    ```

    ---

    ### 🖥️ Retournez sur Nagios pour définir des Hôtes
    Après avoir configuré votre machine Debian pour NRPE, vous devez maintenant définir cet hôte sur votre serveur Nagios. Cela permettra à Nagios de commencer à surveiller la machine.

    <div style="border: 1px solid #007BFF; border-radius: 5px; padding: 10px; margin: 1em 0;">
        **📝 Méthodes de Configuration**  
        Il existe deux approches pour gérer les fichiers de configuration des hôtes dans Nagios :
        1. **Un seul fichier .cfg :** Regroupez toutes les machines dans un seul fichier. Cette méthode peut rendre la gestion plus complexe.
        2. **Fichiers séparés :** Créez un fichier .cfg pour chaque machine. C'est la méthode recommandée car elle facilite la gestion et la maintenance.
        
        Dans ce guide, nous allons opter pour la méthode des **fichiers séparés**.
    </div>

    ---

    ### Création du Fichier de Configuration pour l'Hôte (SrvDeb)
    
    **Créer le fichier de configuration :**  
    Accédez au répertoire approprié et créez le fichier pour votre machine (SrvDeb).
    ```bash
    touch /usr/local/nagios/etc/servers/SrvDeb.cfg
    ```

    ---

    **Éditer le fichier :**  
    Ouvrez le fichier créé pour ajouter les informations nécessaires.
    ```bash
    vim /usr/local/nagios/etc/servers/SrvDeb.cfg
    ```

    - **Ajouter les définitions de l'hôte :**  
    Insérez le code suivant dans le fichier :
    ```bash
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

    ---

    **Redémarrez vos services :**
    ```bash
    systemctl restart apache2
    systemctl restart nagios
    ```

    ---

    Cliquez sur l'onglet `host` à gauche, vous pouvez maintenant voir votre machine qui y est référencée, pour mon cas j'ai remonté une machine debian ayant pour nom `AP4-GLPI` :

    ![Image de la configuration Nagios](assets/images/host_debian_nagios.png)

    ---

    ## Récapitulatif des Étapes de Configuration de Nagios et NRPE

    ### Sur le Serveur Nagios (étape précédente) :
    - Installation du plugin NRPE
    - Copie des plugins dans le bon répertoire `/usr/local/nagios/libexec/`
    - Activation et création du répertoire contenant les futurs emplacements pour définir les hôtes en modifiant le fichier ...
</div>

<script>
    function toggleContent() {
        var content = document.getElementById("content");
        if (content.style.display === "none") {
            content.style.display = "block"; // Affiche le contenu
        } else {
            content.style.display = "none"; // Cache le contenu
        }
    }
</script>
