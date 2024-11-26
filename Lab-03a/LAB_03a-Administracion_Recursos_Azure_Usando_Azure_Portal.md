---
lab:
  title: "Laboratorio\_03a: Administración de recursos de Azure con Azure Portal"
  module: Administer Azure Resources
---

# Laboratorio 03a: Administración de recursos de Azure con Azure Portal


## Escenario del laboratorio

Tiene que explorar las funcionalidades básicas de administración de Azure asociadas con el aprovisionamiento de recursos y su organización en función de los grupos de recursos, incluido el movimiento de recursos entre grupos de recursos. También quiere explorar las opciones para proteger los recursos de disco frente a la eliminación accidental, a la vez que se permita modificar las características de rendimiento y tamaño.

## Objetivos

En este laboratorio, aprenderemos a:

+ Tarea 1: Crear grupos de recursos e implementar recursos en ellos
+ Tarea 2: Mover recursos entre grupos de recursos
+ Tarea 3: Implementar y probar los bloqueos de recursos

## Tiempo estimado: 20 minutos

## Diagrama de Arquitectura
![lab03a](https://github.com/user-attachments/assets/c2bd32f5-23a2-4b6a-8c9c-ee8270ed5980)

### Instrucciones

## Ejercicio 1

## Tarea 1: Crear grupos de recursos e implementar recursos en ellos

En esta tarea, usará Azure Portal para crear grupos de recursos y crear un disco en el grupo de recursos.

1. Inicie sesión en [**Azure Portal**](http://portal.azure.com).

1. En Azure Portal, busque y seleccione **Discos**, haga clic en **+ Crear** y especifique los siguientes valores:

    |Configuración|Value|
    |---|---|
    |Suscripción| Nombre de la suscripción de Azure donde creó el grupo de recursos |
    |Grupo de recursos| Nombre de un nuevo grupo de recursos **az104-03a-rg1** |
    |Nombre del disco| **az104-03a-disk1** |
    |Region| **(EE. UU.) Este de EE. UU.** |
    |Zona de disponibilidad| **No se requiere redundancia de la infraestructura** |
    |Tipo de origen| **None** |
   
<img width="530" alt="02" src="https://github.com/user-attachments/assets/77a0ea5e-81e4-46ff-93ef-84c47a137929">

    >**Nota**: Al crear un recurso, tiene la opción de crear un grupo de recursos o usar uno existente.

1. Cambie el tipo y tamaño de disco a **HDD estándar** y **32 GiB**, respectivamente.

1. Haga clic en **Revisar + crear** y, después, en **Crear**.
<img width="319" alt="03" src="https://github.com/user-attachments/assets/14b6540f-d3cf-4f4c-ac47-de6a5ca4886f">
<img width="354" alt="04" src="https://github.com/user-attachments/assets/9e46f488-cf24-4137-8e1b-f8872c798506">

    >**Nota**: Espere hasta que se cree el disco. Debería tardar menos de un minuto.

## Tarea 2: Mover recursos entre grupos de recursos 

En esta tarea, moveremos el recurso de disco que creó en la tarea anterior a un nuevo grupo de recursos. 

1. Busque y seleccione **Grupos de recursos**. 
<img width="273" alt="05" src="https://github.com/user-attachments/assets/48e5521f-0a75-4abf-a69a-2258abef9d0c">

1. En la hoja **Grupos de  recursos**, haga clic en la entrada que representa el grupo de recursos **az104-03a-rg1** que creó en la tarea anterior.
<img width="895" alt="06" src="https://github.com/user-attachments/assets/4c883362-c78c-4289-bc61-0892b4988d4b">

1. En la hoja **Información general** del grupo de recursos, en la lista de recursos del grupo de recursos, seleccione la entrada que representa el disco recién creado, haga clic en **Mover** en la barra de herramientas y, en la lista desplegable, seleccione **Mover a otro grupo de recursos**.
<img width="922" alt="07" src="https://github.com/user-attachments/assets/b301517d-f26b-4913-84b8-c9f43f19dbe7">

    >**Nota**: Este método permite mover varios recursos al mismo tiempo.

1. Debajo del cuadro de texto **Grupo de recursos**, haga clic en **Crear nuevo** y luego **az104-03a-rg2** en el cuadro de texto. En la pestaña Revisión, seleccione la casilla **Comprendo que las herramientas y los scripts asociados con recursos movidos no funcionarán hasta que los actualice para que usen nuevos identificadores de recursos** y haga clic en **Mover**.
<img width="568" alt="09" src="https://github.com/user-attachments/assets/a0977c03-ad89-4a49-b848-32aecee9aff3">
<img width="913" alt="10" src="https://github.com/user-attachments/assets/d8794f55-71d0-4d36-b519-c82ef94f4206">
<img width="633" alt="11" src="https://github.com/user-attachments/assets/c89fd7b3-db51-486f-9bc6-b772a8b0fddc">

    >**Nota**: No espere a que se complete el movimiento, sino que avance a la siguiente tarea. Esto puede tardar unos 10 minutos. Para saber si la operación se completó, supervise las entradas del registro de actividad del grupo de recursos de origen o de destino. Vuelva a visitar este paso una vez que complete la siguiente tarea.

## Tarea 3: Implementar bloqueos de recursos

En esta tarea, aplicará un bloqueo de recursos a un grupo de recursos de Azure que contenga un recurso de disco.

1. En Azure Portal, busque y seleccione **Discos**, haga clic en **+ Crear** y especifique los siguientes valores:

    |Configuración|Value|
    |---|---|
    |Suscripción| Nombre de la suscripción que está usando en este laboratorio |
    |Grupo de recursos| Haga clic en **Crear nuevo** grupo de recursos y asígnele el nombre **az104-03a-rg3** |
    |Nombre del disco| **az104-03a-disk2** |
    |Region| Nombre de la región de Azure donde creó los otros grupos de recursos en este laboratorio |
    |Zona de disponibilidad| **No se requiere redundancia de la infraestructura** |
    |Tipo de origen| **None** |


1. Establezca el tipo y tamaño de disco a **HDD estándar** y **32 GiB**, respectivamente.

<img width="529" alt="12" src="https://github.com/user-attachments/assets/9c10ad08-35a0-4ecd-b8cb-da893114195a">

1. Haga clic en **Revisar + crear** y, después, en **Crear**.

1. Haga clic en **Ir al recurso**.

1. En la página Información general del disco, haga clic en el nombre del grupo de recursos, **az104-03a-rg3**.
<img width="196" alt="13" src="https://github.com/user-attachments/assets/dd75cadc-fc10-4497-882a-299aba1f42eb">

1. En la hoja del grupo de recursos **az104-03a-rg3**, haga clic en **Bloqueos** y luego en **+ Agregar** y configure las opciones siguientes:

    |Configuración|Valor|
    |---|---|
    |Nombre del bloqueo| **az104-03a-delete-lock** |
    |Tipo de bloqueo| **Eliminar** |
   
<img width="170" alt="14" src="https://github.com/user-attachments/assets/044bb14f-db59-41a2-83d9-ba8068f46610">
<img width="258" alt="15" src="https://github.com/user-attachments/assets/2d3a6d3e-d78e-437f-b808-17ecf28b170e">

1. Haga clic en **Aceptar**    

1. En la hoja del grupo de recursos **az104-03a-rg3**, haga clic en **Información general**, en la lista de recursos del grupo de recursos, seleccione la entrada que representa el disco que creó anteriormente en esta tarea y haga clic en **Eliminar** en la barra de herramientas. 
<img width="940" alt="16" src="https://github.com/user-attachments/assets/14e0294d-06ad-4647-9fbd-8c38bd0bd0de">

1. Cuando se le pregunte **¿Quiere eliminar todos los recursos seleccionados?** , en el cuadro de texto **Confirmar eliminación**, escriba **sí** y haga clic en **Eliminar**.

1. Debería ver un mensaje de error que notifica el error de la operación de eliminación. 
<img width="213" alt="18" src="https://github.com/user-attachments/assets/ad01f6b7-3064-4888-8295-9ef2fff6233b">

    >**Nota**: Como indica el mensaje de error, esto es esperable debido al bloqueo de eliminación aplicado a nivel del grupo de recursos.

1. Vuelva a la lista de recursos del grupo de recursos **az104-03a-rg3** y haga clic en la entrada que representa el recurso **az104-03a-disk2**. 

1. En la hoja **az104-03a-disk2**, en la sección **Configuración**, haga clic en **Tamaño y rendimiento**, establezca el tipo y tamaño de disco en **SSD prémium** y **64 GiB**,respectivamente, y haga clic en **Guardar** para aplicar el cambio. Compruebe que el cambio se realizó correctamente.

<img width="415" alt="20" src="https://github.com/user-attachments/assets/1f20e761-2e9d-4c85-8701-4a794eda76af">

    >**Nota**: Esto es lo esperado, ya que el bloqueo a nivel de grupo de recursos solo se aplica a las operaciones de eliminación. 

## Limpieza de recursos

   >**Nota**: No elimine los recursos que implementó en este laboratorio. Los va a usar en el siguiente laboratorio de este módulo. Quite solo el bloqueo de recursos que creó en este laboratorio.

1. Vaya a la hoja del grupo de recursos **az104-03a-rg3**, muestre su hoja **Bloqueos** y quite el bloqueo **az104-03a-delete-lock** haciendo clic en el vínculo **Eliminar** en el lado derecho de la entrada de bloqueo **Eliminar**.
