## Fase 2
### Comandos para ponerle nombre a los switches
```
enable
configure terminal
hostname SW-A1
```

## Fase 3
### Comandos para configurar el vtp y la versión
```
enable
configure terminal
vtp version 2
vtp mode client
vtp domain C2_NetCore
vtp password proyecto12026
exit
show vtp status
```

## Fase 4
### Creación de VLANs
```
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
```

## Fase 5
### Configurar puertos con la VLAN
```
enable
show interfaces status
show cdp neighbors
configure terminal
interface fastEthernet 0/1
switchport mode access
switchport access vlan 12
no shutdown
exit
```

### Configuración de trunk
```
enable
configure terminal
interface fastEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
exit
show interfaces trunk
```

### Comandos de verificación
```
show vlan brief           ! Ver puertos access y su VLAN
show interfaces trunk     ! Ver puertos trunk y VLANs permitidas
show interfaces status    ! Ver estado de todos los puertos
show cdp neighbors        ! Ver los dispositivos que están conectados
show mac address-table    ! Ver MAC que está en cada puerto
show interface fa0/1      ! Ver el estado de un puerto en específico
```

## Fase 6
### Configuración EtherChannel - PAgP
```
enable
configure terminal
interface range fastEthernet 0/1-2
channel-group 1 mode desirable
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
exit
show etherchannel summary
```