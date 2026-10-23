\# TechNova S.A. — VLANs, Trunking y Router-on-a-Stick



\## 1. Descripción del laboratorio



Laboratorio de redes empresariales desarrollado en Cisco Packet Tracer para practicar la segmentación de una LAN mediante VLANs, enlaces troncales IEEE 802.1Q y enrutamiento entre VLANs mediante Router-on-a-Stick.



\*\*Empresa ficticia:\*\* TechNova S.A.  

\*\*Tipo de laboratorio:\*\* Switching y routing inter-VLAN  

\*\*Herramienta:\*\* Cisco Packet Tracer



\## 2. Objetivos



\- Crear y configurar VLANs para separar departamentos.

\- Asignar puertos de acceso a sus VLAN correspondientes.

\- Configurar enlaces trunk entre switches y router.

\- Implementar subinterfaces en el router para el enrutamiento inter-VLAN.

\- Verificar la conectividad dentro de una VLAN y entre VLANs.

\- Documentar la configuración y los resultados para su reproducción.



\## 3. Topología



Dispositivos utilizados:



\- 1 router Cisco 2911: R1.

\- 2 switches Cisco 2960: S1 y S2.

\- 13 computadoras.

\- 3 VLANs departamentales.



Conexiones principales:



\- R1 G0/0 ↔ S1 Gi0/1: enlace trunk.

\- S1 Fa0/24 ↔ S2 Fa0/24: enlace trunk.



El router utiliza subinterfaces sobre G0/0 para proporcionar la puerta de enlace de cada VLAN.



\## 4. Diseño de VLANs y direccionamiento IP



| Departamento | VLAN | Red | Gateway |

|---|---:|---|---|

| Administración | 10 | 192.168.10.0/24 | 192.168.10.1 |

| Sistemas | 20 | 192.168.20.0/24 | 192.168.20.1 |

| Recursos Humanos | 30 | 192.168.30.0/24 | 192.168.30.1 |



\*\*Máscara de subred:\*\* 255.255.255.0



\### Asignación de computadoras



| PC | Switch | VLAN | Dirección IP |

|---|---|---:|---|

| PC0 | S1 | 10 | 192.168.10.10 |

| PC1 | S1 | 10 | 192.168.10.11 |

| PC2 | S1 | 20 | 192.168.20.10 |

| PC3 | S1 | 20 | 192.168.20.11 |

| PC4 | S1 | 20 | 192.168.20.12 |

| PC5 | S1 | 30 | 192.168.30.10 |

| PC6 | S1 | 30 | 192.168.30.11 |

| PC7 | S2 | 10 | 192.168.10.12 |

| PC8 | S2 | 10 | 192.168.10.13 |

| PC9 | S2 | 20 | 192.168.20.13 |

| PC10 | S2 | 20 | 192.168.20.14 |

| PC11 | S2 | 20 | 192.168.20.15 |

| PC12 | S2 | 30 | 192.168.30.12 |



Cada PC utiliza la máscara `255.255.255.0` y el gateway correspondiente a su VLAN.



\## 5. Configuración de los switches



\### Creación de VLANs



Ejecutar en S1 y S2:



```text

enable

configure terminal

vlan 10

&#x20;name ADMINISTRACION

vlan 20

&#x20;name SISTEMAS

vlan 30

&#x20;name RECURSOS\_HUMANOS

end

```



\### Puertos de acceso de S1



```text

configure terminal

interface range fa0/1-2

&#x20;switchport mode access

&#x20;switchport access vlan 10

exit

interface range fa0/3-5

&#x20;switchport mode access

&#x20;switchport access vlan 20

exit

interface range fa0/6-7

&#x20;switchport mode access

&#x20;switchport access vlan 30

end

```



\### Puertos de acceso de S2



```text

configure terminal

interface range fa0/1-2

&#x20;switchport mode access

&#x20;switchport access vlan 10

exit

interface range fa0/3-5

&#x20;switchport mode access

&#x20;switchport access vlan 20

exit

interface fa0/6

&#x20;switchport mode access

&#x20;switchport access vlan 30

end

```



\### Trunk entre S1 y S2



En S1:



```text

configure terminal

interface fa0/24

&#x20;switchport mode trunk

&#x20;switchport trunk allowed vlan 10,20,30

end

```



En S2:



```text

configure terminal

interface fa0/24

&#x20;switchport mode trunk

&#x20;switchport trunk allowed vlan 10,20,30

end

```



\### Trunk entre S1 y R1



En S1:



```text

configure terminal

interface gi0/1

&#x20;switchport mode trunk

&#x20;switchport trunk allowed vlan 10,20,30

&#x20;no shutdown

end

```



\*\*Nota:\*\* los comandos exactos disponibles pueden variar según el modelo y la versión de Packet Tracer. Si un switch rechaza un comando, comprobar primero la compatibilidad con `?`.



\## 6. Configuración de Router-on-a-Stick



En R1, la interfaz física G0/0 conecta con S1 Gi0/1.



```text

enable

configure terminal

interface gigabitEthernet0/0

&#x20;no ip address

&#x20;no shutdown

exit



interface gigabitEthernet0/0.10

&#x20;encapsulation dot1Q 10

&#x20;ip address 192.168.10.1 255.255.255.0

exit



interface gigabitEthernet0/0.20

&#x20;encapsulation dot1Q 20

&#x20;ip address 192.168.20.1 255.255.255.0

exit



interface gigabitEthernet0/0.30

&#x20;encapsulation dot1Q 30

&#x20;ip address 192.168.30.1 255.255.255.0

exit



end

```



Las subinterfaces permiten que el router reciba tráfico etiquetado de las tres VLANs y enrute paquetes entre sus redes.



\## 7. Verificación y pruebas



\### Comandos de verificación



En los switches:



```text

show vlan brief

show interfaces trunk

```



En el router:



```text

show ip interface brief

show running-config

```



En cada PC, verificar la dirección IP, la máscara y el gateway mediante la configuración IP de Packet Tracer.



\### Resultados observados



| Prueba | Resultado |

|---|---|

| PC0 → PC8, misma VLAN 10 | Exitosa |

| PC2 → PC11, misma VLAN 20 | Exitosa |

| PC5 → PC12, misma VLAN 30 | Exitosa |

| Comunicación entre VLANs a través de R1 | Exitosa |



Las pruebas de conectividad se realizaron con `ping`. En ocasiones, el primer intento presentó pérdida de paquetes y las pruebas posteriores respondieron correctamente, posiblemente debido a la resolución inicial de ARP.



\## 8. Problema encontrado y solución



\*\*Problema:\*\* el enlace entre el switch S1 y el router no estaba funcionando inicialmente como enlace troncal.



\*\*Diagnóstico:\*\* se revisó el estado de los puertos y la configuración de trunk.



\*\*Solución:\*\* se configuró S1 Gi0/1 en modo trunk, permitiendo las VLANs 10, 20 y 30, y se comprobó que las VLANs estuvieran activas y en estado de reenvío.



\*\*Verificación:\*\* `show interfaces trunk` confirmó que S1 Fa0/24 y Gi0/1 transportaban las VLANs configuradas.



\## 9. Conceptos aprendidos



\- Segmentación lógica de una LAN mediante VLANs.

\- Diferencias entre puertos access y trunk.

\- Etiquetado de tramas mediante IEEE 802.1Q.

\- Enrutamiento inter-VLAN con subinterfaces.

\- Uso de gateways por departamento.

\- Diagnóstico mediante comandos de verificación y pruebas `ping`.



\## 10. Archivos del laboratorio



\- `TechNova-VLAN-Trunk-Router-on-a-Stick.pkt`: archivo de Cisco Packet Tracer.



\## 11. Mejoras futuras



\- Implementar ACLs para controlar la comunicación entre departamentos.

\- Añadir DHCP para asignar direcciones automáticamente.

\- Configurar seguridad de puertos en los switches.

\- Incorporar redundancia y analizar STP.

\- Añadir capturas de pantalla como evidencias de configuración y conectividad.



\## 12. Autorización y alcance



Laboratorio educativo realizado en Cisco Packet Tracer con una topología ficticia. Las configuraciones y pruebas se limitan al entorno de simulación.



