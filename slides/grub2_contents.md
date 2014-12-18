## Introducción

* *Boot loader*  por defecto de Ubuntu desde la versión 9.10
* Totalmente reescrito respecto a GRUB 1
* Incrementa flexibilidad y rendimiento

Note:
GRUB 2 is the default boot loader and manager for Ubuntu since version 9.10 (Karmic Koala). GRUB 2 is a descendant of GRUB (GRand Unified Bootloader). It has been completely rewritten
to provide the user significantly increased flexibility and performance. GRUB 2 is Free Software.



## Configurando GRUB 2 (I)

* Es muy parecida a la de GRUB 1
* Las principales diferencias son:
  * El fichero de configuración es `/boot/grub/grub.cfg`
  * Añade nuevas opciones como carga de módulos
  * Permite añadir cierta lógica al menú de arranque.

Note:
In principle, configuring GRUB 2 is much like configuring GRUB Legacy; however, some important details differ.
First, the GRUB 2 configuration file is /boot/grub/grub.cfg. GRUB 2 adds a number of features, such as support for
loadable modules for specific filesystems and modes of operation, that aren’t present in GRUB Legacy. (The insmod
command in the GRUB 2 configuration file loads modules.) GRUB 2 also supports conditional logic statements,
enabling loading modules or displaying menu entries only if particular conditions are met.



## Configurando GRUB 2 (II)

Ejemplo fichero de configuración

``` bash
#
# Kernel Image Options:
#
menuentry "Fedora (2.6.32)" {
set root=(hd0,1)
linux /vmlinuz-2.6.32 ro root=/dev/hda5 mem=2048M
initrd /initrd-2.6.32
}
menuentry "Debian (2.6.36-experimental)" {
set root=(hd0,1)
linux (hd0,1)/bzImage-2.6.36-experimental ro root=/dev/hda6
}
#
# Other operating systems
#
menuentry "Windows" {
set root=(hd0,2)
chainloader +1
}
```

Note:
If you merely want to add or change a single OS entry, you’ll find the most important changes are to the per-
image options. Listing 1.2 shows GRUB 2 equivalents to the image options shown in Listing 1.1.



## Configurando GRUB 2 (III)
### Diferencias respecto a  GRUB 1

* La entrada `menuentry` sustituye a `title`
* Los nombres de los núcleos se enrecomillan
* Las propiedades de cada entrada van en {}
* La palabra `set` precede a `root`
* No existe `rootnoverify`, ahora es `root`
* <div class="alert alert-danger"> **Las particiones empiezan en 1** </div>

Note:
Important changes compared to GRUB Legacy include the following:
* The title keyword is replaced by menuentry.
* The menu title is enclosed in quotation marks.
* An opening curly brace ({) follows the menu title, and each entry ends with a closing curly brace (}).
The set keyword precedes the root keyword, and an equal sign (=) separates root from the partition
specification.
* The rootnoverify keyword has been eliminated; you use root instead.
*Partitions are numbered starting from 1 rather than from 0. A similar change in disk numbering is not
implemented. This change can be very confusing. The most recent versions of GRUB 2 also support a
more complex partition identification scheme to specify the partition table type or partitions that are
embedded.



## Configurando GRUB 2 (IV)
### Diferencias respecto a  GRUB 1 (II)

* Proporciona una serie de script para mantener el fichero `/boot/grub/grub.cfg`
* **No debemos editarlo manualmente, deberemos editar  `/etc/grub.d` y `/etc/default/grub`**
* Los ficheros en `/etc/grub.d`  controlan distintas acciones del GRUB, como la búsqueda automática de nuevos núcleos.
```bash
ijfviana@linda:/etc/grub.d$ ls
00_header        20_linux_xen   30_os-prober      41_custom
05_debian_theme  20_memtest86+  30_uefi-firmware  README
10_linux         25_custom      40_custom
ijfviana@linda:/etc/grub.d$
```
Note:
GRUB 2 makes further changes, in that it employs a set of scripts and other tools that help automatically
maintain the /boot/grub/grub.cfg file. The intent is that system administrators need never explicitly edit this file.
Instead, you would edit files in /etc/grub.d, and the /etc/default/grub file, to change your GRUB 2 configuration.
Files in /etc/grub.d control particular GRUB OS probers. These scripts scan the system for particular OSs and
kernels and add GRUB entries to /boot/grub/grub.cfg to support those OSs.



## Configurando GRUB 2 (V)
### Diferencias respecto a  GRUB 1 (III)

* Podemos añadir nuestro kernel editando `40_custom`

``` bash
ijfviana@linda:/etc/grub.d$ vi 40_custom
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
#
# Kernel Image Options:
#
menuentry "Fedora (2.6.32)" {
set root=(hd0,1)
linux /vmlinuz-2.6.32 ro root=/dev/hda5 mem=2048M
initrd /initrd-2.6.32
}
menuentry "Debian (2.6.36-experimental)" {
set root=(hd0,1)
linux (hd0,1)/bzImage-2.6.36-experimental ro root=/dev/hda6
}
#
# Other operating systems
#
menuentry "Windows" {
set root=(hd0,2)
chainloader +1
}
```

Note:
You can add custom kernel entries, such as those shown in Listing 1.2, to the 40_custom file to support your own locally compiled kernels or unusual OSs
that GRUB doesn’t automatically detect.
The /etc/default/grub file controls the defaults created by the GRUB 2 configuration scripts. For instance, if you
want to adjust the timeout, you might change the following line:
GRUB_TIMEOUT=10
A distribution that’s designed to use GRUB 2, such as recent versions of Ubuntu, will automatically run the
configuration scripts after certain actions, such as installing a new kernel with the distribution’s package manager.
If you need to make changes yourself, you can type update-grub after you’ve edited /etc/default/grub or files in
/etc/grub.d. This command re-reads these configuration files and writes a fresh /boot/grub/grub.cfg file.



## Configurando GRUB 2 (VI)
### Diferencias respecto a  GRUB 1 (IV)

* Opciones globales definidas en `/etc/default/grub`

``` bash
ijfviana@linda:/etc/default$ vi grub
# If you change this file, run 'update-grub' afterwards to update
# /boot/grub/grub.cfg.
# For full documentation of the options in this file, see:
#   info -f grub -n 'Simple configuration'

GRUB_DEFAULT=0
#GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""

# Uncomment to enable BadRAM filtering, modify to suit your needs
# This works with Linux (no patch required) and with any kernel that obtains
# the memory map information from GRUB (GNU Mach, kernel of FreeBSD ...)
#GRUB_BADRAM="0x01234567,0xfefefefe,0x89abcdef,0xefefefef"

# Uncomment to disable graphical terminal (grub-pc only)
#GRUB_TERMINAL=console

# The resolution used on graphical terminal
# note that you can use only modes which your graphic card supports via VBE
# you can see them in real GRUB with the command `vbeinfo'
#GRUB_GFXMODE=640x480

# Uncomment if you don't want GRUB to pass "root=UUID=xxx" parameter to Linux
#GRUB_DISABLE_LINUX_UUID=true

# Uncomment to disable generation of recovery mode menu entries
#GRUB_DISABLE_RECOVERY="true"

# Uncomment to get a beep at grub start
#GRUB_INIT_TUNE="480 440 1"

```

Note:
The /etc/default/grub file controls the defaults created by the GRUB 2 configuration scripts. For instance, if you
want to adjust the timeout, you might change the following line:
GRUB_TIMEOUT=10



## Pasos para añadir un núcleo

* Si el núcleo está situado en un sitio "estándar"
  * Ejecutamos ```update-grub```
* Si el núcleo no está situado en un sitio "estándar" y el GRUB no lo detecta
 * Hacemos una copia de ```/etc/grub.d/40_custom```
 * Editamos el fichero /etc/grub.d/40_custom y añadimos el nuevo núcleo
 * Ejecutamos ```update-grub```

Note:
The /etc/default/grub file controls the defaults created by the GRUB 2 configuration scripts. For instance, if you
want to adjust the timeout, you might change the following line:
GRUB_TIMEOUT=10
A distribution that’s designed to use GRUB 2, such as recent versions of Ubuntu, will automatically run the
configuration scripts after certain actions, such as installing a new kernel with the distribution’s package manager.
If you need to make changes yourself, you can type update-grub after you’ve edited /etc/default/grub or files in
/etc/grub.d. This command re-reads these configuration files and writes a fresh /boot/grub/grub.cfg file.

