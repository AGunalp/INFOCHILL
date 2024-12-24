<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [NagiosCore Debian](../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Superviser avec NRPE</a>


<div style="background-color: #333; color: #fff; border-left: 5px solid #ff9900; border-right: 5px solid #ff9900; padding: 20px 25px; margin-bottom: 20px; text-align: center;">
  <strong style="font-size: 24px; color: #ff9900;">📚 PREPARATION DU SERVEUR NAGIOS</strong>
</div>

<!-- Alerte importante concernant la distribution et les droits d'utilisateur -->
<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">

  <p>Ce guide suppose les éléments suivants :</p>
  <ul>
    <li><strong>Distribution :</strong> Vous utilisez la distribution <strong>Debian</strong>.</li>
    <li><strong>Accès administrateur :</strong> Vous êtes connecté en tant que <code>root</code> (via la commande <code>su -</code>).</li>
  </ul>
  <p>Si ce n'est pas le cas, ajoutez <code>sudo</code> devant chaque commande pour l'exécuter avec les privilèges administratifs.</p>
</div>


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## Objectif

Après avoir installé et vérifié que votre serveur Nagios est en cours d'exécution ("Daemon running") via votre navigateur, l'objectif est de superviser efficacement vos machines et de collecter des informations sur leur état.

Pour cela, vous devez installer et configurer l'agent NRPE. Cet agent fait le lien entre votre serveur Nagios et les hôtes que vous voulez superviser. Il permet au serveur Nagios d'envoyer des commandes aux systèmes supervisés, qui à leur tour communiquent les résultats au serveur.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS VOTRE SERVEUR NAGIOS :</strong>
</div>

**Modifiez le fichier de configuration Nagios :**  

Nous devons maintenant indiquer à Nagios le chemin exact du répertoire où sont stockés les fichiers de configuration des éléments à superviser.
```
vim /usr/local/nagios/etc/nagios.cfg
```

- Décommentez (rendre actif) la ligne suivante :

  ```
  cfg_dir=/usr/local/nagios/etc/servers
  ```
Une fois activée, cette ligne précise à Nagios d’utiliser ce répertoire pour consulter les fichiers **.cfg** qui définissent les machines à superviser.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Créez le répertoire (si nécessaire) :**

Si le répertoire activé n'existe pas encore, créez-le manuellement avec la commande suivante :

```
mkdir -p /usr/local/nagios/etc/servers
```
- -p : Permet de créer notamment les répertoires parents si ils n'existent pas.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Changez les droits d'accès :**

Attribuez le répertoire nouvellement créé à l’utilisateur et au groupe Nagios pour qu’il puisse y accéder sans restriction :

```
chown nagios:nagios /usr/local/nagios/etc/servers
```
```
chmod 764 /usr/local/nagios/etc/servers
```
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

**Installez le paquet :**  

```
apt install nagios-nrpe-server
```
- L'installation de **l'agent NRPE** est indispensable, car c'est cet agent, (présent notament sur les machines qu'on veut superviser), qui permet l'échange des informations entre le serveur Nagios et les machines supervisées.

### **[↩️ Retour](../../linux/nagioscore-debian/index.md)**