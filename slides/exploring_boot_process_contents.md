## El proceso de arranque (I)

<a class="fancybox" href="img/exploring_boot_process.png" data-fancybox-group="gallery" title="Fases de arranque">
<img height="550px" src="img/exploring_boot_process.redimensionado640x480.png" alt="Fases de arranque">
</a>

Note:
Although the process normally proceeds smoothly once you press a computer’s power button, booting a computer
involves a large number of steps, ranging from hardware initialization to the launch of potentially dozens of
programs.



## El proceso de arranque (II)

<a class="fancybox" href="img/bios_boot.png" data-fancybox-group="gallery" title="Operaciones iniciales de la bios">
<img height="550px" src="img/bios_boot.redimensionado640x480.png" alt="Operaciones iniciales de la bios">
</a>

Note:
1. The CPU initializes itself.
2. The CPU examines a particular memory address for code to run. This address corresponds to part of the
computer’s firmware, which contains instructions on how to proceed.
3. The firmware initializes the computer’s major hardware subsystems, performs basic memory checks, and so
on.



## El proceso de arranque (III)

<a class="fancybox" href="img/bios_boot_sequence.png" data-fancybox-group="gallery" title="Selección de la secuencia de arranque">
<img height="550px" src="img/bios_boot_sequence.png" alt="Selección de la secuencia de arranque">
</a>

Note:
4. The firmware directs the computer to look for boot code on a storage device, such as a hard disk, a removable disk, or an optical disc. This code, which is known as a stage 1 boot loader, is loaded and run.



## El proceso de arranque (IV)

<a class="fancybox" href="img/mbr.png" data-fancybox-group="gallery" title="Detalle del MBR">
<img height="550px" src="img/mbr.redimensionado640x480.png" alt="Detalle del MBR">
</a>

Note:
This boot loader code is located in the MBR



## El proceso de arranque (V)

<a class="fancybox" href="img/grub_stage1.png" data-fancybox-group="gallery" title="Stage 1 del grub">
<img height="550px" src="img/grub_stage1.redimensionado640x480.png" alt="Stage 1 del grub">
</a>



## El proceso de arranque (VI)

<a class="fancybox" href="img/grub_stage2.png" data-fancybox-group="gallery" title="Menú de selección del grub">
<img height="550px" src="img/grub_stage2.redimensionado640x480.png" alt="Menú de selección del grub">
</a>

Note:
The stage 1 boot loader code may direct the system to load additional stages of itself. Ultimately, the boot
loader code loads the operating system’s kernel and runs it. From this point on, the kernel is in control of the
computer.



## El proceso de arranque (VII)

<a class="fancybox" href="img/kernel_loading.png" data-fancybox-group="gallery" title="Carga y arranque del núcleo">
<img height="550px" src="img/kernel_loading.redimensionado640x480.png" alt="Carga y arranque del núcleo">
</a>

Note:
El boot loader carga el núcleo y este se ejecuta y realiza diversas operaciones de inicialización



## El proceso de arranque (VIII)

<a class="fancybox" href="img/init.png" data-fancybox-group="gallery" title="Proceso init">
<img height="550px" src="img/init.png" alt="Proceso init">
</a>

Note:
The kernel looks for its first process file. In Linux, this file is usually /sbin/init, and its process, once running, is
known as init. The init process reads one or more configuration files, which tell it what other programs it should launch.

Linux systems typically launch processes both directly from init and under the direction of System V (SysV)
startup scripts or the Upstart system.

During the startup process, the computer mounts disks using the mount utility under direction of the /etc/fstab
file. It may also perform disk checks using the fsck utility.
As part of the init-directed startup procedures, Linux systems normally launch user login tools,
such as the text-mode login or an X Display Manager (XDM) login screen. These programs enable ordinary users to
log in and use the system. Server computers are normally configured to launch server programs, some of which
provide similar login possibilities for remote users. Once users log in, they can of course launch additional
programs.

