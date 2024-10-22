<link rel="stylesheet" type="text/css" href="../../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../../index.md) > [NagiosCore Debian](../../nagioscore-debian/index.md) > [Superviser avec NRPE](../supervision-nrpe.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Superviser Système Linux</a>


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
Assurez-vous que votre système estf à jour pour éviter les problèmes de compatibilité.

```
apt update && apt upgrade
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Installer les paquets nécessaires :**  
Installez le serveur NRPE et les plugins Nagios.

```
apt install nagios-nrpe-server nagios-plugins
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Modifier le fichier de configuration NRPE :**  
Ouvrez le fichier de configuration NRPE pour autoriser les connexions depuis votre serveur Nagios.

```
vim /etc/nagios/nrpe.cfg
```

- **Configurer les adresses IP autorisées :**  
  Ajoutez l'adresse IP de votre serveur Nagios à la ligne suivante (par exemple, pour l'IP `192.168.13.2`):

  ```
  allowed_hosts=127.0.0.1,::1, 192.168.13.2
  ```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Redémarrer le service NRPE :**  
Appliquez vos modifications en redémarrant le service NRPE.

```
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

```
touch /usr/local/nagios/etc/servers/SrvDeb.cfg
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Éditer le fichier :**  
Ouvrez le fichier créé pour ajouter les informations nécessaires.

```
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

#### Redémarrez votre machine :

```
reboot
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

Cliquez sur l'onglet `host` à gauche, vous pouvez maintenant voir votre machine qui y est référencée, pour mon cas j'ai remonté une machine debian ayant pour nom `AP4-GLPI` :

![alt text](/assets/images/host_debian_nagios.png)

---

## Étapes pour Définir des Services dans des Templates

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;"><hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

# Ne pas executez ces commandes :
**Accéder au Fichier de Configuration des Templates**

Vous devez d'abord localiser le fichier de configuration des templates dans Nagios. Ce fichier est souvent situé dans ``/usr/local/nagios/etc/templates.cfg``. Ouvrez-le avec votre éditeur de texte préféré :

```
vim /usr/local/nagios/etc/templates.cfg
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Définir un Template de Service Générique**

Ajoutez une définition de template pour un service générique, que vous pouvez ensuite réutiliser pour d'autres services. Voici un exemple de template :

```plaintext
define service {
    name                    generic-service       ; Nom du template
    notification_interval    30                   ; Intervalle de notification
    notification_period     24x7                 ; Notifications actives 24h/24 7j/7
    max_check_attempts      3                    ; Nombre de tentatives avant l'alerte
    check_interval          5                    ; Intervalle de vérification (en minutes)
    retry_interval          1                    ; Intervalle entre deux tentatives de vérification
    notification_options    w,u,c,r              ; Notifications pour les états Warning, Unknown, Critical, et Recovery
}
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Édition du Fichier de Configuration pour les Services de SrvDeb**

Pour configurer les services associés à l'hôte SrvDeb, nous allons modifier le fichier de configuration que nous avons précédemment créé pour définir l'hôte. Cela nous permettra d'organiser efficacement la supervision des services tout en maintenant une structure claire.

```
touch /usr/local/nagios/etc/services/SrvDeb-services.cfg
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Éditez le Fichier de Configuration des Services**

Ouvrez le fichier que vous venez de créer pour définir les services à superviser. Voici comment vous pourriez le faire :

```
vim /usr/local/nagios/etc/services/SrvDeb-services.cfg
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Définir les Services à Superviser en Utilisant le Template : 

Pour chaque service, utilisez le template que vous avez défini. Voici quelques exemples :

**Service : Vérification de l'État de Fonctionnement (Allumé/Éteint) :**
```plaintext
define service {
    use                     generic-service      ; Utilise un modèle de service générique
    host_name               SrvDeb               ; Nom de l'hôte
    service_description     System Status        ; Description du service
    check_command           check_nrpe!check_procs!'-c 1:0'  ; Vérifie si le système est actif
    max_check_attempts      3                    ; Tentatives avant l'alerte
    check_interval          5                    ; Intervalle de vérification (en minutes)
    retry_interval          1                    ; Intervalle entre deux tentatives de vérification
    notification_interval   30                   ; Intervalle de notification (en minutes)
    notification_period     24x7                 ; Notifications actives 24h/24 7j/7
    notification_options    w,u,c,r              ; Notifications pour les états Warning, Unknown, Critical, et Recovery
}
```
**Service : Vérification de la Charge du Système :**
```plaintext
define service {
    use                     generic-service      ; Utilise un modèle de service générique
    host_name               SrvDeb               ; Nom de l'hôte
    service_description     CPU Load             ; Description du service
    check_command           check_nrpe!check_load!'-w 5,4,3 -c 10,6,4'  ; Vérifie la charge CPU
    max_check_attempts      3                    ; Tentatives avant l'alerte
    check_interval          5                    ; Intervalle de vérification (en minutes)
    retry_interval          1                    ; Intervalle entre deux tentatives de vérification
    notification_interval   30                   ; Intervalle de notification (en minutes)
    notification_period     24x7                 ; Notifications actives 24h/24 7j/7
    notification_options    w,u,c,r              ; Notifications pour les états Warning, Unknown, Critical, et Recovery
}
```

**Service : Vérification de l'Utilisation du Disque Dur :**  

```plaintext
define service {
    use                     generic-service      ; Utilise un modèle de service générique
    host_name               SrvDeb               ; Nom de l'hôte
    service_description     Disk Usage           ; Description du service
    check_command           check_nrpe!check_disk!'-w 20% -c 10%'  ; Vérifie l'espace disque
    max_check_attempts      3                    ; Tentatives avant l'alerte
    check_interval          5                    ; Intervalle de vérification (en minutes)
    retry_interval          1                    ; Intervalle entre deux tentatives de vérification
    notification_interval   30                   ; Intervalle de notification (en minutes)
    notification_period     24x7                 ; Notifications actives 24h/24 7j/7
    notification_options    w,u,c,r              ; Notifications pour les états Warning, Unknown, Critical, et Recovery
}
```


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

Recharger la Configuration de Nagios
Après avoir ajouté les définitions de services, vous devez recharger la configuration de Nagios pour appliquer les modifications :

```
systemctl restart nagios.service
```


Une fois la configuration rechargée, vous pourrez voir et surveiller les nouveaux services sur l'hôte `SrvDeb` dans l'interface de Nagios.

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

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

- Définition de l'hôte dans un fichier de configuration dans le répertoire `/usr/local/nagios/etc/servers/`

# A VENIR : 
<div style="border: 2px solid red; color: red; padding: 10px; background-color: #ffe6e6; border-radius: 5px; width: fit-content; margin: 10px 0;">
    ⚠️ <strong>Avis :</strong> La rédaction des commandes pour superviser les services arrive très bientôt. Merci de votre patience !
</div>

---

Les commandes pour superviser les services individuels sur vos hôtes seront ajoutées ici.

---

### **[↩️ Retour](../../nagioscore-debian/supervision-nrpe.md)**