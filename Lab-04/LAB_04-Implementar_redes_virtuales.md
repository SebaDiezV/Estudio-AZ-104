---
lab:
  title: "Laboratorio\_04: Implementación de redes virtuales"
  module: Implement Virtual Networking
---

# Laboratorio 04: Implementación de redes virtuales

## Introducción al laboratorio

Este es el primero de tres laboratorios que se centra en las redes virtuales. En este laboratorio, aprenderá los conceptos básicos de las redes virtuales y las subredes. Aprenderá a proteger la red con grupos de seguridad de red y grupos de seguridad de aplicaciones. También obtendrá información sobre las zonas y registros DNS. 

Este laboratorio requiere una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Es posible cambiar la región, pero los pasos se describen para **Este de EE. UU.**

## Tiempo estimado: 50 minutes

## Escenario del laboratorio 

Su organización global planea implementar redes virtuales. El objetivo inmediato es dar cabida a todos los recursos existentes. Sin embargo, la organización está en una fase de crecimiento y quiere asegurarse de que hay capacidad adicional para ello.

La red virtual **CoreServicesVnet** tiene el mayor número de recursos. Se prevé una gran cantidad de crecimiento, por lo que se necesita un gran espacio de direcciones para esta red virtual.

La red virtual **ManufacturingVnet** contiene sistemas para las operaciones de las instalaciones de fabricación. La organización prevé un gran número de dispositivos conectados internos para que sus sistemas recuperen datos. 


## Diagrama de la arquitectura

![az104-lab04-architecture](https://github.com/user-attachments/assets/b65b19c8-274f-4d34-93e6-e6536d4de86d)


Estas redes virtuales y subredes están estructuradas de manera que se adaptan a los recursos existentes, pero permiten el crecimiento previsto. A continuación se crearán estas redes virtuales y subredes para sentar las bases de la infraestructura de red.

## Aptitudes de trabajo

+ Tarea 1: Cree una red virtual con subredes mediante el portal.
+ Tarea 2: Cree una red virtual y subredes mediante una plantilla.
+ Tarea 3: Cree y configure la comunicación entre un grupo de seguridad de aplicaciones y un grupo de seguridad de red.
+ Tarea 4: Configure zonas DNS de Azure públicas y privadas.
  
## Tarea 1: Creación de una red virtual con subredes mediante el portal

La organización planea una gran cantidad de crecimiento para los servicios principales. En esta tarea, creará la red virtual y las subredes asociadas para acomodar los recursos existentes y el crecimiento planeado. En esta tarea, usará Azure Portal. 

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.
   
1. Busque y seleccione `Virtual Networks`.

<img width="234" alt="01" src="https://github.com/user-attachments/assets/ae3d627d-b72e-4e2e-a9a3-60e89bdb18ca" />


1. En la página Redes virtuales, seleccione **Crear**.

1. Complete la pestaña **Aspectos básicos** de CoreServicesVnet.  

    |  **Opción**         | **Valor**            |
    | ------------------ | -------------------- |
    | Grupo de recursos     | `az104-rg4` (si es necesario, cree uno nuevo) |
    | Nombre               | `CoreServicesVnet`     |
    | Región             | (EE. UU.) **Este de EE. UU.**         |
   
<img width="463" alt="02" src="https://github.com/user-attachments/assets/07b81dfc-1673-4c94-ae05-c48aa254677a" />


1. Vaya a la pestaña **Direcciones IP**.

    |  **Opción**         | **Valor**            |
    | ------------------ | -------------------- |
    | Espacio de direcciones IPv4 | Reemplace el espacio de direcciones IPv4 rellenado previamente por `10.20.0.0/16` (separe las entradas)  |

<img width="481" alt="03" src="https://github.com/user-attachments/assets/0497225f-d925-4d04-bb37-5ffd8127d4f6" />

1. Seleccione **+ Agregar una subred**. Complete la información de nombre y dirección de cada subred. Asegúrese de seleccionar **Agregar** para cada nueva subred. Asegúrese de eliminar la subred predeterminada, ya sea antes o después de crear las otras subredes.

    | **Subred**             | **Opción**           | **Valor**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nombre de subred          | `SharedServicesSubnet`   |
    |                        | Dirección inicial     | `10.20.10.0`          |
    |                        | Size                 | `/24` |
    | DatabaseSubnet         | Nombre de subred          | `DatabaseSubnet`         |
    |                        | Dirección inicial     | `10.20.20.0`        |
    |                        | Size                 | `/24` |

    >**Nota:** Cada red virtual debe tener al menos una subred. Recuerde que siempre se reservarán cinco direcciones IP, así que téngalo en cuenta en la planificación. 
<img width="843" alt="04" src="https://github.com/user-attachments/assets/67960609-22c0-4308-b914-3ad8fff9b934" />
<img width="468" alt="05" src="https://github.com/user-attachments/assets/9697722b-6eba-444d-ad2c-f97ca711a316" />


1. Para terminar de crear la red virtual CoreServicesVnet y las subredes asociadas, seleccione **Revisar y crear**.

1. Compruebe que la configuración superó la validación y, luego, seleccione **Crear**.
<img width="314" alt="06" src="https://github.com/user-attachments/assets/86f2e9e6-55c6-4381-b437-6f209fa9fd36" />

1. Espere a que se implemente la red virtual y seleccione **Ir al recurso**.

1. Dedique un minuto a comprobar el **Espacio de direcciones** y las **Subredes**. Observe las demás opciones en la hoja **Configuración**. 
<img width="640" alt="07" src="https://github.com/user-attachments/assets/48cf9d28-687f-4426-9808-e490bfa356d9" />
<img width="512" alt="08" src="https://github.com/user-attachments/assets/d3ce5f15-c8e2-430d-ad73-db18a1400ef5" />

1. En la sección **Automatización**, seleccione **Exportar plantilla** y espere a que se genere la plantilla.
<img width="891" alt="10" src="https://github.com/user-attachments/assets/bdd3e9ee-173e-494a-a621-a0734c4d84bf" />

1. **Descargue** la plantilla.

1. Vaya a la máquina local a la carpeta **Descargas** y **Extraiga todos** los archivos del archivo ZIP descargado. 
<img width="237" alt="11" src="https://github.com/user-attachments/assets/4aba5a2a-4d36-4f2d-8712-ef7d92d1b64b" />

1. Antes de continuar, asegúrese de que tiene el archivo **template.json**. Usará esta plantilla para crear la ManufacturingVnet en la siguiente tarea. 
 <img width="448" alt="12" src="https://github.com/user-attachments/assets/5a4e4033-5519-4e27-9acb-d2e926fa6e64" />

## Tarea 2: Creación de una red virtual y subredes mediante una plantilla

En esta tarea, creará la red virtual ManufacturingVnet y las subredes asociadas. La organización prevé el crecimiento de las oficinas de fabricación, por lo que las subredes están dimensionadas para el crecimiento previsto. Para esta tarea, se usa una plantilla para crear los recursos. 

1. Busque el archivo **template.json** exportado en la tarea anterior. Debe estar en la carpeta **Descargas**.

1. Edite el archivo con el editor que prefiera. Muchos editores tienen una característica de *cambio de todas las apariciones*. Si usa Visual Studio Code, asegúrese de que está trabajando en una **ventana de confianza** y no en el **modo restringido**. Consulte el diagrama de arquitectura para comprobar los detalles. 

### Realización de cambios en la red virtual ManufacturingVnet

1. Reemplace todas las apariciones de **CoreServicesVnet** por `ManufacturingVnet`. 

1. Reemplace todas las apariciones de **10.20.0.0** por `10.30.0.0`. 

### Realización de cambios en las subredes de ManufacturingVnet

1. Cambie todas las apariciones de **SharedServicesSubnet** a `SensorSubnet1`.

1. Cambie todas las apariciones de **10.20.10.0/24** a `10.30.20.0/24`.

1. Cambie todas las apariciones de **DatabaseSubnet** a `SensorSubnet2`.

1. Cambie todas las apariciones de **10.20.20.0/24** a `10.30.21.0/24`.

1. Vuelva a leer el archivo y asegúrese de que todo es correcto.

1. Asegúrese de **Guardar** los cambios.

>**Nota:** Hay archivos de plantilla completados en el directorio de archivos de laboratorio. 

### Realización de cambios en el archivo de parámetros

1. Localice el archivo **parameters.json** exportado en la tarea anterior. Debe estar en la carpeta **Descargas**.

1. Edite el archivo con el editor que prefiera.

1. Reemplace una aparición de **CoreServicesVnet** por `ManufacturingVnet`.

1. Guarde los cambios mediante **Guardar**.
   
### Implementación de la plantilla personalizada

1. En el portal, busque y seleccione **Implementar una plantilla personalizada**.
<img width="302" alt="01" src="https://github.com/user-attachments/assets/73f4dada-ee48-4ac7-af39-755e44ee67d0" />

1. Seleccione **Cree su propia plantilla en el editor** y, a continuación, **Cargar archivo**.
<img width="451" alt="02" src="https://github.com/user-attachments/assets/336a7a34-14c7-4da3-a156-097805e149ff" />
<img width="666" alt="03" src="https://github.com/user-attachments/assets/464fb98e-19ae-4164-9b08-226bf9f27e60" />

1. Seleccione el archivo **templates.json** con los cambios de fabricación y, a continuación, seleccione **Guardar**.
<img width="480" alt="04" src="https://github.com/user-attachments/assets/00f0d5c4-36ee-44ea-90bb-f2d8ce752a63" />
<img width="497" alt="06" src="https://github.com/user-attachments/assets/59d9a35b-4ec5-4916-a119-be6f76b01e36" />

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.
<img width="523" alt="07" src="https://github.com/user-attachments/assets/a92de701-5e80-49af-a245-c79004d3015d" />

1. Espere a que se implemente la plantilla y, a continuación, confirme (en el portal) que se han creado la red virtual de fabricación y las subredes.
<img width="177" alt="08" src="https://github.com/user-attachments/assets/578c05a6-382d-4b6a-afca-e9664a0e73bd" />

>**Nota:** Si tiene que implementar más de una vez, puede encontrar que algunos recursos se completaron correctamente y se produce un error en la implementación. Puede quitar manualmente esos recursos e intentarlo de nuevo. 
   
## Tarea 3: Cree y configure la comunicación entre un grupo de seguridad de aplicaciones y un grupo de seguridad de red

En esta tarea, creamos un grupo de seguridad de aplicaciones y un grupo de seguridad de red. El grupo de seguridad de red tendrá una regla de seguridad de entrada que permita el tráfico desde el ASG. El grupo de seguridad de red también tendrá una regla de salida que deniega el acceso a Internet. 

### Creación del grupo de seguridad de aplicaciones (ASG)

1. En Azure Portal, busque y seleccione `Application security groups`.
<img width="365" alt="01" src="https://github.com/user-attachments/assets/d71e806a-491b-46ae-8fcc-8f5b51c2d54b" />

1. Haga clic en **Crear** y proporcione la información básica.

    | Configuración | Valor |
    | -- | -- |
    | Suscripción | *su suscripción* |
    | Resource group | **az104-rg4** |
    | Nombre | `asg-web` |
    | Región | **Este de EE. UU.**  |

1. Haga clic en **Revisar y crear** y luego, después de la validación, haga clic en **Crear**.
<img width="525" alt="02" src="https://github.com/user-attachments/assets/3512f1e5-d463-4edb-92f3-4948e0916e60" />

### Cree el grupo de seguridad de red y asócielo a la subred del ASG

1. En Azure Portal, busque y seleccione `Network security groups`.
<img width="311" alt="03" src="https://github.com/user-attachments/assets/042e08f0-2663-4974-9580-e7dbe3e2dc4b" />

1. Seleccione **+ Crear** y proporcione información sobre la pestaña **Conceptos básicos**. 

    | Configuración | Valor |
    | -- | -- |
    | Suscripción | *su suscripción* |
    | Resource group | **az104-rg4** |
    | Nombre | `myNSGSecure` |
    | Región | **Este de EE. UU.**  |

1. Haga clic en **Revisar y crear** y luego, después de la validación, haga clic en **Crear**.
<img width="544" alt="04" src="https://github.com/user-attachments/assets/7986b726-99ba-48f6-b242-b3d8c160f498" />

1. Una vez implementado el grupo de seguridad de red, haga clic en **Ir al recurso**.

1. En **Configuración** haga clic en **Subredes** y, a continuación, **Asociar**.

    | Configuración | Value |
    | -- | -- |
    | Virtual network | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** |
<img width="422" alt="06" src="https://github.com/user-attachments/assets/f657269a-2db7-4111-b816-3b6228e618b9" />
<img width="352" alt="07" src="https://github.com/user-attachments/assets/5e0f2e2c-b8c7-4d08-b63d-db439cfa34e6" />

1. Haga clic en **Aceptar** para guardar la asociación.

### Configuración de una regla de seguridad de entrada para permitir el tráfico del ASG

1. Continúe trabajando con el grupo de seguridad de red. En el área **Configuración**, seleccione **Reglas de seguridad de entrada**.

1. Revise las reglas de entrada predeterminadas. Observe que solo se permite el acceso a otras redes virtuales y equilibradores de carga.
<img width="889" alt="08" src="https://github.com/user-attachments/assets/84c369c8-247c-4703-9a18-8ddeb872843b" />

1. Seleccione **+Agregar**.

1. En la hoja **Agregar regla de seguridad de entrada**, use la siguiente información para agregar una regla de puerto de entrada. Esta regla permite el tráfico del ASG. Cuando haya terminado, seleccione **Agregar**.

    | Configuración | Valor |
    | -- | -- |
    | Source | **Grupo de seguridad de aplicaciones** |
    | Grupos de seguridad de la aplicación de origen | **asg-web** |
    | Rangos del puerto origen |  * |
    | Destino | **Cualquiera** |
    | Service | **Personalizada** (observe las otras opciones) |
    | Intervalos de puertos de destino | **80, 443** |
    | Protocolo | **TCP** |
    | Acción | **Permitir** |
    | Prioridad | **100** |
    | Nombre | `AllowASG` |
   
<img width="351" alt="09" src="https://github.com/user-attachments/assets/ddc42f6e-3255-49cc-bf62-47eb324fd9d0" />
<img width="353" alt="10" src="https://github.com/user-attachments/assets/8e29aaf7-7eaf-498e-8049-5ca5f7e512f3" />
<img width="783" alt="11" src="https://github.com/user-attachments/assets/7cdbd57b-01d2-4d86-bb39-b321fda7963c" />

### Configuración de una regla del NSG saliente que deniega el acceso a Internet

1. Después de crear la regla del NSG de entrada, seleccione **Reglas de seguridad de salida**. 
<img width="946" alt="12" src="https://github.com/user-attachments/assets/8d1a358c-a1e8-4efb-8fc8-91f7cadf36db" />

1. Observe la regla **AllowInternetOutboundRule**. Observe también que la regla no se puede eliminar y la prioridad es 65001.

1. Seleccione **+ Agregar** y configure una regla de salida que deniegue el acceso a Internet. Cuando haya terminado, seleccione **Agregar**.

    | Configuración | Valor |
    | -- | -- |
    | Origen | **Cualquiera** |
    | Intervalos de puertos de origen |  * |
    | Destination | **Etiqueta de servicio** |
    | Etiqueta de servicio de destino | **Internet** |
    | Service | **Personalizada** |
    | Intervalos de puertos de destino | **8080** |
    | Protocolo | **Cualquiera** |
    | Acción | **Deny** |
    | Prioridad | **4096** |
    | Nombre | **DenyAnyCustom8080Outbound** |

<img width="357" alt="13" src="https://github.com/user-attachments/assets/edfbf0ef-7c73-4604-bb7d-1f6f9ef2cdb0" />
<img width="355" alt="14" src="https://github.com/user-attachments/assets/fb0da809-a8a6-48b3-b792-b41c44db23cd" />
<img width="773" alt="15" src="https://github.com/user-attachments/assets/f8c9fc8b-b352-4485-938d-c427c5f138fb" />

## Tarea 4: Configuración de zonas DNS de Azure públicas y privadas

En esta tarea, creará y configurará zonas DNS públicas y privadas. 

### Configuración de una zona DNS pública

Puede configurar Azure DNS para resolver nombres de host en el dominio público. Por ejemplo, si ha adquirido el nombre de dominio contoso.xyz de un registrador de nombres de dominio, puede configurar Azure DNS para hospedar el dominio `contoso.com` y resolver www.contoso.xyz en la dirección IP del servidor web o la aplicación web.

1. En el portal, busque y seleccione `DNS zones`.
<img width="317" alt="01" src="https://github.com/user-attachments/assets/936ceb27-08a5-4219-b361-180d8a8a69fc" />

1. Seleccione **+ Create** (+ Crear).

1. Configure la pestaña **Aspectos básicos**.

    | Propiedad | Valor    |
    |:---------|:---------|
    | Suscripción | **Selecciona la suscripción** |
    | Resource group | **az-104-rg4** |
    | Nombre | `contoso.com` (si se reserva, ajuste el nombre) |
    | Region |**Este de EE. UU.** (revise el icono informativo) |
   
<img width="533" alt="02" src="https://github.com/user-attachments/assets/2d9ded65-e35b-4edd-a35d-89d723de0ac2" />

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.
   
1. Espere a que se implemente la zona DNS y seleccione **Ir al recurso**.

1. En la hoja **Información general**, observe los nombres de los cuatro servidores de nombres DNS de Azure asignados a la zona. **Copie** una de las direcciones del servidor de nombres. La necesitará en un paso posterior. 
  <img width="915" alt="03" src="https://github.com/user-attachments/assets/dadf30c3-7ab4-4807-b975-49dee72c4d2f" />

1. Seleccione **+ Conjunto de registros**. Agregue un registro de vínculo de red virtual para cada red virtual que necesite compatibilidad con la resolución de nombres privados.

    | Propiedad | Valor    |
    |:---------|:---------|
    | Nombre | **www** |
    | Tipo | **A**
           |
    | TTL | **1** |
    | Dirección IP | **10.1.1.4** |

>**Nota:**  En un escenario real, escribiría la dirección IP pública del servidor web.

1. Seleccione **Aceptar** y compruebe que **contoso.com** tiene un conjunto de registros A denominado **www**.

1. Abra un símbolo del sistema y ejecute el comando siguiente:

   ```sh
   nslookup www.contoso.com <name server name>
   ```
<img width="486" alt="06" src="https://github.com/user-attachments/assets/c8459eef-5ce1-4e31-898a-d8908b00ab03" />
   
1. Compruebe que el nombre de host www.contoso.com se resuelve en la dirección IP proporcionada. Esto confirma que la resolución de nombres funciona correctamente.
<img width="424" alt="07" src="https://github.com/user-attachments/assets/852d069f-e12e-4ce3-b6f1-c9481dfccf57" />

### Configuración de una zona DNS privada

Una zona DNS privada proporciona servicios de resolución de nombres dentro de las redes virtuales. Solo se puede acceder a una zona DNS privada desde las redes virtuales a las que está vinculada y no se puede acceder desde Internet. 

1. En el portal, busque y seleccione `Private dns zones`.
<img width="324" alt="08" src="https://github.com/user-attachments/assets/a31a0d47-3b8a-4405-9664-2d1de4f53af3" />

1. Seleccione **+ Create** (+ Crear).

1. En la pestaña **Datos básicos** de Crear zona DNS privada, escriba la información como se muestra en la tabla siguiente:

    | Propiedad | Valor    |
    |:---------|:---------|
    | Suscripción | **Selecciona la suscripción** |
    | Resource group | **az-104-rg4** |
    | Nombre | `private.contoso.com` (ajuste si tenía que cambiar el nombre) |
    | Region |**Este de EE. UU.** |
   
<img width="467" alt="09" src="https://github.com/user-attachments/assets/294d7714-2482-4a72-8f9e-fc45567a2a75" />

1. Seleccione **Revisar y crear** y, a continuación, **Crear**.
   
1. Espere a que se implemente la zona DNS y seleccione **Ir al recurso**.

1. Observe que en la hoja **Información general** no hay registros de servidor de nombres. 
<img width="841" alt="10" src="https://github.com/user-attachments/assets/55b7acde-a20f-4a66-bf24-5eae99d33f79" />

1. Selecciona **Administración de DNS** y después **Vínculos de red virtual**. Configura el vínculo. 
<img width="408" alt="11" src="https://github.com/user-attachments/assets/9aec329b-704d-45fd-9bf8-8e45cd4a23c2" />

    | Propiedad | Valor    |
    |:---------|:---------|
    | Nombre del vínculo | `manufacturing-link` |
    | Red virtual | `ManufacturingVnet` |
   
<img width="521" alt="12" src="https://github.com/user-attachments/assets/7ba36019-1d9a-4082-9246-c745022b30bb" />

1. Selecciona **Crear** y espera a que se cree el vínculo. 
<img width="601" alt="13" src="https://github.com/user-attachments/assets/cf5b3e2c-67bb-4d31-bf05-aa223a45aded" />

1. En la hoja **Administración de DNS** selecciona **+ Conjuntos de registros**. Ahora agregaría un registro para cada máquina virtual que necesite compatibilidad con la resolución de nombres privada.
<img width="570" alt="14" src="https://github.com/user-attachments/assets/030d4e55-56e2-4de3-aeef-3e9a580abcbf" />

    | Propiedad | Valor    |
    |:---------|:---------|
    | Nombre | **sensorvm** |
    | Tipo | **A**
           |
    | TTL | **1** |
    | Dirección IP | **10.1.1.4** |

<img width="352" alt="15" src="https://github.com/user-attachments/assets/62cb6e63-3204-444e-86e8-ca81970dcfee" />
<img width="490" alt="16" src="https://github.com/user-attachments/assets/d12d6acb-6bb4-4843-a722-1cd01e784825" />


 >**Nota:**  En un escenario real, escribiría la dirección IP de una máquina virtual de fabricación específica.

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.
