## examenUF4_M02-Sistemes
Passat a MarkDown

#### PROVA PRÀCTICA UF4

1. [0,5 punts] Desactiveu l'àrea d'intercanvi 'swap'
    Resposta:  

	swapoff

	(desmontar la partició swap):
	swapoff -a /dev/sda7
                
2. [0,5 punts] Elimineu-ne la partició de swap (Hauria de ser de 3GB)
    Resposta: 
  
	veure quina es la partició de swap: lsblk
	En el meu cas la swap està a /dev/sda7

	fdisk /dev/sda
		→ clicar d (delete) per borrar
		→ clicar 7 (la partició de swap)
		→ clicar w (write) per guardar els canvis

	També es pot fer servir el gparted com a root i eliminar-la directament.

	(desmontar la partició swap):
	swapoff -a /dev/sda7
	
                
3. [1 punts] Modifiqueu el fitxer de muntatge en arrencada fstab per tal que ja no carregui mai més la partició swap.
    Resposta: 
  
	geany /etc/fstab
	borrar la linia on es troba la swap, en el meu cas:
UUID=201a38a7-61e4-4c28-b347-d104621360bf swap	 swap  defaults	1 1

	També es pot comentar la linia (#) per si la volem fer servir en un futur.
                
4. [4 punts] Creeu els següents volums lògics sobre aquesta partició:
    Volum lògic de nom 'documents' i de tamany 1GB
        Resposta:   Exemple partició swap → /dev/sda7

		pvcreate /dev/sda7	→ (antiga partició de la swap, 3G, ara PV)
		vgcreate examenUF4	→ (posant nom al grup lògic, VG)
		lvcreate -L 1G -n documents examenUF4
                    
    Volum lògic de nom 'jocs' i de tamany 500MB
        Resposta:  
 
		lvcreate -L 500M -n jocs examenUF4
                                
    Volum lògic de nom 'dades1' i de tamany 100MB
        Resposta:   

		lvcreate -L 100M -n dades1 examenUF4
                                
    Volum lògic de nom 'dades2' i de tamany 100MB
        Resposta:   

		lvcreate -L 100M -n dades2 examenUF4
                                

5. [0,5 punts] Creeu els directoris /mnt/documents i /mnt/jocs
    Resposta: 
  
	  mkdir /mnt/documents /mnt/jocs
                





6. [0,5 punts] Creeu un sistema de fitxers XFS als volums lògics 'documents' i 'jocs'
    Resposta:  
 ```mkfs.xfs /dev/examenUF4/documents
	  mkfs.xfs /dev/examenUF4/jocs    
 ```

7. [1 punts] Munteu els volums lògics de la següent manera:
    lv documents -> /mnt/documents
        Resposta:  
 
		mount /dev/examenUF4/documents /mnt/documents
                    
    lv jocs -> /mnt/jocs
        Resposta:   

		mount /dev/examenUF4/jocs /mnt/jocs

                    
8. [2 punts] Creeu un raid 1 sobre els volums lògics 'dades1' i 'dades2'
    Resposta: 
    ```
  mdadm --create /dev/md0 --level=1 --raid-device=2 /dev/examenUF4/dades1 \ /dev/examenUF4/dades2 
  ```
Comprovar:  
```
cat /proc/mdstat
```
