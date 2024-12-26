<link rel="stylesheet" type="text/css" href="../../assets/css/principal-theme.css">


## Description
Ce guide vous accompagne dans l'installation de **OMV-Extras** sur **OpenMediaVault** (OMV). OMV-Extras est un plugin essentiel qui permet d'étendre les fonctionnalités de votre serveur OMV en ajoutant des plugins supplémentaires. Si vous rencontrez des erreurs lors de l'installation, vous trouverez également des solutions pour résoudre des problèmes courants liés à la connexion réseau.


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