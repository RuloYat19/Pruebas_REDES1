## Planificación y diseño conceptual (Antes de tocar Packet Tracer)
### Fase 1: Seleccionar los 6 hospitales reales
Eligir 6 hospitales o centros de salud del área metropolitana (por ejemplo: Hospital Roosevelt, San Juan de Dios, etc.). Asegurarse de que sean reales.

### Fase 2: Definir la topología física
Decidir qué tipo de topología usarás:

- Estrella
- Anillo
- Híbrida

Justificar por qué se eligió esa topología (eficiencia, redundancia, escalabilidad, etc.).

### Fase 3: Definir los dominios de colisión por hospital
|Hospital|Hosts mínimos|Dominios de colisión requeridos|
|--|--|--|
|1|63|3 dominios|
|2|25|2 dominios|
|3|32|3 dominios|
|4|15|2 dominios|
|5|26|2 dominios|
|6|08|1 dominios|

Nota:

- Usar switches para separar dominios de colisión.

- Usar hubs si se necesita mantener un mismo dominio de colisión con varios hosts.

### Fase 4: Definir las VLANs por área funcional
Identificar las áreas funcionales que necesitas (ej. Quirófanos, UCI, Administración, Laboratorios, etc.).
Cada VLAN tendrá un ID formado por:
`Número de área + último dígito de tu carnet`
Ejemplo: Si tu carnet termina en 5, Área 1 → VLAN 15.

## Subnetting (Cálculo de direcciones IP)
### Fase 5: Propón una red base
ELegir una red privada (ej. 192.168.X.0/24) y realizar subnetting eficiente para asignar una subred por cada VLAN, considerando los hosts requeridos.

### Fase 6: Tabla de subnetting
Se debe de crear una tabla de esta manera:
|VLAN|Área funcional|Subred|Máscara|Gateway|Rango de hosts|
|--|--|--|--|--|--|
|15|Quirófanos|192.168.1.0|255.255.255.0|192.168.1.1|192.168.1.2 - 192.168.1.254|
|...|...|...|...|...|...|

## Implementación en Cisco Packet Tracer
### Fase 7: Construcción de la topología
- Colocar los 6 hospitales con sus switches y hosts.

- Conectar los switches principales entre sí (con 3 enlaces físicos para EtherChannel).

- Usar los cables correctos (crossover entre switches, straight entre switch y host).

### Fase 8: Configuración básica de switches
Por cada switch:

- Nombre (hostname)
- Contraseña de acceso (usando tu carnet)
- VLANs necesarias
- Puertos de acceso asignados a VLANs correspondientes
- Puertos troncales (VLAN nativa 99)
- Puertos no utilizados → VLAN 999 (blackhole)

### Fase 9: Configuración de VTP
- Un switch principal de cada hospital o uno global será servidor VTP.
- Dominio: número de carnet
- Contraseña: area# (donde # es el número de área)
- Los demás switches como clientes.

### Fase 10: Configuración de STP
- Activar Rapid PVST+
- Elegir el switch raíz (Root Bridge) para cada VLAN (normalmente el servidor VTP).

### Fase 11: Configuración de EtherChannel
- Entre switches principales, agrupar 3 enlaces físicos con PAgP.
- Verificar que los enlaces se agrupen correctamente.

### Fase 12: Asignación de direcciones IP
- Configurar IPs en todos los hosts según la tabla de subnetting.
- Configurar gateways en los switches (interfaz VLAN con IP).

## Pruebas y verificación
### Fase 13: Conectividad
- Realizar al menos 10 pings exitosos entre equipos de la misma VLAN.

- Realizar pings entre diferentes VLANs (opcional, depende si usas enrutamiento).

### Fase 14: Capturas
- Capturar pantallas de configuración CLI.
- Capturar pings exitosos.
- Capturar ARP y paquetes en simulación.

## Documentación y entrega
### Fase 15: Manual técnico (Markdown)
Se debe incluir:

- Topología con capturas
- Tabla de subnetting
- Tabla de asignación IP
- Configuración completa de cada dispositivo (comandos)
- Capturas de pruebas de funcionamiento

### Fase 16: Archivo .pkt
Guardar y adjuntar el archivo de Packet Tracer.

### Fase 17: Script de comandos
Archivo con todos los comandos CLI utilizados.

### Fase 18: Informe de desarrollo
- Explicar cómo implementaste la práctica.
- Problemas encontrados y soluciones.
- Capturas de funcionamiento.