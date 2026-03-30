## Orden recomendado para configurar todo el proyecto
### Fase 1: Preparación y cableado físico
1. Diseñar la topología en Packet Tracer.

2. Colocar todos los dispositivos (switches, PCs, laptops, hubs, repetidores, access points).

3. Cablear según la jerarquía de medios:

- Fibra OM3 (cable Serial rojo) entre edificios.

- UTP Cat6 (cobre) para distribución dentro del edificio.

- UTP Cat5e (cobre) para acceso de PCs.

4. Verificar que los cables estén en los puertos correctos.

### Fase 2: Configuración básica de switches
1. Hostname en cada switch.

2. Contraseñas y banner MOTD (en al menos 5 switches, uno por edificio).

3. Deshabilitar `ip domain-lookup` (opcional).

### Fase 3: Configuración VTP
1. Primero configurar SW-A1 como Server.

2. Luego configurar todos los demás switches como Client (excepto SW-E1).

3. Finalmente configurar SW-E1 como Transparente.

4. Verificar con show vtp status que todos tengan el mismo dominio y contraseña.

### Fase 4: Creación de VLANs (solo en el Server y en el Transparente)
1. En SW-A1 (Server), crear todas las VLANs:
```
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
```

2. Verificar con `show vlan brief` que se hayan creado.

3. Los switches Client las recibirán automáticamente por VTP.

### Fase 5: Configuración de puertos (acceso y trunk)
1. Puertos de acceso (conectan PCs, laptops, APs):
```
interface fastEthernet 0/5
switchport mode access
switchport access vlan 12
no shutdown
```

2. Puertos troncales (entre switches):
```
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan all
no shutdown
```

### Fase 6: Configuración de EtherChannel
1. Primero los enlaces intra-edificio (PAgP para carnet par).

2. Luego los enlaces inter-edificios (LACP para carnet par).

3. Verificar con `show etherchannel summary`.

### Fase 7: Configuración de STP
1. Configurar SW-A1 como Root Bridge:
```
enable
configure terminal
spanning-tree mode pvst
spanning-tree vlan 12,22,32,42,52 priority 4096
```

2. En los demás switches, solo activar el modo:
```
enable
configure terminal
spanning-tree mode pvst
```

3. Verificar con `show spanning-tree`.

### Fase 8: Configuración de dispositivos finales
1. Asignar IP estática a cada PC y laptop según su VLAN.

2. Verificar con ipconfig en cada dispositivo.

### Fase 9: Pruebas de conectividad
1. Ping dentro de la misma VLAN (debe funcionar).

2. Ping entre distintas VLANs (debe fallar).

3. Documentar 2 pruebas por VLAN (1 exitosa + 1 fallida).


### Fase 10: Verificaciones finales
```
show vlan brief
show interfaces trunk
show spanning-tree
show etherchannel summary
show vtp status
show running-config
```