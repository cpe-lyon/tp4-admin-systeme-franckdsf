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
