---
lab:
  title: 'Lab 03d: Manage Azure resources by Using Azure CLI (opcional)'
  module: Administer Azure Resources
---

# Laboratorio 03d: Administración de recursos de Azure mediante la CLI de Azure
# Manual de laboratorio para alumnos

## Escenario del laboratorio

Ahora que ha explorado las funcionalidades básicas de administración de Azure asociadas con el aprovisionamiento de recursos y su organización en función de los grupos de recursos mediante Azure Portal, las plantillas de Azure Resource Manager y Azure PowerShell, debe llevar a cabo la tarea equivalente mediante la CLI de Azure. Para evitar la instalación de la CLI de Azure, aprovechará el entorno de Bash disponible en Azure Cloud Shell.


## Objetivos

En este laboratorio, aprenderá a:

+ Tarea 1: Iniciar una sesión de Bash en Azure Cloud Shell
+ Tarea 2: Crear un grupo de recursos y un disco administrado de Azure mediante la CLI de Azure
+ Tarea 3: Configurar el disco administrado mediante la CLI de Azure

## Tiempo estimado: 20 minutos

## Diagrama de la arquitectura

![lab03d](https://github.com/user-attachments/assets/77e8c892-cc1e-4c85-8643-c9fa3cac8b97)


### Instrucciones

## Ejercicio 1

## Tarea 1: Iniciar una sesión de Bash en Azure Cloud Shell

En esta tarea, abrirá una sesión de Bash en Cloud Shell. 

1. Haga clic en el icono de la esquina superior derecha de Azure Portal para abrir **Azure Cloud Shell**.

1. Si se le pide que seleccione **Bash** o **PowerShell**, seleccione **Bash**.

<img width="129" alt="01" src="https://github.com/user-attachments/assets/bcc46082-89a5-42f9-bd90-c5b9de85a5aa" />


    >**Nota**: Si es la primera vez que inicia **Cloud Shell** y aparece el mensaje **No tiene ningún almacenamiento montado**, seleccione la suscripción que utiliza en este laboratorio y haga clic en **Crear almacenamiento**. 

1. Si se le solicite, haga clic en **Crear almacenamiento** y espere hasta que aparezca Azure Cloud Shell. 

1. Asegúrese de que **Bash** aparezca en el menú desplegable superior izquierdo del panel de Cloud Shell.

## Tarea 2: Crear un grupo de recursos y un disco administrado de Azure mediante la CLI de Azure

En esta tarea, creará un grupo de recursos y un disco administrado de Azure mediante una sesión de la CLI de Azure dentro de Cloud Shell.

1. Para crear un grupo de recursos en la misma región de Azure que el grupo de recursos **az104-03c-rg1** que creó en el laboratorio anterior, desde la sesión de Bash en Cloud Shell, ejecute lo siguiente:

   ```sh
   LOCATION=$(az group show --name 'az104-03c-rg1' --query location --out tsv)

   RGNAME='az104-03d-rg1'

   az group create --name $RGNAME --location $LOCATION
   ```
<img width="383" alt="02a" src="https://github.com/user-attachments/assets/4b6cd6ce-d52d-4fe7-8042-23f160b83168" />

<img width="468" alt="03" src="https://github.com/user-attachments/assets/9995447b-cf0f-4b94-b473-f7c3e5814901" />


1. Para recuperar las propiedades del grupo de recursos recién creado, ejecute lo siguiente:

   ```sh
   az group show --name $RGNAME
   ```
<img width="478" alt="04" src="https://github.com/user-attachments/assets/3979007e-1f32-407d-8791-485d6117fbf1" />


1. Para crear un nuevo disco administrado con las mismas características que las que creó en los laboratorios anteriores de este módulo, desde la sesión de Bash dentro de Cloud Shell, ejecute lo siguiente:

   ```sh
   DISKNAME='az104-03d-disk1'

   az disk create \
   --resource-group $RGNAME \
   --name $DISKNAME \
   --sku 'Standard_LRS' \
   --size-gb 32
   ```
<img width="219" alt="05" src="https://github.com/user-attachments/assets/ce041ca2-6a86-4af4-bb35-0d0bde584ecf" />


    >**Nota**: Al usar la sintaxis de varias líneas, asegúrese de que cada línea termine con barra diagonal inversa (`\`) sin espacios finales, y de que no haya espacios iniciales al principio de cada línea.

1. Para recuperar las propiedades del disco recién creado, ejecute lo siguiente:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME
   ```
<img width="391" alt="06" src="https://github.com/user-attachments/assets/45a06f9d-48b0-40e4-ae15-aa4bc7f5ad6e" />


## Tarea 3: Configurar el disco administrado mediante la CLI de Azure

En esta tarea, administrará la configuración del disco administrado de Azure mediante una sesión de la CLI de Azure en Cloud Shell. 

1. Para aumentar el tamaño del disco administrado de Azure a **64 GB**, desde la sesión de Bash en Cloud Shell, ejecute lo siguiente:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```
<img width="454" alt="07" src="https://github.com/user-attachments/assets/e6cc5014-a681-4996-b1f1-06e183a9d65e" />

1. Para comprobar que el cambio ha surtido efecto, ejecute lo siguiente:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGB
   ```
<img width="492" alt="08" src="https://github.com/user-attachments/assets/f694c50d-2f89-4bed-b664-eafc16ba85e1" />

1. Para cambiar la SKU de rendimiento del disco a **Premium_LRS**, desde la sesión de Bash en Cloud Shell, ejecute lo siguiente:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```
<img width="496" alt="09" src="https://github.com/user-attachments/assets/21529678-476d-4dc5-988f-31199cdac0e7" />

1. Para comprobar que el cambio ha surtido efecto, ejecute lo siguiente:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```
<img width="445" alt="10" src="https://github.com/user-attachments/assets/1cb3e596-87cf-4b89-83d4-c97fb4a4ea66" />


## Limpieza de recursos

 > **Nota**: No olvide quitar los recursos de Azure recién creados que ya no use. La eliminación de los recursos sin usar garantiza que no verá cargos inesperados.

 > **Nota:** No se preocupe si los recursos del laboratorio no se pueden quitar inmediatamente. A veces, los recursos tienen dependencias y se tarda más tiempo en eliminarlos. Supervisar el uso de los recursos es una tarea habitual del administrador, así que solo tiene que revisar periódicamente los recursos en el portal para ver cómo va la limpieza. 

1. En Azure Portal, abra la sesión de shell de **Bash** en el panel **Cloud Shell**.

1. Ejecute el comando siguiente para enumerar todos los grupos de recursos que se han creado en los laboratorios de este módulo:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].name" --output tsv
   ```
<img width="439" alt="11" src="https://github.com/user-attachments/assets/dd3e3e7b-b912-4006-b1bc-cde2ec743b36" />

1. Ejecute el comando siguiente para eliminar todos los grupos de recursos que ha creado en los laboratorios de este módulo:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```
<img width="756" alt="12" src="https://github.com/user-attachments/assets/1a993524-cd56-4635-9711-301dd60f56d9" />

    >**Nota**: El comando se ejecuta de forma asincrónica (según determina el parámetro --nowait). Aunque podrá ejecutar otro comando de la CLI de Azure inmediatamente después en la misma sesión de Bash, los grupos de recursos tardarán unos minutos en quitarse.

## Revisar

En este laboratorio, ha:

- Iniciado una sesión de Bash en Azure Cloud Shell
- Creado un grupo de recursos y un disco administrado de Azure mediante la CLI de Azure
- Configurado el disco administrado mediante la CLI de Azure
