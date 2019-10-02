## Exercice 1. Gestion des utilisateurs et des groupes

### 1 - Commencez par créer deux groupes groupe1 et groupe2

`sudo addgroup groupe1 ; sudo addgroup groupe2`

### 2 - Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell

`sudo useradd u{1..4} --create-home`

### 3 - Placez les utilisateurs dans les groupes

groupe1:<br>
`sudo usermod  -a -G groupe1 u1`<br>
`sudo usermod  -a -G groupe1 u2`<br>
`sudo usermod  -a -G groupe1 u4`<br>
groupe2:<br>
`sudo usermod  -a -G groupe2 u2`<br>
`sudo usermod  -a -G groupe2 u4`<br>
`sudo usermod  -a -G groupe2 u3`<br>

### 4 - Donnez deux moyens d’afficher les membres de groupe2

`grep "groupe2" /etc/group`
et installez le paquet members puis utilisez la commande
`members groupe2`

### 5 - Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

`sudo chown u1:groupe1 /home/u1` <br>
`sudo chown u1:groupe1 /home/u2` <br>
`sudo chown u1:groupe2 /home/u3` <br>
`sudo chown u1:groupe2 /home/u4` 

### 6- Remplacez le groupe primaire des utilisateurs
```
usermod -g groupe1 u1 
usermod -g groupe1 u2
usermod -g groupe2 u3
usermod -g groupe2 u4
```
### 7 - Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.

il faut se rendre dans le dossier /home avec `cd ..` <br>
puis créer les dossiers avec `sudo mkdir groupe 1` et `sudo mkdir groupe 2`

### 8 - Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?

On modifie le sticky bit (t)
`sudo chmod +t groupe1` <br>
`sudo chmod +t groupe2` <br>

### 9 - Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

On peut se connecter à u1 avec `su u1`. Cependant u1 n'a pas de mot de passe. On peut lui en ajouter un avec `sudo passwd u1`

### 10 - Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.

Il faut lui ajouter un mdp avec `sudo passwd u1`

### 11 - Quels sont l’uid et le gid de u1 ?

On l'obtient avec `id`
`uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)`

### 12 - Quel utilisateur a pour uid 1003 ?

On recupère les données des utilisateurs et on récupère seulement l'utilisateur avec "1003" dans sa ligne. <br>
`cat /etc/passwd | grep 1003`

### 13 - Quel est l’id du groupe groupe1 ?

`cat /etc/group | grep groupe1`<br>
L'id renvoyée est 1001.

### 14 -  Quel groupe a pour guid 1002 ?

`cat /etc/group | grep 1002`

### 15 -  Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.

On retire un utilisateur avec `gpasswd -d <user> <group>`
ici `gpasswd -d u3 groupe2`
u3 n'est plus dans le groupe 2 et ne peux donc plus modifier ou acceder au dossier /home/groupe2 . 

### 16 - Modifiez le compte de u4 de sorte que :
#### — il expire au 1er juin 2019 — il faut changer de mot de passe avant 90 jours — il faut attendre 5 jours pour modifier un mot de passe — l’utilisateur est averti 14 jours avant l’expiration de son mot de passe — le compte sera bloqué 30 jours après expiration du mot de passe

```
sudo usermod --expiredate 06/01/2019 u4  // le mdp expire le  
sudo chage -M 90 u4  // changer le mdp dans 90 jours
sudo chage -m 5 u4  // attendre 5j pour pouvoir rechanger le mdp 
sudo chage -W 14 u4  // avertissement 14 jours avant l’expiration de son mot de passe 
sudo chage -I 30 u4 // le compte sera bloqué 30 jours après expiration du mot de passe s'il ne change pas son mdp
```

### 17- Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?


### 18- à quoi correspond l’utilisateur nobody ?


### 19- Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?



## Exercice 2. Gestion des permissions

### 1- Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier

### 2- Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
