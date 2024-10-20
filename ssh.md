<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# Guide Pratique : /ssh
## Désactiver SSH (Temporairement)

Cette méthode est temporaire, mais au moins vous pourrez toujours utiliser SSH pour continuer vos TP.
```bash
sudo iptables -A INPUT -j DROP
```
Pour réactiver mettre `OUTPUT` au lieu de `INPUT`



## Trouver l'IP des utilisateurs connectés 

Liste les utilisateurs actuellement __connectés en SSH__ à la machine en affichant leur adresse IP.
```
who
```

## Voir l'historique des connexions à votre session SSH

Afficher toutes les connexions réussie :

```
sudo grep 'Accepted password' /var/log/auth.log
```

Afficher toute les connexions échouées : 

```
sudo grep 'Failed password' /var/log/auth.log
```
