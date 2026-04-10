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