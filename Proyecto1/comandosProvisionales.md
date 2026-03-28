## Comandos para ponerle nombre a los switches
enable
configure terminal
hostname SW-A1

## Comandos para configurar el vtp y la versión
enable
configure terminal
vtp version 2
vtp mode client
vtp domain C2_NetCore
vtp password proyecto12026
exit
show vtp status

## Creación de VLANs
enable
configure terminal
vlan 12
name ADMIN
vlan 22
name DOCENTES
vlan 32
name BIBLIOTECA
vlan 42
name LABORATORIO
vlan 52
name VISITANTES
show vlan brief

## Configurar puertos con la VLAN
enable
show interfaces status
show cdp neighbors
configure terminal
interface fastEthernet 0/1
switchport mode access
switchport access vlan 12
no shutdown
exit

## Configuración de trunk
enable
configure terminal
interface fastEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
exit

## Configuración EtherChannel - PAgP
enable
configure terminal
interface range fastEthernet 0/1-2
channel-group 1 mode desirable
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
exit

## Verificación del trunk y EtherChannel
show interfaces trunk
show etherchannel summary
