## Introducción

<a class="fancybox" href="img/uefi1.png" data-fancybox-group="gallery" title="Desarrollo de UEFI">
<img height="450px" src="img/uefi1.png" alt="Desarrollo de UEFI">
</a>

Note:
* A mediados de 1990, ingenieros de Intel y HP deciden subsanar alguna de las limitaciones del sistema de arranque basado en el uso de BIOS
* Estas limitaciones son, entre otras: modo de procesador de 16 bit, espacio direccionable de 1 MB, gestión de particiones de no más de 2 GB
* El resultado de esto fue la aparición de *Extensible Firmware Interface* (EFI) en 1998.
* En 2005, se creó el *Unified EFI Forum* como una organización abierta para promocionar la adopción y continuar con el desarrollo de la especificación EFI.
  Al utilizar la especificación EFI 1.10 como el punto de partida, este grupo de la industria creó más especificaciones, renombrándolas EFI Unificada.
* Version 2.1 of the UEFI (Unified Extensible Firmware Interface) specification was released on 7 January 2007.  It added cryptography, network authentication and the User Interface Architecture (Human Interface Infrastructure in UEFI)
* The current UEFI specification, version 2.4, was approved in July 2013.



### Descripción

<a class="fancybox" href="img/uefi2.png" data-fancybox-group="gallery" title="Estructura UEFI">
<img height="450px" src="img/uefi2.png" alt="Estructura UEFI">
</a>

Note:
* Define una interfaz entre el SO y un firmware de plataforma
* Esta interfaz consta de tablas de datos, llamadas de servicios de arranque, llamadas de servicio de tiempo de actividad disponibles para el sistema operativo y su correspondiente sistema de carga.
* Estos proporcionan un entorno estándar para arrancar un sistema operativo y ejecutar aplicaciones de arranque previo.

