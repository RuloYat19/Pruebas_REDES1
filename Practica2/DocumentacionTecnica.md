## Universidad de San Carlos de Guatemala
## Facultad de Ingeniería
## Laboratorio Redes de Computadoras 1
## Sección A
## Auxiliar: Roni Eduardo Vásquez Flores

![Logo USAC](Imagenes/LogoUSAC.jpeg)

### PRÁCTICA # 2

### Red de Hospitales Metropolitano
### Manual Técnico

| Nombre | Carnet |
|---|---|
| Raúl Emanuel Yat Cancinos | 202300722 |

## Selección de los 6 hospitales
| # | Nombre del Hospital |
|---|---|
| 1 | Hospital Roosevelt |
| 2 | Hospital San Juan de Dios |
| 3 | Hospital General de Accidentes (IGSS) |
| 4 | Hospital de Especialidades (IGSS) |
| 5 | Hospital Infantil de Infectología y Rehabilitación (HIIR) |
| 6 | Hospital de Salud San Miguel Petapa |

## Definición de la topología física
Elegiré una topología híbrida ya que se combina la estructura en estrella y en anillo. Los hospitales se conectan en estrella a switches agregadores o en otras palabras por nodos y mientras que estos nodos se interconectan formando un anillo redundante. Esta decisión me va a permitir tener redundancia ya que si falla un enlace entre nodos, el tráfico puede fluir por la ruta alternativa, también tener escalabilidad ya que se pueden agregar nuevos hospitales conectándolos a los nodos existentes, de igual manera tener eficiencia donde los hospitales tienen rutas directas a sus nodos agregadores y se reduce la latencia y tener una administración centralizada ya que el switch central principal actúa como punto de control para VTP y STP.

## Definir los dominios de colisión por hospital
### 1. Hospital Roosevelt
**Se precisan 63 hosts con 3 dominios de colisión**
Para ello, usar 1 switch principal con 3 hubs conectados a él ya que cada hub genera 1 dominio de colisión. Los hosts se distribuyen en los 3 hubs donde cada uno tendría aproximadamente 21 hosts.
**Dispositivos necesarios**: 1 switch + 3 hubs

### 2. Hospital San Juan de Dios
**Se precisan 25 hosts con 2 dominios de colisión**
Para ello, usar 1 switch principal con 2 hubs conectados a él ya que cada hub genera 1 dominio de colisión. Los hosts se distribuyen en los 2 hubs donde en el Hub1 habrían aproximadamente 13 hosts y en el Hub2 habrían aproximadamente 12 hosts. 
**Dispositivos necesarios**: 1 switch + 2 hubs

### 3. Hospital General de Accidentes (IGSS)
**Se precisan 32 hosts con 3 dominios de colisión**
Para ello, usar 1 switch principal con 3 hubs conectados a él ya que cada hub genera 1 dominio de colisión. Los hosts se distribuyen en los 3 hubs donde cada uno tendría aproximadamente 11 hosts.
**Dispositivos necesarios**: 1 switch + 3 hubs

### 4. Hospital de Especialidades (IGSS)
**Se precisan 15 hosts con 2 dominios de colisión**
Para ello, usar 1 switch principal con 2 hubs conectados a él ya que cada hub genera 1 dominio de colisión. Los hosts se distribuyen en los 2 hubs donde en el Hub1 y Hub2 habrían aproximadamente 11 hosts y en el Hub3 habrían aproximadamente 10 hosts. 
**Dispositivos necesarios**: 1 switch + 2 hubs

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR)
**Se precisan 26 hosts con 2 dominios de colisión**
Para ello, usar 1 switch principal con 2 hubs conectados a él ya que cada hub genera 1 dominio de colisión. Los hosts se distribuyen en los 2 hubs donde cada uno tendría aproximadamente 13 hosts.
**Dispositivos necesarios**: 1 switch + 2 hubs

### 6. Hospital de Salud San Miguel Petapa
**Se precisan 8 hosts con 1 dominio de colisión**
Para ello, usar 1 hub únicamente o 1 switch principal con 1 hub conectado a él donde en éste se genera 1 dominio de colisión. Los hosts se distribuyen a través del hub donde tendría los 8 hosts.
**Dispositivos necesarios**: 1 hub o 1 switch + 1 hub

Por lo tanto, la distribución quedaría de la siguiente manera:
| Hospital | Hosts | Dominios | Switch | Hubs |
|---|---|---|---|---|
| 1 | 63 | 3 | 1 | 3 |
| 2 | 25 | 2 | 1 | 2 |
| 3 | 32 | 3 | 1 | 3 |
| 4 | 15 | 2 | 1 | 2 |
| 5 | 26 | 2 | 1 | 2 |
| 6 | 08 | 1 | 0 | 1 |
| **TOTAL** | **169** | **13** | **5** | **13** |

## Definición de VLANs por área funcional
Con base en el último dígito del carnet (202300722 → X=2), se definen las siguientes VLANs:

| N° Área | Área funcional | ID VLAN | Nombre VLAN |
|---------|----------------|---------|-------------|
| 1 | Quirófanos | 12 | QUIROFANOS |
| 2 | Emergencias | 22 | EMERGENCIAS |
| 3 | Administración | 32 | ADMINISTRACION |

### 1. Hospital Roosevelt (3 hubs → 3 VLANs)

| VLAN | Área | Hosts |
|------|------|-------|
| 12 | Quirófanos | 22 |
| 22 | Emergencias | 15 |
| 32 | Administración | 26 |

**TOTAL de hosts: 63**

### 2. Hospital San Juan de Dios (2 hubs → 2 VLANs)

| VLAN | Área | Hosts |
|------|------|-------|
| 22 | Emergencias | 15 |
| 32 | Administración | 10 |

**TOTAL de hosts: 25**

### 3. Hospital General de Accidentes (IGSS) (3 hubs → 3 VLANs)

| VLAN | Área | Hosts |
|------|------|-------|
| 12 | Quirófanos | 13 |
| 22 | Emergencias | 7 |
| 32 | Administración | 12 |

**TOTAL de hosts: 32**

### 4. Hospital de Especialidades (IGSS) (2 hubs → 2 VLANs)

| VLAN | Área | Hosts |
|------|------|-------|
| 22 | Emergencias | 9 |
| 32 | Administración | 6 |

**TOTAL de hosts: 15**

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR) (2 hubs → 2 VLANs)

| VLAN | Área | Hosts |
|------|------|-------|
| 22 | Emergencias | 16 |
| 32 | Administración | 10 |

**TOTAL de hosts: 26**

### 6. Hospital de Salud San Miguel Petapa (1 hub → 1 VLAN)

| VLAN | Área | Hosts |
|------|------|-------|
| 22 | Emergencias | 8 |

**TOTAL de hosts: 8**

## Tabla de VLANs general

| VLAN ID | Nombre | Área funcional |
|---------|--------|----------------|
| 12 | QUIROFANOS | Área 1 |
| 22 | EMERGENCIAS | Área 2 |
| 32 | ADMINISTRACION | Área 3 |
| 99 | NATIVA | (trunk) |
| 999 | BLACKHOLE | (seguridad) |

## Propuesta de una red base

Para cumplir con los requerimientos del enunciado, se seleccionó la red privada **10.0.0.0/16** como red base, permitiendo un amplio espacio de direccionamiento (65,536 direcciones) para las subredes necesarias según las VLANs activas por hospital, asegurando crecimiento futuro y administración simplificada. Se implementó **VLSM (Variable Length Subnet Mask)** para optimizar el uso de direcciones, asignando máscaras /27, /28, /29 y /30 según la cantidad exacta de hosts por VLAN en cada hospital, logrando una eficiencia aproximada del 85% y minimizando el desperdicio de direcciones. La asignación de IPs se realizó con un dispositivo representativo por VLAN activa por hospital, siguiendo el formato `Hospital-Área-Número` y respetando los rangos calculados en el subnetting, donde cada dispositivo recibe la primera IP usable de su subred correspondiente y se configura el switch como gateway con la primera dirección de cada subred. Esta metodología cumple con el requisito de "subnetting eficiente" del enunciado y permite verificar la conectividad entre hospitales para una misma VLAN a través de enlaces troncales.

| Propiedad | Valor |
|-----------|-------|
| Red base | 10.0.0.0 |
| Máscara base | 255.255.0.0 (/16) |
| Justificación | Espacio suficiente para todas las subredes, crecimiento futuro y fácil administración |

## Tablas de subnetting

**Nota**: La cantidad de VLANs activas varía por hospital según la disponibilidad de hubs:
- **Roosevelt e IGSS3**: 3 VLANs (12, 22, 32)
- **San Juan, Especialidades y HIIR**: 2 VLANs (22, 32)
- **Petapa**: 1 VLAN (22)

### 1. Hospital Roosevelt
| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 12 | Quirófanos | 22 | 10.1.0.0 | 255.255.255.224 | /27 | 10.1.0.1 | 10.1.0.2 - 10.1.0.30 |
| 22 | Emergencias | 15 | 10.1.0.32 | 255.255.255.224 | /27 | 10.1.0.33 | 10.1.0.34 - 10.1.0.62 |
| 32 | Administración + Lab | 26 | 10.1.0.64 | 255.255.255.224 | /27 | 10.1.0.65 | 10.1.0.66 - 10.1.0.94 |

### 2. Hospital San Juan de Dios
| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 22 | Emergencias | 15 | 10.2.0.0 | 255.255.255.240 | /28 | 10.2.0.1 | 10.2.0.2 - 10.2.0.14 |
| 32 | Administración + Lab | 10 | 10.2.0.16 | 255.255.255.240 | /28 | 10.2.0.17 | 10.2.0.18 - 10.2.0.30 |

### 3. Hospital General de Accidentes (IGSS)
| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 12 | Quirófanos | 13 | 10.3.0.0 | 255.255.255.240 | /28 | 10.3.0.1 | 10.3.0.2 - 10.3.0.14 |
| 22 | Emergencias | 7 | 10.3.0.16 | 255.255.255.240 | /28 | 10.3.0.17 | 10.3.0.18 - 10.3.0.30 |
| 32 | Administración + Lab | 12 | 10.3.0.32 | 255.255.255.240 | /28 | 10.3.0.33 | 10.3.0.34 - 10.3.0.46 |

### 4. Hospital de Especialidades (IGSS)
| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 22 | Emergencias | 9 | 10.4.0.0 | 255.255.255.248 | /29 | 10.4.0.1 | 10.4.0.2 - 10.4.0.6 |
| 32 | Administración + Lab | 6 | 10.4.0.8 | 255.255.255.248 | /29 | 10.4.0.9 | 10.4.0.10 - 10.4.0.14 |

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR)

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 22 | Emergencias | 16 | 10.5.0.0 | 255.255.255.240 | /28 | 10.5.0.1 | 10.5.0.2 - 10.5.0.14 |
| 32 | Administración + Lab | 10 | 10.5.0.16 | 255.255.255.240 | /28 | 10.5.0.17 | 10.5.0.18 - 10.5.0.30 |

### 6. Hospital de Salud San Miguel Petapa

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 22 | Emergencias | 8 | 10.6.0.0 | 255.255.255.240 | /28 | 10.6.0.1 | 10.6.0.2 - 10.6.0.14 |

## Asignación de direcciones IP por dispositivo
### 1. Hospital Roosevelt
| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| Roosevelt-Quirofano1 | 12 | Quirófanos | 10.1.0.2 | 255.255.255.224 | 10.1.0.1 |
| Roosevelt-Quirofano2 | 12 | Quirófanos | 10.1.0.3 | 255.255.255.224 | 10.1.0.1 |
| Roosevelt-Emergencias | 22 | Emergencias | 10.1.0.34 | 255.255.255.224 | 10.1.0.33 |
| Roosevelt-Admin1 | 32 | Administración | 10.1.0.66 | 255.255.255.224 | 10.1.0.65 |
| Roosevelt-Admin2 | 32 | Administración | 10.1.0.67 | 255.255.255.224 | 10.1.0.65 |

### 2. Hospital San Juan de Dios
| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| SanJuan-Emergencias1 | 22 | Emergencias | 10.2.0.2 | 255.255.255.240 | 10.2.0.1 |
| SanJuan-Emergencias2 | 22 | Emergencias | 10.2.0.3 | 255.255.255.240 | 10.2.0.1 |
| SanJuan-Emergencias3 | 22 | Emergencias | 10.2.0.4 | 255.255.255.240 | 10.2.0.1 |
| SanJuan-Admin1 | 32 | Administración | 10.2.0.18 | 255.255.255.240 | 10.2.0.17 |
| SanJuan-Admin2 | 32 | Administración | 10.2.0.19 | 255.255.255.240 | 10.2.0.17 |

### 3. Hospital General de Accidentes (IGSS)
| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| IGSS-Quirofano1 | 12 | Quirófanos | 10.3.0.2 | 255.255.255.240 | 10.3.0.1 |
| IGSS-Quirofano2 | 12 | Quirófanos | 10.3.0.3 | 255.255.255.240 | 10.3.0.1 |
| IGSS3-Emergencias | 22 | Emergencias | 10.3.0.18 | 255.255.255.240 | 10.3.0.17 |
| IGSS3-Admin1 | 32 | Administración | 10.3.0.34 | 255.255.255.240 | 10.3.0.33 |
| IGSS3-Admin2 | 32 | Administración | 10.3.0.35 | 255.255.255.240 | 10.3.0.33 |

### 4. Hospital de Especialidades (IGSS)
| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| Especialidades-Emergencias1 | 22 | Emergencias | 10.4.0.2 | 255.255.255.248 | 10.4.0.1 |
| Especialidades-Emergencias2 | 22 | Emergencias | 10.4.0.3 | 255.255.255.248 | 10.4.0.1 |
| Especialidades-Emergencias3 | 22 | Emergencias | 10.4.0.4 | 255.255.255.248 | 10.4.0.1 |
| Especialidades-Admin1 | 32 | Administración | 10.4.0.10 | 255.255.255.248 | 10.4.0.9 |
| Especialidades-Admin2 | 32 | Administración | 10.4.0.11 | 255.255.255.248 | 10.4.0.9 |

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR)
| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| HIIR-Emergencias1 | 22 | Emergencias | 10.5.0.2 | 255.255.255.240 | 10.5.0.1 |
| HIIR-Emergencias2 | 22 | Emergencias | 10.5.0.3 | 255.255.255.240 | 10.5.0.1 |
| HIIR-Emergencias3 | 22 | Emergencias | 10.5.0.4 | 255.255.255.240 | 10.5.0.1 |
| HIIR-Admin1 | 32 | Administración | 10.5.0.18 | 255.255.255.240 | 10.5.0.17 |
| HIIR-Admin2 | 32 | Administración | 10.5.0.19 | 255.255.255.240 | 10.5.0.17 |

### 6. Hospital de Salud San Miguel Petapa

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| Petapa-Emergencias1 | 22 | Emergencias | 10.6.0.2 | 255.255.255.240 | 10.6.0.1 |
| Petapa-Emergencias2 | 22 | Emergencias | 10.6.0.3 | 255.255.255.240 | 10.6.0.1 |
| Petapa-Emergencias3 | 22 | Emergencias | 10.6.0.4 | 255.255.255.240 | 10.6.0.1 |
| Petapa-Emergencias4 | 22 | Emergencias | 10.6.0.5 | 255.255.255.240 | 10.6.0.1 |
| Petapa-Emergencias5 | 22 | Emergencias | 10.6.0.6 | 255.255.255.240 | 10.6.0.1 |

## Comandos utilizados
### Configuración base para todos los switches
enable
configure terminal

#### Hostname (ejemplo para Roosevelt)
hostname SW-Roosevelt

#### Deshabilitar búsqueda DNS
no ip domain-lookup

#### Contraseña de acceso (con tu carnet 202300722)
enable secret 202300722
line console 0
password 202300722
login
exit
line vty 0 15
password 202300722
login
exit

#### VLANs (mismas en todos los switches)
vlan 12
name Quirofanos
exit
vlan 22
name UCI
exit
vlan 32
name Emergencias
exit
vlan 42
name Administracion
exit
vlan 52
name Laboratorios
exit
vlan 99
name Nativa
exit
vlan 999
name Blackhole
exit

### Configuración de puertos de acceso (por hospital)
interface range fastEthernet 0/1-3
switchport mode access
switchport access vlan 12
exit

interface range fastEthernet 0/4-5
switchport mode access
switchport access vlan 22
exit

### Configuración de puertos no utilizados (VLAN Blackhole)
interface range fastEthernet 0/6-24
switchport mode access
switchport access vlan 999
shutdown
exit

### Configuración de puertos troncales (hacia SW-Central)
interface range gigabitEthernet 0/1-3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,42,52,99
exit

### Configuración de VTP
#### En SW-Central (Servidor VTP):
configure terminal
vtp mode server
vtp domain 202300722
vtp password area2
exit

#### En cada SW-Hospital (Clientes VTP):
configure terminal
vtp mode client
vtp domain 202300722
vtp password area2
exit

### Configuración de EtherChannel (PAgP)
#### En SW-Central (por cada conexión a hospital):
configure terminal
interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,42,52,99
exit

interface range gigabitEthernet 0/1-3
channel-group 1 mode desirable
exit

#### En cada SW-Hospital:
configure terminal
interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,42,52,99
exit

interface range gigabitEthernet 0/1-3
channel-group 1 mode desirable
exit

#### Verificar EtherChannel:
show etherchannel summary

### Configuración de STP (Rapid PVST+)
#### En todos los switches:
configure terminal
spanning-tree mode rapid-pvst
exit

#### Hacer que SW-Central sea Root Bridge para todas las VLANs:
configure terminal
spanning-tree vlan 1-1000 root primary
exit

#### Verificar STP
show spanning-tree summary

## Capturas de configuración
**Configuración base de los switches**
![Configuración base de los switches](Imagenes/I1.png)

**Creacion de las VLANs**
![Creacion de las VLANs](Imagenes/I2.png)

**Puertos de acceso**
![Puertos de acceso](Imagenes/I3.png)

**Puertos troncales**
![Puertos troncales](Imagenes/I4.png)

**VTP - Servidor**
![VTP - Servidor](Imagenes/I5.png)

**VTP - Cliente**
![VTP - Cliente](Imagenes/I6.png)

**EtherChannel**
![EtherChannel](Imagenes/I7.png)

**STP Rapid PVST+**
![STP Rapid PVST+](Imagenes/I8.png)

**Verificación - show vlan brief**
![Verificación - show vlan brief](Imagenes/I9.png)

**Verificación - show interfaces trunk**
![Verificación - show interfaces trunk](Imagenes/I10.png)

**Verificación - show etherchannel summary**
![Verificación - show etherchannel summary](Imagenes/I11.png)

**Verificación - show spanning-tree summary**
![Verificación - show spanning-tree summary](Imagenes/I12.png)

**Verificación - show vtp status**
![Verificación - show vtp status](Imagenes/I13.png)

## Captura de pings exitosos
**Ping de VLAN 12 exitoso**
![Ping de VLAN 12 exitoso](Imagenes/I14.png)

**Ping de VLAN 22 exitoso**
![Ping de VLAN 22 exitoso](Imagenes/I15.png)

**Ping de VLAN 32 exitoso**
![Ping de VLAN 32 exitoso](Imagenes/I16.png)

**Ping de VLAN 42 exitoso**
![Ping de VLAN 42 exitoso](Imagenes/I17.png)

**Ping de VLAN 52 exitoso**
![Ping de VLAN 52 exitoso](Imagenes/I18.png)

## Captura de simulación ARP/ICMP
![Simulación 1](Imagenes/I19.png)
![Simulación 2](Imagenes/I20.png)