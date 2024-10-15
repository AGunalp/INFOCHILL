<link rel="stylesheet" type="text/css" href="/assets/css/blue-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../../index.md) > [NagiosCore Debian](../../nagioscore-debian/index.md) > [Superviser avec NRPE](../supervision-nrpe.md) > [Supervisier Système Linux](linux-debian.md)

# 📚 Superviser un sysème Linux (debian) avec le plugin NRPE

Bienvenue dans ce guide dédié à l'installation et à la configuration du plugin **NRPE** sur un système Debian. Vous allez apprendre comment mettre en place NRPE pour assurer la supervision de votre machine par le serveur Nagios.


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

L'objectif de ce guide est de comprendre comment superviser efficacement vos machines et collecter des informations sur leur état. Pour établir un lien entre le serveur Nagios et un hôte cible, nous devons installer et configurer le plugin NRPE sur les deux machines.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## Installation et Configuration de NRPE

### 🖥️ Depuis un Système Linux (Debian)

Pour superviser un système Linux (Debian) avec le plugin NRPE, suivez les étapes ci-dessous. Cela vous permettra de configurer efficacement la machine afin qu'elle soit surveillée par votre serveur Nagios.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Étapes à Suivre

**Mettre à jour le système :**  
Assurez-vous que votre système est à jour pour éviter les problèmes de compatibilité.

```bash
apt update && apt upgrade
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Installer les paquets nécessaires :**  
Installez le serveur NRPE et les plugins Nagios.

```bash
apt install nagios-nrpe-server nagios-plugins
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

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

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Redémarrer le service NRPE :**  
Appliquez vos modifications en redémarrant le service NRPE.

```bash
systemctl restart nagios-nrpe-server.service
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 🖥️ Retournez sur Nagios pour définir des Hôtes

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

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

#### Création du Fichier de Configuration pour l'Hôte (SrvDeb)

**Créer le fichier de configuration :**  
Accédez au répertoire approprié et créez le fichier pour votre machine (SrvDeb).

```bash
touch /usr/local/nagios/etc/servers/SrvDeb.cfg
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Éditer le fichier :**  
Ouvrez le fichier créé pour ajouter les informations nécessaires.

```bash
vim /usr/local/nagios/etc/servers/SrvDeb.cfg
```

- **Ajouter les définitions de l'hôte :**  
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

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

#### Redémarrez vos services :

```bash
systemctl restart apache2
systemctl restart nagios
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

Cliquez sur l'onglet `host` à gauche, vous pouvez maintenant voir votre machine qui y est référencée, pour mon cas j'ai remonté une machine debian ayant pour nom `AP4-GLPI` :

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

# A VENIR : 
* ajout commandes pour supervisier les services