# Guide Pratique : /sshattack

## Se connecter à un compte SSH
Pour vous connecter à un compte SSH, utilisez la commande suivante :  
```
ssh -Y user@hostname
```
- **`user`** : nom d'utilisateur du compte distant.
- **`hostname`** : adresse IP ou nom de domaine du serveur.
- **`-Y`** : pour avoir plus de permissions.

---

## Vérification des utilisateurs connectés
### Voir quel utilisateur est actuellement connecté
Pour voir tous les utilisateurs actuellement connectés à la machine :  
```
who
```

### Informations détaillées sur les utilisateurs
Pour afficher les informations détaillées sur les utilisateurs :  
```
w
```

---

## Accès aux sessions utilisateurs
### Se mettre en root
#### a. Passer en mode super utilisateur (root) :
```
sudo -i
```

#### b. Passer à la session d’un autre utilisateur
Pour passer à la session d'un utilisateur spécifique :  
```
su - username
```
- **`username`** : nom d'utilisateur de la personne à qui vous souhaitez accéder.

---

## Ouvrir un terminal à distance
Pour ouvrir un terminal à distance, vous devez vous connecter via SSH et avoir des droits d'accès. Une fois connecté, vous pouvez simplement exécuter des commandes.  
```
gnome-terminal
```

---

## Envoyer un texte sur un terminal actif
Pour envoyer un message à un terminal ouvert connecté :  
```
echo "TEXTE" > /dev/pts/0
```
- Remplacez "TEXTE" par votre message.
- **`pts/0`** : Chaque terminal `pts/x` correspond à une session d'utilisateur distant. Pour voir les utilisateurs connectés, utilisez la commande `who`.

---

## Autres actions amusantes
### Installer un "troll" comme cowsay
Si vous voulez un message amusant, installez `cowsay` (s'il est disponible) :  
```
sudo apt install cowsay
cowsay "Bonjour, je suis un message drôle !"
```

### Faire un "loop" qui fait un ping
```
ping -c 4 8.8.8.8
```

---

## Modifications dans le .bashrc
### Troll message
Modifier le fichier `.bashrc`. Vous pouvez ajouter une commande pour qu’elle s’exécute à chaque fois qu’un terminal est ouvert.  
```
echo "echo 'Vous avez été piraté !'" >> /home/username/.bashrc
```

---

## Ajouter des alias à des commandes
### Exemple avec SSH :
Ouvrez le fichier `.bashrc` de l'utilisateur :  
```
vim /home/user/.bashrc
```
Ajoutez à la fin :  
```
alias ssh='ssh sio@192.168.1.1'
```
Lors de l'exécution de 'ssh' depuis ce poste, peu importe ce qu'il écrit, cela va rediriger vers la commande entre guillemets.

**Recharger le fichier .bashrc**  
Pour appliquer les modifications que vous venez d'effectuer, rechargez le fichier .bashrc :

```
source /home/user/.bashrc
```
---

## Autres trolls visuels
### 1. Afficher un train
Installez le paquet `sl` (Steam Locomotive) qui affiche un train en mouvement dans le terminal.  
```
apt install sl
```  
Exécutez la commande pour afficher le train sur le terminal d'un autre utilisateur.  
```
sl > /dev/pts/0
``` 

### 2. Cowsay
Installez le paquet `cowsay` qui génère un message dans un format amusant avec une vache qui parle.  
```
apt install cowsay
``` 
Affichez le message "Meuuuh" dans le terminal d'un autre utilisateur via cowsay.  
```
echo "Meuuuh" | cowsay > /dev/pts/0
```

### 3. Matrix effect
Installez `cmatrix` pour simuler l'effet de pluie du film Matrix dans le terminal.  
```
apt install cmatrix
``` 
Exécutez `cmatrix` pour faire apparaître l'effet sur le terminal d'un autre utilisateur.  
```
cmatrix > /dev/pts/0
```  

### 4. Toilet message
Installez `toilet` pour formater le texte de manière artistique.  
```
apt install toilet
``` 
Envoyez un message stylisé "GAYPRIDE" dans le terminal d'un autre utilisateur.  
```
echo "GAYPRIDE" | toilet --gay > /dev/pts/0
``` 

### 5. Aquarium simulation
Installez `asciiquarium` pour afficher un aquarium animé dans le terminal.  
```
sudo snap install asciiquarium
``` 
Exécutez `asciiquarium` pour afficher l'aquarium sur le terminal d'un autre utilisateur.  
```
asciiquarium > /dev/pts/0
``` 

---

## Gestion des fichiers de log
### 1. Vider le fichier auth.log
Cette commande vide le contenu du fichier de log d'authentification.  
```
sudo truncate -s 0 /var/log/auth.log
``` 

### 2. Effacer l'historique des commandes bash
D'abord vérifier s'il existe :  
```
less /home/user/.bash_history
``` 
Si oui alors :  
```
rm /home/user/.bash_history
``` 
Et pour vider l'historique en mémoire :  
```
history -c
```
(C'est pas assez, flemme de chercher des commandes)