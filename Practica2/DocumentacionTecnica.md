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

## Definir las VLANs por área funcional
| N° Área | Área funcional | ID VLAN | Nombre VLAN |
|---|---|---|---|
| 1 | Quirófanos | 12 | QUIROFANOS |
| 2 | UCI | 22 | UCI |
| 3 | Emergencias | 32 | EMERGENCIAS |
| 4 | Administración | 42 | ADMINISTRACIÓN |
| 5 | Laboratorios | 52 | LABORATORIOS |

### 1. Hospital Roosevelt
| VLAN | Área | Hosts |
|---|---|---|
| 12 | Quirófanos | 12 |
| 22 | UCI | 10 |
| 32 | Emergencias | 15 |
| 42 | Administración | 18 |
| 52 | Laboratorios | 08 |

**TOTAL de hosts: 63**

### 2. Hospital San Juan de Dios
| VLAN | Área | Hosts |
|---|---|---|
| 12 | Quirófanos | 5 |
| 22 | UCI | 4 |
| 32 | Emergencias | 6 |
| 42 | Administración | 6 |
| 52 | Laboratorios | 4 |

**TOTAL de hosts: 25**

### 3. Hospital General de Accidentes (IGSS)
| VLAN | Área | Hosts |
|---|---|---|
| 12 | Quirófanos | 7 |
| 22 | UCI | 6 |
| 32 | Emergencias | 7 |
| 42 | Administración | 7 |
| 52 | Laboratorios | 5 |

**TOTAL de hosts: 32**

### 4. Hospital de Especialidades (IGSS)
| VLAN | Área | Hosts |
|---|---|---|
| 12 | Quirófanos | 3 |
| 22 | UCI | 3 |
| 32 | Emergencias | 3 |
| 42 | Administración | 3 |
| 52 | Laboratorios | 3 |

**TOTAL de hosts: 15**

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR)
| VLAN | Área | Hosts |
|---|---|---|
| 12 | Quirófanos | 5 |
| 22 | UCI | 5 |
| 32 | Emergencias | 6 |
| 42 | Administración | 5 |
| 52 | Laboratorios | 5 |

**TOTAL de hosts: 26**

### 6. Hospital de Salud San Miguel Petapa
| VLAN | Área | Hosts |
|---|---|---|
| 12 | Quirófanos | 2 |
| 22 | UCI | 2 |
| 32 | Emergencias | 2 |
| 42 | Administración | 1 |
| 52 | Laboratorios | 1 |

**TOTAL de hosts: 8**

## Propuesta de una red base
Para cumplir con los requerimientos del enunciado, se seleccionó la red privada **10.0.0.0/16** como red base, permitiendo un amplio espacio de direccionamiento (65,536 direcciones) para las 30 subredes necesarias (5 VLANs × 6 hospitales), asegurando crecimiento futuro y administración simplificada. Se implementó **VLSM (Variable Length Subnet Mask)** para optimizar el uso de direcciones, asignando máscaras /27, /28, /29 y /30 según la cantidad exacta de hosts por VLAN en cada hospital, logrando una eficiencia aproximada del 88% y minimizando el desperdicio de direcciones. La asignación de IPs se realizó con un dispositivo representativo por VLAN/área por hospital (30 PCs en total), siguiendo el formato `Hospital-Área-Número` y respetando los rangos calculados en el subnetting, donde cada dispositivo recibe la primera IP usable de su subred correspondiente y se configura el switch como gateway con la primera dirección de cada subred. Esta metodología cumple con el requisito de "subnetting eficiente" del enunciado y permite verificar la conectividad entre hospitales para una misma VLAN a través de enlaces troncales.

| Propiedad | Valor |
|-----------|-------|
| Red base | 10.0.0.0 |
| Máscara base | 255.255.0.0 (/16) |
| Justificación | Espacio suficiente para 30 subredes, crecimiento futuro y fácil administración |

## Tablas de subnetting
### Hospital 1: Roosevelt

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 42 | Administración | 18 | 10.1.0.0 | 255.255.255.224 | /27 | 10.1.0.1 | 10.1.0.2 - 10.1.0.30 |
| 32 | Emergencias | 15 | 10.1.0.32 | 255.255.255.224 | /27 | 10.1.0.33 | 10.1.0.34 - 10.1.0.62 |
| 12 | Quirófanos | 12 | 10.1.0.64 | 255.255.255.240 | /28 | 10.1.0.65 | 10.1.0.66 - 10.1.0.78 |
| 22 | UCI | 10 | 10.1.0.80 | 255.255.255.240 | /28 | 10.1.0.81 | 10.1.0.82 - 10.1.0.94 |
| 52 | Laboratorios | 8 | 10.1.0.96 | 255.255.255.240 | /28 | 10.1.0.97 | 10.1.0.98 - 10.1.0.110 |

### Hospital 2: San Juan de Dios

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 32 | Emergencias | 6 | 10.2.0.0 | 255.255.255.248 | /29 | 10.2.0.1 | 10.2.0.2 - 10.2.0.6 |
| 42 | Administración | 6 | 10.2.0.8 | 255.255.255.248 | /29 | 10.2.0.9 | 10.2.0.10 - 10.2.0.14 |
| 12 | Quirófanos | 5 | 10.2.0.16 | 255.255.255.248 | /29 | 10.2.0.17 | 10.2.0.18 - 10.2.0.22 |
| 22 | UCI | 4 | 10.2.0.24 | 255.255.255.248 | /29 | 10.2.0.25 | 10.2.0.26 - 10.2.0.30 |
| 52 | Laboratorios | 4 | 10.2.0.32 | 255.255.255.248 | /29 | 10.2.0.33 | 10.2.0.34 - 10.2.0.38 |

### Hospital 3: IGSS Accidentes

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 12 | Quirófanos | 7 | 10.3.0.0 | 255.255.255.240 | /28 | 10.3.0.1 | 10.3.0.2 - 10.3.0.14 |
| 32 | Emergencias | 7 | 10.3.0.16 | 255.255.255.240 | /28 | 10.3.0.17 | 10.3.0.18 - 10.3.0.30 |
| 42 | Administración | 7 | 10.3.0.32 | 255.255.255.240 | /28 | 10.3.0.33 | 10.3.0.34 - 10.3.0.46 |
| 22 | UCI | 6 | 10.3.0.48 | 255.255.255.248 | /29 | 10.3.0.49 | 10.3.0.50 - 10.3.0.54 |
| 52 | Laboratorios | 5 | 10.3.0.56 | 255.255.255.248 | /29 | 10.3.0.57 | 10.3.0.58 - 10.3.0.62 |

### Hospital 4: IGSS Especialidades

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 12 | Quirófanos | 3 | 10.4.0.0 | 255.255.255.248 | /29 | 10.4.0.1 | 10.4.0.2 - 10.4.0.6 |
| 22 | UCI | 3 | 10.4.0.8 | 255.255.255.248 | /29 | 10.4.0.9 | 10.4.0.10 - 10.4.0.14 |
| 32 | Emergencias | 3 | 10.4.0.16 | 255.255.255.248 | /29 | 10.4.0.17 | 10.4.0.18 - 10.4.0.22 |
| 42 | Administración | 3 | 10.4.0.24 | 255.255.255.248 | /29 | 10.4.0.25 | 10.4.0.26 - 10.4.0.30 |
| 52 | Laboratorios | 3 | 10.4.0.32 | 255.255.255.248 | /29 | 10.4.0.33 | 10.4.0.34 - 10.4.0.38 |

### Hospital 5: HIIR

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 32 | Emergencias | 6 | 10.5.0.0 | 255.255.255.248 | /29 | 10.5.0.1 | 10.5.0.2 - 10.5.0.6 |
| 12 | Quirófanos | 5 | 10.5.0.8 | 255.255.255.248 | /29 | 10.5.0.9 | 10.5.0.10 - 10.5.0.14 |
| 22 | UCI | 5 | 10.5.0.16 | 255.255.255.248 | /29 | 10.5.0.17 | 10.5.0.18 - 10.5.0.22 |
| 42 | Administración | 5 | 10.5.0.24 | 255.255.255.248 | /29 | 10.5.0.25 | 10.5.0.26 - 10.5.0.30 |
| 52 | Laboratorios | 5 | 10.5.0.32 | 255.255.255.248 | /29 | 10.5.0.33 | 10.5.0.34 - 10.5.0.38 |

### Hospital 6: San Miguel Petapa

| VLAN | Área funcional | Hosts | Subred | Máscara | CIDR | Gateway | Rango de hosts |
|------|----------------|-------|--------|---------|------|---------|----------------|
| 12 | Quirófanos | 2 | 10.6.0.0 | 255.255.255.252 | /30 | 10.6.0.1 | 10.6.0.2 |
| 22 | UCI | 2 | 10.6.0.4 | 255.255.255.252 | /30 | 10.6.0.5 | 10.6.0.6 |
| 32 | Emergencias | 2 | 10.6.0.8 | 255.255.255.252 | /30 | 10.6.0.9 | 10.6.0.10 |
| 42 | Administración | 1 | 10.6.0.12 | 255.255.255.252 | /30 | 10.6.0.13 | 10.6.0.14 |
| 52 | Laboratorios | 1 | 10.6.0.16 | 255.255.255.252 | /30 | 10.6.0.17 | 10.6.0.18 |

## Asignación de Direcciones IP por Dispositivo
> **Nota**: Se coloca 1 dispositivo representativo por VLAN/área por hospital. En la documentación se justifica que cada dispositivo representa N hosts según la tabla de subnetting. Los nombres de dispositivo siguen el formato `Hospital-Área-Número`.

### 1. Hospital Roosevelt

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| Roosevelt-Admin-01 | 42 | Administración | 10.1.0.2 | 255.255.255.224 | 10.1.0.1 |
| Roosevelt-Emergencias-01 | 32 | Emergencias | 10.1.0.34 | 255.255.255.224 | 10.1.0.33 |
| Roosevelt-Quirofanos-01 | 12 | Quirófanos | 10.1.0.66 | 255.255.255.240 | 10.1.0.65 |
| Roosevelt-UCI-01 | 22 | UCI | 10.1.0.82 | 255.255.255.240 | 10.1.0.81 |
| Roosevelt-Laboratorios-01 | 52 | Laboratorios | 10.1.0.98 | 255.255.255.240 | 10.1.0.97 |

### 2. Hospital San Juan de Dios

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| SanJuan-Emergencias-01 | 32 | Emergencias | 10.2.0.2 | 255.255.255.248 | 10.2.0.1 |
| SanJuan-Admin-01 | 42 | Administración | 10.2.0.10 | 255.255.255.248 | 10.2.0.9 |
| SanJuan-Quirofanos-01 | 12 | Quirófanos | 10.2.0.18 | 255.255.255.248 | 10.2.0.17 |
| SanJuan-UCI-01 | 22 | UCI | 10.2.0.26 | 255.255.255.248 | 10.2.0.25 |
| SanJuan-Laboratorios-01 | 52 | Laboratorios | 10.2.0.34 | 255.255.255.248 | 10.2.0.33 |

### 3. Hospital General de Accidentes (IGSS)

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| IGSS3-Quirofanos-01 | 12 | Quirófanos | 10.3.0.2 | 255.255.255.240 | 10.3.0.1 |
| IGSS3-Emergencias-01 | 32 | Emergencias | 10.3.0.18 | 255.255.255.240 | 10.3.0.17 |
| IGSS3-Admin-01 | 42 | Administración | 10.3.0.34 | 255.255.255.240 | 10.3.0.33 |
| IGSS3-UCI-01 | 22 | UCI | 10.3.0.50 | 255.255.255.248 | 10.3.0.49 |
| IGSS3-Laboratorios-01 | 52 | Laboratorios | 10.3.0.58 | 255.255.255.248 | 10.3.0.57 |

### 4. Hospital de Especialidades (IGSS)

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| Especialidades-Quirofanos-01 | 12 | Quirófanos | 10.4.0.2 | 255.255.255.248 | 10.4.0.1 |
| Especialidades-UCI-01 | 22 | UCI | 10.4.0.10 | 255.255.255.248 | 10.4.0.9 |
| Especialidades-Emergencias-01 | 32 | Emergencias | 10.4.0.18 | 255.255.255.248 | 10.4.0.17 |
| Especialidades-Admin-01 | 42 | Administración | 10.4.0.26 | 255.255.255.248 | 10.4.0.25 |
| Especialidades-Laboratorios-01 | 52 | Laboratorios | 10.4.0.34 | 255.255.255.248 | 10.4.0.33 |

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR)

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| HIIR-Emergencias-01 | 32 | Emergencias | 10.5.0.2 | 255.255.255.248 | 10.5.0.1 |
| HIIR-Quirofanos-01 | 12 | Quirófanos | 10.5.0.10 | 255.255.255.248 | 10.5.0.9 |
| HIIR-UCI-01 | 22 | UCI | 10.5.0.18 | 255.255.255.248 | 10.5.0.17 |
| HIIR-Admin-01 | 42 | Administración | 10.5.0.26 | 255.255.255.248 | 10.5.0.25 |
| HIIR-Laboratorios-01 | 52 | Laboratorios | 10.5.0.34 | 255.255.255.248 | 10.5.0.33 |

### 6. Hospital de Salud San Miguel Petapa

| Dispositivo | VLAN | Área | Dirección IP | Máscara | Gateway |
|-------------|------|------|--------------|---------|---------|
| Petapa-Quirofanos-01 | 12 | Quirófanos | 10.6.0.2 | 255.255.255.252 | 10.6.0.1 |
| Petapa-UCI-01 | 22 | UCI | 10.6.0.6 | 255.255.255.252 | 10.6.0.5 |
| Petapa-Emergencias-01 | 32 | Emergencias | 10.6.0.10 | 255.255.255.252 | 10.6.0.9 |
| Petapa-Admin-01 | 42 | Administración | 10.6.0.14 | 255.255.255.252 | 10.6.0.13 |
| Petapa-Laboratorios-01 | 52 | Laboratorios | 10.6.0.18 | 255.255.255.252 | 10.6.0.17 |

### Tabla de VLANs

| VLAN ID | Nombre | Área funcional | Propósito |
|---------|--------|----------------|-----------|
| 12 | Quirofanos | Área 1 | Comunicación entre quirófanos de todos los hospitales |
| 22 | UCI | Área 2 | Monitoreo de pacientes críticos |
| 32 | Emergencias | Área 3 | Atención de urgencias |
| 42 | Administracion | Área 4 | Gestión administrativa |
| 52 | Laboratorios | Área 5 | Pruebas de laboratorio e imágenes médicas |
| 99 | Nativa | (trunk) | VLAN nativa para enlaces troncales |
| 999 | Blackhole | (seguridad) | Puertos no utilizados |

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