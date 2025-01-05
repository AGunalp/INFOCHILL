<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">



###### 📂 Vous êtes ici : [Accueil](../../index.md) > [OpenMediaVault](../openmediavault/index.md) > <a href="." style="color: #ff9900; text-decoration: underline;">Installation d'OMV-Extras</a>

<div style="background-color: #333; color: #fff; border-left: 5px solid #ff9900; border-right: 5px solid #ff9900; padding: 18px 22px; margin-bottom: 18px; text-align: center;">
  <strong style="font-size: 22px; color: #ff9900;">📚 INSTALLATION D'OMV-EXTRAS SUR OPENMEDIAVAULT</strong>
</div>

<div style="color: #d9534f; font-weight: bold; margin-bottom: 1em;">
  <p>Ce guide vous aide à installer et configurer **OMV-Extras**, un plugin permettant d'étendre les fonctionnalités d'OpenMediaVault en ajoutant des plugins supplémentaires. En cas de problèmes liés à la connexion réseau, des solutions sont également proposées pour résoudre ces erreurs courantes.</p>
  <ul>
    <li><strong>Prérequis :</strong> Vous avez déjà installé OpenMediaVault sur votre serveur.</li>
    <li><strong>Objectif :</strong> Installer OMV-Extras et résoudre les erreurs liées au réseau si nécessaire.</li>
  </ul>
</div>

<hr style="border: 1px solid #ccc; height: 1px; background-color: #ccc; border: none;">


### Télécharger et installer OMV-Extras

Cette commande permet de télécharger et d'executer le script d'installation pour obtenir "OMV-Extras"
```
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/install | bash
```
- **-O** : Cette option est utilisée pour rediriger le fichier téléchargé vers la sortie standard (c'est-à-dire le terminal). Cela permet de traiter immédiatement le contenu du fichier téléchargé sans avoir à le sauvegarder sur disque
- **[Forum](https://forum.openmediavault.org/index.php?thread/5549-omv-extras-org-plugin/)** : Pour voir la commande à jour.
- **[Vidéo Tutoriel](https://www.youtube.com/watch?v=6gvfBm1xGO8)** : Pour voir comment le tutoriel.


### ⚠️ Problèmes courants et dépannage

Vous aurez ce problème si vous n'avez pas configurer d'interface.

Si vous rencontrez une erreur du type **"wget : unable to resolve host address 'github.com'"**, cela signifie probablement un problème de réseau ou de DNS sur votre serveur. Pour résoudre ce problème, suivez les étapes ci-dessous :

   **Solution à l'erreur "unable to resolve host address"**

   1. Vérifiez que votre serveur OMV dispose d'une connexion réseau fonctionnelle.
   2. Si vous avez un problème avec les serveurs DNS, vous pouvez reconfigurer l'interface réseau en utilisant **`omv-firstaid`**.
   

1. **Configurer l'interface réseau**

   Si vous avez des problèmes de connexion réseau, vous pouvez configurer votre interface réseau en utilisant **`omv-firstaid`**.

   Pour ce faire, ouvrez un terminal et tapez :

   ```
   omv-firstaid
   ```

   Ce programme vous guidera pour configurer ou vérifier la configuration réseau de votre serveur.

### **[↩️ Retour](../openmediavault-debian/index.md)**