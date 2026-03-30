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
### Configuración EtherChannel
#### 1. EtherChannel con LACP (fibra entre edificios)
##### En SW-A1 (lado activo):
```
enable
configure terminal
interface range fastEthernet 0/1-2
channel-group 1 mode active
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
end
write
```

##### En SW-B1 (lado pasivo):
```
enable
configure terminal
interface range fastEthernet 0/1-2
channel-group 1 mode passive
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
end
write
```

#### 2. EtherChannel con PAgP (UTP dentro del mismo edificio)
##### En SW-A2 (lado desirable - inicia):
```
enable
configure terminal
interface range fastEthernet 0/3-4
channel-group 2 mode desirable
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
end
write
```

##### En SW-A3 (lado auto - espera):
```
enable
configure terminal
interface range fastEthernet 0/2-3
channel-group 2 mode auto
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
end
write
```

#### 3. Configurar el Port-Channel (opcional pero recomendado)
##### Después de crear el EtherChannel, puedes configurar directamente la interfaz lógica:
```
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42,52
no shutdown
exit
```

### Comandos de verificación
```
show etherchannel summary
show etherchannel port-channel
show interfaces trunk
```

##### Ejemplo de salida exitosa:
```
Group  Port-channel  Protocol  Ports
------+-------------+---------+----------------------
1      Po1(SU)       LACP     Fa0/1(P)  Fa0/2(P)
2      Po2(SU)       PAgP     Fa0/3(P)  Fa0/4(P)
```

## Tabla de EtherChannels

| Enlace | Switches | Puertos | Protocolo | Modo A | Modo B |
|--------|----------|---------|-----------|--------|--------|
| Fibra A ↔ B | SW-A1 ↔ SW-B1 | Fa0/1-2 | LACP | active | passive |
| Fibra A ↔ C | SW-A1 ↔ SW-C4 | Fa0/3-4 | LACP | active | passive |
| Fibra C ↔ D | SW-C4 ↔ SW-D1 | Fa0/1-2 | LACP | active | passive |
| Fibra D ↔ B | SW-D5 ↔ SW-B2 | Fa0/1-2 | LACP | active | passive |
| Fibra B interno | SW-B1 ↔ SW-B2 | Fa0/3-4 | LACP | active | passive |
| UTP A interno | SW-A2 ↔ SW-A3 | Fa0/3-4 | PAgP | desirable | auto |

## Fase 7
#### Configurar SW-A1 como Root Bridge:
```
enable
configure terminal
spanning-tree mode pvst
spanning-tree vlan 12,22,32,42,52 priority 4096
```

#### En los demás switches, solo activar el modo:
```
enable
configure terminal
spanning-tree mode pvst
```