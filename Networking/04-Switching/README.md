# Switching — Fundamentos

## 1. Objetivos

- Comprender cómo funciona un switch Ethernet.
- Entender cómo aprende y mantiene su tabla MAC.
- Diferenciar forwarding, flooding, filtering y aging.
- Analizar el comportamiento de un switch mediante ejercicios prácticos.

## 2. ¿Qué es un switch?

Un switch es un dispositivo de red que conecta equipos dentro de una red local (LAN). En una red Ethernet, utiliza las direcciones MAC para decidir por qué puerto debe reenviar las tramas.

El switch registra las direcciones MAC que aprende y las asocia con los puertos por los que recibe las tramas.

## 3. Tabla MAC

La tabla MAC relaciona las direcciones MAC con los puertos del switch.

Ejemplo:

| Dirección MAC | Puerto |
|---|---|
| AA-AA | Fa0/1 |
| BB-BB | Fa0/2 |
| CC-CC | Fa0/3 |

Si el switch recibe una trama destinada a `BB-BB` y conoce que está conectada a `Fa0/2`, puede reenviarla por ese puerto.

## 4. Aprendizaje de direcciones MAC (Learning)

Cuando recibe una trama Ethernet, el switch examina la MAC de origen y aprende su asociación con el puerto de entrada.

Ejemplo:

- Puerto de entrada: `Fa0/1`
- MAC de origen: `AA-AA`
- Acción: aprende o actualiza la asociación `AA-AA → Fa0/1`.

Si la asociación ya existe en ese mismo puerto, el switch puede refrescar el temporizador de la entrada dinámica.

**Regla:** el switch aprende utilizando la MAC de origen.

## 5. Reenvío selectivo (Forwarding)

Si la MAC de destino está en la tabla MAC y corresponde a un puerto diferente del puerto de entrada, el switch reenvía la trama por el puerto asociado al destino.

Ejemplo:

- Origen: `AA-AA`
- Destino: `BB-BB`
- Puerto de entrada: `Fa0/1`
- Puerto de destino: `Fa0/2`

Acción: reenvía la trama por `Fa0/2`.

## 6. Unknown Unicast Flooding

Si el switch recibe una trama unicast cuyo destino no aparece en la tabla MAC, realiza flooding dentro de la VLAN correspondiente.

La trama se envía por los puertos elegibles de esa VLAN, excepto por el puerto de entrada.

Ejemplo:

- Origen: `AA-AA`
- Destino: `XX-XX`
- Puerto de entrada: `Fa0/1`
- El switch desconoce `XX-XX`.

Acción: realiza flooding por los demás puertos elegibles de la misma VLAN.

El switch no aprende el puerto de destino simplemente por inundar la trama. Aprende la ubicación de una MAC cuando recibe una trama cuya MAC de origen corresponde a esa dirección.

## 7. Filtrado de tramas (Filtering)

Si la MAC de destino está asociada al mismo puerto por el que entró la trama, el switch no necesita reenviarla por otro puerto y la filtra.

Ejemplo:

- MAC de origen: `AA-AA`
- MAC de destino: `AA-AA`
- Puerto de entrada: `Fa0/1`
- La tabla asocia `AA-AA → Fa0/1`.

Acción: la trama no se reenvía por otros puertos.

## 8. Envejecimiento de la tabla MAC (Aging)

Las entradas MAC dinámicas pueden caducar después de un período sin actividad, según el temporizador configurado en el switch.

Cuando una entrada caduca, el switch la elimina de su tabla.

Si posteriormente recibe una trama destinada a esa MAC y ya no conoce su puerto, puede realizar unknown unicast flooding. Si después recibe una trama con esa MAC como origen, podrá aprender nuevamente su asociación.

## 9. Resumen de comportamientos

| Comportamiento | Descripción |
|---|---|
| Learning | Aprende la MAC de origen y su puerto. |
| Forwarding | Reenvía por el puerto conocido del destino. |
| Unknown unicast flooding | Inunda los puertos elegibles cuando desconoce el destino unicast. |
| Filtering | No reenvía la trama por otros puertos cuando el destino está asociado al puerto de entrada. |
| Aging | Elimina entradas dinámicas que han caducado. |

## 10. Ejercicios resueltos en clase

### Ejercicio 1 — Aprendizaje y flooding

Un switch recibe una trama con origen `AA-AA` por `Fa0/1`. Su tabla está vacía y el destino es `BB-BB`.

**Resultado:**
- Aprende `AA-AA → Fa0/1`.
- Como desconoce `BB-BB`, realiza flooding por los puertos elegibles de la misma VLAN, excepto `Fa0/1`.

### Ejercicio 2 — Reenvío selectivo

La tabla contiene `AA-AA → Fa0/1` y `BB-BB → Fa0/2`. PC-C, conectado a `Fa0/3`, envía una trama destinada

 

## 11. Laboratorio práctico: Spanning Tree Protocol (STP)

### 11.1 Objetivo

Implementar una topología redundante de tres switches en Cisco Packet Tracer para comprobar cómo STP evita bucles de capa 2 y mantiene la conectividad cuando falla un enlace.

### 11.2 Topología

Se utilizaron tres switches Cisco 2960 conectados inicialmente en forma de triángulo:

- **S1:** Root Bridge.
- **S2:** switch intermedio.
- **S3:** switch con una ruta alternativa.
- **PC0:** `192.168.10.10/24`.
- **PC1:** `192.168.10.20/24`.

Las dos PCs se configuraron en la misma subred para probar la conectividad de capa 2.

### 11.3 Configuración del Root Bridge

En S1 se configuró una prioridad inferior a la predeterminada:

```text
enable
configure terminal
spanning-tree vlan 1 priority 4096
end
show spanning-tree vlan 1
```

**Resultado:** S1 fue elegido Root Bridge para la VLAN 1. La prioridad efectiva mostrada fue 4097, correspondiente a la prioridad configurada más el identificador de VLAN.

### 11.4 Verificación de roles y estados

Comando utilizado:

```text
show spanning-tree vlan 1
```

Campos importantes:

- `Root ID`: identifica el Root Bridge.
- `Bridge ID`: identifica al switch local.
- `Root Cost`: costo acumulado de la ruta seleccionada hacia el Root Bridge.
- `Root`: puerto que proporciona la mejor ruta hacia la raíz.
- `Desg`: puerto designado para el segmento.
- `Altn`: puerto alternativo.
- `FWD`: estado Forwarding.
- `BLK`: estado Blocking en STP clásico.
- `LRN`: estado Learning.

En la topología triangular, STP mantuvo una ruta alternativa sin reenviar tráfico de usuario, evitando que todos los enlaces redundantes estuvieran activos simultáneamente para el reenvío.

### 11.5 Prueba de falla del enlace

Se desconectó el enlace directo entre S1 y S3.

Antes de la desconexión, S3 utilizaba un puerto como Root Port y mantenía otro como Alternate/Blocking.

Después de la desconexión:

- S3 perdió su ruta directa hacia S1.
- STP detectó el cambio de topología.
- El puerto alternativo pasó a desempeñar la función de Root Port.
- La ruta alternativa pasó a Forwarding tras la convergencia.
- S3 alcanzó S1 a través de S2.

El costo acumulado hacia la raíz pasó de 19 a 38:

```text
19 + 19 = 38
```

Esto se debe a que la nueva ruta utiliza dos enlaces, cada uno con costo 19.

### 11.6 Prueba de conectividad

Direcciones configuradas:

| Equipo | Dirección IPv4 | Máscara |
|---|---|---|
| PC0 | `192.168.10.10` | `255.255.255.0` |
| PC1 | `192.168.10.20` | `255.255.255.0` |

Desde PC0 se ejecutó:

```text
ping 192.168.10.20
```

**Resultado observado:** 4 paquetes enviados, 4 recibidos y 0 % de pérdida.

La prueba confirmó que PC0 podía comunicarse con PC1 a través de la ruta disponible.

### 11.7 Verificación de la tabla MAC

Comando utilizado:

```text
show mac address-table dynamic
```

La tabla mostró una dirección MAC dinámica asociada a un puerto de S3.

El switch aprende las direcciones MAC de origen de las tramas recibidas y las asocia con el puerto de entrada. Después consulta la MAC de destino para decidir por dónde reenviar la trama.

### 11.8 Conclusiones

1. STP elige un Root Bridge mediante el Bridge ID.
2. Los switches no raíz seleccionan un Root Port según los criterios de STP.
3. Los puertos alternativos permiten conservar redundancia sin crear bucles.
4. Una falla de enlace puede provocar una reconvergencia y cambiar los roles y estados de los puertos.
5. El costo de ruta aumenta cuando se utiliza un camino con más enlaces.
6. La tabla MAC permite asociar direcciones MAC con los puertos por los que se aprendieron.
7. La prueba de ping confirmó la conectividad entre las PCs después de la recuperación de la ruta.

### 11.9 Comandos de diagnóstico utilizados

| Comando | Propósito |
|---|---|
| `show spanning-tree vlan 1` | Consultar el Root Bridge, roles, estados y costos STP. |
| `show mac address-table dynamic` | Consultar las direcciones MAC dinámicas aprendidas. |
| `ping 192.168.10.20` | Comprobar conectividad IP entre las PCs. |

