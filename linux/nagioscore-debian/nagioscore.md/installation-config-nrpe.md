<link rel="stylesheet" type="text/css" href="/assets/css/light-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore sur Debian](../index.md) > [Installation et configuration du plugin NRPE](.)

# 📚 Installation et configuration du plugin NRPE

Bienvenue dans la section dédiée à l'installation et à la configuration du plugin **NRPE**. Vous êtes actuellement dans le guide d'installation du plugin NRPE pour Nagios.

---

Après avoir terminé l'installation et vérifié que votre serveur Nagios est en "Daemon running" via votre navigateur, l'objectif est désormais de comprendre comment superviser efficacement vos machines et collecter des informations sur leur état. À savoir : Pour faire un lien Nagios entre notre serveur Nagios et un hôte (cible que nous allons remonter sur Nagios), il faut que dans les deux machines nous installions le plugin NRPE et que nous le configurions.  

## Depuis votre serveur Nagios
Depuis votre serveur Nagios, installez d’abord ces paquets :

```bash
sudo apt install nagios-nrpe-server nagios-plugins
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

## Depuis une machine debian (que vous souhaitez superviser)
Exécutez ces commandes sur la machine Debian que vous souhaitez superviser (que nous appellerons **SrvDeb**) :

```bash
sudo apt update && sudo apt upgrade
```
```bash
apt install nagios-nrpe-server nagios-plugins
```

Modifiez le fichier de configuration NRPE : 

```bash
vim /etc/nagios/nrpe.cfg
```

Référenciez l'IP de votre serveur Debian à la fin de la ligne ci-dessous (pour mon cas c'est 192.168.13.2) :

```bash
allowed_hosts=127.0.0.1,::1, 192.168.13.2
```

Redémarrez le service : 

```bash
systemctl restart nagios-nrpe-server.service
```

## Depuis votre serveur Nagios
Une fois que vous êtes sur votre serveur Nagios et que la machine cible **SrvDeb**, a correctement référencé l'adresse IP de votre serveur Nagios dans ses fichiers de configuration, il est maintenant temps de définir l'hôte depuis le serveur Nagios.

### Les 2 méthodes pour définir l'host :
Il existe deux solutions pour gérer les fichiers de configuration des hôtes :

1. **Un seul fichier .cfg :** Vous pouvez mettre toutes les machines dans un seul fichier. Cela peut être plus compliqué à gérer.
2. **Fichiers séparés :** Il est préférable de créer un fichier .cfg pour chaque machine. Cela rend la gestion et la maintenance plus faciles.

Dans notre exemple, nous choisissons la méthode des fichiers séparés (recommandée).

### Définir l'host :
Créer le fichier de configuration : Créez ou modifiez le fichier de configuration pour SrvDeb en y spécifiant le nom de l'hôte, l'adresse IP et les services à surveiller.


Depuis votre serveur Nagios, allez dans le répertoire approprié et créez le fichier `/usr/local/nagios/etc/server/SrvDeb.cfg` Ce fichier définira l'hôte avec l'extension `.cfg` : 

```bash
touch /usr/local/nagios/etc/server/SrvDeb.cfg
```

Et nous pouvons rajouter les lignes suivantes : 

```
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

### Récapitulatif des Étapes de Configuration de Nagios et NRPE

#### Sur le Serveur Nagios :

* Installation du plugin NRPE
* Copie des plugins dans le bon répertoire `/usr/local/nagios/libexec/`

* Activation et création du répertoire contenant les futurs emplacement pour définir l'les hôtes (cibles) en modifiant le fichier `nagios.cfg`

#### Sur la Machine Cible (SrvDeb) :
* Installation du plugin NRPE

* Configuration du fichier de configuration NRPE pour autoriser l'adresse IP du serveur Nagios

#### Retour sur le Serveur Nagios :
* Définition de l'hôte dans un fichier de configuration dans le répertoire `/usr/local/nagios/etc/servers/`