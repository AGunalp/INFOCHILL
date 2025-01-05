<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [OpenMediaVault](../openmediavault/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Installation d'OpenMediaVault</a>

<div style="background-color: #333; color: #fff; border-left: 5px solid #ff9900; border-right: 5px solid #ff9900; padding: 18px 22px; margin-bottom: 18px; text-align: center;">
  <strong style="font-size: 22px; color: #ff9900;">📚 INSTALLATION D'OPENMEDIAVAULT SUR DEBIAN</strong>
</div>

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  <p>Ce guide suppose les éléments suivants :</p>
  <ul>
    <li><strong>Distribution :</strong> Vous utilisez la distribution <strong>Debian</strong>.</li>
    <li><strong>Accès administrateur :</strong> Vous êtes connecté en tant que <code>root</code> (via la commande <code>su -</code>).</li>
  </ul>
  <p>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande pour l'exécuter avec les privilèges administratifs.</p>
</div>

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## 🛠️ GUIDES D'INSTALLATION ET DE CONFIGURATION

### 1. Mettez à jour le système :  
Avant toute installation, il est important de mettre à jour les dépôts et les paquets de votre système Debian pour garantir que vous avez les dernières versions disponibles des paquets.
```
apt update && apt upgrade -y
```

### 1. Installez GNUPG  
Le paquet **gnupg** est un outil de gestion des clés GPG, utilisées pour signer numériquement les paquets et garantir leur intégrité et leur authenticité.

```
apt install gnupg -y
```

- **gnupg** : Ce paquet est essentiel pour vérifier que les paquets téléchargés n'ont pas été modifiés en cours de route et qu'ils proviennent bien des dépôts officiels.
- **-y** : Cette option permet d'accepter automatiquement toutes les confirmations, ce qui permet d'automatiser l'installation sans interaction manuelle.

### 3. Ajout de la clé GPG OpenMediaVault
Ensuite, vous devez télécharger la clé publique qui permettra de valider les paquets OpenMediaVault. Cette commande récupère la clé de signature et l'enregistre dans le système.

```
wget --quiet --output-document=- https://packages.openmediavault.org/public/archive.key | gpg --dearmor --yes --output "/usr/share/keyrings/openmediavault-archive-keyring.gpg"
```

- **wget** : Télécharge le fichier contenant la clé de signature.
- **gpg --dearmor** : Convertit la clé en un format utilisable par APT pour les vérifications de sécurité.
- **--output** : Spécifie l'endroit où enregistrer la clé convertie.

### 4. Ajout du dépôt OpenMediaVault
Créez un fichier de configuration dans le répertoire **/etc/apt/sources.list.d/**. Ce répertoire est réservé aux sources supplémentaires pour les dépôts APT.

```
cat <<EOF >> /etc/apt/sources.list.d/openmediavault.list
deb [signed-by=/usr/share/keyrings/openmediavault-archive-keyring.gpg] https://packages.openmediavault.org/public sandworm main
# deb [signed-by=/usr/share/keyrings/openmediavault-archive-keyring.gpg] https://downloads.sourceforge.net/project/openmediavault/packages sandworm main
## Uncomment the following line to add software from the proposed repository.
# deb [signed-by=/usr/share/keyrings/openmediavault-archive-keyring.gpg] https://packages.openmediavault.org/public sandworm-proposed main
# deb [signed-by=/usr/share/keyrings/openmediavault-archive-keyring.gpg] https://downloads.sourceforge.net/project/openmediavault/packages sandworm-proposed main
## This software is not part of OpenMediaVault, but is offered by third-party
## developers as a service to OpenMediaVault users.
# deb [signed-by=/usr/share/keyrings/openmediavault-archive-keyring.gpg] https://packages.openmediavault.org/public sandworm partner
# deb [signed-by=/usr/share/keyrings/openmediavault-archive-keyring.gpg] https://downloads.sourceforge.net/project/openmediavault/packages sandworm partner
EOF
```

- **<<EOF** : Démarre un bloc multi-lignes dans le terminal, permettant d'ajouter plusieurs lignes de texte dans un fichier.
- **>>** : Redirige la sortie vers un fichier et ajoute les nouvelles lignes (du dessous) à la fin de ce fichier.
- **EOF** : Termine le bloc multi-lignes.

### 5. Installez le paquet OpenMediaVault
Une fois le dépôt ajouté, vous pouvez installer OpenMediaVault avec la commande suivante.

```
export LANG=C.UTF-8
export DEBIAN_FRONTEND=noninteractive
export APT_LISTCHANGES_FRONTEND=none
apt-get update
apt-get --yes --auto-remove --show-upgraded \
    --allow-downgrades --allow-change-held-packages \
    --no-install-recommends \
    --option DPkg::Options::="--force-confdef" \
    --option DPkg::Options::="--force-confold" \
    install openmediavault
```

- **export LANG=C.UTF-8** : Définit la variable d'environnement **LANG** pour s'assurer que le système utilise un encodage UTF-8 et que les messages sont en anglais.
- **export DEBIAN_FRONTEND=noninteractive** : Désactive les invites interactives pendant l'installation, ce qui permet d'automatiser le processus.
- **apt-get update** : Met à jour la liste des paquets disponibles.
- **apt-get install** : Installe OpenMediaVault avec plusieurs options pour éviter d'installer des paquets inutiles et éviter des conflits.

### 6. Peuplage de la base de données OpenMediaVault
Enfin, vous devez peupler la base de données OpenMediaVault avec la configuration du système existant, ce qui permet de récupérer les paramètres réseau et autres.

```
omv-confdbadm populate
```

Cela va configurer OpenMediaVault avec les paramètres réseau actuels de votre système.

### 7. Redéployer la configuration réseau
L’étape suivante consiste à redéployer la configuration réseau pour s’assurer que tous les paramètres de réseau nécessaires sont bien appliqués.

```
omv-salt deploy run systemd-networkd
```

- **omv-salt** : Outil de gestion de configuration utilisé par OpenMediaVault.
- **deploy run systemd-networkd** : Applique la configuration réseau en utilisant **systemd-networkd**, qui gère les interfaces réseau sur le système.

Cela garantit que les paramètres réseau sont bien appliqués et que le système fonctionne correctement.

### 8. Vos identifiants de connexion (par défaut) :
- **Login** : admin  
- **Password** : openmediavault  

Vous pouvez changer votre mot de passe en exécutant la commande suivante :  

```
passwd admin
```

### 9. Accéder à l'interface web d'OpenMediaVault :
Allez sur votre navigateur et tapez l'IP de votre serveur OMV dans la barre d'adresse pour accéder à l'interface d'administration d'OpenMediaVault.  

### **[↩️ Retour](../openmediavault-debian/index.md)**