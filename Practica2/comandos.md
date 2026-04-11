## 0. Configuración base para todos los switches
enable
configure terminal

**Nombre del switch**
hostname SW-Central

**Deshabilitar búsqueda DNS**
no ip domain-lookup

**Configurar contraseña de enable**
enable secret 202300722

**Configurar contraseña de consola**
line console 0
password 202300722
login
exit

**Configurar contraseña para acceso remoto (telnet/ssh)**
line vty 0 15
password 202300722
login
exit

**Guardar configuración inicial**
end
write memory

## 1. Configuración de Principal-SW (Primero)
### 1.1 Configuración base (arriba)
### 1.2 Crear VLANs manualmente
enable
configure terminal

vlan 12
name Quirofanos
exit

vlan 22
name Emergencias
exit

vlan 32
name Administracion
exit

vlan 99
name Nativa
exit

vlan 999
name Blackhole
exit

### 1.3 Configurar VTP como Servidor
enable
configure terminal

vtp mode server
vtp domain 202300722
vtp password area2

**Verificar VTP**
show vtp status

### 1.4 Configurar EtherChannel hacia Este-SW
enable
configure terminal

**Crear Port-Channel 1**
interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Asignar interfaces físicas al Port-Channel 1**
interface range gigabitEthernet 0/1/1-3
channel-group 1 mode desirable
exit

**Verificar EtherChannel**
show etherchannel summary

### 1.5 Configurar EtherChannel hacia Oeste-SW
enable
configure terminal

**Crear Port-Channel 2**
interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Asignar interfaces físicas al Port-Channel 2**
interface range gigabitEthernet 0/2/1-3
channel-group 2 mode desirable
exit

**Verificar EtherChannel**
show etherchannel summary

### 1.6 Configurar STP como Root Bridge
enable
configure terminal

**Activar Rapid PVST+**
spanning-tree mode rapid-pvst

**Hacer este switch Root Bridge para todas las VLANs**
spanning-tree vlan 1-1000 root primary

**Verificar STP**
show spanning-tree summary

## 2. Configuración de Este-SW
### 2.1 Configuración base (cambiar hostname a Este-SW)
### 2.2 Configurar VTP como Cliente
enable
configure terminal

vtp mode client
vtp domain 202300722
vtp password area2

**Verificar VTP (debe mostrar "Client")**
show vtp status

**Verificar que recibió las VLANs**
show vlan brief

### 2.3 Configurar EtherChannel hacia Principal-SW
enable
configure terminal

interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

interface range gigabitEthernet 0/1/1-3
channel-group 1 mode desirable
exit

**Verificar EtherChannel**
show etherchannel summary

### 2.4 Configurar EtherChannel hacia Oeste-SW
enable
configure terminal

interface port-channel 3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Asignar las 3 interfaces físicas hacia NodoOeste**
interface range gigabitEthernet 0/3/1-3
channel-group 3 mode desirable
exit

### 2.5 Configurar puertos hacia los hospitales (acceso normal, sin EtherChannel)
enable
configure terminal

**Puerto hacia Hospital de Especialidades (IGSS)**
interface gigabitEthernet 0/2/1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Puerto hacia Hospital Infantil de Infectología y Rehabilitación (HIIR)**
interface gigabitEthernet 0/2/2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Puerto hacia Hospital de Salud San Miguel Petapa**
interface gigabitEthernet 0/2/3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Verificar puertos trunk**
show interfaces trunk

## 3. Configuración de Oeste-SW (similar a Este-SW)
### 3.1 Configuración base (hostname Oeste-SW)
### 3.2 Configurar VTP como Cliente
enable
configure terminal

vtp mode client
vtp domain 202300722
vtp password area2

**Verificar VTP (debe mostrar "Client")**
show vtp status

**Verificar que recibió las VLANs**
show vlan brief

### 3.3: EtherChannel hacia Principal-SW (Port-Channel 2)
enable
configure terminal

interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

interface range gigabitEthernet 0/1/1-3
channel-group 2 mode desirable
exit

**Verificar EtherChannel**
show etherchannel summary

### 3.4 Configurar EtherChannel hacia Este-SW
enable
configure terminal

interface port-channel 3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Asignar las 3 interfaces físicas hacia Este**
interface range gigabitEthernet 0/3/1-3
channel-group 3 mode desirable
exit

### 3.5 Configurar puertos hacia los hospitales (acceso normal, sin EtherChannel)
enable
configure terminal

**Puerto hacia Hospital Roosevelt**
interface gigabitEthernet 0/2/1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Puerto hacia Hospital San Juan de Dios**
interface gigabitEthernet 0/2/2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Puerto hacia Hospital General de Accidentes (IGSS)**
interface gigabitEthernet 0/2/3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

**Verificar puertos trunk**
show interfaces trunk

## 4-9. Configuración de Switches de Hospital
### X.1 Configuración base (hostname según hospital)
### X.2 VTP como Cliente
enable
configure terminal

vtp mode client
vtp domain 202300722
vtp password area2

show vtp status

**Debe mostrar VLANs 12,22,32,99,999**
show vlan brief

### X.3 Configurar puerto trunk hacia el nodo (uplink)
enable
configure terminal

interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 12,22,32,99
exit

### X.4 Configurar puertos de acceso para hubs (según cada hospital)
#### Ejemplo para Hospital Roosevelt (3 hubs, 5 PCs):
enable
configure terminal

**Hub1 (VLAN12) - puerto Fa0/1**
interface fastEthernet 0/1
switchport mode access
switchport access vlan 12
exit

**Hub2 (VLAN22) - puerto Fa0/2**
interface fastEthernet 0/2
switchport mode access
switchport access vlan 22
exit

**Hub3 (VLAN32,42,52) - puerto Fa0/3**
interface fastEthernet 0/3
switchport mode access
switchport access vlan 32
exit

**Nota: En el Hub3 van conectados 3 PCs (VLAN32,42,52) pero el puerto del switch es uno solo**
**Las VLANs de los PCs dentro del mismo hub deben ser iguales.**
**Ajusta según tu cableado real.**

### X.5 Configurar puertos no utilizados (Blackhole)
enable
configure terminal

**Asume que los puertos Fa0/4 al Fa0/24 están sin usar**
interface range fastEthernet 0/4-24
switchport mode access
switchport access vlan 999
shutdown
exit

### X.6 Verificar configuración
show vlan brief
show interfaces status
show interfaces trunk

## 10. Configuración de PCs
| Campo | Valor según tabla de subnetting |
|---------|--------|
|IP Address|(ver tabla)|
|Subnet Mask|(ver tabla)|
|Default Gateway|(ver tabla)|

## 11. Comandos útiles en switches
**Ver VLANs y puertos asignados**
show vlan brief              

**Ver puertos trunk**
show interfaces trunk

**Ver EtherChannel**
show etherchannel summary

**Ver STP**
show spanning-tree summary

**Ver estado VTP**
show vtp status

**Ver configuración completa**
show running-config