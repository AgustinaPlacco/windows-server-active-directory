# Guía de implementación de Active Directory en Windows Server 2022

## Índice
1.	Introducción 
2.	Objetivos 
3.	Tecnologías utilizadas 
4.	Requisitos del entorno 
5.	Preparación del entorno de virtualización 
6.	Configuración del almacenamiento del servidor 
7.	Instalación y configuración de Active Directory 
8.	Organización del Directorio Activo 
9.	Configuración de red e incorporación del cliente al dominio 
10.	Administración remota mediante RSAT 
11.	Configuración de recursos compartidos 
12.	Implementación de Directivas de Grupo (GPO) 
13.	Validación del funcionamiento 
14.	Incidencias y soluciones aplicadas 
15.	Conclusiones 
16.	Referencias


## 1.	Introducción
La empresa ficticia Medicalia es una organización dedicada a la gestión y distribución de medicamentos con presencia en distintas ciudades de España. Debido al crecimiento de su infraestructura tecnológica, surge la necesidad de implantar un sistema centralizado que permita administrar usuarios, equipos, recursos compartidos y políticas de seguridad de forma eficiente.

Para dar respuesta a esta necesidad, se diseñó e implementó un dominio basado en Windows Server 2022 utilizando Active Directory Domain Services (AD DS) como servicio principal de directorio. La infraestructura se desarrolló en un entorno virtualizado con VirtualBox, incorporando un servidor Windows Server 2022 como controlador de dominio y un equipo cliente con Windows 11 unido al dominio.

Durante el proyecto se configuraron unidades organizativas, grupos de seguridad, usuarios, perfiles móviles, recursos compartidos, directivas de grupo (GPO) y herramientas de administración remota. Asimismo, se aplicaron políticas de seguridad orientadas a restringir determinadas acciones de los usuarios y mejorar la gestión de los equipos dentro de un entorno empresarial.

El resultado es una infraestructura funcional que simula el funcionamiento de una red corporativa, permitiendo la administración centralizada de usuarios y recursos conforme a los requisitos planteados en el caso práctico.


## 2. Objetivos
El objetivo principal del proyecto fue implementar un dominio Windows que permitiera centralizar la administración de la infraestructura informática de la empresa Medicalia mediante Active Directory.
Para ello se plantearon los siguientes objetivos específicos:
  - Implementar un controlador de dominio utilizando Windows Server 2022. 
  - Crear la estructura organizativa del dominio mediante Unidades Organizativas (OU). 
  - Administrar usuarios, grupos y equipos desde Active Directory. 
  - Configurar perfiles móviles para los usuarios del departamento de Operaciones. 
  - Compartir recursos de red aplicando permisos según departamentos y usuarios. 
  - Implementar Directivas de Grupo (GPO) para reforzar la seguridad y estandarizar la configuración de los equipos. 
  -	Unir un equipo cliente con Windows 11 al dominio. 
  -	Habilitar la administración remota del Directorio Activo desde el cliente mediante RSAT. 
  -	Verificar el correcto funcionamiento de todos los servicios implementados.


## 3. Tecnologías utilizadas
Durante el desarrollo del proyecto se emplearon las siguientes tecnologías y herramientas:
  -	Windows Server 2022 como sistema operativo del servidor y controlador de dominio. 
  -	Windows 11 Pro como equipo cliente integrado en el dominio. 
  -	Active Directory Domain Services (AD DS) para la gestión centralizada de usuarios, grupos y equipos. 
  -	Group Policy (GPO) para la aplicación de políticas de seguridad y configuración. 
  -	PowerShell para tareas de administración y automatización. 
  -	RSAT (Remote Server Administration Tools) para la administración remota del Directorio Activo. 
  -	NTFS para la configuración de permisos sobre carpetas y archivos. 
  -	SMB (Server Message Block) para compartir recursos de red. 
  -	VirtualBox para la creación y administración del entorno virtualizado.


## 4. Requisitos del entorno

Para la implementación del dominio se preparó un entorno virtualizado utilizando Oracle VirtualBox. La infraestructura estuvo compuesta por un servidor con Windows Server 2022 y un equipo cliente con Windows 11, ambos configurados para simular un entorno empresarial.

Los requisitos utilizados fueron los siguientes:

Servidor

  -	Sistema operativo: Windows Server 2022 Standard Evaluation (Desktop Experience).
  -	4 GB de memoria RAM.
  -	2 procesadores virtuales.
  -	Tres discos duros virtuales:
      -	Disco 0: Sistema operativo.
      -	Disco 1: Recursos compartidos de la empresa y copias de seguridad.
      -	Disco 2: Copias de seguridad del Directorio Activo.
  -	Adaptador de red NAT para acceso a Internet.
  -	Adaptador de red interna para la comunicación con el equipo cliente.

Equipo cliente

  -	Sistema operativo: Windows 11 Pro.
  -	4 GB de memoria RAM.
  -	3 procesadores virtuales.
  -	Un disco duro virtual de 40 GB.
  -	Adaptador de red NAT para acceso a Internet.
  -	Adaptador de red interna para la comunicación con el servidor.
  -	Software utilizado
  -	Oracle VirtualBox.
  -	Imagen ISO de Windows Server 2022.
  -	Imagen ISO de Windows 11 Pro.
  -	VirtualBox Guest Additions.
  -	PowerShell.
  -	Herramientas RSAT.


## 5. Preparación del entorno de virtualización

Antes de iniciar la implementación del dominio, se preparó un entorno de virtualización utilizando Oracle VirtualBox. La infraestructura estuvo compuesta por dos máquinas virtuales: un servidor con Windows Server 2022 y un equipo cliente con Windows 11. Ambos equipos fueron configurados para simular el entorno de red de la empresa Medicalia y permitir la implementación y validación de todos los servicios requeridos.

### 5.1 Creación de la máquina virtual Windows Server 2022

Se creó una máquina virtual denominada **MEDICALIA-SRV**, destinada a desempeñar el papel de controlador de dominio. Durante su configuración se asignaron los recursos de hardware necesarios y se añadieron tres discos duros virtuales, cumpliendo con los requisitos establecidos para el proyecto.

Una vez finalizada la instalación del sistema operativo, se cambió el nombre del servidor, se verificó la conectividad a Internet mediante el adaptador NAT y se realizaron las configuraciones iniciales del sistema.

### 5.2 Configuración inicial del servidor

Como parte de la preparación del servidor, se inicializaron y formatearon los discos adicionales mediante la herramienta **Administración de discos** de Windows. Posteriormente, se organizó el almacenamiento según las necesidades del proyecto, diferenciando el disco destinado al sistema operativo, el almacenamiento de los recursos compartidos y las futuras copias de seguridad del Directorio Activo.

Asimismo, se comprobó el correcto funcionamiento de la red antes de comenzar la instalación de los servicios del dominio.

### 5.3 Creación de la máquina virtual Windows 11

Se creó una segunda máquina virtual denominada **MEDICALIA-W11**, destinada a simular el equipo cliente de la infraestructura.

Tras instalar Windows 11 Pro, se configuró una cuenta de administrador local y se verificó el acceso a Internet para garantizar el correcto funcionamiento del sistema antes de incorporarlo al dominio.

### 5.4 Instalación de VirtualBox Guest Additions

En ambas máquinas virtuales se instalaron las herramientas **VirtualBox Guest Additions**, con el objetivo de mejorar la integración entre el sistema anfitrión y las máquinas virtuales.

Esta instalación permitió optimizar el rendimiento del entorno virtual, además de habilitar funcionalidades como el uso de carpetas compartidas y una mejor interacción entre ambos sistemas.

### 5.5 Configuración de carpetas compartidas

Se configuró una carpeta compartida entre el equipo anfitrión y las máquinas virtuales para facilitar la transferencia de archivos necesarios durante el desarrollo del proyecto, como documentación, imágenes y otros recursos utilizados durante la implementación.

### 5.6 Creación de instantáneas

Antes de comenzar la instalación y configuración de Active Directory se creó una instantánea inicial de ambas máquinas virtuales.

Estas instantáneas permitieron disponer de un punto de restauración estable, facilitando la recuperación del entorno en caso de errores durante la configuración del dominio y reduciendo significativamente el tiempo necesario para repetir el proceso de implementación.


## 6. Configuración del almacenamiento del servidor

Una vez finalizada la instalación del sistema operativo, se configuró el almacenamiento del servidor siguiendo los requisitos establecidos en el proyecto.

La máquina virtual del servidor disponía de tres discos virtuales con funciones diferenciadas para facilitar la organización de la información y garantizar la disponibilidad del servicio.

La distribución del almacenamiento fue la siguiente:

- **Disco 0:** destinado exclusivamente a la instalación del sistema operativo Windows Server 2022.
- **Disco 1:** dividido en dos particiones, una destinada al almacenamiento de los recursos compartidos de la empresa y otra reservada para las copias de seguridad del Directorio Activo.
- **Disco 2:** destinado íntegramente al almacenamiento de copias de seguridad adicionales del dominio.

Esta organización permitió separar el sistema operativo de los datos de la empresa y de las tareas de respaldo, facilitando la administración del servidor y mejorando la recuperación ante posibles incidencias.

Tras la creación de las particiones, los discos fueron inicializados, formateados y etiquetados para identificar fácilmente su finalidad durante la administración del sistema.


## 7. Instalación y configuración de Active Directory

Una vez preparado el entorno y configurado el almacenamiento del servidor, se procedió a la instalación del servicio **Active Directory Domain Services (AD DS)** con el objetivo de convertir el servidor en el controlador de dominio de la infraestructura.

### 7.1 Cambio de nombre del servidor

Antes de instalar los servicios del dominio se modificó el nombre predeterminado del servidor, asignándole el nombre **BITCORE-SRV**, siguiendo la nomenclatura definida para el proyecto.

Posteriormente se reinició el sistema para aplicar los cambios realizados.

### 7.2 Instalación del rol Active Directory Domain Services

Desde el **Administrador del servidor** se instaló el rol **Active Directory Domain Services (AD DS)** junto con las características necesarias para su funcionamiento.

Una vez finalizada la instalación, el servidor quedó preparado para ser promovido como controlador de dominio.

### 7.3 Promoción del servidor a controlador de dominio

El servidor fue promovido como controlador de dominio mediante la creación de un nuevo bosque con el dominio **bitcore.local**.

Durante este proceso se configuró la contraseña correspondiente al modo de restauración de servicios de directorio (DSRM), necesaria para futuras tareas de recuperación y mantenimiento del dominio.

Tras completarse el asistente de configuración, el servidor se reinició automáticamente, quedando configurado como controlador de dominio principal.

### 7.4 Verificación de la instalación

Finalizada la promoción del servidor, se comprobó el correcto funcionamiento del dominio verificando la instalación de Active Directory y el acceso a las herramientas de administración.

Asimismo, se confirmó la creación del dominio **bitcore.local**, asegurando que el servidor estaba preparado para la incorporación de usuarios, grupos, equipos y demás objetos del Directorio Activo.


## 8. Organización del Directorio Activo

Una vez instalado y configurado el controlador de dominio, se diseñó la estructura del Directorio Activo siguiendo el organigrama proporcionado por la empresa Medicalia. El objetivo fue organizar los recursos de forma lógica para facilitar la administración del dominio, la asignación de permisos y la aplicación de directivas de grupo.

### 8.1 Creación de Unidades Organizativas (OU)

Se crearon diferentes Unidades Organizativas (OU) para representar la estructura organizativa de la empresa. Cada departamento dispone de su propia OU, permitiendo administrar de forma independiente los usuarios, grupos y políticas asociadas.

Las Unidades Organizativas creadas fueron:

  - Almacén
  - Financiero
  - Operaciones
  - Recursos Humanos (RRHH)
  - Nutrición
  - Presidencia
  - Empleados

Esta organización facilita la aplicación de directivas específicas a cada departamento y mejora la administración del dominio.

### 8.2 Creación de grupos de seguridad

Dentro de cada Unidad Organizativa se creó un grupo de seguridad destinado a gestionar los permisos de acceso a los recursos compartidos de cada departamento.

Además, se creó un grupo general para todos los empleados del dominio, utilizado para asignar permisos comunes y aplicar determinadas configuraciones de forma centralizada.

El uso de grupos de seguridad simplifica la administración de permisos, evitando asignarlos individualmente a cada usuario.

### 8.3 Creación de usuarios mediante plantillas

Los usuarios del dominio se crearon utilizando plantillas de usuario específicas para cada departamento.

Cada plantilla contenía la configuración común correspondiente, como la pertenencia a grupos de seguridad y otros parámetros de configuración. Posteriormente, estas plantillas se duplicaron para generar los distintos usuarios del dominio, modificando únicamente los datos personales de cada empleado.

Este procedimiento permitió agilizar la creación de usuarios y mantener una configuración homogénea en toda la organización.

### 8.4 Configuración de perfiles móviles

Los usuarios del departamento de Operaciones fueron configurados para utilizar perfiles móviles.

Los perfiles se almacenaron en un recurso compartido del servidor, permitiendo que la configuración personal del usuario estuviera disponible independientemente del equipo desde el que iniciara sesión dentro del dominio.

Esta configuración garantiza una mayor flexibilidad para los usuarios y facilita la administración de los perfiles desde el servidor.

### 8.5 Organización de equipos

El equipo cliente con Windows 11 fue incorporado al dominio y administrado desde Active Directory como un objeto del dominio.

Su incorporación permitió aplicar las políticas de seguridad definidas, gestionar el acceso de los usuarios y validar el correcto funcionamiento de la infraestructura implementada.

### 8.6 Beneficios de la estructura implementada

La organización del Directorio Activo permitió disponer de una infraestructura ordenada, escalable y fácil de administrar.

La separación de usuarios por departamentos, el uso de grupos de seguridad y la estructura basada en Unidades Organizativas facilitaron la aplicación de permisos y Directivas de Grupo, reduciendo la complejidad de la administración y mejorando la seguridad del entorno.


## 9. Configuración de red e incorporación del cliente al dominio

Para permitir la comunicación entre el servidor y el equipo cliente, se configuró una infraestructura de red basada en dos adaptadores virtuales en ambas máquinas.

El primer adaptador se configuró en modo **NAT**, proporcionando acceso a Internet para la instalación de actualizaciones, descarga de herramientas y comprobación de la conectividad.

El segundo adaptador se configuró como **Red Interna**, creando una red privada entre el servidor Windows Server 2022 y el equipo cliente Windows 11. Esta configuración permitió el intercambio de información entre ambos equipos sin depender de la red del equipo anfitrión.

### 9.1 Configuración de direccionamiento IP

Una vez creada la red interna, se asignaron direcciones IP estáticas a ambos equipos para garantizar una comunicación estable dentro de la infraestructura.

El servidor fue configurado como servidor DNS principal, permitiendo al cliente localizar el controlador de dominio y autenticarse correctamente en el dominio.

Tras completar la configuración, se verificó la conectividad mediante pruebas de comunicación entre ambas máquinas, confirmando el correcto funcionamiento de la red.

### 9.2 Incorporación del cliente al dominio

Con la infraestructura de red operativa, el equipo cliente **MEDICALIA-W11** fue incorporado al dominio **bitcore.local**.

Durante el proceso de unión se utilizaron credenciales con privilegios de administración del dominio. Una vez completada la incorporación, el equipo se reinició para aplicar la nueva configuración.

A partir de ese momento, los usuarios del dominio pudieron iniciar sesión desde el equipo cliente utilizando sus credenciales de Active Directory.

### 9.3 Verificación del funcionamiento

Tras la incorporación del cliente al dominio se realizaron diferentes comprobaciones para validar el correcto funcionamiento de la infraestructura.

Entre ellas se verificó el inicio de sesión con usuarios del dominio, la correcta resolución de nombres mediante el servidor DNS, la comunicación entre servidor y cliente y la aplicación de las configuraciones definidas en Active Directory.

Estas comprobaciones confirmaron que el equipo cliente formaba parte del dominio y que la comunicación con el controlador de dominio era completamente funcional.


## 10. Administración remota mediante RSAT

Con el objetivo de administrar el dominio sin necesidad de acceder directamente al servidor, se habilitó la administración remota desde el equipo cliente con Windows 11 mediante las herramientas **Remote Server Administration Tools (RSAT)**.

### 10.1 Instalación de RSAT

Las herramientas RSAT se instalaron en el equipo cliente utilizando las características opcionales de Windows 11. Entre ellas se incorporaron las herramientas necesarias para administrar **Active Directory Domain Services (AD DS)**.

Una vez completada la instalación, el equipo cliente dispuso de las mismas utilidades de administración presentes en el controlador de dominio.

### 10.2 Habilitación de la administración remota

Para permitir la comunicación entre el cliente y el servidor se habilitó **Windows Remote Management (WinRM)**, configurando los servicios necesarios para la administración remota.

Asimismo, se verificó que las reglas correspondientes del Firewall de Windows estuvieran habilitadas, garantizando el acceso remoto a los servicios de administración del dominio.

### 10.3 Verificación del funcionamiento

Finalmente, se comprobó el correcto funcionamiento de la administración remota accediendo desde el equipo Windows 11 a las herramientas de Active Directory.

Esta configuración permitió administrar usuarios, grupos, equipos y demás objetos del dominio desde el cliente, sin necesidad de iniciar sesión directamente en el servidor.



## 11. Configuración de recursos compartidos

Una de las necesidades planteadas por Medicalia consistía en centralizar el almacenamiento de la información de la empresa mediante recursos compartidos accesibles desde el dominio.

Para ello se configuraron diferentes carpetas compartidas en el servidor, aplicando permisos específicos según el tipo de usuario y el departamento al que pertenecía.

### 11.1 Carpetas personales de los usuarios

Se creó una carpeta destinada al almacenamiento de los directorios personales de cada usuario del dominio.

Cada empleado dispone de una carpeta exclusiva sobre la que tiene permisos de lectura, escritura y modificación, impidiendo el acceso al resto de usuarios del dominio.

Los permisos NTFS fueron configurados para garantizar la privacidad de la información, manteniendo únicamente el acceso completo para el propietario de la carpeta, el sistema y los administradores del dominio.

### 11.2 Carpetas departamentales

Además de las carpetas personales, se creó una estructura de carpetas compartidas para cada departamento de la organización.

Los permisos fueron asignados mediante grupos de seguridad de Active Directory, permitiendo que únicamente los miembros del departamento correspondiente pudieran acceder y modificar su información.

Este enfoque simplifica la administración de permisos y facilita futuras incorporaciones de usuarios al dominio.

### 11.3 Permisos especiales

Siguiendo los requisitos del proyecto, la cuenta correspondiente a la Presidencia recibió permisos de lectura y ejecución sobre las carpetas departamentales, manteniendo restringido el acceso a los directorios personales de los empleados.

De este modo se respetó la política de confidencialidad establecida para la organización.

### 11.4 Automatización de las carpetas personales

Para agilizar la creación de los directorios personales se utilizó un script de PowerShell que generó automáticamente una carpeta para cada usuario existente en el dominio.

Durante este proceso también se aplicaron automáticamente los permisos NTFS correspondientes, garantizando que cada usuario únicamente pudiera acceder a su propia información.

Esta automatización redujo el tiempo de administración y evitó errores derivados de la configuración manual de los permisos.


## 12. Implementación de Directivas de Grupo (GPO)

Una vez configurados los usuarios, grupos y recursos compartidos, se implementaron diferentes **Directivas de Grupo (Group Policy Objects - GPO)** con el objetivo de reforzar la seguridad del dominio, estandarizar la configuración de los equipos y automatizar determinadas tareas de administración.

Las políticas fueron aplicadas tanto a nivel general para todos los empleados como de forma específica para cada departamento, utilizando las Unidades Organizativas (OU) creadas previamente.

### 12.1 Directivas generales de seguridad

Se creó una GPO común para los empleados del dominio con el fin de restringir determinadas acciones que pudieran comprometer la seguridad del sistema.

Entre las configuraciones implementadas se incluyen:

  - Restricción del acceso al Panel de control.
  - Bloqueo del acceso al Editor del Registro de Windows.
  - Restricción de la ejecución de PowerShell.
  - Limitación de la ejecución de determinadas aplicaciones del sistema.
  - Restricción para modificar la hora del sistema.
  - Configuración de políticas relacionadas con el almacenamiento de documentos.

Estas políticas no fueron aplicadas a los administradores del dominio, permitiendo mantener las tareas de administración sin restricciones.

### 12.2 Políticas de contraseñas y bloqueo de cuentas

A nivel del dominio se configuró la política de contraseñas para mejorar la seguridad de las cuentas de usuario.

Las principales configuraciones fueron:

  - Caducidad de la contraseña cada 120 días.
  - Historial de las 12 últimas contraseñas utilizadas.
  - Bloqueo de la cuenta tras tres intentos fallidos de autenticación.
  - Duración del bloqueo de cinco minutos antes de permitir un nuevo intento de acceso.

Estas políticas ayudan a reducir el riesgo de accesos no autorizados y ataques por fuerza bruta.

### 12.3 Directivas específicas por departamento

Además de las políticas generales, se crearon GPO específicas para cada departamento de la organización.

Cada una de ellas permitió personalizar el entorno de trabajo de los usuarios mediante:

  - Configuración de un fondo de escritorio corporativo asociado al departamento.
  - Asignación automática de las unidades de red correspondientes.
  - Aplicación de configuraciones específicas según las necesidades de cada área.

Las imágenes utilizadas para los fondos de escritorio fueron almacenadas en el recurso compartido **SYSVOL**, permitiendo su distribución automática a todos los equipos del dominio.

### 12.4 Administración centralizada mediante GPO

La utilización de Directivas de Grupo permitió administrar de forma centralizada la configuración de todos los equipos del dominio, evitando realizar modificaciones manuales en cada estación de trabajo.

Este enfoque facilita el mantenimiento de la infraestructura, mejora la seguridad y garantiza una configuración homogénea para todos los usuarios de la organización.


## 13. Validación del funcionamiento

Una vez finalizada la implementación del dominio, se realizaron diferentes pruebas con el objetivo de comprobar el correcto funcionamiento de todos los servicios configurados.

Las verificaciones permitieron confirmar que la infraestructura cumplía los requisitos establecidos en el proyecto.

Entre las principales comprobaciones realizadas se encuentran:

  - Inicio de sesión de los usuarios del dominio desde el equipo Windows 11.
  - Correcta incorporación del equipo cliente al dominio.
  - Funcionamiento de los perfiles móviles para los usuarios del departamento de Operaciones.
  - Acceso a las carpetas personales respetando los permisos configurados.
  - Acceso a las carpetas departamentales únicamente por los miembros autorizados.
  - Aplicación automática de las Directivas de Grupo al iniciar sesión.
  - Funcionamiento de las unidades de red asignadas mediante GPO.
  - Administración remota del Directorio Activo desde el equipo cliente utilizando RSAT.
  - Comunicación correcta entre el servidor y el equipo cliente dentro de la red interna.

Los resultados obtenidos demostraron que la infraestructura funcionaba correctamente y que todos los componentes implementados operaban de forma integrada, proporcionando una administración centralizada de usuarios, equipos y recursos conforme a los requisitos planteados para la empresa Medicalia.


## 14. Incidencias y soluciones aplicadas

Durante el desarrollo del proyecto surgieron diversas incidencias técnicas propias de la implantación de un entorno basado en Active Directory. Todas ellas fueron analizadas y resueltas hasta conseguir una infraestructura completamente funcional.

Las principales incidencias detectadas fueron las siguientes:

  - **Configuración de la red interna:** inicialmente el servidor y el equipo cliente no podían comunicarse correctamente. El problema se resolvió configurando un segundo adaptador de red en modo Red Interna, asignando direcciones IP estáticas y estableciendo el servidor como DNS principal.

  - **Conectividad mediante ICMP (ping):** el equipo cliente no respondía a las pruebas de conectividad debido a las reglas del Firewall de Windows. Se habilitó el tráfico ICMP para verificar correctamente la comunicación entre ambos equipos.

  - **Asignación de permisos sobre recursos compartidos:** durante la configuración de las carpetas personales y departamentales fue necesario revisar tanto los permisos NTFS como los permisos de uso compartido para garantizar que únicamente los usuarios autorizados pudieran acceder a la información correspondiente.

  - **Aplicación de Directivas de Grupo:** algunas configuraciones no se aplicaban de forma inmediata en los equipos cliente. La situación se resolvió actualizando manualmente las políticas mediante `gpupdate /force` y reiniciando la sesión de los usuarios.

La resolución de estas incidencias permitió completar satisfactoriamente la implantación del dominio y asegurar el correcto funcionamiento de todos los servicios configurados.


## 15. Conclusiones

La implantación del dominio basado en Active Directory permitió centralizar la administración de usuarios, equipos y recursos de la empresa Medicalia mediante una infraestructura organizada y segura.

Durante el desarrollo del proyecto se configuraron Unidades Organizativas, grupos de seguridad, usuarios, perfiles móviles, recursos compartidos, políticas de seguridad y herramientas de administración remota, reproduciendo el funcionamiento de un entorno empresarial real.

Asimismo, el uso de Directivas de Grupo facilitó la aplicación centralizada de configuraciones y restricciones sobre los equipos del dominio, mejorando la seguridad y simplificando las tareas de administración.

La realización de este proyecto permitió adquirir conocimientos prácticos sobre la implantación y administración de infraestructuras Windows Server, así como comprender la importancia de Active Directory como elemento fundamental en la gestión de redes corporativas.
