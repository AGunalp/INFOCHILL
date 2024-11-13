<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">

###### 📂 Vous êtes ici : [Accueil](../../index.md) > [GLPI Debian](../glpi-debian/index.md) > <a href="" style="color: #ff9900; text-decoration: underline;">Remonter Debian sur GLPI</a>


Installer Rsyslog :
```
apt isntall rsyslog
```


Accéder au fichier de configuration de Rsyslog  
```
vim /etc/rsyslog.conf
```

Dé-commanter :  
```
$ModLoad imudp
$UDPServerRun 514  
```

