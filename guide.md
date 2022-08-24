# Quagga-en-Ubuntu ⚙️

##  🔧 Intalación del servicio de Quagga para Ubuntu LTS 16.04

El archivo .txt adjunto contiene un instructivo para instalar Quagga en el S.O. Ubuntu LTS 16.04 de acuerdo a los requerimientos del sistema.

* **Para iniciar:**

	    sudo apt-get update

* **Instalar quagga con:** 

	    sudo apt-get install quagga
  
* **Configuarar el "demonio" para activar "zebra" y "ripd":**

	    # nano /etc/quagga/daemons

* **Modificar los valores de zebra y ripd a "yes" como se muestra a continuación:**

	  zebra=yes
  
	  bgpd=no
  
	  ospfd=no
  
	  ospf6d=no
  
	  ripd=yes
  
	  ripngd=no
  
	  isisd=no
  
	  babeld=no
	
  
	Guardar con ctrl+o
  
	Enter (para aceptar)
  
	Salir con ctrl+x
  
* **Copiar los archivos de zebra y ripd a /etc/quagga con los siguientes comandos:**

	  # cp /usr/share/doc/quagga/examples/zebra.conf.sample /etc/quagga/zebra.conf

	  # cp /usr/share/doc/quagga/examples/ripd.conf.sample /etc/quagga/ripd.conf


* **Dar permisos para que se puedan ejecutar los servicios de zebra y ripd con:**

	  # sudo chown quagga:quagga /etc/quagga/*.conf

	  # sudo chmod 640 /etc/quagga/*.conf


* **Iniciar el servicio de quagga con:**
	
	  # sudo /etc/init.d/quagga start

_NOTA: Debe aparecer una señal color verde, esto indica que todo ha salido bien. Si algo sale mal la señal será de color rojo._

* ** Cambiar al directorio /etc/network con:**

	      # cd /etc/network


* **Ejecutar el siguiente comando en una nueva linea de comandos (consola) para averiguar el nombre de la interfaz de red física del equipo:**

	      # ifconfig

	Aparecerán múltiples líneas con información, del lado izquierdo habrá diferentes nombres como "enp1s0", "lo" y "wlp1s0" pero para efectos de esta práctica solo se considerará en el nombre de la tarjeta de red (eth0, enp1s0, enp2s0, enp3s0, ect).

	_NOTA: si no sirve este comando, entonces se tienen que instalar las herramientas de red con:_

      	# sudo apt install net-tools

	Cerrar consola actual y volver a la anterior.


* **Editar el archivo "Interfaces" con nano para crear interfaces de red virtuales que ayudarán a  segmentar una red en diferentes sitios (pueden crearse las que se crean necesarias):**
 
	       # nano interfaces


* **Comentar cada una de las líneas que hay en el archivo anteponiendo un "#" en cada una de ellas, ejemplo:**

	      # interfaces(5) file used by ifup(8) and ifdown(8)
	      # auto lo
	      # iface lo inet loopback


* **Crear la interfaz principal de la tarjeta real siguiendo el siguiente orden:**

	  auto enp1s0
  
          iface enp1s0 inet static
        
          address 192.168.0.254
        
          netmask 255.255.255.0
          
* **Crear _n_ tarjetas virtuales bajo el siguiente formato:**

        auto enp1s0:101      
              iface enp1s0:101 inet static            
              address 192.168.1.254            
              netmask 255.255.255.0


              auto enp1s0:102            
              iface enp1s0:102 inet static            
              address 192.168.2.254            
              netmask 255.255.255.0


              auto enp1s0:103            
              iface enp1s0:103 inet static            
              address 192.168.3.254            
              netmask 255.255.255.0

           ... n

	Guardar con ctrl+o
  
	Enter (para aceptar)
  
	Salir con ctrl+x
  

* **Reiniciar el servicio "networking" para que pueda admitir las nuevas interfaces:**

	  # /etc/init.d/networking restart


* **Ejecutar ifconfig para apreciar los cambios y ver las interfaces de red que se crearon:**

    Resultado ifconfig:

               enp1s0    Link encap:Ethernet  direcciónHW 10:7d:1a:11:e3:c5  
                        Direc. inet:192.168.0.254  Difus.:192.168.0.255  Másc:255.255.255.0
                        ACTIVO DIFUSIÓN MULTICAST  MTU:1500  Métrica:1
                        Paquetes RX:94 errores:0 perdidos:0 overruns:0 frame:0
                        Paquetes TX:187 errores:0 perdidos:0 overruns:0 carrier:0
                        colisiones:0 long.colaTX:1000 
                        Bytes RX:10391 (10.3 KB)  TX bytes:24538 (24.5 KB)

                enp1s0:101 Link encap:Ethernet  direcciónHW 10:7d:1a:11:e3:c5  
                        Direc. inet:192.168.1.254  Difus.:192.168.1.255  Másc:255.255.255.0
                        ACTIVO DIFUSIÓN MULTICAST  MTU:1500  Métrica:1

                enp1s0:102 Link encap:Ethernet  direcciónHW 10:7d:1a:11:e3:c5  
                        Direc. inet:192.168.2.254  Difus.:192.168.2.255  Másc:255.255.255.0
                        ACTIVO DIFUSIÓN MULTICAST  MTU:1500  Métrica:1

                enp1s0:103 Link encap:Ethernet  direcciónHW 10:7d:1a:11:e3:c5  
                        Direc. inet:192.168.3.254  Difus.:192.168.3.255  Másc:255.255.255.0
                        ACTIVO DIFUSIÓN MULTICAST  MTU:1500  Métrica:1

          lo        Link encap:Bucle local  
                        Direc. inet:127.0.0.1  Másc:255.0.0.0
                        Dirección inet6: ::1/128 Alcance:Anfitrión
                        ACTIVO BUCLE FUNCIONANDO  MTU:65536  Métrica:1
                        Paquetes RX:5524 errores:0 perdidos:0 overruns:0 frame:0
                        Paquetes TX:5524 errores:0 perdidos:0 overruns:0 carrier:0
                        colisiones:0 long.colaTX:1000 
                        Bytes RX:899778 (899.7 KB)  TX bytes:899778 (899.7 KB)

                wlp1s0    Link encap:Ethernet  direcciónHW f8:da:0c:71:6a:fb  
                        Direc. inet:172.16.18.81  Difus.:172.16.31.255  Másc:255.255.240.0
                        Dirección inet6: fe80::fa28:2838:c93:7912/64 Alcance:Enlace
                        ACTIVO DIFUSIÓN FUNCIONANDO MULTICAST  MTU:1500  Métrica:1
                        Paquetes RX:4008560 errores:0 perdidos:1 overruns:0 frame:0
                        Paquetes TX:355318 errores:0 perdidos:0 overruns:0 carrier:0
                        colisiones:0 long.colaTX:1000 
                        Bytes RX:1065649899 (1.0 GB)  TX bytes:38482937 (38.4 MB)


* **Dar de alta las interfaces en los servicios de quagga para que haga el enrutamiento:**

	  # sudo /etc/init.d/quagga restart

_NOTA: Debe aparecer una señal color verde, esto indica que todo ha salido bien. Si algo sale mal la señal será de color rojo._

* **Acceder a zebra para dar de alta la red principal con: **
	
	    # telnet localhost 2601

	Contraseña: _zebra_
  
	    # Router> enable
      
	Contraseña: _zebra_


* **Entrar en modo configuración:**

	    # Router# config terminal

	
* **Dar de alta la interfaz de red real:**

      # Router(config)# interface enp1s0

      # Router(config-if)# ip address 192.168.0.254/24

      # Router(config-if)# no shutdown

	Y salir con:

      # Router(config-if)# exit

      # Router(config)# exit


 * **Guardar cambios con:**

	    # Router# write


* **Salir de zebra con:**

	    # Router# exit


* **Acceder a ripd para dar de alta todos los bloques de esta red:**

 	    # telnet localhost 2602

	Contraseña: _zebra_
  
      # ripd> enable
      # ripd# config terminal


* **Escribir 'router rip' para poder asignar los diferentes sitios de nuestra restart:**

	    # ripd(config)# router rip

* **Dar de alta todas nuestras interfaces, haciendo referencia a la ip cero:**

      # ripd(config-router)# network 192.168.0.0/24
      # ripd(config-router)# network 192.168.1.0/24
      # ripd(config-router)# network 192.168.2.0/24
      # ripd(config-router)# network 192.168.3.0/24
      # ripd(config-router)# network 192.168.4.0/24
      # ripd(config-router)# network 192.168.5.0/24
      # ripd(config-router)# network 192.168.6.0/24
      # ripd(config-router)# network 192.168.7.0/24
      # ripd(config-router)# network 192.168.8.0/24
      # ripd(config-router)# network 192.168.9.0/24
      # ripd(config-router)# network 192.168.10.0/24

* **Ejecutar exit dos veces:**

      # ripd(config-router)# exit
      # ripd(config)# exit


* **Guardar cambios escribiendo write:**

	    # ripd# write

	Y exit de nuevo para salir del ripd.


* **Configurar nuestra tarjeta de red de ethernet:**

	    # nano /etc/network/interfaces


* **Reiniciar nuestro servicio de red:**

	    # sudo /etc/init.d/networking restart


* **Activar el enrutamiento con:**

		    echo "1" > /proc/sys/net/ipv4/ip_forward


* **Escribir la configuración con:**

		    echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf





