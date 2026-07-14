# Enterprise Active Directory Infrastructure

## Descripción

Este proyecto consiste en la implantación de una infraestructura empresarial basada en **Windows Server 2022** para una organización ficticia. Se implementó un dominio con **Active Directory**, organizando usuarios, equipos y departamentos mediante Unidades Organizativas (OU), grupos de seguridad y Directivas de Grupo (GPO).

Además, se configuraron recursos compartidos con permisos específicos, perfiles móviles, administración remota desde Windows 11 y diferentes políticas de seguridad para simular un entorno empresarial real.


## Objetivos del proyecto

- Implantar un dominio empresarial mediante Active Directory en Windows Server 2022.
- Organizar usuarios, equipos y departamentos mediante Unidades Organizativas (OU).
- Configurar grupos de seguridad para la administración de permisos.
- Implementar Directivas de Grupo (GPO) para reforzar la seguridad y estandarizar la configuración de los equipos.
- Configurar recursos compartidos y permisos de acceso según el departamento.
- Implementar perfiles móviles para los usuarios del departamento de Operaciones.
- Habilitar la administración remota del dominio desde un equipo cliente Windows 11.


## Tecnologías utilizadas

- Windows Server 2022
- Windows 11
- Active Directory Domain Services (AD DS)
- Group Policy (GPO)
- PowerShell
- VirtualBox
- NTFS
- Recursos compartidos SMB
- Administración remota (RSAT)


## Funcionalidades implementadas

- Instalación y configuración de Active Directory.
- Promoción del servidor a controlador de dominio.
- Creación de Unidades Organizativas (OU).
- Administración de usuarios, grupos y equipos.
- Configuración de grupos de seguridad.
- Unión de equipos Windows 11 al dominio.
- Configuración de perfiles móviles.
- Creación de carpetas personales y departamentales con permisos específicos.
- Implementación de políticas de seguridad mediante GPO.
- Administración remota del dominio utilizando RSAT desde Windows 11.


## Arquitectura de la infraestructura

La infraestructura se implementó en un entorno virtualizado mediante VirtualBox y está compuesta por un servidor Windows Server 2022 que actúa como controlador de dominio y un equipo cliente con Windows 11 unido al dominio.

El servidor centraliza la autenticación de usuarios mediante Active Directory, administra las Directivas de Grupo (GPO), los recursos compartidos y los permisos de acceso. El equipo cliente se utiliza para validar la autenticación de los usuarios, comprobar la aplicación de las políticas del dominio y realizar tareas de administración remota mediante RSAT.


## Organización del Directorio Activo

Se diseñó una estructura jerárquica de Active Directory basada en Unidades Organizativas (OU) para organizar a los usuarios según su departamento.

Dentro de cada OU se crearon grupos de seguridad y usuarios utilizando plantillas para facilitar la administración. La infraestructura incluyó departamentos como Almacén, Finanzas, Operaciones, Recursos Humanos, Nutrición y Presidencia, además de una OU general para empleados.

Esta organización permitió aplicar permisos y Directivas de Grupo (GPO) de forma centralizada y diferenciada según las necesidades de cada departamento.


## Directivas de Grupo (GPO)

Se implementaron diferentes Directivas de Grupo para reforzar la seguridad y estandarizar la configuración de los equipos del dominio.

Entre las principales políticas configuradas se incluyen:

-- Restricción de acceso al Panel de control.
- Bloqueo del Editor del Registro.
- Restricción del uso de PowerShell y determinadas aplicaciones.
- Configuración de la política de contraseñas del dominio.
- Bloqueo temporal de cuentas tras varios intentos fallidos de autenticación.
- Configuración de fondos de escritorio personalizados por departamento.
- Asignación automática de unidades de red.
- Configuración de políticas de acceso para los usuarios del dominio.


## Recursos compartidos

Se configuraron recursos compartidos para facilitar el acceso a la información según el departamento de cada usuario.

Se implementaron carpetas personales con permisos exclusivos para cada empleado y carpetas departamentales con permisos basados en grupos de seguridad. Además, se asignaron automáticamente las unidades de red correspondientes mediante Directivas de Grupo (GPO).


## Administración remota

Se habilitó la administración remota del dominio desde un equipo cliente con Windows 11 mediante las herramientas RSAT (Remote Server Administration Tools), permitiendo gestionar usuarios, grupos y otros elementos de Active Directory sin necesidad de acceder directamente al servidor.


## Demostración en vídeo

Durante el desarrollo del proyecto se realizó un vídeo demostrativo donde se muestran las principales configuraciones realizadas, el funcionamiento del dominio, la autenticación de usuarios, la aplicación de las GPO y la gestión de los recursos compartidos.

https://youtu.be/AEZz-USselc


## Competencias adquiridas
- Administración de Windows Server 2022.
- Implementación de Active Directory Domain Services (AD DS).
- Gestión de usuarios, grupos y Unidades Organizativas (OU).
- Configuración de Directivas de Grupo (GPO).
- Administración de permisos NTFS y recursos compartidos.
- Configuración de perfiles móviles.
- Administración remota mediante RSAT.
- Resolución de incidencias en entornos Windows Server.
- Virtualización con VirtualBox.
- Documentación técnica de infraestructuras informáticas.


## Autor
Estudiante de Desarrollo de Aplicaciones Web (DAW) en la Universitat Oberta de Catalunya (UOC).
