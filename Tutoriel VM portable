# Mise en place VM + environnement de travail portable

# 1. Preparer l’emplacement de la VM

Creer un dossier qui accueillera la machine virtuelle, possibilite de la mettre directement sur une clef usb.

Si clef USB, formater en format exFAT pour permettre au disque de la VM d’etre cree dessus:

→ Linux : 

[How to Format a USB Drive as exFAT on Linux](https://linuxopsys.com/topics/how-to-format-usb-drive-as-exfat-on-linux)

→Windows : 

[Comprehensive Guide to Formatting USB Drive to exFAT[2021]](https://recoverit.wondershare.com/usb-tips/format-usb-drive-to-exfat.html)

---

# 2. Creer la VM sur virtualbox

Sur Virtualbox creer une nouvelle machine virtuelle et selectionner en tant que “Machine Folder” la clef usb ou le dossier cree a cet effet (/mnt/nfs/homes/dsaada/sgoinfre dans mon cas)

![VM_new_machine_folder.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_new_machine_folder.png)

Lui allouer une memoire RAM suffisante 

![VM_new_machine_ram.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_new_machine_ram.png)

 Puis selectionner “Create a virtual hard disk” 

![VM_new_machine_virtualdisk.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_new_machine_virtualdisk.png)

On souhaite avoir un format .vdi afin de pouvoir importer cette VM depuis n’importe quel terminal avec Virtualbox installe

![VM_new_machine_vdi.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_new_machine_vdi.png)

Concernant le choix entre “Dynamically allocated” et “Fixed Size” tout depend du besoin, sur une clef usb qui ne servira qu’a stocker la VM, le fixed size la rendra plus performante. Pour un stockage sur un disque dur interne avec d’autres utilites, la dynamically allocated permettra d’occuper moins de place. 

![VM_new_machine_dynamic.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_new_machine_dynamic.png)

Laisser l’emplacement du disque par defaut (un sous dossier du dossier principal de la VM portant le nom de cette derniere) concernant la taille, un minimum de 8Go est recommande, je choisis d’accorder 20Go pour avoir de la marge. En cas de Dynamically allocated, c’est la taille maximale que pourra atteindre le disque, en cas de fixed size ce sera sa taille par defaut.

![VM_new_machine_disk.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_new_machine_disk.png)

Une fois ces operations effectuees notre disque est cree dans le dossier, il faut maintenant proceder a l’installation de l’OS.

---

# 3. Installation de l’OS sur la VM

Au lancement de la VM , fournir l’iso Debian et proceder a l’installation de l’OS

(installation graphique pour le moment , je remplirais plus tard une installation minimale )

Rien de particulier pour le moment

---

# 4. Parametrer l’affichage et la resolution de la VM:

### 4.1. Parametres Virtualbox

Avec la VM eteinte, dans virtualbox selectionner la VM → settings →display et augmenter la memoire video dediee (128 Mb) + Enable 3D acceleration.

![VM_display_settings.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_display_settings.png)

### 4.2. Creation d’user sudo et installation de packages

Lancer la machine, ouvrir un terminal et log en tant que superuser (root)

```
su -l
```

(Optionnel : Ajouter un nouvel utilisateur, remplacer <username> par le nom sans chevrons)

```
adduser <username>
```

On vous demande ensuite de choisir un mot de passe pour cet user et de remplir des informations optionnelles (possibilite de skip sans rien remplir)

Ajouter l’utilisateur au group sudo 

```
usermod -aG sudo <username>
```

Pour verifier que l’ajout a fonctionne, afficher les groupes de l’user et verifier la presence de sudo. 

```
groups <username>
```

![VM_adduser.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_adduser.png)

On passe sur l’user en question qui possede desormais les droits sudo

```
su - <username>
```

On cherche maintenant a installer les packages necessaires afin de gerer proprement l’affichage de la machine guest.

 

```
sudo apt update
```

```
sudo apt install build-essential dkms linux-headers-$(uname -r)
```

“uname -r” nous donne la version de noyau linux sur laquelle est basee notre machine virtuelle. 

Une fois l’installation de ces packages effectuee, nous allons monter l’iso correspondant aux VboxGuestAdditions puis les installer.

### 4.3. Monter le CD Guest Additions (iso)

Afin de monter l’iso VboxGuestAdditions.iso , on retourne sur virtualbox, on selectionne la VM et dans la partie “Storage” → “Optical drive” → cliquer “Choose a disk filChargez le fichier VboxGuestAdditions.iso ( je vous conseille de le mettre au meme niveau que le repo de la VM ).

![VM_guest_additions1.png](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_guest_additions1.png)

Une fois fait il faut reboot la VM

```
sudo reboot
```

---

Une fois la VM relancee, se connecter sur l’utilisateur possedant les droits sudo. Nous allons maintenant monter le CD virtuel dans un nouveau dossier:

- On commence par creer le dossier

```
sudo mkdir /mnt/cdrom
```

- On monte le disque dans ce nouveau dossier

```
sudo mount /dev/cdrom /mnt/cdrom
```

On obtient alors ce message: 

![VM_mount.PNG](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_mount.png)

On doit maintenant se rendre dans le dossier pour pouvoir lancer le script d’installation des guest additions.

```
cd /mnt/cdrom
```

 
Un “ls” dans ce dossier nous donne :

![VM_ls_mnt_cdrom.PNG](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_ls_mnt_cdrom.png)

Lancer le script [VboxLinuxAdditions.run](http://VboxLinuxAdditions.run) avec l’option —nox11 (pour ne pas ouvrir de Xterm )

```
sudo sh ./VBoxLinuxAdditions.run —nox11
```

Une fois l’installation terminee (peut prendre un peu de temps suivant la machine) on nous demande de redemarrer la machine pour appliquer les effets des actions effectuees:

![VM_end_guest.PNG](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_end_guest.png)

```
sudo reboot
```

Et voila ! Il est desormais possible de mettre la VM en full screen avec une mise a l’echelle automatique a la taille de l’ecran.

### 4.4. Activer le drag/drop et le copier/coller entre host et guest

Dans virtualbox, selectionner la VM et cliquer sur “Settings” → “General” → “Advanced” puis selectionner “Bidirectional” pour les 2 champs.

![VM_drag_drop_copy_paste.PNG](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_drag_drop_copy_paste.png)

A partir de ce moment, la VM est parametree pour fonctionner sur nimporte quelle machine hote avec la meilleure resolution possible , bootable directement depuis une clef usb avec des options de copier/coller et drag/drop activees par defaut.  

---

# 5. Importer la VM sur un nouvel ordinateur depuis la clef usb

Maintenant que l’environnement de travail est mis en place sur une VM qui reste de maniere perenne sur un disque dur/ clef usb, il ne reste plus qu’a l’importer sur nimporte quel ordinateur possedant VirtualBox installe. 

Pour cela ouvrir VirtualBox et cliquer sur “Add” , naviguer dans le dossier de la clef USB, dans le dossier de la VM, puis cliquer sur le fichier .vbox

Ici je fais l’import depuis une machine Windows:

![VM_add_VM.PNG](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_add_VM.png)

L’ajout est immediat et il n’y a plus qu’a lancer la VM.

![VM_verif.PNG](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/VM_verif.png)

On voit bien ici dans la partie display que la memoire video a 128Mb a bien ete conservee, idem pour VboxGuestAdditions.iso toujours monte sur l’optical drive, ainsi que le disque dur virtuel (ici Debian.vdi).

![great_success.jpg](Mise%20en%20place%20VM%20+%20environnement%20de%20travail%20portab%205dc01721a0e04842a0cc4764f63592fa/great_success.jpg)
