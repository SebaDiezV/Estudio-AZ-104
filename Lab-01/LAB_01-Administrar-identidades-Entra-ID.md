---
lab:
    titulo: 'Lab 01: Administrar Identidades de Microsoft Entra ID'
    module: 'Administrar Identidad'
---

# Lab 01 - Administrar Identidades de Microsoft Entra ID

## Introducción

En este laboratorio, aprenderás sobre los usuarios y los grupos. Los usuarios y los grupos son los componentes básicos de una solución de identidad.

## Tiempo estimado: 30 minutos

## Escenario

Su organización está creando un nuevo entorno de laboratorio para las pruebas de preproducción de aplicaciones y servicios.  Se está contratando a algunos ingenieros para administrar el entorno de laboratorio, incluidas las máquinas virtuales. Para permitir que los ingenieros se autentiquen mediante Microsoft Entra ID, se le ha asignado la tarea de aprovisionar usuarios y grupos. Para minimizar la sobrecarga administrativa, la pertenencia a los grupos debe actualizarse automáticamente en función de los puestos de trabajo. 


## Diagrama de Arquitectura
![az104-lab01-architecture](https://github.com/user-attachments/assets/e318f7e5-8ab9-4a8a-8431-c405d074bcac)

## Habilidades

+ Tarea 1: Crear y configurar cuentas de usuarios.
+ Tarea 2: Crear grupos y agregar miembros.

## Tarea 1: Crear y configurar cuentas de usuarios

En esta tarea, creará y configurará cuentas de usuario. Las cuentas de usuario almacenarán datos de usuario como nombre, departamento, ubicación e información de contacto.

1. Ingresar al **Portal de Azure** - `https://portal.azure.com`.
2. Busca y selecciona `Microsoft Entra ID`. Microsoft Entra ID es la solución de identidad y acceso basada en nube de Azure.
   <img width="548" alt="01" src="https://github.com/user-attachments/assets/5ff62800-bc39-4c9b-a22c-54f90806ef9d">

## Crear un nuevo usuario

1. Selecciona **Usuarios**, luego, en el menú desplegable **Nuevo Usuario** selecciona **Crear un usuario nuevo**.
   <img width="259" alt="02" src="https://github.com/user-attachments/assets/4022d37a-2db0-4f79-ac08-88cbe4729f2d">
   
2. Crea un nuevo usuario con la siguiente configuración (deja el resto por defecto).

    | Configuración | Valor |
    | --- | --- |
    | User principal name | `az104-user1` |
    | Display name | `az104-user1` |
    | Auto-generate password | **checked** |
    | Account enabled | **checked** |
    | Job title (Properties tab) | `IT Lab Administrator` |
    | Department (Properties tab) | `IT` |
    | Usage location (Properties tab) | **Chile** |
<img width="483" alt="03-1" src="https://github.com/user-attachments/assets/98fe4a34-1a0c-4cbf-88d0-3448d3138bb0">
<img width="422" alt="03-2" src="https://github.com/user-attachments/assets/796d9b27-a316-417e-af41-225ba3a8d6d4">

4. Una vez finalizado, selecciona **Revisar + Crear** y luego **Crear**
   
<img width="360" alt="03-3" src="https://github.com/user-attachments/assets/76b35a5f-25f0-460c-856d-c078efd4971d">

5. Refresca la página y confirma que el nuevo usuario fue creado

### Invita a un usuario externo

1. En el menú desplegable **Nuevo Usuario** selecciona **Invitar usuario externo**.

<img width="236" alt="05" src="https://github.com/user-attachments/assets/c177230a-f686-4c8f-ae4b-17b8855214fb">


    | Configuración | Valor |
    | --- | --- |
    | Email | `Tu cuenta de correo` |
    | Display name | `Tu Nombre` |
    | Send invite message | **check the box** |
    | Message | `Bienvenido a Azure y a nuestro grupo de proyecto`|
   
<img width="396" alt="06" src="https://github.com/user-attachments/assets/47710446-6ca0-47e7-b128-74b437a1d6b4">

2. Ingresa a la pestaña **Propiedades**. Completa la información básica, incluyendo estos campos. 

    | Configuración | Valor|
    | --- | --- |
    | Job title  | `IT Lab Administrator` |
    | Department  | `IT` |
    | Usage location (Properties tab) | **Chile** |
   
   
<img width="389" alt="07" src="https://github.com/user-attachments/assets/36746205-4cdb-4a4c-9043-ce21f9fd5788">

4. Una vez finalizado, selecciona **Revisar + Crear** y luego **Crear**
<img width="178" alt="08" src="https://github.com/user-attachments/assets/55d5b988-1a37-4940-a808-30bb96d2275c">

5. Refresca la página y confirma que el usuario invitado fue creado.
<img width="190" alt="09" src="https://github.com/user-attachments/assets/64da1840-4014-4285-a0c6-964a98371610">

## Tarea 2: Crear un grupo y añadir miembros
En esta tarea, se crea una cuenta de grupo. Las cuentas de grupo pueden incluir cuentas de usuario o dispositivos. Estas son dos formas básicas en que los miembros se asignan a los grupos: estáticamente y dinámicamente. Los grupos estáticos requieren que los administradores agreguen y eliminen miembros manualmente.  Los grupos dinámicos se actualizan automáticamente en función de las propiedades de una cuenta de usuario o dispositivo. Por ejemplo, el puesto de trabajo. 

1. En el portal de Azure, busca y selecciona `Microsoft Entra ID`. En **Administrar**, selecciona **Grupos**
<img width="175" alt="01" src="https://github.com/user-attachments/assets/7982961b-5611-4f55-95db-7480d1220a97">

2.  En el menú **Agregar**, selecciona **Grupo** y crea un nuevo grupo.
   
<img width="136" alt="02" src="https://github.com/user-attachments/assets/4c74b64a-718b-46aa-9a7f-a98977e2ff0f">

    | Configuración | Valor |
    | --- | --- |
    | Group type | **Seguridad** |
    | Group name | `IT Lab Administrators` |
    | Group description | `Administradores que administran el laboratorio de IT` |
    | Membership type | **Asignada** |

<img width="467" alt="03" src="https://github.com/user-attachments/assets/2ad4f708-9cef-4aad-a4e2-5803d1a01bce">


1. Selecciona **No se ha seleccionado ningún propietario**.

1. En la página **Añadir propietarios**, busca y **selecciona** a ti mismo (se muestra en la esquina superior derecha) como el propietario.Se puede seleccionar más de un propietario.
   
<img width="416" alt="04" src="https://github.com/user-attachments/assets/c87dc4dc-e65d-4c1a-8381-883eb7432abd">

1. Selecciona **No hay miembros seleccionados**.
   
<img width="169" alt="05" src="https://github.com/user-attachments/assets/e83a78a8-e110-42aa-80bf-aed202a382d6">

1. En el panel de **Añadir miembros**, busca y **seleccionar** la cuenta **az104-user1** y a **Usuario invitado** que invitaste. Añade a ambos como usuarios del grupo.

<img width="670" alt="06" src="https://github.com/user-attachments/assets/daf3bb80-2a91-44b4-9346-4b904804d6be">

1. Selecciona **Crear** para desplegar el grupo.
   
<img width="467" alt="07" src="https://github.com/user-attachments/assets/c5affe36-3c69-420b-be85-81e49824dd18">

1. **Refresca** la pagina para asegurar que el grupo fue creado.

<img width="378" alt="08" src="https://github.com/user-attachments/assets/453f9e67-daa6-4fcb-ad80-ab06bd146a6a">




  
