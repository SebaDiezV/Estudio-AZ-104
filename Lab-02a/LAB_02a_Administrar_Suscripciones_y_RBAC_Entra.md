---
lab:
    titulo: 'Lab 02a: Administrar Suscripciones y RBAC'
    modulo: 'Administrar gobernanza y cumplimiento'
---

# Lab 02a - Administrar Suscripciones y RBAC

## Introducción

En este laboratorio, aprenderá sobre el control de acceso basado en roles. Aprenderá a usar permisos y ámbitos para controlar qué acciones pueden y no pueden realizar las identidades. También aprenderá a facilitar la administración de suscripciones mediante grupos de administración. 

## Escenario

Para simplificar la administración de los recursos de Azure en su organización, se le ha asignado la tarea de implementar la siguiente funcionalidad:

- Crear un grupo de administración que incluya todas las suscripciones de Azure.

- Conceder permisos para enviar solicitudes de soporte técnico para todas las suscripciones del grupo de administración. Los permisos deben limitarse solo a: 

- Crear y administrar máquinas virtuales
    - Crear tickets de solicitud de soporte técnico (no incluye la adición de proveedores de Azure) 

## Diagrama de Arquitectura

![az104-lab02a-architecture](https://github.com/user-attachments/assets/381eb5a7-a419-4936-93d2-60c240725648)

## Tarea 1: Implementar un Grupo de Administración

En esta tarea, creará y configurará grupos de administración. Los grupos de administración se usan para organizar y segmentar lógicamente las suscripciones. Permiten que RBAC y Azure Policy se asignen y hereden a otros grupos de administración y suscripciones. Por ejemplo, si su organización tiene un equipo de soporte técnico dedicado a Europa, puede organizar las suscripciones europeas en un grupo de administración para proporcionar al personal de soporte técnico acceso a esas suscripciones (sin proporcionar acceso individual a todas las suscripciones). En nuestro escenario, todos los miembros del departamento de soporte técnico deberán crear una solicitud de soporte técnico en todas las suscripciones. 
1. Inicie sesión en el **Portal de Azure** - 'https://portal.azure.com'.

1. Busque y seleccione 'Microsoft Entra ID'.

1. En el menú desplegable **Administrar**, seleccione **Propiedades**.

1. Revise el área **Administración de acceso para recursos de Azure**. Asegúrese de que puede administrar el acceso a todas las suscripciones y grupos de administración de Azure en el inquilino.
   <img width="480" alt="01" src="https://github.com/user-attachments/assets/abc288c7-b03f-46c7-80c6-0786de2e5a22">
   
1. Busque y seleccione 'Grupos de administración'.
  <img width="307" alt="02" src="https://github.com/user-attachments/assets/b290fabf-43b5-4367-8a1f-c6147add9e4e">
  
6. En la hoja **Grupos de administración**, haga clic en **+ Crear**.
  <img width="422" alt="03" src="https://github.com/user-attachments/assets/27d4f7d9-8ccc-4f34-9305-db3a1de2abe1">
  
7. Cree un grupo de administración con la siguiente configuración. Seleccione **Enviar** cuando haya terminado.

    | Configuración | Valor |
    | --- | --- |
    | Management group ID | `az104-mg1` (Debe ser único en el directorio) |
    | Management group display name | `az104-mg1` |

![04](https://github.com/user-attachments/assets/2d2847f1-3da3-4f10-aab2-4f0876ce8880)

8. **Actualice** la página del grupo de administración para asegurarse de que se muestre el nuevo grupo de administración. Esto puede tardar un minuto.
  <img width="295" alt="05" src="https://github.com/user-attachments/assets/7a4e0788-ef9c-4008-a8d0-529ca3b00316">

  ## Tarea 2: Revisar y asignar un rol integrado de Azure

En esta tarea, revisará los roles integrados y asignará el rol de colaborador de VM a un miembro del departamento de soporte técnico. Azure proporciona un gran número de [roles integrados](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles). 

1. Seleccione el grupo de administración **az104-mg1**.

1. Seleccione la hoja **Control de acceso (IAM)** y, a continuación, la pestaña **Roles**.
  <img width="450" alt="01" src="https://github.com/user-attachments/assets/e2dc3c39-2ffd-4b79-86e5-0e5f1a65ac9e">

3. Desplácese por las definiciones de roles integradas que están disponibles. **Vea** un rol para obtener información detallada sobre los **Permisos**, **JSON** y **Asignaciones**. A menudo usará *propietario*, *colaborador* y *lector*. 

4. Seleccione **+ Agregar**, en el menú desplegable, seleccione **Agregar asignación de roles**. 
  <img width="354" alt="02" src="https://github.com/user-attachments/assets/141fb44e-50d9-428f-874f-67d6e97903d3">

5. En la hoja **Agregar asignación de roles**, busque y seleccione el **Colaborador de la máquina virtual**. El rol de colaborador de máquina virtual le permite administrar máquinas virtuales, pero no acceder a su sistema operativo ni administrar la red virtual y la cuenta de almacenamiento a las que están conectadas. Este es un buen papel para el servicio de asistencia. Seleccione **Siguiente**.
![03](https://github.com/user-attachments/assets/2a6bd76c-5533-4032-b32e-4bd5bcd9f155)
6.  En la pestaña **Miembros**, **Seleccionar miembros**.
 <img width="315" alt="04" src="https://github.com/user-attachments/assets/63b777e4-0dd1-4bea-9b43-ea126b1fa763">
   
    >**Nota:** El siguiente paso asigna el rol al grupo **helpdesk**. Si no tiene un grupo helpdesk, tómese un minuto para crearlo.

7. Busque y seleccione el grupo 'Helpdesk'. Haga clic en **Seleccionar**. 
 <img width="291" alt="05" src="https://github.com/user-attachments/assets/d2b60a91-2521-4b44-b0f5-b2c83a7b8273">

8. Haga clic en **Revisar + asignar** dos veces para crear la asignación de roles.
 <img width="195" alt="06" src="https://github.com/user-attachments/assets/9d8afaf4-0c36-47f0-8b58-a48ff11fad34">

9. Continúe en la hoja **Control de acceso (IAM)**. En la pestaña **Asignaciones de roles**, confirme que el grupo **helpdesk** tiene el rol **Virtual Machine Contributor**.
    
  <img width="376" alt="07" src="https://github.com/user-attachments/assets/e7f4fea5-8054-4cfb-9d80-5690d056462e">

## Tarea 3: Crear un rol RBAC personalizado

En esta tarea, creará un rol RBAC personalizado. Los roles personalizados son una parte fundamental de la implementación del principio de privilegios mínimos para un entorno. Es posible que los roles integrados tengan demasiados permisos para su escenario. También crearemos un nuevo rol y eliminaremos los permisos que no sean necesarios. ¿Tiene un plan para administrar los permisos superpuestos?

1. Continúe trabajando en su grupo de administración. Navegue hasta la hoja **Control de acceso (IAM)**.

2. Seleccione **+ Agregar**, en el menú desplegable, seleccione **Agregar rol personalizado**.
  <img width="275" alt="01" src="https://github.com/user-attachments/assets/067a3e62-0d04-447b-938d-cce84548e274">

3. En la pestaña Conceptos básicos, complete la configuración.

    | Configuración | Valor |
    | --- | --- |
    | Nombre de rol personalizado | 'Custom Support Request' |
    | Descripción | 'Un rol de colaborador personalizado para las solicitudes de soporte.' |
  
4. Para **Permisos de línea base**, seleccione **Clonar un rol**. En el menú desplegable **Rol para clonar**, seleccione **Colaborador de solicitud de soporte**.
  <img width="471" alt="02" src="https://github.com/user-attachments/assets/28dfae60-5f62-4810-b1d6-b9ee11de22e5">


5. Seleccione **Siguiente** para ir a la pestaña **Permisos** y, a continuación, seleccione **+ Excluir permisos**.
  <img width="543" alt="03" src="https://github.com/user-attachments/assets/ba0070b7-36c5-497a-8d33-13dde1755eeb">

6. En el campo de búsqueda del proveedor de recursos, escriba '. Support' y seleccione **Microsoft.Support**.
  <img width="207" alt="04" src="https://github.com/user-attachments/assets/6e2f21ec-c456-4077-9a88-4995d43f89d2">

7. En la lista de permisos, coloque una casilla de verificación junto a **Otro: Registers Support Resource Provider** y, a continuación, seleccione **Agregar**. El rol debe actualizarse para incluir este permiso como *NotAction*.
  <img width="460" alt="05" src="https://github.com/user-attachments/assets/26d74b31-2da4-4974-8845-3ec41df2d4e0">

>**Nota:** Un proveedor de recursos de Azure es un conjunto de operaciones de REST que habilitan la funcionalidad para un servicio de Azure específico. No queremos que el servicio de asistencia pueda tener esta capacidad, por lo que se elimina del rol clonado. 

8. En la pestaña **Ámbitos asignables**, asegúrese de que su grupo de administración aparezca en la lista y, a continuación, haga clic en **Siguiente**.
  <img width="531" alt="06" src="https://github.com/user-attachments/assets/2010308c-f35d-4a9b-ba1c-782d1ab7d09f">

9. Revise el JSON de las *Actions*, *NotActions* y *AssignableScopes* que se personalizan en el rol.
  <img width="462" alt="07" src="https://github.com/user-attachments/assets/989ec225-54d1-47fb-9bbc-010f0c426fff">


11. Seleccione **Revisar + Crear** y, a continuación, seleccione **Crear**.

# Tarea 4: Supervisar las asignaciones de roles con el registro de actividad

En esta tarea, verá el registro de actividad para determinar si alguien ha creado un nuevo rol. 

1. En el portal, busque el recurso **az104-mg1** y seleccione **Registro de actividad**. El registro de actividad proporciona información sobre los eventos de nivel de suscripción. 
  <img width="646" alt="01" src="https://github.com/user-attachments/assets/650adc89-ab3e-43d0-bfbf-e4c7ba9832c3">

2. Revise las actividades para las asignaciones de roles. El registro de actividad se puede filtrar para operaciones específicas. 
  <img width="372" alt="03" src="https://github.com/user-attachments/assets/932d3655-f2fb-4f62-a204-25e0cc7ce3d7">
