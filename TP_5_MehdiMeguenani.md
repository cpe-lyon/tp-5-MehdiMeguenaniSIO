# TP 5 - Systèmes de fichiers, partitions et disques Mehdi Meguenani

## Exercice 1. Disques et partitions

### 1. Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement alloués ; puis démarrez la VM

Pour ajouter un disque dur sur Vsphere il faut aller dans les configuration de la VM puis ajouter un périphérique : 

Photo 1 

### 2. Vérifiez que ce nouveau disque dur est bien détecté par le système

Afin de vérifier que le nouveau disque est bien détecté il faut faire la commande ``` ll /dev/sd* ``` qui va permettre d'afficher les disques détectés
Photo2

### 3. Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7)

Afin de faire partitioner le disk il faut dans un premier temps utilisé la commande ``` fdisk /dev/sdb ```. Appuyer sur la touche ``` n ``` puis sélectionner 
``` primary en appuyant sur p ``` puis il faut décider du numéro de la partition et du nombre d'espace alloué.
Photo 3

### 4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions ( pensez à consulter le manuel !)

Photo 4

### 5. Pourquoi la commande df -T, qui affiche le type de système de fichier des partitions, ne fonctionne-telle pas sur notre disque ?

La commande df -T ne marche pas sur le disque car il n'est pas monté 

### 6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)
Photo 6 

### 7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration

Photo 7

## Exercice 2. Partitionnement LVM

### 1. On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab

### 2. Supprimez les deux partitions du disque, et créez une partition unique de type LVM
