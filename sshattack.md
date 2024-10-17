# Guide Pratique : /sshattack

## 1. Connexion à un Compte SSH
Pour vous connecter à un compte SSH, utilisez la commande suivante :  
```
ssh -Y user@hostname
```
- **`user`** : nom d'utilisateur du compte distant.
- **`hostname`** : adresse IP ou nom de domaine du serveur.
- **`-Y`** : option pour obtenir des permissions supplémentaires.

---

## 2. Vérification des Utilisateurs Connectés
### a. Utilisateurs Actuellement Connectés
Pour afficher tous les utilisateurs connectés à la machine :  
```
who
```

### b. Informations Détaillées sur les Utilisateurs
Pour afficher des informations détaillées sur les utilisateurs connectés :  
```
w
```

---

## 3. Accès aux Sessions Utilisateurs
### a. Passer en Mode Super Utilisateur (Root)
Pour obtenir des droits d'administration :  
```
sudo -i
```

### b. Passer à la Session d’un Autre Utilisateur
Pour accéder à la session d'un utilisateur spécifique :  
```
su - username
```
- **`username`** : nom d'utilisateur de la personne à laquelle vous souhaitez accéder.

---

## 4. Ouvrir un Terminal à Distance
Une fois connecté via SSH, vous pouvez ouvrir un terminal à distance avec la commande suivante :  
```
gnome-terminal
```

---

## 5. Envoyer un Message à un Terminal Actif
Pour envoyer un message à un terminal ouvert :  
```
echo "TEXTE" > /dev/pts/0
```
- Remplacez **`TEXTE`** par votre message.
- **`pts/0`** : représente la session de l'utilisateur. Utilisez la commande `who` pour voir quel terminal est actif.

---

## 6. Modifications dans le Fichier .bashrc
### a. Ajouter un Message de Troll
Vous pouvez modifier le fichier `.bashrc` pour exécuter une commande à chaque ouverture de terminal :  
```
echo "echo 'Vous avez été piraté !'" >> /home/username/.bashrc
```

---

## 7. Ajouter des Alias à des Commandes
### a. Exemple d'Aliasing pour SSH
Pour rediriger une commande SSH vers un autre utilisateur :  
1. Ouvrez le fichier `.bashrc` :  
```
vim /home/user/.bashrc
```
2. Ajoutez la ligne suivante à la fin :  
```
alias ssh='ssh sio@192.168.1.1'
```

### b. Recharger le Fichier .bashrc
Pour appliquer les modifications :  
```
source /home/user/.bashrc
```

---

## 8. Autres Trolls Visuels
### a. Afficher un Train
Installez le paquet `sl` pour afficher un train dans le terminal :  
```
apt install sl
```
Pour exécuter le train sur le terminal d'un autre utilisateur :  
```
sl > /dev/pts/0
```

### b. Cowsay
Installez `cowsay` pour afficher un message avec une vache parlante :  
```
apt install cowsay
```
Pour afficher "Meuuuh" :  
```
echo "Meuuuh" | cowsay > /dev/pts/0
```

### c. Effet Matrix
```
apt install cmatrix
```
Pour exécuter l'effet :  
```
cmatrix > /dev/pts/0
```

### d. Message Toiletté
Installez `toilet` pour formater le texte :  
```
apt install toilet
```
Pour envoyer un message "GAYPRIDE" :  
```
echo "GAYPRIDE" | toilet --gay > /dev/pts/0
```

### e. Simulation d'Aquarium
Installez `asciiquarium` pour afficher un aquarium animé :  
```
sudo snap install asciiquarium
```
Pour exécuter l'aquarium :  
```
asciiquarium > /dev/pts/0
```

---

## 9. Gestion des Fichiers de Log
### a. Vider le Fichier `auth.log`
Pour vider le contenu du fichier de log d'authentification :  
```
sudo truncate -s 0 /var/log/auth.log
```

### b. Effacer l'Historique des Commandes Bash
#### i. Vérifier l'Existence de l'Historique
Pour vérifier si le fichier d'historique existe :  
```
less /home/user/.bash_history
```


#### iv. Effacer l'Historique de la Session
Pour supprimer le fichier d'historique :  
```
rm /home/user/.bash_history
```

#### v. Vider l'Historique en Mémoire
Pour vider l'historique des commandes en mémoire :  
```
history -c
```

---

## 10. Déconnexion
Pour vous déconnecter de la session SSH :  
```
exit
```
---

**Remarque** : Utilisez ces commandes avec précaution et uniquement sur des systèmes où vous avez l'autorisation d'effectuer ces actions. Ces opérations peuvent affecter d'autres utilisateurs sur le système.
