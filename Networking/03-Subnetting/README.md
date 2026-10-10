# Subnetting IPv4 

## Descripción
Es dividir una red grande en pequeñas subredes.

## Ipv4 y CDIR.
- Una Ipv4 tiene 32 bits.
- Tiene 4 octetos.
- Cada octeto tiene 8 bits.
- /24 es el prefijo CIDR que indica 24 bits de red y 8 de host, permitiendo 256 direcciones totales.

## Máscaras y tamaños de subred
| CIDR | Bits de Hosts | Máscara decimal| Direcciones totales | Subredes | Máscara en binario |
|---|---|---|---|---|---|
| /24 | 8 | 255.255.255.0 | 256 direcciones | 1 | 11111111.11111111.11111111.00000000 |
| /25 | 7 | 255.255.255.128 | 128 direcciones | 2 | 11111111.11111111.11111111.10000000 |
| /26 | 6 | 255.255.255.192 | 64 direcciones | 4 | 11111111.11111111.11111111.11000000 |
| /27 | 5 | 255.255.255.224 | 32 direcciones | 8 | 11111111.11111111.11111111.11100000 |
| /28 | 4 | 255.255.255.240 | 16 direcciones | 16 | 11111111.11111111.11111111.11110000 |
| /29 | 3 | 255.255.255.248 | 8 direcciones | 32 | 11111111.11111111.11111111.11111000 |
| /30 | 2 | 255.255.255.252 | 4 direcciones | 64 | 11111111.11111111.11111111.11111100 |

## Hosts utilizables
Los hosts se calculan con 2^n - 2, n representa los bits de host.

## Tamaño del bloque

### Ejemplo /27
/27 → 255.255.255.224
256 - 224 = 32
32 - 27 = 5
2^5 = 32
32 -2 = 30 host utilizables ya que uno se utiliza para Network y otr para Broadcast.
- 0 - 31
- 32 - 63 
- 64 - 95
- 96 - 127
- 128 - 159
- 160 - 191 
- 192 - 223
- 224 - 255
  
### Ejercicio 1 Network, Broadcast y Hosts
Calcula: 
192.168.10.50/27  

32 - 27 = 5
2^5 - 2= 30  Host utlizables  
- Tamaño del bloque: 32 direcciones
- Network Address: 192.168.10.32    Es la primera red 
- Broadcast Address: 192.168.10.63  Es la ultima red 
- Rango de hosts utilizables: 192.168.10.33  --  192.168.10.62  

### Ejercicio 2 Network, Broadcast y Hosts
Calcula: 
192.168.10.140/26  

32 - 26 = 6
2^5 - 2= 62  Host utlizables  
- Máscara decimal: 255.255.255.192
- Tamaño del bloque: 64 host
- ¿En qué bloque cae .140?: Cae en el tercero.
- Network Address: 192.168.10.128
- Broadcast Address: 192.168.10.191
- Primer host: 192.168.10.129
- Último host: 192.168.10.190
- Hosts utilizables: 62 Host utilizables

## FLSM (Máscara de Subred de Longitud Fija)

### Ejercicio 4 — Subredes FLSM

Red:
192.168.50.0/24

Necesitamos:
12 subredes

- Bits prestados: 4
- Nuevo prefijo: /28
- Máscara decimal: 255.255.255.240
- Subredes posibles: 16
- Bits de host: 4
- Direcciones por subred: 16
- Hosts utilizables: 14
- Tamaño del bloque: 16

Subredes:

| Subred | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|
| 1 | 192.168.50.0 | 192.168.50.1 | 192.168.50.14 | 192.168.50.15 |
| 2 | 192.168.50.16 | 192.168.50.17 | 192.168.50.30 | 192.168.50.31 |
| 3 | 192.168.50.32 | 192.168.50.33 | 192.168.50.46 | 192.168.50.47 |
| 4 | 192.168.50.48 | 192.168.50.49 | 192.168.50.62 | 192.168.50.63 |
| 5 | 192.168.50.64 | 192.168.50.65 | 192.168.50.78 | 192.168.50.79 |
| 6 | 192.168.50.80 | 192.168.50.81 | 192.168.50.94 | 192.168.50.95 |


### Ejercicio 5 - VLSM
Red:

192.168.100.0/24

Necesitamos:

- Administración: 50 hosts
- Desarrollo: 25 hosts
- Servidores: 10 hosts
- WAN: 2 hosts

- Administración: 6 bits de host
- Desarrollo: 5 bits de host
- Servidores: 4 bits de host
- WAN: 2 bits de host
- Nuevo prefijo: /26, /27, /28, /30
- Direcciones por subred: 64, 32, 16, 4
- Hosts utilizables: 62, 30, 14, 2

Subredes:

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| 1 | Administración | 192.168.100.0 | 192.168.100.1 | 192.168.100.62 | 192.168.100.63 |
| 2 | Desarrollo | 192.168.100.64 | 192.168.100.65 | 192.168.100.94 | 192.168.100.95 |
| 3 | Servidores | 192.168.100.96 | 192.168.100.97 | 192.168.100.110 | 192.168.100.111 |
| 4 | WAN | 192.168.100.112 | 192.168.100.113 | 192.168.100.114 | 192.168.100.115 |

Direcciones libres:
192.168.100.116 → 192.168.100.255

Total de direcciones libres:
140

### Ejercicio 6 
Red:

192.168.200.0/24

Necesitamos:

- Administración: 100 hosts
- Desarrollo: 45 hosts
- Servidores: 20 hosts
- Cámaras: 10 hosts
- WAN: 2 hosts

- Administración: 7 bits de host
- Desarrollo: 6 bits de host
- Servidores: 5 bits de host
- Cámaras: 4 bits de host
- WAN: 2 bits de host
- Nuevo prefijo: /25, /26, /27, /28, /30
- Direcciones por subred: 128, 64, 32, 16, 4
- Hosts utilizables: 126, 62, 30, 14, 2

Subredes:

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| 1 | Administración | 192.168.200.0 | 192.168.200.1 | 192.168.200.126 | 192.168.200.127 |
| 2 | Desarrollo | 192.168.200.128 | 192.168.200.129 | 192.168.200.190 | 192.168.200.191 |
| 3 | Servidores | 192.168.200.192 | 192.168.200.193 | 192.168.200.222 | 192.168.200.223 |
| 4 | Cámaras | 192.168.200.224 | 192.168.200.225 | 192.168.200.238 | 192.168.200.239 |
| 5 | WAN | 192.168.200.240 | 192.168.200.241 | 192.168.200.242 | 192.168.200.243 |

Direcciones libres:
192.168.200.244 → 192.168.200.255

Total de direcciones libres:
12

### Ejercicio 7 Redes en desoreden 

Red:

10.10.10.0/24

Necesitamos:

- Servidores: 50 hosts
- Cámaras: 30 hosts
- Administración: 25 hosts
- Oficina: 12 hosts
- WAN: 2 hosts

- Servidores: 6 bits de host
- Cámaras: 5 bits de host
- Administración: 5 bits de host
- Oficina: 4 bits de host
- WAN: 2 bits de host
- Nuevo prefijo: /26, /27, /27, /28, /30
- Direcciones por subred: 64, 32, 32, 16, 4
- Hosts utilizables: 62, 30, 30, 14, 2

Subredes:

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| 1 | Servidores | 10.10.10.0 | 10.10.10.1 | 10.10.10.62 | 10.10.10.63 |
| 2 | Cámaras | 10.10.10.64 | 10.10.10.65 | 10.10.10.94 | 10.10.10.95 |
| 3 | Administración | 10.10.10.96 | 10.10.10.97 | 10.10.10.126 | 10.10.10.127 |
| 4 | Oficina | 10.10.10.128 | 10.10.10.129 | 10.10.10.142 | 10.10.10.143 |
| 5 | WAN | 10.10.10.144 | 10.10.10.145 | 10.10.10.146 | 10.10.10.147 |

Direcciones libres:
10.10.10.148 → 10.10.10.255

Total de direcciones libres:
108

### Ejercicio 8
Red:

172.16.50.0/24

Necesitamos:

- Recursos Humanos: 60 hosts
- Desarrollo: 35 hosts
- Ventas: 20 hosts
- Servidores: 12 hosts
- Cámaras: 6 hosts
- WAN: 2 hosts

- Recursos Humanos: 6 bits de host
- Desarrollo: 6 bits de host
- Ventas: 5 bits de host
- Servidores: 4 bits de host
- Cámaras: 3 bits de host
- WAN: 2 bits de host
- Nuevo prefijo: /26, /26, /27, /28, /29, /30
- Direcciones por subred: 64, 64, 32, 16, 8, 4
- Hosts utilizables: 62, 62, 30, 14, 6, 2

Subredes:

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| 1 | Recursos Humanos | 172.16.50.0 | 172.16.50.1 | 172.16.50.62 | 172.16.50.63 |
| 2 | Desarrollo | 172.16.50.64 | 172.16.50.65 | 172.16.50.126 | 172.16.50.127 |
| 3 | Ventas | 172.16.50.128 | 172.16.50.129 | 172.16.50.158 | 172.16.50.159 |
| 4 | Servidores | 172.16.50.160 | 172.16.50.161 | 172.16.50.174 | 172.16.50.175 |
| 5 | Cámaras | 172.16.50.176 | 172.16.50.177 | 172.16.50.182 | 172.16.50.183 |
| 6 | WAN | 172.16.50.184 | 172.16.50.185 | 172.16.50.186 | 172.16.50.187 |

Direcciones libres:
172.16.50.188 → 172.16.50.255

Total de direcciones libres:
68

## Ejercicio 9 Red Empresarial

Red:

192.168.10.0/23

Necesitamos:

- Administración: 200 hosts
- Desarrollo: 100 hosts
- Servidores: 50 hosts
- Cámaras: 20 hosts
- WAN: 2 hosts

- Administración: 8 bits de host
- Desarrollo: 7 bits de host
- Servidores: 6 bits de host
- Cámaras: 5 bits de host
- WAN: 2 bits de host
- Nuevo prefijo: /24, /25, /26, /27, /30
- Direcciones por subred: 256, 128, 64, 32, 4
- Hosts utilizables: 254, 126, 62, 30, 2

Subredes:

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| 1 | Administración | 192.168.10.0 | 192.168.10.1 | 192.168.10.254 | 192.168.10.255 |
| 2 | Desarrollo | 192.168.11.0 | 192.168.11.1 | 192.168.11.126 | 192.168.11.127 |
| 3 | Servidores | 192.168.11.128 | 192.168.11.129 | 192.168.11.190 | 192.168.11.191 |
| 4 | Cámaras | 192.168.11.192 | 192.168.11.193 | 192.168.11.222 | 192.168.11.223 |
| 5 | WAN | 192.168.11.224 | 192.168.11.225 | 192.168.11.226 | 192.168.11.227 |

Direcciones libres:
192.168.11.228 → 192.168.11.255

Total de direcciones libres:
28

### Ejercicio 10 VLSM: Alineación y asignación de subredes

## Objetivo
Comprobar la alineación de las subredes, evitar solapamientos y asignar bloques según la cantidad de hosts necesarios.

Red:
192.168.60.0/24

**Necesitamos:**
- Administración: 50 hosts
- Desarrollo: 25 hosts
- Servidores: 10 hosts
- Área adicional: 10 hosts
- Cámaras: 25 hosts
- Área adicional 2: 25 hosts
- Nueva área: 50 hosts

**Asignación de subredes:**
- Administración: /26
- Desarrollo: /27
- Servidores: /28
- Área adicional: /28
- Cámaras: /27
- Área adicional 2: /27
- Nueva área: /26

**Subredes:**

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| /26 | Administración | 192.168.60.0 | 192.168.60.1 | 192.168.60.62 | 192.168.60.63 |
| /27 | Desarrollo | 192.168.60.64 | 192.168.60.65 | 192.168.60.94 | 192.168.60.95 |
| /28 | Servidores | 192.168.60.96 | 192.168.60.97 | 192.168.60.110 | 192.168.60.111 |
| /28 | Área adicional | 192.168.60.112 | 192.168.60.113 | 192.168.60.126 | 192.168.60.127 |
| /27 | Cámaras | 192.168.60.128 | 192.168.60.129 | 192.168.60.158 | 192.168.60.159 |
| /27 | Área adicional 2 | 192.168.60.160 | 192.168.60.161 | 192.168.60.190 | 192.168.60.191 |
| /26 | Nueva área | 192.168.60.192 | 192.168.60.193 | 192.168.60.254 | 192.168.60.255 |

**Direcciones libres:**
Ninguna.

**Total de direcciones libres:**
0

**Conclusión:**
Todas las subredes están alineadas correctamente, no se solapan y pertenecen a la red principal `192.168.60.0/24`. Se utilizaron las 256 direcciones disponibles, con 242 direcciones de host utilizables en las subredes asignadas.


## CIDR — Resumen de rutas

### Objetivo

Agrupar varias redes consecutivas en una sola ruta, reduciendo la cantidad de rutas necesarias en una tabla de enrutamiento.

### Conceptos fundamentales

- CIDR significa *Classless Inter-Domain Routing*.
- El prefijo indica cuántos bits pertenecen a la red.
- Para resumir redes, deben compartir los bits de red necesarios.
- El bloque resumido debe estar correctamente alineado.
- El resumen no debe incluir redes adicionales si se requiere una agrupación exacta.

### Fórmulas

**Cantidad de redes agrupadas:**

`2^n`

**Nuevo prefijo:**

`Prefijo original - n`

Donde `n` es la cantidad de bits que se reducen al agrupar las redes.

### Ejercicio 1 — Resumen de cuatro redes

**Redes:**

- `192.168.8.0/24`
- `192.168.9.0/24`
- `192.168.10.0/24`
- `192.168.11.0/24`

**Cálculo:**

- Redes agrupadas: 4
- Bits reducidos: 2
- Nuevo prefijo: `/22`
- Máscara decimal: `255.255.252.0`

**Resultado:**

`192.168.8.0/22`

**Rango:**

- Network: `192.168.8.0`
- Primer host: `192.168.8.1`
- Último host: `192.168.11.254`
- Broadcast: `192.168.11.255`

### Ejercicio 2 — Redes que no se pueden resumir exactamente en un solo bloque

**Redes:**

- `192.168.10.0/24`
- `192.168.11.0/24`
- `192.168.12.0/24`
- `192.168.13.0/24`

**Análisis:**

Las cuatro redes son consecutivas y tienen el mismo prefijo, pero no están alineadas para formar un único bloque `/22`.

**Resultado:**

Se necesitan dos rutas para cubrir exactamente las redes originales:

- `192.168.10.0/23`
- `192.168.12.0/23`

### Resumen de prefijos

| Redes `/24` agrupadas | Bits reducidos | Prefijo resultante |
|---:|---:|---:|
| 2 | 1 | `/23` |
| 4 | 2 | `/22` |
| 8 | 3 | `/21` |
| 16 | 4 | `/20` |

### Conclusión

CIDR permite resumir rutas mediante prefijos. Para hacerlo correctamente, debemos comprobar la cantidad de redes, los bits compartidos, la alineación y el rango cubierto por el resumen.

## Ejercicio 3 — CIDR con huecos

Red:

`192.168.50.0/24`

### Necesitamos:

- Red original: `/24`
- Subredes originales: `/28`
- Tamaño de cada subred original: 16 direcciones
- Hueco 1: `192.168.50.96/28`
- Hueco 2: `192.168.50.176/28`
- Objetivo: resumir las subredes asignadas utilizando la menor cantidad posible de rutas CIDR, sin incluir los huecos.

### Subredes:

| Subred | Área | Network | Primer host | Último host | Broadcast |
|---|---|---|---|---|---|
| /26 | Grupo 1 | 192.168.50.0 | 192.168.50.1 | 192.168.50.62 | 192.168.50.63 |
| /27 | Grupo 2 | 192.168.50.64 | 192.168.50.65 | 192.168.50.94 | 192.168.50.95 |
| /28 | Grupo 3 | 192.168.50.112 | 192.168.50.113 | 192.168.50.126 | 192.168.50.127 |
| /27 | Grupo 4 | 192.168.50.128 | 192.168.50.129 | 192.168.50.158 | 192.168.50.159 |
| /28 | Grupo 5 | 192.168.50.160 | 192.168.50.161 | 192.168.50.174 | 192.168.50.175 |
| /26 | Grupo 6 | 192.168.50.192 | 192.168.50.193 | 192.168.50.254 | 192.168.50.255 |

### Direcciones libres:

- `192.168.50.96` → `192.168.50.111`
- `192.168.50.176` → `192.168.50.191`

### Total de direcciones libres:

32

### Resultado:

- Rutas CIDR finales: 6
- Direcciones libres: 32
- No existen solapamientos entre las rutas.
- Los dos huecos permanecen excluidos.
- Las rutas están alineadas correctamente.

### Conclusión:

El resumen CIDR permite reducir la cantidad de rutas agrupando redes consecutivas y alineadas. Cuando existen huecos que deben excluirse, es necesario dividir el resumen en varios bloques, respetando los límites de cada prefijo.
