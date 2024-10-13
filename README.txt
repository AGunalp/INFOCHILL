1. Vérifier l'état du projet
    Dans le terminal, naviguez vers le dossier de votre projet et tapez :
        git status
    (Cela vous montrera les fichiers modifiés ou nouveaux)

2. Ajouter les fichiers modifiés à la zone de staging
    Pour ajouter tous les fichiers modifiés à la zone de staging, tapez :
        git add .

    Si vous souhaitez ajouter un fichier spécifique, utilisez cette commande :
        git add nom_du_fichier.txt

3. Vérifier à nouveau l'état
    Pour vérifier que les fichiers ont bien été ajoutés à la zone de staging (ils apparaîtront en vert), tapez à nouveau :
        git status

4. Committer les modifications
    Une fois les fichiers ajoutés à la zone de staging, vous pouvez enregistrer vos modifications avec un message explicatif :
        git commit -m "Votre message ici"
    (Le message de commit doit être clair et décrire les changements que vous avez apportés)

5. Pousser les commits vers le dépôt distant
    Pour envoyer vos commits vers votre dépôt distant (par exemple, sur GitHub), tapez :
        git push -u origin main
    (La commande -u (ou --set-upstream) configure la branche locale main pour qu'elle suive automatiquement la branche distante main. Cela vous permettra d'utiliser la simple commande git push pour les prochaines fois)

    Si vous avez déjà configuré la branche et souhaitez simplement pousser les changements, utilisez :
        git push origin main

    Si vous voulez forcer l'envoi (en écrasant les changements distants), utilisez avec prudence :
        git push --force origin main

7. Apporter les modifications du dépot distant au dépot local
    Si vous avez une erreur indiquant que le dépôt distant a été mis à jour avant votre commit, vous devez récupérer les modifications distantes avant de pousser les vôtres :
        git pull origin main

    Ensuite, poussez vos changements :
        git push origin main

