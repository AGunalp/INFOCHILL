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