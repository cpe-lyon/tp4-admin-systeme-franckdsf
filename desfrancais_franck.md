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

L'interpréteur de commandes de l'utilisateur est /bin/bash ( bash ) 
commande : `echo $SHELL`

### 18- à quoi correspond l’utilisateur nobody ?

nobody est un compte utilisateur qui possède tous les privilèges que les "autres utilisateurs" ont, sans plus. C'est un compte à qui aucun fichier n'appartient.
Ce compte est réservé aux commandes qui ne necessitent aucune permission pour avoir un résultat le plus neutre. Le plus souvent réservé aux services vulnérables (httpd, etc).

### 19- Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?

Le temps par défaut est 15 minute.
la commande pour forcer sudo à oublier son mot de passe est `sudo -k`.

## Exercice 2. Gestion des permissions

### 1- Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier

`cd ; sudo mkdir test ; cd test ; nano fichier`

avec `ls -l` dans le $HOME
les droits du dossier test sont 755 (droits de base pour un dossier sous linux) et les droits du fichier sont 664.

### 2- Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

dans $HOME
`chmod 000 test`

Je ne peux pas acceder en tant que user mais je peux y acceder en tant que root. Root passe outre les permissions.

### 3- Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

j'ai fait `chmod 300 fichier ; echo "echo hello world" > fichier`. On peut écrire dans le fichier.

### 4 - Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

Ca ne va pas marcher car on a pas les droits d'execution. Cependant en Root ça va marcher.

### 5- Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test

```
sudo chmod u-r ../test
ls 
cannot open directory '.': Permission denied
cat fichier
cat: fichier: Permission denied
```
On en déduit que les droits d'un dossier sont supérieurs à ceux des fichiers contenus.

### 6- Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez- vous déduire de toutes ces manipulations ?

```
touch nouveau
mkdir sstest
chmod u-w nouveau
chmod u-w ../test
nano nouveau
[ Error writing nouveau: Permission denied ]
chmod u+w ../test
nano nouveau
[ Error writing nouveau: Permission denied ] 
rm nouveau
rm: remove write-protected regular file 'nouveau'? yes
ls 
output : fichier sstest
```
On en déduit qu'il n'y pas d'héritage des droits pour les fichiers situé dans les répertoires

### 7- Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

```
chmod u-x test
 nano test/nouveau
[ Path 'test' is not accessible ]
 cd test/
-bash: cd: test/: Permission denied
ls test
ls: cannot access 'test/sstest': Permission denied
ls: cannot access 'test/fichier': Permission denied
fichier  sstest
```
On ne peut rien faire car on a pas les droits.

### 8- Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?

```
chmod u+x ../test
cd test
chmod u-x ../test
touch new
touch: cannot touch 'new': Permission denied
rm fichier
rm: cannot remove 'fichier': Permission denied
nano fichier
```
on ne peut pas sauvegarder les changements du fichier _fichier_

```
cd sstest
```
On ne peut pas se déplacer dans sstest
```
ls sstest
ls: cannot access 'sstest': Permission denied
```
On en conclu qu'enlever les droit à un dossier applique le changement aux fichiers enfants. 
On peut retourner dans le répertoire courant car nous avons toujours les droits sur celui-ci.

### 9- Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

commande `chmod 640 fichier`

### 10- Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

commande `umask 077 test`<br>
`mkdir ndir`<br>
commande `umask 077 dir`<br>
On ne peut acceder à aucun des deux.

### 11- Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

commande `umask 011 test`<br>
commande `umask 011 dir`<br>

### 12- Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

commande `umask 066 test`<br>
commande `umask 066 dir`<br>

### 13- Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses)

commande `chmod u=rx,g=wx,o=r fic` = `chmod 534` = `chmod -r-x--wx-r--`<br>
commande `chmod uo+w,g-rx fic` = `chmod 706` = `chmod -rwx-x-rw-`<br>

### 14- Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

`cd /etc`<br>
j'ai fait `ls -l passwd` pour afficher les droits sur le programme.
On remarque les droits suivants : `-rw-r--r-- 1 root root` <br>
Ainsi seul root a le droit d'écriture sur le fichier. Les autres (groupe et autres) ne peuvent que le lire.
