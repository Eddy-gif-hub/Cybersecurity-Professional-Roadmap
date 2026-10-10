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
