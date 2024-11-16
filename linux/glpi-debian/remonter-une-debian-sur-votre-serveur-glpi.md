<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [GLPI Debian](../glpi-debian/index.md) > <a href="" style="color: #ff9900; text-decoration: underline;">Remonter Debian sur GLPI</a>

## Guide d'installation de l'Agent GLPI sur Debian

### 1. **Mettez à jour les dépôts et le système :**
Avant d'installer l'agent GLPI, il est conseillé de mettre à jour les dépôts et les paquets de votre système Debian pour vous assurer d'avoir les dernières versions des logiciels installés.

```
apt update && apt upgrade -y
```
- `-y` : Accepte automatiquement toutes les confirmations demandées.

### 2. **Déplacez-vous dans le répertoire `/opt/` :** (Recommandé)
Le répertoire `/opt/` est un emplacement standard dans un système Linux pour installer des applications tierces ou des logiciels qui ne font pas partie des paquets standards. Il permet de garder les applications manuelles isolées et bien organisées.

```
cd /opt
```


### 3. **Téléchargement du Script d'Installation de l'Agent GLPI :**
Téléchargez le script d'installation de l'agent GLPI à l'aide de la commande `wget`. Ce script en Perl permet de configurer et d'installer l'agent GLPI sur votre machine Debian.

```
wget https://github.com/glpi-project/glpi-agent/releases/download/1.11/glpi-agent-1.11-linux-installer.pl
```

### 4. **Installation de Perl :**
Le script d'installation étant écrit en Perl (d'où l'extension **.pl**), assurez-vous que Perl est installé sur votre machine. Si ce n'est pas le cas, vous pouvez l'installer avec la commande suivante :

```
apt install perl
```

### 5. **Exécution du Script d'Installation (avec perl) :**
Lancez le script Perl pour installer l'agent GLPI. Ce script vous guidera tout au long du processus d'installation, en configurant l'agent pour qu'il envoie les informations à votre serveur GLPI.

```
perl glpi-agent-1.11-linux-installer.pl
```

### 6. **Configuration de l'URL du Serveur GLPI :**
Lors de l'exécution du script, vous serez invité à fournir l'URL de votre serveur GLPI. Entrez l'URL suivante (en remplaçant `192.168.1.205` par l'adresse de votre serveur GLPI si nécessaire) :

```
http://192.168.1.205/glpi
```

### 7. **Vérification de l'installation :**
Cette commande permet de lancer l'agent GLPI sur une machine, collectant des informations système et les envoyant à un serveur GLPI pour l'inventaire et la gestion des équipements.
```
glpi-agent
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### **[↩️ Retour](../glpi-debian/index.md)**