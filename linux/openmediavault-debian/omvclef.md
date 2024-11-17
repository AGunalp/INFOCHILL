<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

# Sauvegardes : 

### Créez un dossier partagés : 
Lorsque vous créez un dossier partagé, vous transformez un répertoire local en un répertoire accessible via le réseau. Cela permet à d'autres appareils sur le réseau local (ou même à distance, selon la configuration) d'accéder à ce répertoire.

Depuis le Menu Stockage>Dossiers partagés, créez un partage : 

![](/assets/images/omv-create.png)

- Administrateur -> Lecture/écriture
- Utilisateurs -> Lecture/écriture

![](/assets/images/omv-dossierspartages.png)

Ci-dessous, le chemin de notre partage (dans notre serveur OMV)
![](/assets/images/omv-cheminsave.png)
---

### Créez une analyse d'accès : 

![](/assets/images/omv-analysedaccess.png)
![](/assets/images/omv-analysedaccess2.png)
![](/assets/images/omv-analysedaccess3.png)

### Mise en place d'une sauvegarde : 

On va donc sauvegarder le "répertoire de publication" du serveur GLPI : `/var/www/html/UneMachineDebian`.

**Depuis votre UneMachineDebian :**
```
apt install rsync
```

```
/etc/ssh/sshd_config
``` 
Et ajoutez :
```
PermitRootLogin yes
```
```
systemctl restart ssh
```

**Depuis OMV (mode console) :**  
Cette commande synchronise notre répertoire `/var/www/html/UneMachineDebian` avec le le répertoire de sauvergarde `/srv/dev...`

```
rsync -e ssh -aruvz root@192.168.1.205:/Répertoire/Machine /Répertoire/SauvegardeUneMachineDebian
```

Exemple dans notre cas :

```
rsync -e ssh -aruvz root@192.168.1.205:/var/www/html/glpi /srv/dev-disk-by-id-md-name-OpenMediaVault-0/SauvegardeUneMachineDebian
```
- 192.168.1.205 : Notre serveur GLPI
- /var/www/html/glpi : Répertoire de la machine qu'on veut sauvegarder
- /srv/dev.../Sauvegarde... : Répertoire de l'OMV où y aura la sauvegarde.


Les deux répertoires sont bien synchronisés.

### Création de la clef :

![](/assets/images/omv-clef.png)
![](/assets/images/omv-clef2.png)
![](/assets/images/omv-clef3.png)

Une fois que vous avez créé la paire de clés SSH via l'interface d'OpenMediaVault (OMV), vous pouvez vérifier la présence de ces clés sur votre serveur OMV. Les clés SSH sont généralement stockées dans le répertoire /etc/ssh.

```
ls -l /etc/ssh
```
![](/assets/images/omv-clefrsa.png)
- **Clé privée :** Ce fichier est sans extension (exemple : id_rsa), et il doit être gardé privé sur votre serveur. Elle est utilisée pour établir une connexion sécurisée.
- **Clé publique :** Ce fichier aura l'extension .pub (exemple : id_rsa.pub). Il peut être partagé avec d'autres serveurs ou services pour autoriser la connexion via la clé privée correspondante.

### Transferez votre clef publique à UneMachineDebian

Depuis votre OMV :

Permet de copier notre clef publique à notre UneMachineDebian, sera ajouté dans le trousseau de clef `/root/.ssh/authorized_keys`.
Le fichier authorized_key (de UneMachineDebian) stocke toutes les clefs publiques des machines qui auront le droit de s'y connecter pour sauvegarder un répertoire.
```
ssh-copy-id -i /etc/ssh/openmediavault-f4f63f16-943f-4708-8b41-0b0b8acd8d43.pub root@192.168.1.205
```

Une fois la clef transférée, on peut maintenant autoriser l'authentification par clef :

```
vim /etc/ssh/sshd_config
```
Et dé-commentez ces lignes :
```
# PubkeyAuthentification yes
# AuthorizedKeysFile      .ssh/authorized_keys    .ssh/authorized_keys2
```
```
systemctl restart ssh
```
Maintenant notre OMV peut donc se connecter avec sa clef privé à UneMachineDebian pour sauvegarder un répertoire.

### Faire une nouvelle tâche de synchronisation :

![](/assets/images/omv-tache.png)
![](/assets/images/omv-rsync2.png)
![](/assets/images/omv-rsync3.png)

### Voir si la tâche fonctionne : 
![](/assets/images/omv-rsync4.png)
![](/assets/images/omv-rsync5.png)

### **[↩️ Retour](../openmediavault-debian/index.md)**