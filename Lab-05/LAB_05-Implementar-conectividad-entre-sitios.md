---
lab:
  title: "Laboratorio\_05: Implementación de la conectividad entre sitios"
  module: Administer Intersite Connectivity
---

# Laboratorio 05: Implementación de la conectividad entre sitios

## Introducción al laboratorio

En este laboratorio, explorará la comunicación entre redes virtuales. Implemente el emparejamiento de redes virtuales y pruebe las conexiones. También creará una ruta personalizada. 

Para este laboratorio se necesita una suscripción de Azure. El tipo de suscripción puede afectar a la disponibilidad de las características de este laboratorio. Es posible cambiar la región, pero los pasos se describen para **Este de EE. UU.** 

## Tiempo estimado: 50 minutes
    
## Escenario del laboratorio 

Su organización segmenta los servicios y aplicaciones de TI principales (como DNS y servicios de seguridad) de otras partes de la empresa, incluido el departamento de fabricación. Sin embargo, en algunos escenarios, las aplicaciones y los servicios del área principal necesitan comunicarse con aplicaciones y servicios en el área de fabricación. En este laboratorio, configurará la conectividad entre las áreas segmentadas. Este es un escenario común para separar la producción del desarrollo o separar una filial de otra.  


## Diagrama de la arquitectura

![az104-lab05-architecture](https://github.com/user-attachments/assets/48d6a128-0ae9-4a0c-ba23-d53790b1ffa6)


## Aptitudes de trabajo

+ Tarea 1: Cree una máquina virtual en una red virtual.
+ Tarea 2: Cree una máquina virtual en otra red virtual.
+ Tarea 3: Utilice Network Watcher para probar la conexión entre máquinas virtuales. 
+ Tarea 4: Configure emparejamientos de red virtual entre diferentes redes virtuales.
+ Tarea 5: Use Azure PowerShell para probar la conexión entre máquinas virtuales.
+ Tarea 6: Crear una ruta personalizada. 

## Tarea 1:  Creación de una máquina virtual de servicios principales y una red virtual

En esta tarea, creará una red virtual de servicios principales con una máquina virtual. 

1. Inicie sesión en **Azure Portal** - `https://portal.azure.com`.

1. Busque y seleccione `Virtual Machines`.
<img width="508" alt="01" src="https://github.com/user-attachments/assets/6969cc4d-b832-4c2c-a8f7-e72305886fb8" />

1. En la página máquinas virtuales, seleccione **Crear**, después, seleccione **Azure Virtual Machine**.
<img width="210" alt="02" src="https://github.com/user-attachments/assets/9b8393e4-f947-4b52-90ab-7398accd0648" />

1. En la pestaña Aspectos básicos, use la siguiente información para completar el formulario y, a continuación, seleccione **Siguiente: Discos >**. Para cualquier configuración no especificada, deje el valor predeterminado.
 
    | Configuración | Valor | 
    | --- | --- |
    | Suscripción |  *su suscripción* |
    | Resource group |  `az104-rg5` (Si es necesario, **Crear nuevo**. )
    | Nombre de la máquina virtual |    `CoreServicesVM` |
    | Región | **(EE. UU.) Este de EE. UU.** |
    | Opciones de disponibilidad | No se requiere redundancia de la infraestructura |
    | Tipo de seguridad | **Estándar** |
    | Imagen | **Windows Server 2019 Datacenter: x64 Gen2** (observe las otras opciones) |
    | Size | **Standard_DS2_v3** |
    | Nombre de usuario | `localadmin` | 
    | Contraseña | **Proporcionar una contraseña compleja** |
    | Puertos de entrada públicos | **Ninguno** |

<img width="509" alt="03" src="https://github.com/user-attachments/assets/afa3a335-d985-4410-adc8-bb9000a96f5b" />
<img width="502" alt="04" src="https://github.com/user-attachments/assets/3cbb8249-f5ac-4ae7-b3cd-eff2f646d8e8" />
<img width="496" alt="05" src="https://github.com/user-attachments/assets/6d8281e2-3c48-4cb1-a303-06d118599e36" />
    
   
1. En la pestaña **Disk**, tome los valores predeterminados y, a continuación, seleccione **Siguiente: Redes >**.

1. En la pestaña **Redes**, en Red virtual, seleccione **Crear nuevo**.
<img width="290" alt="06" src="https://github.com/user-attachments/assets/69d0a040-85d7-4128-bfc7-86d52a0da622" />

1. Use la siguiente información para configurar la red virtual y, a continuación, seleccione **Aceptar**. Si es necesario, quite o reemplace la información existente.

    | Configuración | Value | 
    | --- | --- |
    | Nombre | `CoreServicesVnet` (Crear nuevo) |
    | Intervalo de direcciones | `10.0.0.0/16`  |
    | Nombre de subred | `Core` | 
    | Intervalo de direcciones de subred | `10.0.0.0/24` |
<img width="509" alt="07" src="https://github.com/user-attachments/assets/a426b8f3-de9c-411f-a8ae-af822f523f3d" />

1. Seleccione la pestaña **Supervisión**. En Diagnósticos de arranque, seleccione **Deshabilitar**.
<img width="551" alt="08" src="https://github.com/user-attachments/assets/d8f381bd-de1d-4a9e-b98b-07222bc47e9b" />

1. Seleccione **Revisar y crear** y, luego, **Crear**.

1. No es necesario esperar a que se creen los recursos. Continúe con la siguiente tarea.

    >**Nota:** ¿Ha observado que en esta tarea creó la red virtual a medida que creó la máquina virtual? También puede crear la infraestructura de red virtual y, a continuación, agregar las máquinas virtuales. 

## Tarea 2: Cree una máquina virtual en otra red virtual

En esta tarea, creará una red virtual de servicios principales con una máquina virtual. 

1. En Azure Portal, busque y seleccione **Máquinas virtuales**.

1. En la página máquinas virtuales, seleccione **Crear**, después, seleccione **Azure Virtual Machine**.

1. En la pestaña Aspectos básicos, use la siguiente información para completar el formulario y, a continuación, seleccione **Siguiente: Discos >**. Para cualquier configuración no especificada, deje el valor predeterminado.
 
    | Configuración | Valor | 
    | --- | --- |
    | Suscripción |  *su suscripción* |
    | Resource group |  `az104-rg5` |
    | Nombre de la máquina virtual |    `ManufacturingVM` |
    | Región | **(EE. UU.) Este de EE. UU.** |
    | Tipo de seguridad | **Estándar** |
    | Opciones de disponibilidad | No se requiere redundancia de la infraestructura |
    | Imagen | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Size | **Standard_DS2_v3** | 
    | Nombre de usuario | `localadmin` | 
    | Contraseña | **Proporcionar una contraseña compleja** |
    | Puertos de entrada públicos | **Ninguno** |
<img width="532" alt="09" src="https://github.com/user-attachments/assets/6f408c84-01c5-415e-8795-2e60589aab18" />
<img width="529" alt="10" src="https://github.com/user-attachments/assets/9238aecd-58c1-4194-aafa-d1377000f34c" />
<img width="582" alt="11" src="https://github.com/user-attachments/assets/01a76e45-30f3-4282-add4-6d509f4c2e9d" />

1. En la pestaña **Disk**, tome los valores predeterminados y, a continuación, seleccione **Siguiente: Redes >**.

1. En la pestaña Redes, en Red virtual, seleccione **Crear nuevo**.
<img width="256" alt="12" src="https://github.com/user-attachments/assets/32bea744-0b0e-44a6-8082-593cbceb8b2b" />

1. Use la siguiente información para configurar la red virtual y, a continuación, seleccione **Aceptar**.  Si es necesario, quite o reemplace el intervalo de direcciones existente.

    | Configuración | Value | 
    | --- | --- |
    | Nombre | `ManufacturingVnet` |
    | Intervalo de direcciones | `172.16.0.0/16`  |
    | Nombre de subred | `Manufacturing` |
    | Intervalo de direcciones de subred | `172.16.0.0/24` |
<img width="495" alt="13" src="https://github.com/user-attachments/assets/f9584f5a-3219-4e42-be51-0ecea3d6692c" />

1. Seleccione la pestaña **Supervisión**. En Diagnósticos de arranque, seleccione **Deshabilitar**.

1. Seleccione **Revisar y crear** y, luego, **Crear**.

## Tarea 3: Utilice Network Watcher para probar la conexión entre máquinas virtuales 


En esta tarea, comprobará que los recursos de las redes virtuales emparejadas pueden comunicarse entre sí. Network Watcher se usará para probar la conexión. Antes de continuar, asegúrese de que ambas máquinas virtuales se han implementado y se están ejecutando. 

1. En Azure Portal, busque y seleccione `Network Watcher`.
<img width="328" alt="01" src="https://github.com/user-attachments/assets/a14dd59b-04ed-4566-b720-8a003acb3310" />

1. En Network Watcher, en el menú Herramientas de diagnóstico de red, seleccione **Solución de problemas de conexión**.
<img width="164" alt="02" src="https://github.com/user-attachments/assets/f4608a7e-8e8f-4492-ba0f-fc6fad4dfcb0" />

1. Utilice la siguiente información para completar los campos de la página **Solución de problemas de conexión**.

    | Campo | Valor | 
    | --- | --- |
    | Tipo de origen           | **Máquina virtual**   |
    | Máquina virtual       | **CoreServicesVM**    | 
    | Tipo de destino      | **Máquina virtual**   |
    | Máquina virtual       | **ManufacturingVM**   | 
    | Versión de IP preferida  | **Ambos**              | 
    | Protocolo              | **TCP**               |
    | Puerto de destino      | `3389`                |  
    | Puerto de origen           | *Blank*         |
    | Pruebas de diagnóstico      | *Defaults*      |

<img width="465" alt="03" src="https://github.com/user-attachments/assets/a706e177-125a-4e51-9f38-8e30d440ae2b" />
<img width="463" alt="04" src="https://github.com/user-attachments/assets/7f5205b4-9f1b-441e-896d-647e398cbb91" />


1. Seleccione **Ejecutar pruebas de diagnóstico**.
<img width="629" alt="05" src="https://github.com/user-attachments/assets/e0cb7025-7b9a-42d7-a6f8-754be78beea2" />

    >**Nota**: Los resultados pueden tardar un par de minutos en devolverse. Las selecciones de pantalla se atenuarán mientras se recopilan los resultados. Observe que la **Prueba de conectividad** muestra **UnReachable**. Esto tiene sentido porque las máquinas virtuales están en diferentes redes virtuales. 

 
## Tarea 4: Configuración de emparejamientos de redes virtuales entre redes virtuales

En esta tarea, creará un emparejamiento de red virtual para habilitar las comunicaciones entre los recursos de las redes virtuales. 

1. En Azure Portal, seleccione la `CoreServicesVnet` red virtual.
<img width="340" alt="01" src="https://github.com/user-attachments/assets/51870951-6ad8-437a-8717-b290e17ad0a8" />

1. En CoreServicesVnet, en **Configuración**, seleccione **Emparejamientos**.
<img width="287" alt="02" src="https://github.com/user-attachments/assets/aca4b622-7a11-4af6-8c24-9d5b1231ee77" />

1. En CoreServicesVnet | Emparejamientos, seleccione **+ Agregar**. Si no se especifica, tome el valor predeterminado. 

| **Parámetro**                                    | **Valor**                             |
| --------------------------------------------- | ------------------------------------- |                                
| Nombre del vínculo de emparejamiento                             | `CoreServicesVnet-to-ManufacturingVnet` |
| Red virtual    | **ManufacturingVM-net (az104-rg5)**  |
| Permitir que ManufacturingVNet acceda a CoreServicesVNet  | seleccionado (valor predeterminado)                       |
| Permitir que ManufacturingVnet reciba tráfico reenviado desde CoreServicesVnet | seleccionados                        |
| Nombre del vínculo de emparejamiento                             | `ManufacturingVnet-to-CoreServicesVnet` |
| Permitir que CoreServicesVNet acceda a la red virtual emparejada            | seleccionado (valor predeterminado)                       |
| Permitir que CoreServicesVNet reciba tráfico reenviado de la red virtual emparejada | seleccionados                       |
<img width="496" alt="03" src="https://github.com/user-attachments/assets/ae646b33-282e-4e8f-aad6-230cc3ac13bc" />
<img width="574" alt="04" src="https://github.com/user-attachments/assets/b0b33113-f290-427c-9a47-12d470d83493" />


1. En CoreServicesVnet | Emparejamientos, compruebe que se muestra el emparejamiento **CoreServicesVnet-to-ManufacturingVnet**. Actualice la página para asegurarse de que el **estado de emparejamiento** es **Conectado**.
<img width="913" alt="05" src="https://github.com/user-attachments/assets/b848451e-25ba-4183-aad0-c47aded4ea43" />

1. Cambie al **ManufacturingVnet** y compruebe que ** se muestra el emparejamiento ManufacturingVnet-to-CoreServicesVnet**. Asegúrese de que el **Estado de emparejamiento** es **Conectado**. Es posible que tenga que **actualizar** la página. 
<img width="942" alt="06" src="https://github.com/user-attachments/assets/59ebc32d-529a-4d33-8850-6b3ad9b1b0e1" />

## Tarea 5: Uso de Azure PowerShell para probar la conexión entre máquinas virtuales

En esta tarea, se vuelve a probar la conexión entre las máquinas virtuales de diferentes redes virtuales. 

### Comprobación de la dirección IP privada de CoreServicesVM

1. En Azure Portal, busque y seleccione la `CoreServicesVM` máquina virtual.

1. En la hoja **Información general**, en la sección **Redes**, registre la **dirección IP privada** del equipo. Necesita esta información para probar la conexión.
<img width="873" alt="07" src="https://github.com/user-attachments/assets/567d5f8e-fbc4-40fa-9338-52212034918a" />
   
### Pruebe la conexión a CoreServicesVM desde el **ManufacturingVM**.

1. Cambie a la `ManufacturingVM` máquina virtual.

1. En la hoja **Operations**, seleccione la hoja **Ejecutar comando**.
<img width="310" alt="09" src="https://github.com/user-attachments/assets/6e1fbd66-b83d-4116-b696-42a2e220609d" />

1. Seleccione **runPowerShellScript** y ejecute el comando **Test-NetConnection**. Asegúrese de usar la dirección IP privada del **CoreServicesVM**.

    ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
    ```
<img width="395" alt="10" src="https://github.com/user-attachments/assets/a587ddeb-295d-49fd-9008-f7ce33efb105" />
<img width="378" alt="11" src="https://github.com/user-attachments/assets/cc31e413-5335-4eef-b0bc-6819f01b5b0e" />

1. El script de comandos puede tardar un par de minutos en finalizar. En la parte superior de la página se muestra un mensaje informativo *Ejecución de script en curso*.

   
1. La conexión de prueba debe realizarse correctamente porque se ha configurado el emparejamiento. El nombre del equipo y la dirección remota de este gráfico pueden ser diferentes. 
   
<img width="275" alt="12" src="https://github.com/user-attachments/assets/66905958-ee58-489f-b3ed-b80ec32c7bb9" />


## Tarea 6: Creación de una ruta personalizada 

En esta tarea, desea controlar el tráfico de red entre la subred perimetral y la subred de servicios centrales internos. Se instalará una aplicación de red virtual en la subred de servicios principales y todo el tráfico debe enrutarse allí. 

1. Busque seleccionar `CoreServicesVnet`.

1. Seleccione **Subredes** y, a continuación, **+ Crear**. Asegúrese de **Guardar** los cambios. 

    | Configuración | Value | 
    | --- | --- |
    | Nombre | `perimeter` |
    | Intervalo de direcciones de subred | `10.0.1.0/24`  |

<img width="158" alt="01" src="https://github.com/user-attachments/assets/dc45b972-6f6c-413d-8581-15bd23e25df6" />
<img width="957" alt="02" src="https://github.com/user-attachments/assets/ed802bb4-b71e-43c0-87ae-16a30b2d7bbe" />

1. En Azure Portal, busque y seleccione `Route tables`y, a continuación, seleccione **Crear**. 

    | Configuración | Valor | 
    | --- | --- |
    | Suscripción | su suscripción |
    | Resource group | `az104-rg5`  |
    | Region | **Este de EE. UU.** |
    | Nombre | `rt-CoreServices` |
    | Propagar las rutas de la puerta de enlace | **No** |
   
<img width="334" alt="03" src="https://github.com/user-attachments/assets/bdfa97ba-0e25-4901-8c6c-07737f2c2f56" />
<img width="489" alt="04" src="https://github.com/user-attachments/assets/35f74299-2a18-4dfc-8630-e216a423c74c" />

1. Después de implementar la tabla de rutas, seleccione **Ir al recurso**.

1. Seleccione **Rutas** y después **Agregar**. Cree una ruta desde la aplicación virtual de red futura a la red virtual CoreServices. 

    | Configuración | Value | 
    | --- | --- |
    | Nombre de ruta | `PerimetertoCore` |
    | Tipo de destino | **Direcciones IP** |
    | Direcciones IP de destino | `10.0.0.0/16` (red virtual de servicios principales) |
    | Tipo de próximo salto | **Aplicación virtual** (observe las demás opciones) |
    | Siguiente dirección de salto | `10.0.1.7` (NVA futura) |
<img width="341" alt="05" src="https://github.com/user-attachments/assets/8463a566-a45a-46b3-bd02-a957e03c161a" />
<img width="345" alt="06" src="https://github.com/user-attachments/assets/01c2df23-656d-4373-a6ed-7a0fa21d5d08" />

1. Seleccione **+ Agregar** cuando se complete la ruta. Lo último que debe hacer es asociar la ruta a la subred.

1. Seleccione **Subredes** y, después, seleccione **Asociar**. Complete la configuración.

    | Configuración | Value | 
    | --- | --- |
    | Virtual network | **CoreServicesVnet** |
    | Subnet | **Principal** |    
<img width="319" alt="07" src="https://github.com/user-attachments/assets/280ef100-9b78-4e34-81c2-10ec482e7c36" />
<img width="349" alt="08" src="https://github.com/user-attachments/assets/c2882635-e941-423a-8854-f0d9f685824a" />

>**Nota**: Ha creado una ruta definida por el usuario para dirigir el tráfico desde la red perimetral a la nueva aplicación virtual de red.  

## Limpieza de los recursos

Si utiliza **su propia suscripción**, dedique un minuto a eliminar los recursos del laboratorio. De esta forma estará seguro de que los recursos se liberan y de que se minimiza el costo. La forma más fácil de eliminar los recursos de laboratorio es eliminar el grupo de recursos del laboratorio. 

+ En Azure Portal, seleccione el grupo de recursos, seleccione **Eliminar el grupo de recursos**, **Escribir el nombre del grupo de recursos** y, después, haga clic en **Eliminar**.
+ Mediante Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Mediante la CLI, `az group delete --name resourceGroupName`.

## Puntos clave

Enhorabuena por completar el laboratorio. Estas son las principales conclusiones de este laboratorio. 

+ De forma predeterminada, los recursos de diferentes redes virtuales no se pueden comunicar.
+ El emparejamiento de red virtual permite conectar sin problemas dos o más redes virtuales en Azure.
+ Las redes virtuales compartidas aparecen como una sola a efectos de conectividad.
+ El tráfico entre las máquinas virtuales de la red virtual emparejada usa la infraestructura de la red troncal de Microsoft.
+ Las rutas definidas por el sistema se crean automáticamente para cada subred de una red virtual. Las rutas definidas por el usuario invalidan o agregan a las rutas del sistema predeterminadas. 
+ Azure Network Watcher proporciona un conjunto de herramientas para supervisar, diagnosticar y ver métricas y registros de los recursos de IaaS de Azure.
