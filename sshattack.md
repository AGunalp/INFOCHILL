<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# Ubuntu - Guide Pratique : /sshattack

## 0. Création d'une Interface pour se Connecter en SSH sur un Poste 
Ajoutez une nouvelle interface dans : 
```
vim /etc/netplan/50-cloud-init.yaml
```
Puis, pour forcer l'utilisation de cette interface lors de votre connexion en SSH :
```
ssh -b AdresseIP
```

## 1. Connexion en SSH à une Session
Pour vous connecter à un compte SSH, utilisez la commande suivante :  
```
ssh user@hostname
```
- **`user`** : nom d'utilisateur du compte distant.
- **`hostname`** : adresse IP.

---

## 2. Accès aux Sessions Utilisateurs

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

## 3. Ouvrir un Terminal à Distance (avec Condition)
Si vous avez réussi à vous connecter directement sur la session de l'utilisateur actuellement connecté, vous pouvez ouvrir un terminal à distance avec la commande suivante :  
```
gnome-terminal
```

---

## 4. Envoyer un Message à un Terminal Actif
Pour envoyer un message à un terminal ouvert :  
```
echo "TEXTE" > /dev/pts/0
```
- Remplacez **`TEXTE`** par votre message.
- **`pts/0`** : représente le numéro du terminal (si en faisant `who`, vous ne voyez pas `pts/0`, cela veut dire que l'utilisateur est en train de l'utiliser).

---

## 5. Même Message à Chaque Ouverture de Terminal

### Ajouter un Message de Troll
Vous pouvez modifier le fichier `.bashrc` pour exécuter une commande à chaque ouverture de terminal :  
```
echo "echo 'Vous avez été piraté !'" >> /home/username/.bashrc
```

---

## 6. Ajouter des Alias à des Commandes

### a. Exemple d'Aliasing pour SSH
Pour rediriger une commande SSH vers un autre utilisateur :  
1. Ouvrez le fichier `.bashrc` :  
```
vim /home/user/.bashrc
```
2. Ajoutez l'alias à la fin du fichier :  
```
alias ssh='ssh sio@192.168.1.1'
```

### b. Recharger le Fichier .bashrc
Pour appliquer les modifications :  
```
source /home/user/.bashrc
```
A chaque fois que l'utilisateur utilise la commande `ssh`, il exécutera automatiquement l'alias que vous avez écrit.

---

## 7. Autres Trolls Visuels

### a. Afficher un Train
Installez le paquet `sl` pour afficher un train dans le terminal :  
```
apt install sl
```
Pour exécuter le train sur le terminal d'un autre utilisateur :  
```
sl > /dev/pts/0
```
![Train](/assets/images/sl.png)

### b. Cowsay
Installez `cowsay` pour afficher un message avec une vache parlante :  
```
apt install cowsay
```
Pour afficher "Meuuuh" :  
```
echo "Meuuuh" | cowsay > /dev/pts/0
```
![Matrix](/assets/images/cowsay.png)

### c. Effet Matrix
```
apt install cmatrix
```
Pour exécuter l'effet :  
```
cmatrix > /dev/pts/0
```
![Matrix](/assets/images/cmatrix.png)

### d. Message Toilette
Installez `toilet` pour formater le texte :  
```
apt install toilet
```
Pour envoyer un message "GAYPRIDE" :  
```
echo "GAYPRIDE" | toilet --gay > /dev/pts/0
```
![Toilette](/assets/images/toilet.png)

### e. Simulation d'Aquarium
Installez `asciiquarium` pour afficher un aquarium animé :  
```
sudo snap install asciiquarium
```
Pour exécuter l'aquarium :  
```
asciiquarium > /dev/pts/0
```
![Asciiquarium](/assets/images/asciiquarium.png)

---

## 8. Gestion des Fichiers de Log

### a. Vider le Fichier `auth.log`
Pour vider le contenu du fichier de log d'authentification :  
```
sudo truncate -s 0 /var/log/auth.log
```
(en soi, quand on se déconnecte, cela fera de nouveaux logs, mais ce n'est pas grave).

### b. Effacer l'Historique des Commandes Bash

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

## 9. Déconnexion
Pour vous déconnecter de la session SSH :  
```
exit
```

---

## 10. Supprimer l'Interface que Vous Avez Créée pour la Connexion
Avant de supprimer, regardez si votre nouvelle IP est bien dans les logs :
```
grep -i 'ADDRESSE_IP' /var/log/syslog
```
Dans tous les cas, nous allons supprimer l'interface que nous avons créée au début de ce document :
```
vim /etc/netplan/50-cloud-init.yaml
```
(Puis supprimer l'interface manuellement).

---

## 11. Redémarrer le Service Réseau
N'oubliez pas de redémarrer votre service réseau : 
```
sudo netplan apply
```
(Si message d'erreur, mettez les droits RW seulement au root).

---

## 12. Supprimer Maintenant le Fichier de Log
```
sudo rm /var/log/syslog
```
