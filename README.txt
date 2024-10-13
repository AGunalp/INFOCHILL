1. Documentation Git pour Modifier des Fichiers

2. Ouvrir un fichier : Utilisez votre éditeur de texte pour ouvrir un fichier et le modifier.

3. Enregistrer les modifications : Enregistrez le fichier.

4. Vérifier l'état : Ouvrez le terminal, allez dans le dossier de votre projet, et tapez :
git status

5. Ajouter à la zone de staging : Tapez pour ajouter tous les fichiers modifiés :
git add .

6. Revérifiez l'état (si ils sont bien en vert)
git status

    Ou pour un fichier spécifique :
git add nom_du_fichier.txt

7. Commiter les modifications : Tapez pour enregistrer les modifications avec un message :
git commit -m "Votre message ici"

8. Pousser vers le dépôt distant : Tapez pour envoyer vos commits :
git push origin main

    Si vous avez une erreur, utilisez :

git pull origin main
ou
git push --force origin main