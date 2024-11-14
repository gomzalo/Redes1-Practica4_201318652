# MANUAL DE CONFIGURACIÓN
## Requisitos

* **GNS3**
Para esta practica se utilizará la versión 2.2.11.
![AcercaDeGNS.png](https://www.dropbox.com/s/ywpvuz7fudo65n5/AcercaDeGNS.png?dl=0&raw=1)

* **Imagen Router**
Se descarga la imagen del [router](https://ingenieriausacedu-my.sharepoint.com/:u:/g/personal/3003608000101_ingenieria_usac_edu_gt/Edt3tNlwlx5PrK4hHTVCpBwBdBbYaRe2zexDQJB4wg0Y2Q?e=Vsh7Ad) que utilizaremos en la practica, pues GNS3 no la trae.
![Router.png](https://www.dropbox.com/s/cjg84l62b4o3rru/Router.png?dl=0&raw=1)

* **Imagen EtherSwitch**
Se debe de descargar, también, la imagen del [etherswitch](https://ingenieriausacedu-my.sharepoint.com/personal/3003608000101_ingenieria_usac_edu_gt/_layouts/15/onedrive.aspx?id=%2Fpersonal%2F3003608000101%5Fingenieria%5Fusac%5Fedu%5Fgt%2FDocuments%2Fcompartida%2Fc3725%2Dadventerprisek9%2Dmz124%2D15%2Eimage&parent=%2Fpersonal%2F3003608000101%5Fingenieria%5Fusac%5Fedu%5Fgt%2FDocuments%2Fcompartida&originalPath=aHR0cHM6Ly9pbmdlbmllcmlhdXNhY2VkdS1teS5zaGFyZXBvaW50LmNvbS86dTovZy9wZXJzb25hbC8zMDAzNjA4MDAwMTAxX2luZ2VuaWVyaWFfdXNhY19lZHVfZ3QvRVdmbHNsNEZlQTlKazdrT1ByVG91OE1CUWhNOFEyckFYemROaHlQMkFtZFVHQT9ydGltZT1xT0d3My1SdjJFZw)
![r1p4_etherswithicon.png](https://www.dropbox.com/s/0spqgm6rvzpldk4/r1p4_etherswithicon.png?dl=0&raw=1)

* **ISO Tiny Core Linux**
También usaremos una máquina virtual para simular una fisica, en este caso utilizaremos [Tiny Core Linux](http://tinycorelinux.net/11.x/x86/release/CorePlus-current.iso). (en su ultima versión)
![tlinux.png](https://www.dropbox.com/s/nkn9ggm9tao1apt/tlinux.png?dl=0&raw=1)

### Creación del proyecto

Creamos el proyecto en GNS3, nuestro proyecto se llamara Redes1-Practica2_201318652.

![cracionp4r1.png](https://www.dropbox.com/s/37ryqq84ur08fq2/cracionp4r1.png?dl=0&raw=1)

## Diseño
 
Al crear el proyecto se nos muestra un espacio en blanco, como se ve en la imagen. 
 ![creacion.png](https://www.dropbox.com/s/klqkrccmuthhwsz/creacion.png?dl=0&raw=1)
Se debe de presionar el menu lateral izquierdo donde se encuentran los diferentes equipos que utilizaremos para nuestra topologia.

#### Infraestructura

Nuestra infraestructura estara compuesta por los siguientes elementos:
- 1 Router
- 4 EtherSwitches
- 2 Switches
- 4 Maquinas host (cliente)
    -  3 VPCs
    -  1 Virtual

#### Topología

##### VTP

Se configurara de la siguiente:

* Dominio y contraseña: redes1_<carnet>
En nuestro caso, quedaria de la siguiente manera:
    
    | Dominio   	| Contraseña           	|
    |------	|--------------	|
    | redes1_**201318652**   	| redes1_**201318652**           	|

Los VTP, serán un servidor y dos clientes.
Debiendo quedar de la siguiente manera: 

| EtherSwitch 	| Servidor 	| Cliente 	|
|-------	|------	|--------------	|
| ESW1   	| X   	|            	|
| ESW2   	|    	| X           	|
| ESW3   	|    	| X           	|

##### VLANs
Se configuraran los modos de acceso y trunk en los ESW1, ESW2, ESW3 que correspondan para garantizar el tráfico de VLAN.

Se configurara una topologia para una empresa, la cual tiene 2 VLANs definidas de la siguiente manera:

| Departamento | VLAN             |
|--------------|------------------|
| Ventas       | 10               |
| Contabilidad | 20               | 

Cada una de estas tendrá su propia subred para poder llevar una mejor gestión y control de cada departamento, pero también se necesitan que todas puedan comunicarse entre sí por medio de 1 router.


| HOSTS 	| VLAN 	| Virtualizada 	|
|-------	|------	|--------------	|
| PC1   	| 10   	| SI           	|
| PC2   	| 20   	| NO           	|
| PC3   	| 10   	| SI           	|
| PC4   	| 20   	| NO           	|

##### Port-Chanel

| Port-Channel | EtherSwitches             |
|--------------|------------------|
| Grupo 1       | ESW1 - ESW2               |
| Grupo 2       | ESW1 - ESW3               | 
| Grupo 3       | ESW2 - ESW3               | 

##### Centro de datos

Se tienen 2 redes, las que estaran representadas por las direcciones de red:  192.168.1**X**.0/24 y 192.168.2**X**.0/24.

Nota: La X representa el último dígito del número de carné.

Respecto a la nota anterior, el carné es 20131865**2**, por lo tanto el valor de X será:
* **X = 2**

Aplicando dichos valores a las IPs que se usaran, quedarian de la siguiente forma:

| VLAN | Dirección de red      | Primera dirección asignable | Última dirección asignable | Dirección de Broadcast |
|------|-----------------------|-----------------------------|----------------------------|------------------------|
| 10 | **192.168.12.0/24**   | 192.168.12.1                | 192.168.12.253              | 192.168.12.254         |
| 20 | **192.168.22.0/24**  | 192.168.22.1               | 192.168.2.253             | 192.168.22.254         |

##### Diagrama

La topología queda como se ve en la siguiente imagen.

![topologiar1p4.png](https://www.dropbox.com/s/cut24i1kh60hjq1/topologiar1p4.png?dl=0&raw=1)

### Conexión
* ESW1 se conecta al router.
* ESW1, ESW2 y ESW3 se conectan entre ellos.
* Virtual y VPC2 se conecta a SW1.
* VPC2 y VPC3 se conectan a SW2.

## Asignación de IPs

Luego de haber finalizado con la conexión y el etiquetado de las IPs correspondientes a cada nodo en la topología, se procedera a asignar estas a cada equipo y a *levantar* los equipos.

##### Router
El principal nodo, conector de nuestra red, contiene dos interfaces, de estas solo una va conectada a un etherswitch.
Para configurar este dispositivo, y su interfaz, debemos seguir los siguientes pasos.

1. Iniciar el dispositivo
![IniciarRouter.png](https://www.dropbox.com/s/gva3wqxcohc3d49/IniciarRouter.png?dl=0&raw=1)0

    *   Una vez el dispositivo este *corriendo*, se cambiara el color de la conexión a verde.
    ![RouterCorriendo.png](https://www.dropbox.com/s/j4clbftt7x4k13l/RouterCorriendo.png?dl=0&raw=1)

2. Configurar el dispositivo
Ahora toca asignar la ip, daremos click derecho sobre el router y seleccionaremos la opción que dice *Console*, para acceder a la consola que nos permitira ejecutar los comandos necesarios para llevar a cabo esta acción.
![RouterConsole.png](https://www.dropbox.com/s/9jx0792h31z3l61/RouterConsole.png?dl=0&raw=1)
    * Se nos abrira una sesión en consola de Putty, como se puede observar.
    ![RouterPutty.png](https://www.dropbox.com/s/tj6zs1guthjkdjz/RouterPutty.png?dl=0&raw=1)
    * Presionamos enter, para poder iniciar la configuración. A continuación, usaremos el siguiente comando para ver el estado de las interfaces.
        ```sh
        $ sh ip int brief
        ```
        ![Routershipintbrief.png](https://www.dropbox.com/s/4w7u79c8unw5455/Routershipintbrief.png?dl=0&raw=1)
        Al no haber configurado ninguna, nos aparecera como la imagen superior.
        
    * Escribiremos el siguiente comando para inciar la configuración de las interfaces

        ```sh
        $ conf t
        ```

    * Empezamos por configurar la interfaz FastEthernet0/0, con el siguiente comando:
        ```sh
        conf t
        int f0/0
        no shut
        end
        wr
        ```
        ![RouterConfIntF0.png](https://www.dropbox.com/s/jbb0vekihopfdfx/RouterConfIntF0.png?dl=0&raw=1)

    * Ya que en esta practica se trabajara con subinterfaces, se configurara con un comando diferente al que se usa para asignar IP a una interfaz.
    
        ```sh
        $ ip address <IP> <Mascara de red>
        ```
        El siguiente comando, nos creará una subinterfaz.
        ```sh
        conf t
        int <interfaz>.<subinterfaz>
        encapsulation dot1q <subinterfaz>
        ip address <Broadcast> <Mascara de red>
        end
        wr
        ```
        En nuestro caso, para crear la subinterfaz utilizada para la VLAN 10; seria la siguiente:
        ```sh
        conf t
        int f0/0.10
        encapsulation dot1q 10
        ip address 192.168.12.254 255.255.255.0
        end
        wr
        ```
        
        **Para la subinterfaz dos, es exactamente el mismo comando, pero con la diferencia que seria subinterfaz "20", con ip: 192.168.22.254 255.255.255.0**
     
     *  Por ultimo, se verifica el estado de las interfaces. Se puede observar que los cambios han sido guardados y aplicados correctamente.
    ![r1p4_subinterfaces.png](https://www.dropbox.com/s/vf7u4ta57f3cp5k/r1p4_subinterfaces.png?dl=0&raw=1)

##### EtherSwitches

En estos dispositivos se deben de conectar como se observo en la topologia, y se debe de configurar el port-channel en los ESW indicados. Para tal proposito, se utilizarán los siguientes comandos:
* Para crear un port-channel entre un ESWX hacia un ESWY, el comando requerido es el siguietne:
    
    ```sh
    conf t
    int range <interfaz>/<rango interfaz>
    channel-group # mode on
    end
    wr
    ```
* En nuestro caso, seria de la siguiente manera, para crear el port-channel entre los ESW1-ESW2.

    ```sh
    conf t
    int range f1/0-1
    channel-group 1 mode on
    end
    wr
    ```
    **Para los otros 2 port-channels es el mismo comando, solo se debe de tener cuidado de colocar las interfaces correctas que conectan cada par de ESW**

* Con el siguiente comando se verifica que esten creados los port-channel.
    ```sh
    sh int port-channel #
    ```

![r1p4_shportchannel1.png](https://www.dropbox.com/s/po0g641w4nvr0f1/r1p4_shportchannel1.png?dl=0&raw=1)
Se observa, desde el ESW2 que el port-channel 1 utiliza las interfaces que se definieron anteriormente.

###### VTP
Ademas de los port-channel, tambien se debe configuirar la VTP en los ESW.

```sh
conf t
vtp domain <vtp_name>
vtp password <vtp_password>
vtp mode <client/server>
end
wr
```

En nuestro caso, seria de la siguiente manera:
```sh
conf t
vtp domain redes1_201318652
vtp password redes1_201318652
vtp mode <client/server>
end
wr
```
Este comando se corre en todos los ESW, siendo el ESW1 el server y los otros 2 client.
    
* Con el siguiente comando se verifica que se haya configurado la VTP correctamente
 
```sh
sh vtp status
```

![r1p4_shportchannel1.png](https://www.dropbox.com/s/po0g641w4nvr0f1/r1p4_shportchannel1.png?dl=0&raw=1)
Se observa que en el ESW2 se aplico correctamente el nombre, no se muestra la password por seguridad y se puede observar que es un VTP cliente.

###### VLAN

* Las VLAN se configuran con el siguiente comando:
    ```sh
    conf t
    vlan #
    name <nombre_VLAN>
    end
    wr
    ```
    
* En nuestro caso, para configurar la VLAN 10, se utiliza el sigueinte comando.
El cual se configura solamente en el ESW1, pues es el que replicara a los ESW clientes y los SW.
    
    ```sh
    conf t
    vlan 10
    name VENTAS
    end
    wr
    ```

* Con el siguiente comando se verifica que se haya configurado la VLAN correctamente
 
```sh
sh vlan-sw
```

![r1p4_shvlansw.png](https://www.dropbox.com/s/nu8p5xzkf43v5yz/r1p4_shvlansw.png?dl=0&raw=1)
Se observa que en el ESW2 se están replicando ambas VLANs creadas, correctamente.

##### Switches
Estos dispositivos al no guardar mayor información, su función en esta topología es replicar las conexiones entrantes. No necesita configuración alguna.
Esto quiere decir, que dependiendo a la interfaz a la que se conecten esas seran las IPs que regiran las maquinas que se conecten a estos.

Aun asi, en esta ocasión se debe configurar el modo trunk y access de los puertos de las interfaces que utilizaremos. Para esto, utilizaremos la interfaz de GNS3 por cuestiones de facilidad.

![r1p4_swtrunkaccess.png](https://www.dropbox.com/s/mhhi5z4jhzp8712/r1p4_swtrunkaccess.png?dl=0&raw=1)

##### VPCs
Estos dispositivos emulan una maquina, solamente corriendo en consola (por ahorro de recursos).
Para su configuración se deben seguir los siguientes pasos:

1. Iniciar el dispositivo
Al igual que con el router, al hacerlo la conexión se tornara verde.
![VPCIniciar.png](https://www.dropbox.com/s/xn5c5q0fjl4im8p/VPCIniciar.png?dl=0&raw=1)
2. Configurar el dispositivo
Para asignar la ip de este dispositivo, de igual forma, se presiona click derecho y se selecciona la opción *Console*.
![VPCConsole.png](https://www.dropbox.com/s/si4sdgl6bu00rip/VPCConsole.png?dl=0&raw=1)
    * Al hacer esto se nos abrira una consola, mostrando la IP que tiene asignada actualmente la VPC.
    ![VPCMenuConsole.png](https://www.dropbox.com/s/kjvzc3av5oy188c/VPCMenuConsole.png?dl=0&raw=1)
    (En la imagen ya se le asigno la IP requerida para esta practica a la VPC1)
    * Para asignar la IP se debe usar el siguiente comando:
        ```sh
        $ ip <IP>/<Tamaño mascara> <IP Mascara>
        ```
      En nuestro caso, seria la siguiente:
        ```sh
        $ ip 192.168.12.10/24 192.168.12.254
        ```
    * Para guardar los cambios en la VPC se usa el siguiente comando:
        ```sh
        $ save
        ```
        ![VPCSave.png](https://www.dropbox.com/s/vshppmzstgrdx9p/VPCSave.png?dl=0&raw=1)
    * Por ultimo para ver el estado de la IP, se usara el siguiente comando. Como se puede observar, se ha aplicado y guardado correctamente la IP que se requiria.
        ```sh
        $ sh ip
        ```
        
        ![r1p4_vpc1_ship.png](https://www.dropbox.com/s/panrgy9il3iuyxm/r1p4_vpc1_ship.png?dl=0&raw=1)
        
         **La configuración para las VPC2 y VPC3 es exactamente los mismos pasos con la diferencia de que sus direcciones IP, variaran**
         
##### Linux Virtual
Para la configuración de la IP de esta maquina virtual, se utiliza VMWare Workstation como programa para *levantar* dicha maquina.
* Dando por hecho que la instalación de la maquina ya se ha realizado, hay una configuración extra que debe hacerse para que funcione dicha maquina.
    *  **VMWare**
        1. Ir al menu *edit* y seleccionar la opción *Virtual Network Editor...*
        ![EditVMW.png](https://www.dropbox.com/s/skne2lx7s9e9rrg/EditVMW.png?dl=0&raw=1)
        2. A continuación se mostrara una ventana, elegir la opción *Add a New Network*, como host only.
        ![VMWAddNet.png](https://www.dropbox.com/s/cfa1lm3cnwiowpj/VMWAddNet.png?dl=0&raw=1)
        3. En la configuración de la red que usara la maquina virtual debemos elegir la nueva red virtual recién creada.
        ![VMSett.png](https://www.dropbox.com/s/jh87uvihwwuuml4/VMSett.png?dl=0&raw=1)
    * **GNS3**
        1. Ir al menu *edit* y seleccionar la opción *prefferences*.
        ![GNSedit.png](https://www.dropbox.com/s/uvzifj0mpekt0bk/GNSedit.png?dl=0&raw=1)
        2. En el menu izquierdo, seleccionar *VMware* y en la pestaña *Advanced local settings* presionar *configure*.
        ![GNSVMConf.png](https://www.dropbox.com/s/pkziez1voyje7rb/GNSVMConf.png?dl=0&raw=1)
        3. En el menu izquierdo, seleccionar *VMware VMs* y elegir nuestra maquina virtual.
        ![GNSVM.png](https://www.dropbox.com/s/rlsjnysw95wsf4o/GNSVM.png?dl=0&raw=1)
        4. Una vez finalizado, seleccionamos el icono llamado *Reload all nodes*, para aplicar las nuevas configuraciones.
        ![GNSReload.png](https://www.dropbox.com/s/2ms7m3frpqxzdvl/GNSReload.png?dl=0&raw=1)
Una vez hecho esto, procedemos a asignar la IP.
1. Iniciar el dispositivo.
Al igual que con los otros dispositivos, se presiona click derecho y se elige la opción *Start*.
![VMStart.png](https://www.dropbox.com/s/3ddyzuhuo0bs0ko/VMStart.png?dl=0&raw=1)
Una vez iniciado, la conexión debera mostrarse verde.
2. Configurar la IP.
A diferencia de los otros dispositivos, esto lo haremos desde la maquina virtual en si.
    *   Estando en Tiny Linux, se abre el panel de control desde el dock de aplicaciones.
    ![VMPanel.png](https://www.dropbox.com/s/zqrfpfzpp5ip8hz/VMPanel.png?dl=0&raw=1)
    * Se elige la opción *Network* y colocamos la IP que se requiere en esta topología.
    ![ipvirttlp2.png](https://www.dropbox.com/s/ohlkqxeinc87c7q/ipvirttlp2.png?dl=0&raw=1)
    Se aplican los cambios y se sale.
   
## Pruebas
El siguiente comando se utiliza para comunicarse con otra maquina dentro de la red.
```sh
$ ping <IP>
```
![r1p4_vpc1_pingvpc3.png](https://www.dropbox.com/s/rcwaccc7nu3lu9t/r1p4_vpc1_pingvpc3.png?dl=0&raw=1)

## ESW root

Para verificar si un ESW es root o no, se utiliza el siguiente comando.
```sh
sh sp root
```

![r1p4_esw2_shsproot.png](https://www.dropbox.com/s/jcbi3ngmjj33h0w/r1p4_esw2_shsproot.png?dl=0&raw=1)
Se observa, que no indica que sea root.
![r1p4_esw1_shsproot.png](https://www.dropbox.com/s/h0yjp8tpe0lddpq/r1p4_esw1_shsproot.png?dl=0&raw=1)
Contrario al ESW1, se puede ver que si es el root.

## Captura de paquetes

![r1p4_wireshark.png](https://www.dropbox.com/s/e31035hnyygvfp5/r1p4_wireshark.png?dl=0&raw=1)

## Ruta principal

![Inkedtopologiar1p4 - copia_LIrutoa.jpg](https://www.dropbox.com/s/gpfhvkz5rdk1n7h/Inkedtopologiar1p4%20-%20copia_LIrutoa.jpg?dl=0&raw=1)

****
# GLOSARIO

* **ADDRESS**
Es la dirección IP que se le asigna a un dispositivo dentro de una red.
* **DHCP**
*Dynamic Host Configuration Protocol* es un protocolo usado por el protoco IP en redes, donde un servidor DHCP asigna una IP dinamicamente a otros en la red.
* **INTERFACE**
Componente que conecta un dispositivo a la red.
* **IP**
*Internet Protocol* es el protocolo principal en que se basa la internet, su funcion permite la comunicación en red.
* **MASK**
Combinación de bits que sirve para delimitar el ambito de una red-
* **PING**
Comando que permite hacer una verificación del estado de una determinada conexi´n de un host local con un equipo remoto en una red TCP/IP.
* **PUERTO**
Es un punto de comunicacion.
* **ROUTER**
Dispositivo de red que envia paquetes de datos entre redes de computadoras.
* **SWITCH**
Conmutador, dispositivo de conexión usado para conectar equipos en red, formando lo que se conoce como red de area loca *LAN* siguiendo el estandar Ethernet.
* **VPC**
*Virtual PC Simulator*, permite simular una PC ligera, con soporte para DHCP que apenas consume 2MB de RAM.