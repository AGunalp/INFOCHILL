<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# Guide Pratique : /ssh
## Désactiver SSH d'urgence (rapidement)

Commande temporaire pour désactiver toute connexion (entrant et sortant) jusqu’au redémarrage (recommandé si vous voulez désactiver au plus vite)

```bash
sudo systemctl stop ssh
```
Pour réactiver.
```bash
sudo systemctl start ssh
```

## Désactiver SSH (Recommandé)

Cette méthode est temporaire, mais au moins vous pourrez toujours utiliser SSH pour continuer vos TP.
```bash
sudo iptables -A INPUT -j DROP
```
Pour réactiver.



## Trouver l'IP des utilisateurs connectés 

Liste les utilisateurs actuellement connectés à la machine, avec leur adresse IP et le terminal utilisé.

```
who
```

Affiche des informations détaillées sur l'utilisateurs actuellement connectés :

```
w
```

## Voir l'historique des connexions à votre session SSH

Afficher l'historique des connexions SSH à votre machine.

```
sudo grep 'sshd' /var/log/auth.log
```

Afficher toutes les tentatives de connexion SSH.

```
sudo grep 'Failed password' /var/log/auth.log
```

Afficher toutes les connexions réussies.

```
sudo grep 'Accepted password' /var/log/auth.log
```

