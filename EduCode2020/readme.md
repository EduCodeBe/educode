# Educode 2020
## A propos
TODO

## Comment utiliser
### Modifier le site
1. Faire un fork du projet.
2. Cloner sur votre machine le projet.
3. Lancer la commande `make` afin de :
   * installer lektor,
   * installer le plugin i18n pour Lektor,
   * installer `zip` afin de décompresser l'archive retourner par Crowdin pour les traductions.
4. Lancer la commande `lektor serve`.
5. Ouvrir un navigateur et aller sur http://127.0.0.1:5000/.
6. Modifier le contenu

### Traductions
1. Ajouter un fichier `CrowdinApiKey` contenant la clef d'authentification de Crowdin dans le dossier racine du projet. Cette clef est peut être fournie par les administrateur du projet Crowdin
2. `make crowdin-push` pour mettre les textes sur Crowdin. Il est très important de faire cette manipulation après toute modification (terminée) du site. Par exemple, avant un commit.
3. `make crowdin-pull` pour mettre à jour le contenu avec les traductions de Crowdin