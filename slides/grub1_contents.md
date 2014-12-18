## Introducción

* Es la versión antigua de [Grub](http://www.gnu.org/software/grub/) (*GRand Unified Bootloader*).
* Actualmente la última revisión es la 0.97
* No sigue un desarrollo activo ya que se apuesta por GRUB 2
* Distribuciones como CentOS o RedHat lo siguen usando como *boot loader*  por defecto

Note:
GRUB Legacy Two major versions of GRUB exist: The original GRUB, now known as GRUB Legacy, has been frozen at version 0.97;
and a new version, GRUB 2, is at version 1.98 as I write. GRUB Legacy is no longer being actively developed. GRUB
2 is under active development, and some features may change by the time you read this. The two versions are
similar in broad details and in how users interact with them at boot time; however, they differ in some important
configuration details. I therefore describe GRUB Legacy and then describe how GRUB 2 differs from GRUB Legacy.



## Configurando GRUB Legacy (I)

* El fichero de configuración se denomina `/boot/grub/menu.lst`
* En algunas distribuciones el fichero se denomina `/boot/grub/grub.conf`
* Grub siempre lee estos ficheros durante el proceso de arranque

Note:
The usual location for GRUB Legacy’s configuration file is /boot/grub/menu.lst. Some distributions, such as Fedora,
Red Hat, and Gentoo, use the filename grub.conf rather than menu.lst. GRUB Legacy can read its configuration file
at boot time, which means you needn’t reinstall the boot loader to the boot sector when you change the
configuration file. The GRUB Legacy configuration file is broken into global and per-image sections, each of which
has its own options. Before getting into section details, though, you should understand a few GRUB quirks.



## Configurando GRUB Legacy (I)

Ejemplo fichero de configuración

```bash
# grub.conf/menu.lst
#
# Global Options:
#
default=0
timeout=15
splashimage=/grub/bootimage.xpm.gz
#
# Kernel Image Options:
#
title Fedora (2.6.32)
root (hd0,0)
kernel /vmlinuz-2.6.32 ro root=/dev/hda5 mem=2048M
initrd /initrd-2.6.32

title Debian (2.6.36-experimental)
root (hd0,0)
kernel (hd0,0)/bzImage-2.6.36-experimental ro root=/dev/hda6
#
# Other operating systems
#
title Windows
rootnoverify (hd0,1)
chainloader +1
```

Note:
It can boot several OSs and kernels—Fedora on /dev/hda5, Debian on /dev/hda6, and Windows on /dev/hda2. Fedora and Debian share a /boot partition (/dev/hda1), on which the GRUB configuration resides.

GRUB Legacy refers to disk drives by numbers preceded by the string hd and enclosed in parentheses, as in (hd0)
for the first hard disk GRUB detects.  GRUB Legacy’s drive mappings can be found in the /boot/grub/device.map file.

GRUB Legacy numbers partitions on a drive starting at 0 instead of 1, which is used by Linux. GRUB
separates partition numbers from drive numbers with a comma, .

GRUB Legacy defines its own root partition, which can be different from the Linux root partition. GRUB’s root
partition is the partition in which its configuration file (menu.lst or grub.conf) resides. This file is normally
in Linux’s /boot/grub/ directory, the GRUB Legacy root partition will be the same as Linux’s root partition.



## Configurando GRUB Legacy (II)
** Principales opciones globales **

<div class="table-responsive">
<table class="table table-hover table-condensed table-bordered">
<thead>
<tr>
<th>Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`default`</td>
<td>Núcleo a arrancar por defecto (la numeración empieza por 0)</td>
</tr>
<tr>
<td>`timeout`</td>
<td>Segundos de espera antes de arrancar el núcleo por defecto</td>
</tr>
<tr>
<td>`splashimage`</td>
<td>Imagen de fondo que se mostrará en el proceso de arranque (<em>path</em> relativo a la partición de arranque o absoluto).</td>
</tr>
</tbody>
</table>
</div>

Note:
Default OS The default= option tells GRUB Legacy which OS to boot. Listing 1.1’s default=0 causes the first listed
OS to be booted (remember, GRUB indexes from 0). If you want to boot the second listed operating system, use
default=1, and so on, through all your OSs.

Timeout The timeout= option defines how long, in seconds, to wait for user input before booting the default
operating system.

Background Graphic The splashimage= line points to a graphics file that’s displayed as the background for the
boot process. This line is optional, but most Linux distributions point to an image to spruce up the boot menu.
The filename reference is relative to the GRUB Legacy root partition, so if /boot is on a separate partition, that
portion of the path is omitted. Alternatively, the path may begin with a GRUB device specification, such as
(hd0,5) to refer to a file on that partition.



## Configurando GRUB Legacy (II)
** Principales opciones locales **

<div class="table-responsive">
<table class="table table-hover table-condensed table-bordered">
<thead>
<tr>
<th>Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`title`</td>
<td>Etiqueta a mostrar para cada núcleo</td>
</tr>
<tr>
<td>`root`</td>
<td>Partición raíz para el grub</td>
</tr>
<tr>
<td>`kernel`</td>
<td>Localización del núcleo y opciones del núcleo</td>
</tr>
<tr>
<td>`initrd`</td>
<td>Localización del fichero con la RAM disk</td>
</tr>
<tr>
<td>`rootnoverify`</td>
<td>Como `root` para núcleos que GRUB no puede cargar</td>
</tr>
<tr>
<td>`chainloader`</td>
<td>Grub que pase el control a otro <em>boot loader</em>. Indicamos el sector donde se encuentra</td>
</tr>
</tbody>
</table>
</div>

Note:
Title The title line begins a per-image stanza and specifies the label to display when the boot loader runs. The
GRUB Legacy title can accept spaces and is conventionally fairly descriptive, as shown in Listing 1.1.

GRUB Legacy Root The root option specifies the location of GRUB Legacy’s root partition. This is the /boot
partition if a separate one exists; otherwise, it’s usually the Linux root (/) partition. GRUB can reside on a FAT
partition, on a floppy disk, or on certain other OSs’ partitions, though, so GRUB’s root could conceivably be
somewhere more exotic.

Kernel Specification The kernel setting describes the location of the Linux kernel as well as any kernel options
that are to be passed to it. Paths are relative to GRUB Legacy’s root partition. As an alternative, you can specify
devices using GRUB’s syntax, such as kernel (hd0,5)/vmlinuz ro root=/dev/hda5. Note that you pass most kernel
options on this line. The ro option tells the kernel to mount its root filesystem read-only (it’s later remounted
read/write), and the root= option specifies the Linux root filesystem. Because these options are being passed to
the kernel, they use Linux-style device identifiers, when necessary, unlike other options in the GRUB Legacy
configuration file.

Initial RAM Disk Use the initrd option to specify an initial RAM disk. Most distributions use initial RAM disks to
store loadable kernel modules and some basic tools used early in the boot process; however, it’s often possible
to omit an initial RAM disk if you compile your own kernel.

Non-Linux Root The rootnoverify option is similar to the root option except that GRUB won’t try to access files
on this partition. It’s used to specify a boot partition for OSs for which GRUB Legacy can’t directly load a kernel,
such as DOS and Windows.

Chain Loading The chainloader option tells GRUB Legacy to pass control to another boot loader. Typically, it’s
passed a +1 option to load the first sector of the root partition (usually specified with rootnoverify) and to hand
over execution to this secondary boot loader.



## Pasos para añadir un núcleo

1. Haz una copia de seguridad del fichero `menu.lst` o `grub.conf`
2. Edita el fichero `menu.lst` o `grub.conf`
2. Copia una de las configuraciones que funciona
3. Modifica la linea `title` indicando el nombre del  núcleo
4. Modifica la linea del `kernel` para que apunte al fichero del núcleo. Si se necesita, indicamos nuevas opciones
5. Si hemos añadido, borrado  o modificado la RAM disk, hacemos los cambios oportunos en la linea `initrd`
6. Hacemos modificaciones en las opciones globales
7. Guardamos el fichero.
