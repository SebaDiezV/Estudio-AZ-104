---
lab:
    título: 'Lab 02b: Administrar gobernanza via Azure Policy'
    módulo: 'Administrar la gobernanza y el cumplimiento'
---
# Lab 02b - Administrar gobernanza via Azure Policy

## Introduccion

En este laboratorio, aprenderá a implementar los planes de gobernanza de su organización. Aprenderá cómo las directivas de Azure pueden garantizar que las decisiones operativas se apliquen en toda la organización. Aprenderá a utilizar el etiquetado de recursos para mejorar los informes.

## Escenario

Su organización ha crecido considerablemente en la nube en el último año. Durante una auditoría reciente, descubrió un número sustancial de recursos que no tienen un propietario, un proyecto o un centro de costos definidos. Para mejorar la administración de los recursos de Azure en su organización, decide implementar la siguiente funcionalidad:

- aplicar etiquetas de recursos para adjuntar metadatos importantes a los recursos de Azure

- Exigir el uso de etiquetas de recursos para nuevos recursos mediante Azure Policy

- Actualizar los recursos existentes con etiquetas de recursos

- Usar bloqueos de recursos para proteger los recursos configurados

## Diagrama de arquitectura
  ![az104-lab02b-architecture](https://github.com/user-attachments/assets/96f9c4ea-e2e1-4236-9be2-bdf891b18628)


## Habilidades

+ Tarea 1: Crear y asignar etiquetas a través de Azure Portal.
+ Tarea 2: Aplicar el etiquetado a través de una directiva de Azure.
+ Tarea 3: Aplicar el etiquetado a través de una directiva de Azure.
+ Tarea 4: Configurar y probar bloqueos de recursos.

## Tarea 1: Asignar etiquetas a través de Azure Portal

En esta tarea, creará y asignará una etiqueta a un grupo de recursos de Azure a través de Azure Portal. Las etiquetas son un componente crítico de una estrategia de gobernanza, tal y como se describe en Microsoft Well-Architected Framework y Cloud Adoption Framework. Las etiquetas pueden permitirle identificar rápidamente a los propietarios de los recursos, las fechas de expiración, los contactos de grupo y otros pares de nombre/valor que su organización considere importantes. Para esta tarea, asigne una etiqueta que identifique el rol del recurso ('Infra' para 'Infraestructura').

1. Inicie sesión en **Azure Portal** - 'https://portal.azure.com'.
      
2. Busque y seleccione 'Grupos de recursos'.

3. En los grupos de recursos, seleccione **+ Crear**.

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la suscripción | Tu suscripción |
    | Nombre del grupo de recursos | 'az104-rg2' |
    | Ubicación | **Este de EE. UU.** |



  <img width="388" alt="01" src="https://github.com/user-attachments/assets/13863f51-f8dd-4241-8ba9-4bc0658d5f0a">

4. Seleccione **Siguiente: Etiquetas** y cree una nueva etiqueta.

    | Configuración | Valor |
    | --- | --- |
    | Nombre | 'Centro de costos' |
    | Valor | '000' |

5. Seleccione **Revisar + Crear** y, a continuación, seleccione **Crear**.

  <img width="380" alt="02" src="https://github.com/user-attachments/assets/37cceb52-63ca-40f7-9061-1bf39e01239f">


## Tarea 2: Aplicar el etiquetado a través de una directiva de Azure

En esta tarea, asignará la política integrada *Requerir una etiqueta y su valor en los recursos* al grupo de recursos y evaluará el resultado. Azure Policy se puede usar para aplicar la configuración y, en este caso, la gobernanza, a los recursos de Azure. 

1. En Azure Portal, busque y seleccione "Directiva". 

2. En la hoja **Creación**, seleccione **Definiciones**

   <img width="380" alt="02" src="https://github.com/user-attachments/assets/21edf606-3527-4802-9461-c00f0b5a6814">

3. Haga clic en la entrada que representa la política integrada **Require a tag and its value on resources**. Tómese un minuto para revisar la definición.

   <img width="200" alt="02" src="https://github.com/user-attachments/assets/9b599b10-bf04-49cf-b5c5-5121f5623fc8">
   

   <img width="728" alt="03" src="https://github.com/user-attachments/assets/cff9799d-f3a2-479b-90b6-9d3ceffc96c7">


5. En la hoja de definición de directiva integrada **Require a tag and its value on resources**, haga clic en **Asignar**.
   
   <img width="257" alt="04" src="https://github.com/user-attachments/assets/f37aec82-24e7-49a8-8ec6-97bb24bfb995">

7. Especifique el **Ámbito** haciendo clic en el botón de puntos suspensivos y seleccionando los siguientes valores. Haga clic en **Seleccionar** cuando haya terminado. 

    | Configuración | Valor |
    | --- | --- |
    | Suscripción | *tu suscripción* |
    | Grupo de recursos | **az104-rg2** |

>**Nota**: Puede asignar directivas en el nivel de grupo de administración, suscripción o grupo de recursos. También tiene la opción de especificar exclusiones, como suscripciones individuales, grupos de recursos o recursos. En este escenario, queremos que la etiqueta esté en todos los recursos del grupo de recursos.

  <img width="413" alt="05" src="https://github.com/user-attachments/assets/1b9967e0-6900-4daf-adf3-24f08ca42c16"> -----------------------------         


  <img width="763" alt="06" src="https://github.com/user-attachments/assets/00c0e9f5-41a1-4ca8-a7d7-98863180129f">


  <img width="421" alt="08" src="https://github.com/user-attachments/assets/6d1f1031-08f5-4aad-8c1b-e7593d2bdf35">

    
7. Configure las propiedades **Basics** de la tarea especificando los siguientes ajustes (deje otros con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la tarea | 'Requerir etiqueta de centro de coste con valor predeterminado'|
    | Descripción | 'Requerir etiqueta de centro de coste con valor predeterminado para todos los recursos del grupo de recursos'|
    | Aplicación de políticas | Habilitado |

>**Nota**: El **Nombre de la asignación** se rellena automáticamente con el nombre de la política que seleccionó, pero puede cambiarlo. La **Descripción** es opcional. Tenga en cuenta que puede deshabilitar la política en cualquier momento. 

  <img width="519" alt="07" src="https://github.com/user-attachments/assets/4b2eb008-6a80-46a5-94d0-8640235bde06">
  
8. Haga clic en **Siguiente** y establezca **Parámetros** en los siguientes valores:

   | Configuración | Valor |
   | --- | --- |
   | Nombre | 'Centro de costos' |
   | Valor | '000' |
  <img width="382" alt="09" src="https://github.com/user-attachments/assets/9e11f72b-af25-45f5-8aca-207acb43bed1">

9. Haga clic en **Siguiente** y revise la pestaña **Corrección**. Deje la casilla de verificación **Crear una identidad administrada** sin marcar. 
  <img width="456" alt="10" src="https://github.com/user-attachments/assets/1382e8cb-63d8-49d3-8446-3a7bbcaea47f">


10. Haga clic en **Revisar + Crear** y luego haga clic en **Crear**.

>**Nota**: Ahora comprobará que la nueva asignación de directiva está en vigor intentando crear una cuenta de Azure Storage en el grupo de recursos. Creará la cuenta de almacenamiento sin agregar la etiqueta necesaria. 
    
>**Nota**: La política puede tardar entre 5 y 10 minutos en surtir efecto.

11. En el portal, busque y seleccione "Cuenta de almacenamiento" y seleccione **+ Crear**. 
  <img width="256" alt="11" src="https://github.com/user-attachments/assets/19d37a67-66c9-4576-934d-a4652a5965cd">


12. En la pestaña **Aspectos básicos** de la hoja **Crear cuenta de almacenamiento**, complete la configuración.

    | Configuración | Valor |
    | --- | --- |
    | Grupo de recursos | **az104-rg2** |
    | Nombre de la cuenta de almacenamiento | *cualquier combinación única a nivel mundial de entre 3 y 24 letras minúsculas y dígitos, empezando por una letra* |

13. Seleccione **Revisar** y luego haga clic en **Crear**.
<img width="400" alt="12" src="https://github.com/user-attachments/assets/13ed92c6-e369-4a87-b17c-c5f339146b7e">


14. Debería recibir un mensaje de **Error de validación**. Vea el mensaje para identificar el motivo del error. Compruebe que el mensaje de error indica que la directiva no permitió la implementación de recursos.
  <img width="662" alt="13" src="https://github.com/user-attachments/assets/5a0fc550-3480-415c-aff8-63b7c5ead0af">
  

>**Nota**: Al hacer clic en la pestaña **Error sin procesar**, puede encontrar más detalles sobre el error, incluido el nombre de la definición de rol **Requerir etiqueta de centro de costos con valor predeterminado**. Se produjo un error en la implementación porque la cuenta de almacenamiento que intentó crear no tenía una etiqueta denominada **Centro de costos** con el valor establecido en **Default**.

## Tarea 3: Aplicar etiquetado a través de una directiva de Azure

En esta tarea, usaremos la nueva definición de directiva para corregir los recursos que no cumplan con los requisitos. En este escenario, haremos que los recursos secundarios de un grupo de recursos hereden la etiqueta **Centro de costos** que se definió en el grupo de recursos.

1. En Azure Portal, busque y seleccione "Directiva". 

2. En la sección **Creación**, haga clic en **Asignaciones**. 

3. En la lista de asignaciones, haga clic en el icono de puntos suspensivos de la fila que representa la asignación de política **Requerir etiqueta de centro de coste con valor predeterminado** y utilice el elemento de menú **Eliminar asignación** para eliminar la asignación.

  <img width="742" alt="01" src="https://github.com/user-attachments/assets/6377d7e6-1205-4847-85f8-7268b32a1ec1">

5. Haga clic en **Asignar directiva** y especifique el **Ámbito** haciendo clic en el botón de puntos suspensivos y seleccionando los siguientes valores:
    
    | Configuración | Valor |
    | --- | --- |
    | Suscripción | su suscripción de Azure |
    | Grupo de recursos | 'az104-rg2' |
  <img width="358" alt="02" src="https://github.com/user-attachments/assets/3d59338e-7303-4b27-a370-d6df0ca0894b">
  <img width="766" alt="04" src="https://github.com/user-attachments/assets/99a949ce-90e7-47fa-b666-743a27980af4">

6. Para especificar la **Definición de directiva**, haga clic en el botón de puntos suspensivos y, a continuación, busque y seleccione 'Heredar una etiqueta del grupo de recursos si falta'.
   
<img width="411" alt="05" src="https://github.com/user-attachments/assets/bba82ca2-23ab-45e5-913d-d133aa93cb12"> ----------------------
   
   <img width="228" alt="06" src="https://github.com/user-attachments/assets/057ac067-a0d9-41ff-972b-a9beb0340570"> ----------------------

   
   <img width="304" alt="07" src="https://github.com/user-attachments/assets/afc53ced-ad8f-4d6a-8dc1-2de56d774d08">


8. Seleccione **Agregar** y, a continuación, configure las propiedades **Basics** restantes de la tarea.

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la tarea | 'Heredar la etiqueta de centro de coste y su valor 000 del grupo de recursos si falta' |
    | Descripción | 'Heredar la etiqueta de centro de coste y su valor 000 del grupo de recursos si falta' |
    | Aplicación de políticas | Habilitado |
   <img width="418" alt="08" src="https://github.com/user-attachments/assets/dcf6c552-c764-4537-8a91-525472982be6">

9. Haga clic en **Siguiente** dos veces y establezca **Parámetros** en los siguientes valores:

    | Configuración | Valor |
    | --- | --- |
    | Nombre de la etiqueta | 'Centro de costos' |
  <img width="370" alt="09" src="https://github.com/user-attachments/assets/68a69be1-6721-453d-bcf6-56ad37c05c3c">


11. Haga clic en **Siguiente** y, en la pestaña **Corrección**, configure los siguientes ajustes (deje a los demás con sus valores predeterminados):

    | Configuración | Valor |
    | --- | --- |
    | Creación de una tarea de corrección | Habilitado |
    | Política de corrección | **Heredar una etiqueta del grupo de recursos si falta** |

>**Nota**: Esta definición de política incluye el efecto **Modificar**. Por lo tanto, se requiere una identidad administrada.
  <img width="439" alt="10" src="https://github.com/user-attachments/assets/30d7598d-0bfb-42fa-8584-b35803e18be3">

12. Haga clic en **Revisar + Crear** y luego haga clic en **Crear**.

>**Nota**: Para comprobar que la nueva asignación de directiva está en vigor, creará otra cuenta de almacenamiento de Azure en el mismo grupo de recursos sin agregar explícitamente la etiqueta necesaria. 
    
>**Nota**: La política puede tardar entre 5 y 10 minutos en surtir efecto.

13. Busque y seleccione "Cuenta de almacenamiento" y haga clic en **+ Crear**. 

14. En la pestaña **Aspectos básicos** de la hoja **Crear cuenta de almacenamiento**, compruebe que está usando el grupo de recursos al que se aplicó la directiva y especifique la siguiente configuración (deje otros con sus valores predeterminados) y haga clic en **Revisar**:
    
    | Configuración | Valor |
    | --- | --- |
    | Nombre de la cuenta de almacenamiento | *cualquier combinación única a nivel mundial de entre 3 y 24 letras minúsculas y dígitos, empezando por una letra* |
    
  <img width="404" alt="11" src="https://github.com/user-attachments/assets/318f74b0-a4b9-4d62-a758-fad541eb5c6f">

15. Verifica que esta vez la validación ha pasado y haz clic en **Crear**.

16. Una vez aprovisionada la nueva cuenta de almacenamiento, haga clic en **Ir al recurso**.

17. En la hoja **Etiquetas**, observe que la etiqueta **Centro de costos** con el valor **000** se ha asignado automáticamente al recurso.
  <img width="370" alt="12" src="https://github.com/user-attachments/assets/5a253a30-00c4-4575-8d5c-d341db551569">

## Tarea 4: Configurar y probar bloqueos de recursos

En esta tarea, configurará y probará un bloqueo de recursos. Los bloqueos impiden la eliminación o modificación de un recurso. 

1. Busque y seleccione su grupo de recursos.
  <img width="221" alt="01" src="https://github.com/user-attachments/assets/a0f6eea6-535d-48e1-99d3-5fd03af62b85">

2. En la hoja **Configuración**, seleccione **Bloqueos**.
  <img width="193" alt="02" src="https://github.com/user-attachments/assets/977b0136-1344-4aba-b87a-634022ce4596">


3. Seleccione **Agregar** y complete la información de bloqueo de recursos. Cuando termine, seleccione **Aceptar**. 

    | Configuraciónn | Valor |
    | --- | --- |
    | Nombre de la cerradura | 'Bloqueo rg' |
    | Tipo de cerradura | **eliminar** (observe la selección para solo lectura) |
  <img width="386" alt="03" src="https://github.com/user-attachments/assets/24b0688f-eb18-4049-9578-08ada1f97ed0">

    
4. Vaya a la hoja **Información general** del grupo de recursos y seleccione **Eliminar grupo de recursos**.
   
  <img width="384" alt="04" src="https://github.com/user-attachments/assets/3edcbe22-9a8e-48ef-bc65-c7ab6a1daa6c">

5. En el cuadro de texto **Escriba el nombre del grupo de recursos para confirmar la eliminación**, proporcione el nombre del grupo de recursos, 'az104-rg2'. Tenga en cuenta que puede copiar y pegar el nombre del grupo de recursos.
   
  <img width="284" alt="05" src="https://github.com/user-attachments/assets/00349c18-6937-4cb2-a6d9-e59edb841b5d">

6. Observe la advertencia: La eliminación de este grupo de recursos y sus recursos dependientes es una acción permanente y no se puede deshacer. Seleccione **Eliminar**.
   
  <img width="248" alt="06" src="https://github.com/user-attachments/assets/bd2d09bb-1034-4028-b548-59a734667ebc">

7. Debería recibir una notificación denegando la eliminación.
   
  <img width="183" alt="07" src="https://github.com/user-attachments/assets/b2702891-9cf5-4bbd-b25e-5bc82b37fc47">

>**Nota:** Deberá quitar el bloqueo si tiene la intención de eliminar el grupo de recursos. 
    
## Limpia tus recursos

Si está trabajando con **su propia suscripción**, tómese un minuto para eliminar los recursos del laboratorio. Esto garantizará que se liberen recursos y se minimicen los costos. La manera más sencilla de eliminar los recursos de laboratorio es eliminar el grupo de recursos de laboratorio. 

