## Introducción

* El comando [`grub-install`](http://linux.die.net/man/8/grub-install) se encarga de hacer la instalación.
* Hay que saber cuándo y dónde se instala grub



## grub-install (I)

* Para instalar el GRUB en el MBR del primer disco
```bash
grub-install /dev/hda
```
```bash
grub-install '(hd0)'
```
* Para instalarlo en el sector de arranque de la primera partición
```bash
grub-install /dev/hda1
```
```bash
grub-install '(hd0,0)'
```

Note:
The command for installing both GRUB Legacy and GRUB 2 is grub-install. Also, you must specify the boot sector by
device name when you install the boot loader. The basic command looks like:
# grub-install /dev/hda
or
# grub-install '(hd0)'
Either command will install GRUB into the first sector of your first hard drive. On many systems, you would use
/dev/sda rather than /dev/hda in the first example. In the second example, you need single quotes around the
device name. If you want to install GRUB in the boot sector of a partition rather than in the MBR, you include a
partition identifier, as in /dev/hda1 or (hd0,0). This option doesn’t always work well with GRUB 2, however.



## ¿Cuándo reinstalar?

* La primera vez que se instala el GRUB
* Redimensionado o cambio de la partición *root* del GRUB
* Reinstalación de Windows (borra el MBR)
* Cambio de disco
* GRUB también se puede instalar el un floppy disk
```bash
grub-install /dev/fd0
```

Note:
Remember that you do not need to reinstall GRUB after making changes to its configuration file! (You may need
to run update-grub after updating GRUB 2’s /etc-based configuration files, though.) You need to install GRUB this
way only if you make certain changes to your disk configuration, such as resizing or moving the GRUB root
partition, moving your entire installation to a new hard disk, or possibly reinstalling Windows (which tends to wipe
out MBR-based boot loaders). In some of these cases, you may need to boot Linux via a backup boot loader, such
as GRUB installed to floppy disk. (Type grub-install /dev/fd0 to create one and then label it and store it in a safe
place.)

