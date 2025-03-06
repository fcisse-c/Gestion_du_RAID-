# Gestion du RAID sous Linux
### Étape 1 : Création d'un RAID 1 avec 2 disques
1. **Partitionnement des disques**
   ```bash
   sudo fdisk /dev/sdb
Crée une nouvelle partition primaire qui prend la totalité du disque et de type RAID Linux auto. Répète cette opération pour le second disque /dev/sdc.

Création du RAID 1

bash
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
Formatage du RAID

bash
sudo mkfs.ext4 /dev/md0 -L "PersonalData"
Montage du RAID

bash
sudo mkdir /home/wilder/Data-RAID1 -p
sudo mount /dev/md0 /home/wilder/Data-RAID1/
Pour que le RAID soit monté automatiquement à chaque démarrage, ajoute cette ligne à /etc/fstab :

/dev/md0 /home/wilder/Data-RAID1 ext4 nofail 0 0
Étape 2 : Reconstruction du RAID après une simulation de panne
Simulation d'une panne (déconnecte un des disques)

bash
sudo umount /dev/sdb1
Reconstruction du RAID avec un nouveau disque Partitionne le disque sdd comme précédemment et exécute :

bash
sudo mdadm --manage /dev/md0 --add /dev/sdd1
Étape 3 : Création d'un RAID 5
Partitionnement des disques (minimum 3 disques, sdb, sdc, sdd)

bash
sudo fdisk /dev/sdb
sudo fdisk /dev/sdc
sudo fdisk /dev/sdd
Création du RAID 5

bash
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb1 /dev/sdc1 /dev/sdd1
Formatage et montage du RAID 5

bash
sudo mkfs.ext4 /dev/md0 -L "PersonalData"
sudo mkdir /home/wilder/Data-RAID5 -p
sudo mount /dev/md0 /home/wilder/Data-RAID5/
Ajoute également cette ligne à /etc/fstab pour le montage automatique :

/dev/md0 /home/wilder/Data-RAID5 ext4 nofail 0 0
Historique de la commande history
Filtre les commandes inutiles et les erreurs pour ne garder que les commandes pertinentes. Par exemple :

bash
history | grep "mdadm\|fdisk\|mkfs\|mount\|umount"
