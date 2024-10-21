<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# Ubuntu - Guide Pratique : /sshattack

Ce guide vous présente diverses commandes pour gérer les sessions SSH et quelques astuces pratiques.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 0. Création d'une Interface pour se Connecter en SSH sur un Poste

Ajoutez une nouvelle interface dans :

```bash
vim /etc/netplan/50-cloud-init.yaml
```

Puis, pour forcer l'utilisation de cette interface lors de votre connexion en SSH :

```bash
ssh -b AdresseIP
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 1. Connexion en SSH à une Session

Pour vous connecter à un compte SSH, utilisez la commande suivante :

```bash
ssh user@hostname
```

- **`user`** : nom d'utilisateur du compte distant.
- **`hostname`** : adresse IP.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 2. Accès aux Sessions Utilisateurs
### a. Passer en Mode Super Utilisateur (Root)

Pour obtenir des droits d'administration :

```bash
sudo -i
```

### b. Passer à la Session d’un Autre Utilisateur

Pour accéder à la session d'un utilisateur spécifique :

```bash
su - username
```

- **`username`** : nom d'utilisateur de la personne à laquelle vous souhaitez accéder.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 3. Ouvrir un Terminal à Distance (avec Condition)

Si vous avez réussi à vous connecter directement sur la session de l'utilisateur actuellement connecté, vous pouvez ouvrir un terminal à distance avec la commande suivante :

```bash
gnome-terminal
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 4. Envoyer un Message à un Terminal Actif

Pour envoyer un message à un terminal ouvert :

```bash
echo "TEXTE" > /dev/pts/0
```

- Remplacez **`TEXTE`** par votre message.
- **`pts/0`** : représente le numéro du terminal (utilisez `who` pour vérifier le numéro).

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 5. Même Message à Chaque Ouverture de Terminal

### Ajouter un Message de Troll

Modifiez le fichier `.bashrc` pour exécuter une commande à chaque ouverture de terminal :

```bash
echo "echo 'Vous avez été piraté !'" >> /home/username/.bashrc
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 6. Ajouter des Alias à des Commandes

### a. Exemple d'Aliasing pour SSH

Pour rediriger une commande SSH vers un autre utilisateur :

1. Ouvrez le fichier `.bashrc` :

```bash
vim /home/user/.bashrc
```

2. Ajoutez l'alias à la fin du fichier :

```bash
alias ssh='ssh sio@192.168.1.1'
```

### b. Recharger le Fichier .bashrc

Pour appliquer les modifications :

```bash
source /home/user/.bashrc
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 7. Autres Trolls Visuels

### a. Afficher un Train

Installez le paquet `sl` pour afficher un train dans le terminal :

```bash
apt install sl
```

Pour exécuter le train sur le terminal d'un autre utilisateur :

```bash
sl > /dev/pts/0
```

### b. Cowsay

Installez `cowsay` pour afficher un message avec une vache parlante :

```bash
apt install cowsay
```

Pour afficher "Meuuuh" :

```bash
echo "Meuuuh" | cowsay > /dev/pts/0
```

### c. Effet Matrix

```bash
apt install cmatrix
```

Pour exécuter l'effet :

```bash
cmatrix > /dev/pts/0
```

### d. Message Toilette

Installez `toilet` pour formater le texte :

```bash
apt install toilet
```

Pour envoyer un message "GAYPRIDE" :

```bash
echo "GAYPRIDE" | toilet --gay > /dev/pts/0
```

### e. Simulation d'Aquarium

Installez `asciiquarium` pour afficher un aquarium animé :

```bash
sudo snap install asciiquarium
```

Pour exécuter l'aquarium :

```bash
asciiquarium > /dev/pts/0
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 8. Gestion des Fichiers de Log

### a. Vider le Fichier `auth.log`

Pour vider le contenu du fichier de log d'authentification :

```bash
sudo truncate -s 0 /var/log/auth.log
```

### b. Effacer l'Historique des Commandes Bash

#### Effacer l'Historique de la Session

Pour supprimer le fichier d'historique :

```bash
rm /home/user/.bash_history
```

#### Vider l'Historique en Mémoire

Pour vider l'historique des commandes en mémoire :

```bash
history -c
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 9. Déconnexion

Pour vous déconnecter de la session SSH :

```bash
exit
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 10. Supprimer l'Interface que Vous Avez Créée pour la Connexion

Avant de supprimer, vérifiez si votre nouvelle IP est bien dans les logs :

```bash
grep -i 'ADDRESSE_IP' /var/log/syslog
```

Pour supprimer l'interface que nous avons créée :

```bash
vim /etc/netplan/50-cloud-init.yaml
```

(Puis, supprimez l'interface manuellement).

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 11. Redémarrer le Service Réseau

N'oubliez pas de redémarrer votre service réseau :

```bash
sudo netplan apply
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### 12. Supprimer Maintenant le Fichier de Log

```bash
sudo rm /var/log/syslog
```
