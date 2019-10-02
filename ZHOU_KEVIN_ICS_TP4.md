# Exercice 1. Gestion des utilisateurs et des groupes
**• Commencez par créer deux groupes groupe1 et groupe2**
```bash
kz@serveur:~$ sudo groupadd groupe1
kz@serveur:~$ sudo groupadd groupe2
```
**• Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell**
```bash
kz@serveur:~$ sudo useradd -m u1
kz@serveur:~$ sudo useradd -m u2
kz@serveur:~$ sudo useradd -m u3
kz@serveur:~$ sudo useradd -m u4
```
*Pour mettre bash comme shell :*
```bash
chsh -s /bin/bash u1
chsh -s /bin/bash u2
chsh -s /bin/bash u3
chsh -s /bin/bash u4
```
**• Placez les utilisateurs dans les groupes : - u1, u2, u4 dans groupe1 - u2, u3, u4 dans groupe2 • Donnez deux moyens d’aﬀicher les membres de groupe2**

*u1, u2, u4 dans groupe1 :*
```bash
kz@serveur:~$ sudo usermod -a -G groupe1 u1
kz@serveur:~$ sudo usermod -a -G groupe1 u2
kz@serveur:~$ sudo usermod -a -G groupe1 u4
```
*u2, u3, u4 dans groupe2:*

```bash
kz@serveur:~$ sudo usermod -a -G groupe2 u2
kz@serveur:~$ sudo usermod -a -G groupe2 u3
kz@serveur:~$ sudo usermod -a -G groupe2 u4
```
*Deux moyens d'afficher les membres de groupe2:*
```bash
kz@serveur:~$ grep "groupe2" /etc/group
groupe2:x:1002:u2,u3,u4

```

**• Faites de groupe1 le groupe propriétaire de/home/u1et/home/u2 et degroupe 2 le groupepropriétaire de /home/u3 et /home/u4**
```bash
kz@serveur:~$ sudo chown -R :groupe1 /home/u1 /home/u2
kz@serveur:~$ sudo chown -R :groupe2 /home/u3 /home/u4
```

**• Remplacez le groupe primaire des utilisateurs : - groupe1 pour u1 et u2 - groupe2 pour u3 et u4**

*Modifier groupe primaire u1, u2 = groupe1 :*
```bash
kz@serveur:~$ sudo usermod -g groupe1 u1
kz@serveur:~$ sudo usermod -g groupe1 u2
```
*Modifier groupe primaire u3, u4 = groupe2 :*
```bash
kz@serveur:~$ sudo usermod -g groupe2 u3
kz@serveur:~$ sudo usermod -g groupe2 u4
```
**• Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.**

*Création de 2 répertoires :*
```bash
kz@serveur:~$ sudo mkdir /home/groupe1
kz@serveur:~$ sudo mkdir /home/groupe2
```
*On définit les groupes 1 et 2 comme propriétaire de groupe du répertoire: *
```bash
kz@serveur:/home$ sudo chown -R :groupe2 groupe2
kz@serveur:/home$ sudo chown -R :groupe1 groupe1
```
*Permissions :*
```bash
kz@serveur:/home$ sudo chmod g+w groupe1
kz@serveur:/home$ sudo chmod g+w groupe2
```
*La touche ll permet de visualiser les droits des fichiers et dossiers*

**• Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier?**

```bash
kz@serveur:/home$ sudo chmod go-w groupe1
kz@serveur:/home$ sudo chmod go-w groupe2
```

**• Pouvez-vous vous connecter en tant que u1? Pourquoi?**

*On ne peut pas se connecter parce qu'il manque le mot de passe, on va devoir renseigner le mot de passe pour rendre la compte actif*

**• Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.**

*Changer le mot de passe :*

```bash
kz@serveur:~$ sudo passwd u1
[sudo] password for kz:
New password:
Retype new password:
passwd: password updated successfully
```

**• Quels sont l’uid et le gid de u1?**
```bash
kz@serveur:~$ id u1
uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
```
**• Quel utilisateur a pour uid 1003?**
```bash
kz@serveur:~$ cat /etc/passwd | cut -f3 -d: | grep "1003"
1003
```
**• Quel est l’id du groupe groupe1?**
```bash
kz@serveur:~$ cat /etc/group | grep "groupe1"
groupe1:x:1001:u1,u2,u4
```
*l'ID du groupe1 est 1001*

**• Quel groupe a pour guid 1002? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)**

```bash
kz@serveur:~$ cat /etc/group | grep "1002"
groupe2:x:1002:u2,u3,u4
```
*Le groupe2 a pour ID 1002*

**• Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il? Expliquez.**

```bash
kz@serveur:~$ sudo gpasswd -d u3 groupe2
[sudo] password for kz:
Removing user u3 from group groupe2
```
 *Si on supprime l’unique groupe d’un utilisateur, il revient dans le groupe par
défaut à son nom.*

**• Modifiez le compte de u4 de sorte que : — il expire au 1er juin 2019** 
```bash
kz@serveur:~$ sudo chage -E 2019/06/01 u4
[sudo] password for kz:
```
**— il faut changer de mot de passe avant 90 jours**
```bash
kz@serveur:~$ sudo chage -M 90 u4
```
**— il faut attendre 5 jours pour modifier un mot de passe** 
```bash
kz@serveur:~$ sudo chage -m 5 u4
```
**— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe**
```bash
kz@serveur:~$ sudo chage -W 14 u4
```
**— le compte sera bloqué 30 jours après expiration du mot de passe**
```bash
kz@serveur:~$ sudo chage -I 30 u4
```

**• Quel est l’interpréteur de commandes (Shell) de l’utilisateur root?**
```bash
root@serveur:/home/kz# echo $SHELL
/bin/bash
```
**• à quoi correspond l’utilisateur nobody?**

*Dans la plupart des variantes d'Unix, nobody (personne en anglais) est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont.*

**• Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire? Quelle commande permet de forcer sudo à oublier votre mot de passe?**

*La commande sudo conserve le mot de passe pendant 15m, la commande :*
```bash
exit or ctrl+d
```
*permet de forcer sudo à oublier votre mot de passe*

https://vitux.com/how-to-specify-time-limit-for-a-sudo-session/


# Exercice 2. Gestion des permissions 

**• Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier?**
```bash
kz@serveur:~$ mkdir test
kz@serveur:~$ cd test
kz@serveur:~/test$ touch fichier
```
*Pour voir les droits :*
```bash
kz@serveur:~/test$ ll
total 8
drwxrwxr-x  2 kz kz 4096 oct.   2 10:52 ./
drwxr-xr-x 10 kz kz 4096 oct.   2 10:52 ../
-rw-rw-r--  1 kz kz    0 oct.   2 10:52 fichier
```
**• Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher en tant que root. Conclusion?**

*Pour retirer les droits :*
```bash
kz@serveur:~/test$ chmod 000 fichier
kz@serveur:~/test$ ll
total 8
drwxrwxr-x  2 kz kz 4096 oct.   2 10:52 ./
drwxr-xr-x 10 kz kz 4096 oct.   2 10:52 ../
----------  1 kz kz    0 oct.   2 10:52 fichier
```

```bash
kz@serveur:/home/test$ sudo su
root@serveur:/home/test# ls
fichier
root@serveur:/home/test# nano fichier
root@serveur:/home/test# cat fichier
root@serveur:/home/test#
```
*On observe que root outrepasse les droits et qu'il peut modifier et afficher le fichier"*

**• Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits?**

```bash
kz@serveur:/home/test$ sudo chmod 333 fichier
kz@serveur:/home/test$ ll
total 12
drwxr-xr-x  2 root root 4096 oct.   2 09:49 ./
drwxr-xr-x 10 root root 4096 oct.   2 08:37 ../
--wx-wx-wx  1 root root    7 oct.   2 09:49 fichier*

kz@serveur:~/test$ echo "echo Hello" > fichier
```

*On a donner les droits d'écriture et d'execution, il y a bien "echo Hello" d'écrit dans fichier*

**• Essayez d’exécuter le fichier. Est-ce que cela fonctionne? Et avec sudo? Expliquez.**

```bash
kz@serveur:~/test$ ./fichier
bash: ./fichier: Permission denied
kz@serveur:~/test$ sudo ./fichier
Hello
```
*Je ne sais pas*


**• Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou aﬀichez le contenu du fichier fichier. Qu’en déduisez-vous? Rétablissez le droit en lecture sur test**
```bash
kz@serveur:~$ ll
d-wxrwxr-x  2 kz   kz    4096 oct.   2 10:58 test/
```
*Listez le contenu :*
```bash
kz@serveur:~/test$ ls
ls: cannot open directory '.': Permission denied
```
*Affichage du contenu fichier :*
```bash
kz@serveur:~/test$ cat fichier
cat: fichier: Permission denied
```

*On observe que si on enleve les droits sur le dossier parent, il s'applique aussi sur les sous dossiers du dossier parent*

```bash
kz@serveur:~$ chmod u+r test
kz@serveur:~$ ll
drwxrwxr-x  2 kz   kz    4096 oct.   2 10:58 test/
```
**• Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test.Tentez de modifier le fichier nouveau,puis de le supprimer.Que pouvez vous déduire de toutes ces manipulations?**
```bash
kz@serveur:~/test$ touch nouveau
kz@serveur:~/test$ mkdir sstest
kz@serveur:~/test$ ls
fichier  nouveau  sstest
```
*Retirez au fichier nouveau et au répertoire test le droit en écriture:*

```bash
kz@serveur:~/test$ chmod u-w nouveau
kz@serveur:~/test$ cd ..
kz@serveur:~$ chmod u-w test
```
*Tentez de modifier le fichier nouveau :*
```bash
kz@serveur:~/test$ Echo "Modification" > nouveau
-bash: nouveau: Permission denied
```
*Rétablissez ensuite le droit en écriture au répertoire test :*
```bash
kz@serveur:~$ chmod u+w test
```
*Tentez de modifier le fichier nouveau,puis de le supprimer :*
```bash
 kz@serveur:~/test$ Echo "Modification2" > nouveau
-bash: nouveau: Permission denied
kz@serveur:~/test$ rm nouveau
rm: remove write-protected regular empty file 'nouveau'?
```
*On observe que meme si on a remis le droit d'écriture au fichier test, on ne peut toujours pas ecrire sur le fichier nouveau car il n'a pas les droits*

**• Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.**
```bash
kz@serveur:~$ chmod u-x test
kz@serveur:~$ ll
drw-rwxr-x  3 kz   kz    4096 oct.   2 11:21 test/
```

 **Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires?**
```bash
kz@serveur:~$ cd test
-bash: cd: test: Permission denied
```
*En enlevant le droit en execution, il est impossible de faire toute manipulation sur le dossier test*

**• Rétablissez le droit en exécution du répertoire test.**
```bash
kz@serveur:~$ chmod u+x test
drwxrwxr-x  3 kz   kz    4096 oct.   2 11:21 test/
``` 
**Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant?**
```bash
kz@serveur:~/test$ chmod u-x ../test
```
*Quand on enleve un droit sur le répertoire parent, on observe que le droit enlevé s'applique dans tous les sous dossiers*

**Peut-on retourner dans le répertoire parent avec ”cd ..”? Pouvez-vous donner une explication?**

*Comme on a supprimé le droit d'execution sur le répertoire test, on ne peut plus rentrer dans le dossier test mais on peut en ressortir.*


**• Rétablissez le droit en exécution du répertoire test.**
```bash
kz@serveur:~$ chmod u+x test
```
**Attribuez au fichier fichier les droits suﬀisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.**
```bash
kz@serveur:~/test$ chmod g+r fichier
kz@serveur:~/test$ ll
--wxr---wx  1 kz kz   11 oct.   2 10:57 fichier*
```

**• Définissez un umask très restrictif qui interdit à qui conque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.**

*Repertoire :*
```bash
kz@serveur:~$ umask 0077
kz@serveur:~/test$ mkdir umask
kz@serveur:~/test$ ll
drwx------  2 kz kz 4096 oct.   2 12:51 umask/
```

*Fichier :*
```bash
kz@serveur:~/test$ touch mkdirfichier
kz@serveur:~/test$ ll
-rw-------  1 kz kz    0 oct.   2 12:53 mkdirfichier
```

 **• Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.**


```bash
kz@serveur:~$ umask 0022
kz@serveur:~/test$ touch aaa
kz@serveur:~/test$ mkdir bbbb
kz@serveur:~/test$ ll
-rw-r--r--  1 kz kz    0 oct.   2 13:02 aaaa
drwxr-xr-x  2 kz kz 4096 oct.   2 13:02 bbbb/
```

 
**• Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.**
 
```bash
kz@serveur:~/test$ umask 0037
kz@serveur:~/test$ mkdir a
kz@serveur:~/test$ touch b
kz@serveur:~/test$ ll
drwxr-----  2 kz kz 4096 oct.   2 13:06 a/
-rw-r-----  1 kz kz    0 oct.   2 13:06 b
```

**• Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :** 

**- chmod u=rx,g=wx,o=r fic**

```bash
kz@serveur:~/test$ chmod 534 fic
kz@serveur:~/test$ stat -c %A fic
dr-x-wxr--
kz@serveur:~/test$ stat -c %a fic
534
```
**- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---**
```bash
chmod 602 fic
```

**- chmod 653 fic en sachant que les droits initiaux de fic sont 711**
```bash
kz@serveur:~/test$ chmod 711 fic
kz@serveur:~/test$ chmod u-x,g+r,o+w fic
kz@serveur:~/test$ stat -c %a fic
653
```
**- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---**
```bash
chmod 520 fic
```
**• Aﬀichez les droits sur le programme passwd. Que remarquez-vous? En aﬀichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd?**

```bash
kz@serveur:/etc$ ls -l passwd
-rw-r--r-- 1 root root 1768 sept. 30 14:58 passwd
```
*On remarque que seul root peut lire et ecrire, les groupes et autres peuvent seulement lire. Celà permet de sécuriser le programme afin que les autres utilisateurs ne puissent pas le modifier.*


**• Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl. • Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.**
