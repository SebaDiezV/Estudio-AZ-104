---
lab:
  title: "Laboratorio\_03: Administración de recursos de Azure mediante plantillas de Azure Resource Manager"
  module: Administer Azure Resources
---

# Laboratorio 03: Administración de recursos de Azure mediante plantillas de Azure Resource Manager

## Introducción al laboratorio

En este laboratorio, aprenderá a automatizar las implementaciones de recursos. Obtenga información sobre las plantillas de Azure Resource Manager y las plantillas de Bicep. Obtendrá información sobre las distintas formas de implementar las plantillas. 

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Puede cambiar la región, pero para escribir los pasos se ha usado **Este de EE. UU.** 

## Tiempo estimado: 50 minutos

## Escenario del laboratorio

Su equipo quiere ver formas de automatizar y simplificar las implementaciones de recursos. Su organización busca formas de reducir la sobrecarga administrativa, reducir el error humano y aumentar la coherencia.  

## Diagrama de arquitectura
![az104-lab03-architecture](https://github.com/user-attachments/assets/05d8c6c9-9186-419c-b02e-78ebf109faaa)

## Aptitudes de trabajo

+ Tarea 1: Cree una plantilla de Azure Resource Manager.
+ Tarea 2: Edite una plantilla de Azure Resource Manager y vuelva a implementar la plantilla.
+ Tarea 3: Configuración de Cloud Shell e implementación de una plantilla con Azure PowerShell.
+ Tarea 4: Implementación de una plantilla con la CLI. 
+ Tarea 5: Implementación de un recurso mediante Azure Bicep.

## Tarea 1: crear una Azure Resource Manager plantilla

En esta tarea, crearemos un disco administrado en Azure Portal. Los discos administrados están diseñados para usarse con máquinas virtuales. Una vez implementado el disco, exportará una plantilla que puede usar en otras implementaciones.

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

2. Busque y seleccione `Disks`.

3. En la página Discos, seleccione **Crear**.

4. En la página **Crear un disco administrado**, configure el disco y seleccione **Aceptar**. 
    
    | Configuración | Valor |
    | --- | --- |
    | Suscripción | *su suscripción* | 
    | Grupo de recursos | `az104-rg3` (Si es necesario, seleccione **Crear nuevo**).
    | Nombre del disco | `az104-disk1` | 
    | Region | **Este de EE. UU.** |
    | Zona de disponibilidad | **No se requiere redundancia de la infraestructura** | 
    | Tipo de origen | **Ninguno** |
    | Rendimiento | **HDD estándar** (cambiar tamaño) |
    | Size | **32 GiB** | 

<img width="525" alt="01" src="https://github.com/user-attachments/assets/fccb8b10-d68e-427b-aa45-db4f79aaea72">

  >**Nota:** Estamos creando un disco administrado sencillo para que pueda practicar con plantillas. Los discos administrados de Azure son volúmenes de almacenamiento de nivel de bloque que Azure administra.

5. Haga clic en **Revisar y crear** y seleccione **Crear**.

6. Supervise las notificaciones (superior derecha) y, después de la implementación, seleccione **Ir al recurso**. 

7. En la hoja **Automatización**, seleccione **Exportar plantilla**. 
<img width="881" alt="02" src="https://github.com/user-attachments/assets/d03cd817-e446-4fa0-809d-53a68d4c2b30">

8. Tómese un minuto para revisar los archivos **Plantilla** y **Parámetros**.

9. Haga clic en **Descargar** y guarde las plantillas en la unidad local. Esto crea un archivo comprimido. 
<img width="182" alt="04" src="https://github.com/user-attachments/assets/b3ff30ea-cdc7-4e81-9d17-cb7d0b545fbc">

10. Utilice el Explorador de archivos para extraer el contenido del archivo descargado en la carpeta **Descargas** de su ordenador. Observe que hay dos archivos JSON (plantilla y parámetros). 
<img width="182" alt="04" src="https://github.com/user-attachments/assets/de5e2ede-9345-4837-a57b-417c60df66c3">

## Tarea 2: Edite una plantilla de Azure Resource Manager y, a continuación, vuelva a implementar la plantilla.

En esta tarea, usará la plantilla descargada para implementar un nuevo disco administrado. En esta tarea se describe cómo repetir las implementaciones de forma rápida y sencilla. 

1. En Azure Portal, busque y seleccione `Deploy a custom template`.
<img width="321" alt="05" src="https://github.com/user-attachments/assets/53c2f13c-bf6f-45e2-bfce-b1fd96dd7c4c">

2. En la hoja **Implementación personalizada**, observe que existe la posibilidad de utilizar una **plantilla de inicio rápido**. Hay muchas plantillas integradas, como se muestra en el menú desplegable. 

3. En lugar de usar un inicio rápido, seleccione **Cree su propia plantilla en el editor**.
<img width="527" alt="06" src="https://github.com/user-attachments/assets/06f5c348-9c41-47ba-9bc4-2098fae37999">

4. En la hoja **Editar plantilla**, haga clic en **Cargar archivo** y cargue el archivo **template.json** que descargó en la tarea anterior.
<img width="537" alt="07" src="https://github.com/user-attachments/assets/49112f12-a0c6-491d-aa21-b052ac698efc">
<img width="257" alt="08" src="https://github.com/user-attachments/assets/386c7da3-7017-4aa3-8ba6-eb2cd24777d1">

5. En el panel del editor, realice estos cambios.

    + Cambie **disks_az104_disk1_name** a `disk_name` (dos lugares para cambiar)
    + Cambie **az104-disk1** a `az104-disk2` (un lugar para cambiar)
      
<img width="263" alt="09a" src="https://github.com/user-attachments/assets/0a1968bf-d926-4843-919a-738a584870c3">
<img width="455" alt="09b" src="https://github.com/user-attachments/assets/31a940ba-097b-410b-81de-6d5799db2da2">

6. Observe que se trata de un disco **estándar**. La ubicación es **eastus**. El tamaño del disco es de **32 GB**.

7. Guarde los cambios mediante **Guardar**.

8. No olvide el archivo de parámetros. Seleccione **Editar parámetros**, haga clic en **Cargar archivo** y cargue el **parameters.json**. 
<img width="476" alt="10" src="https://github.com/user-attachments/assets/522dbcbe-aa87-4656-8fa3-b0505de3ee58">
<img width="537" alt="11" src="https://github.com/user-attachments/assets/66b7a928-df4c-4e77-843a-585686774381">

9. Realice este cambio para que coincida con el archivo de plantilla.

    Cambie **disks_az104_disk1_name** a **disk_name** (un lugar para cambiar)
<img width="282" alt="12a" src="https://github.com/user-attachments/assets/79289ebd-bec3-4870-9d2a-39079d56270f">
<img width="319" alt="12b" src="https://github.com/user-attachments/assets/2327fc7b-981e-45d9-9371-0a9315cfa802">

10. Guarde los cambios mediante **Guardar**. 

11. Complete la configuración de implementación personalizada:

    | Configuración | Valor |
    | --- |--- |
    | Suscripción | *su suscripción* |
    | Grupo de recursos | `az104-rg3` |
    | Región | **(EE. UU.) Este de EE. UU.** |
    | Nombre del disco | `az104-disk2` |

12. Seleccione **Revisar y crear** y, luego, **Crear**.

13. Haga clic en **Go to resource** (Ir al recurso). Compruebe que se creó **az104-disk2**.

14. En la hoja **Información general**, seleccione el grupo de recursos **az104-rg3**. Ahora debería tener dos discos.
<img width="405" alt="14" src="https://github.com/user-attachments/assets/a1a8d8d3-70f8-470c-adc0-ea4d49ea05f3">
<img width="297" alt="15" src="https://github.com/user-attachments/assets/2f480ca1-3e52-4e53-886d-6f6c1bec5388">

15. En la sección **Configuración**, haga clic en **Implementaciones**.

    >**Nota:** Todos los detalles de las implementaciones se documentan en el grupo de recursos. Se recomienda revisar las primeras implementaciones basadas en plantillas para garantizar el éxito antes de usar las plantillas para las operaciones a gran escala.

16. Seleccione una implementación y revise el contenido de las hojas **Entrada** y **Plantilla**.

## Tarea 3: Configuración de Cloud Shell e implementación de una plantilla con PowerShell 

En esta tarea, trabajará con Azure Cloud Shell y Azure PowerShell. Azure Cloud Shell es un terminal interactivo, autenticado y al que se puede acceder desde un explorador para administrar recursos de Azure. Ofrece la flexibilidad de poder elegir la experiencia de shell que mejor se adapte a la forma de trabajar de cada uno, Bash o PowerShell. En esta tarea, usará PowerShell para implementar una plantilla. 

1. Seleccione el icono **Cloud Shell** en la parte superior derecha de Azure Portal. Como alternativa, puede navegar directamente a `https://shell.azure.com`.
<img width="104" alt="16" src="https://github.com/user-attachments/assets/f39f1720-84d0-4427-8f18-a8951ca91bc5">
2. Cuando se le pida que seleccione **Bash** o **PowerShell**, seleccione **PowerShell**. 
<img width="455" alt="17" src="https://github.com/user-attachments/assets/0a2a269b-c85b-4c3c-8fc4-5d943c2a510e">

3. En la pantalla **Introducción**, seleccione **Montar una cuenta de almacenamiento**, seleccione la **suscripción de la cuenta de almacenamiento** y, a continuación, seleccione **Aplicar**.
<img width="481" alt="18" src="https://github.com/user-attachments/assets/7f65f9a6-4714-44fc-8459-72fb3807c6c4">


4. Seleccione **Quiero crear una cuenta de almacenamiento** y, a continuación, **Siguiente**. Complete la información de **Crear cuenta de almacenamiento**. 
    
    | Configuración | Valores |
    |  -- | -- |
    | Grupo de recursos | **az104-rg3** |
    | Region | *seleccione la región* | 
    | Cuenta de almacenamiento (Crear nueva) | * debe ser único globalmente, entre 3 y 24 caracteres de longitud y usar números y solo letras minúsculas* |
    | Recurso compartido de archivos (Crear nuevo) | `fs-cloudshell` |

<img width="356" alt="19" src="https://github.com/user-attachments/assets/03664d27-bffc-4f2d-8440-1e212696f7ce">
<img width="482" alt="20" src="https://github.com/user-attachments/assets/920a48d8-532d-4d44-bc33-eababfc21410">

5. Cuando haya terminado, seleccione **Crear**.

    >Tardará un par de minutos en aprovisionar el almacenamiento.

6. Seleccione **Configuración** (barra superior) y, a continuación, **Ir a la versión clásica**.
<img width="258" alt="21" src="https://github.com/user-attachments/assets/49c503f5-a873-45c2-912a-610230e17460">

7. Seleccione el icono **Cargar o descargar archivos** (barra superior) y, a continuación, seleccione **Cargar**.
<img width="161" alt="22" src="https://github.com/user-attachments/assets/10baa0d2-148e-4129-a1db-b4eccd43c8e9">

8. Cargue los archivos de plantilla y parámetros desde el directorio **Descargas**. 

9. Seleccione el icono **Editor (corchetes)** y vaya hasta el archivo JSON de plantilla de la izquierda en el panel de navegación.
<img width="345" alt="23" src="https://github.com/user-attachments/assets/abdd210b-05fa-4e65-a6ec-a0c49564f47f">

10. Realizar un cambio. Por ejemplo, cambie el nombre del disco a **az104-disk3**. Presione **Ctrl+S** para guardar los cambios. 
<img width="664" alt="24" src="https://github.com/user-attachments/assets/235ded01-b338-43bd-bfba-c4485911312f">}

11. Para realizar la implementación en un grupo de recursos, utilice **New-AzResourceGroupDeployment**.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
12. Asegúrese de que el comando se completa y ProvisioningState es **Correcto**.
<img width="745" alt="25" src="https://github.com/user-attachments/assets/4ab96ca6-682c-4ee9-9104-5300ce52b1d3">

13. Confirme que se creó el disco.

   ```powershell
   Get-AzDisk
   ```
<img width="243" alt="26" src="https://github.com/user-attachments/assets/890e853f-fd5e-4adf-9b71-3bf62e01e2c8">
<img width="308" alt="27" src="https://github.com/user-attachments/assets/0c3f43e9-a966-4d6c-8c5e-7a93718784a6">

## Tarea 4: Implementación de una plantilla con la CLI 

1. Continúe en **Cloud Shell**, seleccione **Bash**. **Confirme** la selección.
<img width="83" alt="28" src="https://github.com/user-attachments/assets/11993dbf-9fa6-4dc6-96a1-3dd4dd5409a8">

2. Compruebe que los archivos están disponibles en el almacenamiento de Cloud Shell. Si completó la tarea anterior, los archivos de plantilla deben estar disponibles. 

    ```sh
    ls
    ```
3. Seleccione el icono **Editor** (corchetes) y vaya hasta el archivo JSON de plantilla.

4. Realizar un cambio. Por ejemplo, cambie el nombre del disco a **az104-disk4**. Presione **Ctrl+S** para guardar los cambios. 
<img width="548" alt="29" src="https://github.com/user-attachments/assets/a18f6948-a44d-4701-b215-c60a151e6a97">

    >**Nota**: La implementación de la plantilla puede tener como destino un grupo de recursos, una suscripción, un grupo de administración o un inquilino. Según el ámbito de la implementación, usará comandos diferentes.

5. Para realizar la implementación en un grupo de recursos, use **az deployment group create**.

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
<img width="680" alt="30" src="https://github.com/user-attachments/assets/30f53304-1939-4075-adef-4de411f4a0d2">

6. Asegúrese de que el comando se completa y ProvisioningState es **Correcto**.

7. Confirme que se creó el disco.

     ```sh
     az disk list --output table
     ```
<img width="510" alt="31" src="https://github.com/user-attachments/assets/e3ce3e96-853e-4217-930e-28e489ef31ab">

## Tarea 5: Implementación de un recurso mediante Azure Bicep

En esta tarea, usará un archivo de Bicep para implementar un disco administrado. Bicep es una herramienta de automatización declarativa que se basa en plantillas de ARM.

1. Continúe trabajando en **Cloud Shell** en una sesión **Bash**.

2. Busque y descargue el archivo que se encuentra en el Githun MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator.es-es **\Allfiles\Lab03\azuredeploydisk.bicep**.

3. **Cargue** el archivo bicep en Cloud Shell. 
<img width="468" alt="32" src="https://github.com/user-attachments/assets/94cc62fb-87c9-4a19-86b6-f701ea0c2075">

4. Seleccione el icono **Editor** (corchetes) y vaya al archivo.

5. Dedique un minuto a leer el archivo de plantilla de bicep. Observe cómo se define el recurso de disco. 
   
6. Haga los siguientes cambios:

    + Cambie el valor **managedDiskName** a `Disk4`.
    + Cambie el valor **nombre de SKU** a `StandardSSD_LRS`.
    + Cambie el valor de **diskSizeinGiB** a `32`.

<img width="325" alt="36" src="https://github.com/user-attachments/assets/ecb12791-3e6f-40ca-aa32-25474f0c13b5">

7. Ahora, implemente la plantilla.

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```
<img width="562" alt="34" src="https://github.com/user-attachments/assets/1285b41f-677d-4a9b-b13e-b3e1ffc525bd">

8. Confirme que se creó el disco.

    ```sh
    az disk list --output table
    ```
<img width="474" alt="35" src="https://github.com/user-attachments/assets/fee3c1cd-0cba-487d-a662-8f4e38eabb4d">

    >**Nota:** Ha implementado correctamente cinco discos administrados, cada uno de ellos de forma diferente. ¡Buen trabajo!

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 
