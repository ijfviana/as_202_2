## Introducción (I)

<a class="fancybox" href="img/grub_menu.png" data-fancybox-group="gallery" title="Primera pantalla grub">
<img height="550px" src="img/grub_menu.png" alt="Primera pantalla grub">
</a>

Note:
The first screen the GRUB Legacy or GRUB 2 boot loader shows you is a list of all the operating systems you
specified with the title or menuentry option in your GRUB configuration file. You can wait for the timeout to expire
for the default operating system to boot. To select an alternative, use your arrow keys to highlight the operating
system that you want to boot. Once your choice is highlighted, press the Enter key to start booting.



## Introducción (II)

* Si no sale el menu...
 * Pulsamos ESC en GRUB 1
 * Pulsamos MAYUS en GRUB 2



## Interacción con el menu (I)

1. Con las flechas seleccionamos el núcleo que nos interesa
2. Pulsamos la tecla E. Entraremos en un menú similar a

<a class="fancybox" href="img/grub_edit_menu.png" data-fancybox-group="gallery" title="Primera pantalla grub">
<img height="450px" src="img/grub_edit_menu.png" alt="Primera pantalla grub">
</a>



### Interacción con el menu (II)

Para GRUB 2 es parecido a

<a class="fancybox" href="img/grub2_edit_menu.png" data-fancybox-group="gallery" title="Primera pantalla grub">
<img height="450px" src="img/grub2_edit_menu.png" alt="Primera pantalla grub">
</a>



## Interacción con el menu (III)

4. En GRUB 1, tenemos que pulsar la ```E``` para acceder a cada opción. En GRUB 2 no es necesario.
5. Editamos lo opción
6. Presionamos ```Enter``` para finalizar la edición de la opción. No es necesario en GRUB 2.
7. Presionamos ```B``` para arrancar el núcleo. En GRUB 2 presionamos ```Ctrl+X```.



## Interacción avanzada

Pulsando ```C``` accedemos al modo *shell* de GRUB

<a class="fancybox" href="img/grub_shell.png" data-fancybox-group="gallery" title="Grub shell">
<img height="550px" src="img/grub_shell.redimensionado640x480.png" alt="GRub shell">
</a>

Note:
More advanced boot-time interactions are possible by entering GRUB’s interactive mode. You do this by
pressing the C key at the GRUB menu. You can then type a variety of commands that are similar to Linux shell
commands, such as ls to view files. If you type ls alone at the grub> prompt, you’ll see a list of partitions, using
GRUB’s partition nomenclature. You can add a partition identifier and a slash, as in ls (hd0,3)/, to view the
contents of that partition. By working your way through the partitions, you can probably identify your kernel file
and, if your system uses it, an initial RAM disk file. This information, along with the identification of your root
filesystem, should enable you to build up a working GRUB entry for your system.
