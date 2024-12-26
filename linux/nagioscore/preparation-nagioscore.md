<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [Nagios Core](../nagioscore/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Préparer le serveur</a>


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

L'objectif de cette page est de configurer et préparer le serveur Nagios pour superviser des machines à distance en utilisant l'agent NRPE et les plugins associés.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS VOTRE SERVEUR NAGIOS :</strong>
</div>

### Configuration du répertoire de supervision 
On doit indiquer à Nagios quel répertoire contiendra les fichiers de configuration des machines que nous souhaitons superviser.


**Modifiez le fichier de configuration Nagios :**  

```
vim /usr/local/nagios/etc/nagios.cfg
```

- Décommentez (rendre actif) la ligne suivante :

  ```
  cfg_dir=/usr/local/nagios/etc/servers
  ```


**Créez le répertoire (si nécessaire) :**

Si le répertoire activé n'existe pas encore, créez-le manuellement avec la commande suivante :

```
mkdir -p /usr/local/nagios/etc/servers
```


**Changez les droits d'accès :**

```
chown nagios:nagios /usr/local/nagios/etc/servers
```
```
chmod 750 /usr/local/nagios/etc/servers
```
<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

### Mettre en place le plugin permettant de communiquer avec les agents NRPE distants


**Installer le plugin check_nrpe :**

C'est ce plugin que l'agent NRPE utilisera pour l'envoie de commande.
```
apt install nagios-nrpe-plugin
```
**Déplacez ce plugin à l'endroit où Nagios attends les plugins :**
```
mv /usr/lib/nagios/plugins/check_nrpe /usr/local/nagios/libexec/
```
```
chown nagios:nagios /usr/local/nagios/libexec/check_nrpe
```
```
chmod 750 /usr/local/nagios/libexec/check_nrpe
```
**Définir la commande :**

Pour pouvoir utiliser un plugin depuis notre serveur Nagios, le plugin doit être déclarer  dans `commands.cfg`. Cela permet à Nagios de savoir comment utiliser le plugin `check_nrpe` pour interroger les hôtes distants. Cela définit la méthode d'exécution du plugin afin de récupérer les informations de supervision depuis les machines supervisées.

```
vim /usr/local/nagios/etc/objects/commands.cfg
```

- Ajouter ou vérifier que ce contenu est bien à l'intérieur :
  ```
  define command {
    command_name    check_nrpe
    command_line    /usr/local/nagios/libexec/check_nrpe -H $HOSTADDRESS$ -c $ARG1$ $ARG2$ $ARG3$
  }
  ```

Redémarrez le service :
```
systemctl restart nagios
```

### **[↩️ Retour](../../linux/nagioscore/index.md)**