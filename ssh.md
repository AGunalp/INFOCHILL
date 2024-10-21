<link rel="stylesheet" type="text/css" href="/assets/css/red-theme.css">

# Guide Pratique : /ssh

Vous trouverez ci-dessous plusieurs commandes pratiques pour gérer et surveiller vos connexions SSH.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Désactiver SSH (Temporairement)

Cette méthode permet de désactiver temporairement les connexions SSH, tout en laissant la possibilité de les réactiver.

```bash
sudo iptables -A INPUT -j DROP
```

Pour réactiver SSH, remplacez `INPUT` par `OUTPUT` dans la commande :

```bash
sudo iptables -A OUTPUT -j DROP
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Trouver l'IP des utilisateurs connectés

Cette commande vous permet de lister les utilisateurs actuellement **connectés en SSH** à la machine, avec leur adresse IP.

```bash
who
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Voir l'historique des connexions à votre session SSH

**Afficher toutes les connexions réussies** :

```bash
sudo grep 'Accepted password' /var/log/auth.log
```

**Afficher toutes les connexions échouées** :

```bash
sudo grep 'Failed password' /var/log/auth.log
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### **[↩️ Retour](index.md)**