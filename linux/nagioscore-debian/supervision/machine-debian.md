<link rel="stylesheet" type="text/css" href="../../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../../index.md) > [Nagios Core](../../nagioscore-debian/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Superviser une machine Linux</a>


<div style="background-color: #333; color: #fff; border-left: 5px solid #ff9900; border-right: 5px solid #ff9900; padding: 20px 25px; margin-bottom: 20px; text-align: center;">
  <strong style="font-size: 24px; color: #ff9900;">📚 SUPERVISER UNE MACHINE LINUX</strong>
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

Nous allons voir comment superviser une machine.

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS UNE MACHINE LINUX (A SUPERVISER ):</strong>
</div>

### Pré-requis :  
**Mettez à jour votre système :**  
Assurez-vous que votre système est à jour pour éviter les problèmes de compatibilité.

```
apt update && apt upgrade
```


**Installez les paquets nécessaires :**  

```
apt install nagios-nrpe-server
apt install nagios-plugins
```
- L'installation de **l'agent NRPE** est indispensable, car c'est cet agent, (présent notament sur le serveur NAGIOS), qui permet l'échange des informations entre le serveur Nagios et la machine supervisée.
- L'installation de **plugins** est notamment nécessaire car ce sont des scripts  qui seront exécutés localement sur chaque machine supervisée. L'agent NRPE transmet ensuite les résultats de ces scripts au serveur Nagios.

---

### Etablir une connexion avec notre serveur Nagios  
Le but est de configurer l'agent NRPE pour qu'il accepte les connexions du serveur Nagios en ajoutant son adresse IP à la liste des hôtes autorisés.

**Modifiez le fichier de configuration NRPE :**  
```
vim /etc/nagios/nrpe.cfg
```

- **Configurer les adresses IP autorisées :**  
  A la fin de cette ligne, rajoutez l'adresse IP du serveur Nagios (pour mon cas 192.168.1.200) : 
  ```
  allowed_hosts=127.0.0.1,::1, 192.168.1.200
  ```

  Cela permettra à l'agent NRPE de cette machine, à communiquer avec l'agent NRPE ayant comme IP `192.168.1.200` (donc pouvoir communiquer avec notre serveur)

**Redémarrez le service NRPE :**  
```
systemctl restart nagios-nrpe-server.service
```

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS VOTRE SERVEUR NAGIOS :</strong>
</div>

Après avoir configuré l'agent NRPE sur la machine que vous souhaitez superviser, vous pouvez maintenant définir cette machine en tant qu'hôte sur votre serveur Nagios pour qu'elle soit référencer sur l'interface de surveillance.



### Définir l'hôte :

**Créez un fichier en .cfg destiné à la machine à superviser :**  

Nous allons créer un fichier de configuration pour la machine Debian que nous voulons surveiller, nommée `UneMachineLinux.cfg`.

```
touch /usr/local/nagios/etc/servers/UneMachineLinux.cfg
```

**Éditez le fichier :**  

```
vim /usr/local/nagios/etc/servers/UneMachineLinux.cfg
```

- **Définissez l'hôte (l'hôte = machine à superviser) :**  

Rajoutez ce code dans votre fichier **.cfg** (en ajusatant) afin de définir l'hôte : 

    define host {
        address                 192.168.1.201         ; Adresse IP de l'hôte
        host_name               UneMachineLinux      ; Nom de l'hôte
        alias                   Machine Linux       ; Pour l'affichage sur Nagios
        use                     linux-server          ; Template pré-défini
    }

  - **use :** Les valeurs de ce template seront utilisées si certaines valeurs ne sont pas précisée, vous pouvez trouver ce template (ainsi que les valeurs associées) dans `/usr/local/nagios/etc/objects/templates.cfg`
  - **host_name :** Le nom de la machine à qui est destiné ce fichier (le nom de la machine qu'on souhaite superviser)
  - **alias :** Le nom affiché sur l'interface nagios (pour qu'on reconnaisse directement la machine).
  - **address :** L'adresse IP de la machine à qui est destiné ce fichier.


<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

#### Redémarrez les services nagios (ou reboot) :

```
systemctl restart nagios
```

### Etat de Nagios :
![alt text](../images/nagios_host_ok_unemachinelinux.png)

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">

## Définir un service :

Nous allons devoir maintenant définir un service, afin de donner l'ordre depuis notre serveur Nagios, d'executer un plugin sur notre "UneMachineLinux" par l'intermédiaire de l'agent NRPE.

Nous allons définir un service qui vérifie l'espace disque d'une machine.

<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS UNE MACHINE LINUX (A SUPERVISER ):</strong>
</div>

**Vérifiez si un plugin permettant de voir l'espace libre d'un disque existe :**

```
ls -l /usr/lib/nagios/plugins/
```

**Vérifiez si la commande pour executer ce script est déjà défini :**    
Quand cette machine reçoit une commande de la part de Nagios, il doit savoir faire la liaison entre cette commande et le script a executer.

```
vim /etc/nagios/nrpe.cfg
```
- **Ajoutez ce contenu (si n'existe pas) :**  
  ```
  command[check_disk]=/usr/lib/nagios/plugins/check_disk -w 30% -c 20% -p /
  ```
  - Entre les crochets [check_disk], est la commande que notre serveur nagios enverra à cette machine.

**Rédemarrez le service :**

```
systemctl restart nagios-nrpe-server.service
```

<!-- Section "Depuis votre serveur Nagios" avec un fond sombre, couleurs contrastées et texte clair -->
<div style="background-color: #333; color: #fff; border-left: 5px solid #00bcd4; padding: 10px 10px; margin-bottom: 20px;">
  <strong style="font-size: 17px; color: #00bcd4;">🖥️ DEPUIS VOTRE SERVEUR NAGIOS :</strong>
</div>

**Définissez le service  :**  

```
vim /usr/local/nagios/etc/servers/UneMachineLinux.cfg
```

Ajoutez ce contenu :
  ```
  define service {
    host_name                       UneMachineLinux          ; Nom de l'hôte
    service_description             Disk Usage                 ; Description du service
    check_command                   check_nrpe!check_disk ; Commande de vérification du disque
    use                             generic-service            ; Modèle générique utilisé
  }
  ```
- Execute en local "check_nrpe" pour envouer "check_disk" à la machine distante.


#### Redémarrez le services nagios (ou reboot) :

```
systemctl restart nagios
```


---

### **[↩️ Retour](../index.md)**