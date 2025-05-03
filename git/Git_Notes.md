# Commandes   (Git)
- différents work  spaces  de git ! 

<p align="center">
  <img src="git.png" alt="diffrents-git-work-spaces">
</p>


- `git status` : affiche les fichiers et dossiers qui ne sont pas encore suivis (untracked) ou ceux qui ont été modifiés mais ne sont pas encore ajoutés à la *staging area*.

- `git log` : affiche l'historique complet des commits.

    - `git log --oneline` : affiche l'historique des commits sous forme condensée, chaque commit tenant sur une seule ligne.

    - `git log -n 2` : affiche uniquement les deux derniers commits.

- `git restore` : Restauration du working directory

    La commande git restore permet de restaurer le répertoire de travail (working directory) en le réinitialisant à l'état du dernier commit. Elle possède deux options principales :



    - `--staged` : retire les fichiers de la **staging area**, les ramenant dans le working directory sans supprimer les modifications.

        ```bash
        git restore test.txt --staged #supprimer test.txt de la staging area (resotrer le fichier dans mon work space)
        ```

    - `--source`: resotore le fichier  depuis un commit source 

        ```bash
           git restore --source=3a7e1c2 # monFichier.java restorer le fichier (monfichier) dpuis le commit 3a7e1c2
        ```

- `git branch <branch_name>`: creer une  branch `branch_name`;
- `git branch -d <branch_name>`: supprimer la  branche `branch_name`;

## Merge

<p align="center">
  <img src="branch.png" alt="branch">
</p>



-   pour merger  la branche  `feature` dans  la  branche `master` 
```bash
    git checkout master # depalcer dans  la branche ou  je veux avoir mes  modifs
    git merge feature
```
   - le ` git merge` mege aussi les  commits  des  deux branch dans la branche  cible 

## Conflicts 

Tout ce qui se trouve entre ``<<<<<< HEAD et ======`` correspond aux modifications faites dans la branche principale (courante), tandis que tout ce qui est entre`` ====== et >>>>>> bugFix`` correspond aux modifications de la nouvelle branche que l'on souhaite fusionner.


<p align="center">
  <img src="conflit.png" alt="conflit">
</p>










## Configuration

- Configurer l'email et le nom :
    ```bash
    git config --global user.name "rocks.D.Xebec"  
    git config --global user.email "rocks.D.Xebec@example.com"
    ```

- Afficher l'email et le nom configurés :
    ```bash
    git config --global user.name  
    git config --global user.email
    ```

- Configurer un éditeur :
    ```bash
    git config --global core.editor "code --wait"
    ```
    > `--wait` est une option qui permet à Git de **suspendre l'exécution** jusqu'à ce que l'éditeur soit **fermé après enregistrement**, au lieu de continuer immédiatement.
