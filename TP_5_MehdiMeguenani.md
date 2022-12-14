# TP 5 - Systèmes de fichiers, partitions et disques Mehdi Meguenani

## Exercice 1. Disques et partitions

### 1. Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement alloués ; puis démarrez la VM

Pour ajouter un disque dur sur Vsphere il faut aller dans les configuration de la VM puis ajouter un périphérique : 

![ScreenShotQuestion8](./assetp5/Q1.png)

### 2. Vérifiez que ce nouveau disque dur est bien détecté par le système

Afin de vérifier que le nouveau disque est bien détecté il faut faire la commande ``` ll /dev/sd* ``` qui va permettre d'afficher les disques détectés
![ScreenShotQuestion8](./assetp5/Q2.png)

### 3. Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7)

Afin de faire partitioner le disk il faut dans un premier temps utilisé la commande ``` fdisk /dev/sdb ```. Appuyer sur la touche ``` n ``` puis sélectionner 
``` primary en appuyant sur p ``` puis il faut décider du numéro de la partition et du nombre d'espace alloué.
![ScreenShotQuestion8](./assetp5/Q3.png)

### 4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions ( pensez à consulter le manuel !)

![ScreenShotQuestion8](./assetp5/Q4.png)

### 5. Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-telle pas sur notre disque ?

La commande df -T ne marche pas sur le disque car il n'est pas monté 

### 6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)
![ScreenShotQuestion8](./assetp5/Q6.png)

### 7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration

![ScreenShotQuestion8](./assetp5/Q7.png)

## Exercice 2. Partitionnement LVM

### 1. On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab

Pour démonter les fichiers monté il faut effectuer les commande ``` sudo umount /win ; sudo umount /data ```. Il faut aussi supprimer les lignes écrite dans la question 6 précèdement.

![ScreenShotQuestion8](./assetp5/Q10.PNG)

### 2. Supprimez les deux partitions du disque, et créez une partition unique de type LVM

Pour suprimmer les deux partition il faut utiliser la commande ``` fdisk ``` puis utiliser la lettre ``` d ``` 

![ScreenShotQuestion8](./assetp5/Q11.PNG)

Pour créer une partitio LVM il faut lui mettre le type 8 "AIX" qui est un type Logique .

![ScreenShotQuestion8](./assetp5/Q12.PNG)


### 3. A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en utilisant la commande pvdisplay

Afin de créer un volulme physique avec la commande il pvcreate il faut faire la commande ``` sudo pvcreate PV /dev/sdb1 ```

Puis vérifier si elle a été bien créer avec ``` pvdisplay ``` 

![ScreenShotQuestion8](./assetp5/Q13.PNG)
### 4. A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay.

Pour créer un groupe de volume qui ne contient que le volume créer précèdement il faut effectuer la commande suivante ``` sudo vgcreate VG1 /dev/sdb1 ```  

![ScreenShotQuestion8](./assetp5/Q14.PNG)

### 5. Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.

![ScreenShotQuestion8](./assetp5/Q15.PNG)

### 6. Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data.

Pour créer la partition il faut utiliser  la commande ``` fdisk /dev/mapper/VG1-lvDATA ```

![ScreenShotQuestion8](./assetp5/Q16.PNG)  

Ensuite pour formater la partition il faut effectuer la commande mkfs.ext4 /dev/mapper/VG1-lvDATA

![ScreenShotQuestion8](./assetp5/Q161.PNG)

Puis pour finir il faut modifier le fichier avec la commande ``` /etc/fstab ```
 
![ScreenShotQuestion8](./assetp5/Q162.PNG)

### 7. Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.
![ScreenShotQuestion8](./assetp5/Q17.PNG)
![ScreenShotQuestion8](./assetp5/Q171.PNG)
### 8. Utilisez la commande vgextend <nom_vg> <nom_pv> pour ajouter le nouveau disque au groupe de volumes

Pour ajouter le nouveau disque au groupe il faut effectuer la commande ``` vgextend VG1 /dev/sdc1 ```
![ScreenShotQuestion8](./assetp5/Q18.PNG)

### 9. Utilisez la commande lvresize (ou lvextend) pour agrandir le volume logique. Enfin, il ne faut pas oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.

Pour agrandir le volume logique il faut faire les commandes suivante ``` sudo lvresize -l +100%FREE /dev/VG1/lvDATA ; sudo resize2fs /dev/VG1/lvDATA ```  

![ScreenShotQuestion8](./assetp5/Q19.PNG)

