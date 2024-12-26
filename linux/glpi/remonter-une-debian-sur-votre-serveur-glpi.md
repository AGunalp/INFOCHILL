<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [GLPI Debian](../glpi/index.md) > <a href="" style="color: #ff9900; text-decoration: underline;">Remonter Debian sur GLPI</a>

## Guide d'installation de l'Agent GLPI sur Debian

### 0. Activation de l'inventire : 
Depuis votre interface GLPI, vérifiez si vous avez coché "Activer l'inventaire".

![](/assets/images/glpi_inventaire_actif.png)

### 1. **Mettez à jour les dépôts et le système :**
Avant d'installer l'agent GLPI, il est conseillé de mettre à jour les dépôts et les paquets de votre système Debian pour vous assurer d'avoir les dernières versions des logiciels installés.

```
apt update && apt upgrade -y
```
- `-y` : Accepte automatiquement toutes les confirmations demandées.

### 2. **Déplacez-vous dans le répertoire `/opt/` :** (Recommandé)
Le répertoire `/opt/` est l'endroit idéal pour stocker ce script, car il est spécialement conçu pour accueillir des applications ou scripts tiers sans interférer avec les fichiers système.

```
cd /opt
```


### 3. **Téléchargez du Script d'Installation de l'Agent GLPI :**
Téléchargez le script d'installation de l'agent GLPI à l'aide de la commande `wget`. Ce script en Perl permet de configurer et d'installer l'agent GLPI sur votre machine Debian.

```
wget https://github.com/glpi-project/glpi-agent/releases/download/1.11/glpi-agent-1.11-linux-installer.pl
```
- `wget` : Permet de télécharger des fichiers depuis internet.

### 4. **Installez de Perl :**
Le script d'installation étant écrit en Perl (d'où l'extension **.pl**), assurez-vous que Perl est installé sur votre machine. Si ce n'est pas le cas, vous pouvez l'installer avec la commande suivante :

```
apt install perl
```

### 5. **Exécutez du Script d'Installation (avec perl) :**
Lancez le script Perl pour installer l'agent GLPI. Ce script vous guidera tout au long du processus d'installation, en configurant l'agent pour qu'il envoie les informations à votre serveur GLPI.

```
perl glpi-agent-1.11-linux-installer.pl
```

Lors de l'exécution du script, vous serez invité à fournir l'URL de votre serveur GLPI. Entrez l'URL suivante (en remplaçant `192.168.1.205` par l'adresse de votre serveur GLPI) :

```
http://192.168.1.205/glpi
```
⚠️ **En cas d'erreur :** Si vous vous êtes trompé d'URL lors de l'exécution, vous pouvez modifier cette information dans le fichier de configuration de l'agent. Pour cela, éditez le fichier de configuration correspondant, généralement situé à `/etc/glpi-agent/config.d/00-install.cfg`

### 6. **Lancez l'agent GLPI :**
Cette commande permet de lancer l'agent GLPI sur une machine, collectant des informations système et les envoyant à un serveur GLPI pour l'inventaire et la gestion des équipements.
```
glpi-agent
```

### 7. **Vérifiez si la machine a été remontée :**

![](/assets/images/glpi-machine.png)

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### **[↩️ Retour](../glpi/index.md)**