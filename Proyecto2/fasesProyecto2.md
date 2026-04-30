Este archivo es para organizar por fases la realización del proyecto, de esta manera tengo claro que es lo que debo de hacer y comprobar cada que avance donde no me atrase y llegue a mi objetivo que es terminar esta vaina xd.

## Carnet: 202300722 | XX = 22 | Y = 2

---

## Fase 0: Preparación teórica y cálculo de direccionamiento

### 0.1 Definir red base según carnet
Usaremos como red base: `172.22.0.0/16` (por XX=22)

### 0.2 Subnetting para cada sede (VLSM)

**Principio VLSM aplicado:** Ordenar de mayor a menor número de hosts para evitar solapamiento y optimizar el espacio.

| Sede | VLAN | Función | Hosts | Máscara | Subred | Gateway | Broadcast |
|------|------|---------|-------|---------|--------|---------|-----------|
| **Occidente** | 12 | Cajas | 45 | /26 | 172.22.12.0/26 | 172.22.12.1 | 172.22.12.63 |
| | 22 | Asesores | 30 | /27 | 172.22.12.64/27 | 172.22.12.65 | 172.22.12.95 |
| | 42 | Seguridad | 12 | /28 | 172.22.12.96/28 | 172.22.12.97 | 172.22.12.111 |
| | 32 | Gerencia | 10 | /28 | 172.22.12.112/28 | 172.22.12.113 | 172.22.12.127 |
| **Norte** | 52 | Analistas | 50 | /26 | 172.22.13.0/26 | 172.22.13.1 | 172.22.13.63 |
| | 62 | Auditoría | 25 | /27 | 172.22.13.64/27 | 172.22.13.65 | 172.22.13.95 |
| | 72 | Legal | 14 | /28 | 172.22.13.96/28 | 172.22.13.97 | 172.22.13.111 |
| **Oriente** | 92 | Plataforma | 60 | /26 | 172.22.14.0/26 | 172.22.14.1 | 172.22.14.63 |
| | 82 | Bóveda | 40 | /26 | 172.22.14.64/26 | 172.22.14.65 | 172.22.14.127 |
| **Data Center** | 112 | Web Apps | 28 | /27 | 172.22.15.0/27 | 172.22.15.1 | 172.22.15.31 |
| | 102 | Core BD | 14 | /28 | 172.22.15.32/28 | 172.22.15.33 | 172.22.15.47 |
| | 122 | NOC | 10 | /28 | 172.22.15.48/28 | 172.22.15.49 | 172.22.15.63 |

### 0.3 Enlaces punto a punto (FLSM) - Backbone

Se utiliza FLSM con máscara /30 (255.255.255.252) que proporciona 2 hosts útiles por enlace.

| Enlace | Red | Máscara | IPs utilizables |
|--------|-----|---------|-----------------|
| R-Occidente ↔ Núcleo1 | 172.22.255.0/30 | 255.255.255.252 | 172.22.255.1 - 172.22.255.2 |
| R-Norte ↔ Núcleo2 | 172.22.255.4/30 | 255.255.255.252 | 172.22.255.5 - 172.22.255.6 |
| R-Oriente1 ↔ Núcleo1 | 172.22.255.8/30 | 255.255.255.252 | 172.22.255.9 - 172.22.255.10 |
| R-Central1 ↔ Núcleo2 | 172.22.255.12/30 | 255.255.255.252 | 172.22.255.13 - 172.22.255.14 |
| R-Central2 ↔ Núcleo1 | 172.22.255.16/30 | 255.255.255.252 | 172.22.255.17 - 172.22.255.18 |

---

## Fase 1: Investigación arquitectónica (Bitácora personal)

### 1.1 Protocolos a estudiar (ampliado)

#### VLAN y administración
- VLANs (802.1Q)
- Trunking
- VTP (Server, Client, Transparent)
- DTP (negociación de trunk, opcional)

#### Enrutamiento inter-VLAN
- Router-on-a-Stick (subinterfaces)

#### Redundancia y alta disponibilidad
- HSRP (Hot Standby Router Protocol)
- VRRP (Virtual Router Redundancy Protocol)
- EtherChannel (LACP / PAgP)
- Rapid PVST+ (Spanning Tree)

#### Enrutamiento dinámico
- RIPv2 (distance vector)
- OSPF (link state, área 0)
- EIGRP (advanced distance vector, AS 22)

#### Redistribución de rutas
- Entre RIP, OSPF, EIGRP y rutas estáticas
- Métricas y semillas (seed metrics)

#### Otros
- Rutas estáticas predeterminadas (default route)
- ACL básicas (opcional para seguridad)

### 1.2 Comandos clave a documentar

#### Comandos de verificación (show)
```cisco
show vlan brief
show interfaces trunk
show vtp status
show etherchannel summary
show standby              (HSRP)
show ip route
show running-config
show ip interface brief
show spanning-tree
show etherchannel port-channel
```

### Comandos de configuración (por protocolo)

#### VTP
```cisco
vtp domain bantech22
vtp password cisco
vtp mode server | client | transparent
```

#### VLANs
```cisco
vlan 12
name Cajas
```

#### Puertos acceso/trunk
```cisco
interface fastEthernet 0/1
switchport mode access
switchport access vlan 12
no shutdown

interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan all
```

#### EtherChannel (LACP)
```cisco
interface range gigabitEthernet 0/1-2
channel-group 1 mode active
interface port-channel 1
switchport mode trunk
```

#### STP (Rapid PVST+)
```cisco
spanning-tree mode rapid-pvst
spanning-tree vlan 52 priority 4096
```

#### Router-on-a-Stick
```cisco
interface gigabitEthernet 0/0.12
encapsulation dot1Q 12
ip address 172.22.12.1 255.255.255.192
no shutdown
```

#### HSRP
```cisco
interface vlan 82
ip address 172.22.14.2 255.255.255.192
standby 1 ip 172.22.14.1
standby 1 priority 150
standby 1 preempt
```

#### RIP
```cisco
router rip
version 2
no auto-summary
network 172.22.0.0
```

#### EIGRP
```cisco
router eigrp 22
no auto-summary
network 172.22.0.0
```

#### OSPF
```cisco
router ospf 1
router-id 1.1.1.1
network 172.22.0.0 0.0.255.255 area 0
```

#### Redistribución
```cisco
router ospf 1
redistribute rip subnets
redistribute static subnets
default-metric 20

router rip
redistribute ospf 1 metric 2
```

#### Redistribución
```cisco
ip route 0.0.0.0 0.0.0.0 172.22.255.2
```

### 1.4 Bitácora rápida por sede

| Sede | Tecnología principal | Comandos clave |
|------|---------------------|----------------|
| Occidente | Router-on-a-Stick | subinterfaces, encapsulation dot1Q |
| Norte | Rapid PVST+ | spanning-tree mode, priority |
| Oriente | HSRP | standby ip, priority, preempt |
| Data Center | EtherChannel (LACP) | channel-group mode active, port-channel |
| Backbone | RIP, EIGRP, OSPF, redistribución | router rip/eigrp/ospf, redistribute |

---

## Fase 2: Diseño lógico y físico (antes de Packet Tracer)
**Para colocar los dispositivos finales, se colocan 1/10 de los hosts que pide el enunciado, por ejemplo si pide 45 hosts se pondrán 4.5 donde aproximando serían 5 dispositivos de esa área**

### 2.1 Tabla de VLANs (crear en VTP Server)
| Sede | VLAN | Función | Subred | Gateway |
|------|------|---------|--------|---------|
| Occidente | 12 | Cajas | 172.22.12.0/26 | 172.22.12.1 |
| | 22 | Asesores | 172.22.22.0/27 | 172.22.22.1 |
| | 32 | Gerencia | 172.22.32.0/28 | 172.22.32.1 |
| | 42 | Seguridad | 172.22.42.0/28 | 172.22.42.1 |
| Norte | 52 | Analistas | 172.22.52.0/26 | 172.22.52.1 |
| | 62 | Auditoría | 172.22.62.0/27 | 172.22.62.1 |
| | 72 | Legal | 172.22.72.0/28 | 172.22.72.1 |
| Oriente | 82 | Bóveda | 172.22.82.0/26 | 172.22.82.1 |
| | 92 | Plataforma | 172.22.92.0/26 | 172.22.92.1 |
| Data Center | 102 | Core BD | 172.22.102.0/28 | 172.22.102.1 |
| | 112 | Web Apps | 172.22.112.0/27 | 172.22.112.1 |
| | 122 | NOC | 172.22.122.0/28 | 172.22.122.1 |

### 2.2 Topologías propuestas por sede
**Backbone**
- 2 routers (R-Núcleo1, R-Núcleo2) + 4 routers de las sedes (R-Occidente, R-Norte, R-Oriente, R-Central2)

| Dispositivo 1 | Puerto | Dispositivo 2 | Puerto | Medio | Modo |
|---------------|--------|---------------|--------|-------|------|
| R-Núcleo1 | Gi0/0 | R-Núcleo2 | Gi0/0 | Fibra Óptica | Enlace Principal |
| R-Núcleo1 | Gi0/1 | R-Núcleo2 | Gi0/1 | Fibra Óptica | EtherChannel |
| R-Núcleo1 | Se0/2/0 | R-Occidente | Se0/0/0 | Serial DCE | WAN a Occidente (RIP) |
| R-Núcleo1 | Se0/2/1 | R-Norte | Se0/0/0 | Serial DCE | WAN a Norte (EIGRP) |
| R-Núcleo2 | Se0/2/0 | R-Oriente | Se0/0/0 | Serial DCE | WAN a Oriente (OSPF) |
| R-Núcleo2 | Se0/2/1 | R-Central1 | Se0/0/0 | Serial DCE | WAN a Central (Estático) |

**Occidente (Router-on-a-Stick)**
- 1 router (R-Occidente) + 2 switches de acceso (SW-Occidente1, SW-Occidente2)
- Enlace troncal entre switches
- VTP dominio: bantech22

| Dispositivo 1 | Puerto | Dispositivo 2 | Puerto | Medio |
|---------------|--------|---------------|--------|-------|
| R-Occidente | Gi0/0 | SW-Occidente1 | Gi0/1 | UTP Cat6 |
| SW-Occidente1 | Gi0/2 | SW-Occidente2 | Gi0/1 | UTP Cat6 |
| SW-Occidente1 | Fa0/1 | PC Cajas 1 | Fa0 | UTP Cat5e |
| SW-Occidente1 | Fa0/2 | PC Cajas 2 | Fa0 | UTP Cat5e |
| SW-Occidente1 | Fa0/3 | PC Cajas 3 | Fa0 | UTP Cat5e |
| SW-Occidente1 | Fa0/4 | PC Cajas 4 | Fa0 | UTP Cat5e |
| SW-Occidente1 | Fa0/5 | PC Cajas 5 | Fa0 | UTP Cat5e |
| SW-Occidente2 | Fa0/1 | PC Asesores 1 | Fa0 | UTP Cat5e |
| SW-Occidente2 | Fa0/2 | PC Asesores 2 | Fa0 | UTP Cat5e |
| SW-Occidente2 | Fa0/3 | PC Asesores 3 | Fa0 | UTP Cat5e |
| SW-Occidente2 | Fa0/4 | PC Gerencia | Fa0 | UTP Cat5e |
| SW-Occidente2 | Fa0/5 | PC Seguridad | Fa0 | UTP Cat5e |

**Norte (Redundancia Capa 2 con Rapid PVST+)**
- 1 router (R-Norte) + Triángulo de switches (SW-Norte1, SW-Norte2, SW-Norte3)
- Bucle intencional con STP
- Root Bridge: SW-Norte1 (priority 4096)

| Dispositivo 1 | Puerto | Dispositivo 2 | Puerto | Medio |
|---------------|--------|---------------|--------|-------|
| R-Norte | Gi0/0 | SW-Norte1 | Fa0/1 | UTP Cat6 |
| SW-Norte1 | Gi0/1 | SW-Norte2 | Gi0/1 | UTP Cat6 |
| SW-Norte1 | Gi0/2 | SW-Norte3 | Gi0/1 | UTP Cat6 |
| SW-Norte2 | Gi0/2 | SW-Norte3 | Gi0/2 | UTP Cat6 |
| SW-Norte2 | Fa0/1 | PC Analistas 1 | Fa0 | UTP Cat5e |
| SW-Norte2 | Fa0/2 | PC Analistas 2 | Fa0 | UTP Cat5e |
| SW-Norte2 | Fa0/3 | PC Analistas 3 | Fa0 | UTP Cat5e |
| SW-Norte2 | Fa0/4 | PC Analistas 4 | Fa0 | UTP Cat5e |
| SW-Norte2 | Fa0/5 | PC Analistas 5 | Fa0 | UTP Cat5e |
| SW-Norte3 | Fa0/1 | PC Auditoria 1 | Fa0 | UTP Cat5e |
| SW-Norte3 | Fa0/2 | PC Auditoria 2 | Fa0 | UTP Cat5e |
| SW-Norte3 | Fa0/3 | PC Auditoria 3 | Fa0 | UTP Cat5e |
| SW-Norte3 | Fa0/4 | PC Legal | Fa0 | UTP Cat5e |

**Oriente (HSRP con MS1 y MS2)**
- MS1 y MS2 como switches multicapa
- Gateway virtual HSRP para VLAN 82 y 92
- Prioridad HSRP más alta en MS1

| Dispositivo 1 | Puerto | Dispositivo 2 | Puerto | Medio |
|---------------|--------|---------------|--------|-------|
| R-Oriente | Gi0/0 | SW-Oriente | Fa0/1 | UTP Cat6 |
| SW-Oriente | Gi0/1 | Multilayer Switch 1 | Gi0/1 | UTP Cat6 |
| SW-Oriente | Gi0/2 | Multilayer Switch 2 | Gi0/1 | UTP Cat6 |
| Multilayer Switch 1 | Gi0/2 | Multilayer Switch 2 | Gi0/2 | UTP Cat6 |
| Multilayer Switch 1 | Fa0/1 | PC Bóveda 1 | Fa0 | UTP Cat5e |
| Multilayer Switch 1 | Fa0/2 | PC Bóveda 2 | Fa0 | UTP Cat5e |
| Multilayer Switch 1 | Fa0/3 | PC Bóveda 3 | Fa0 | UTP Cat5e |
| Multilayer Switch 1 | Fa0/4 | PC Bóveda 4 | Fa0 | UTP Cat5e |
| Multilayer Switch 2 | Fa0/1 | PC Plataforma 1 | Fa0 | UTP Cat5e |
| Multilayer Switch 2 | Fa0/2 | PC Plataforma 2 | Fa0 | UTP Cat5e |
| Multilayer Switch 2 | Fa0/3 | PC Plataforma 3 | Fa0 | UTP Cat5e |
| Multilayer Switch 2 | Fa0/4 | PC Plataforma 4 | Fa0 | UTP Cat5e |
| Multilayer Switch 2 | Fa0/5 | PC Plataforma 5 | Fa0 | UTP Cat5e |
| Multilayer Switch 2 | Fa0/6 | PC Plataforma 6 | Fa0 | UTP Cat5e |

**Data Center (EtherChannel con LACP)**
- R-Central2 + 2 switches de acceso (SW-DataCenter1, SW-DataCenter2)
- EtherChannel entre switches y servidores (LACP)
- Enlaces estáticos hacia R-Central1

| Dispositivo 1 | Puerto | Dispositivo 2 | Puerto | Medio |
|---------------|--------|---------------|--------|-------|
| R-Central1 | Gi0/0 | R-Central2 | Gi0/0 | UTP Cat6 |
| R-Central2 | Gi0/1 | SW-Central1 | Gi0/1 | UTP Cat6 |
| SW-Central1 | Fa0/1 | SW-Central2 | Fa0/1 | UTP Cat6 |
| SW-Central2 | Fa0/2 | SW-Central2 | Fa0/2 | UTP Cat6 |
| SW-Central2 | Fa0/3 | S-CoreDB | Fa0 | UTP Cat5e |
| SW-Central2 | Fa0/4 | S-WebApps1 | Fa0 | UTP Cat5e |
| SW-Central2 | Fa0/5 | S-WebApps2 | Fa0 | UTP Cat5e |
| SW-Central2 | Fa0/6 | S-WebApps3 | Fa0 | UTP Cat5e |
| SW-Central2 | Fa0/7 | S-NOC | Fa0 | UTP Cat5e |

### 2.3 Tabla de Direccionamiento IP Completa

#### Backbone Core (Enlaces Punto a Punto)

| Enlace | Red / Máscara | Dispositivo A | Interfaz | IP A | Dispositivo B | Interfaz | IP B |
|--------|---------------|---------------|----------|------|---------------|----------|------|
| Núcleo1 ↔ Núcleo2 | 172.22.255.16/30 | R-Núcleo1 | Port-channel1 | 172.22.255.17 | R-Núcleo2 | Port-channel1 | 172.22.255.18 |
| Núcleo1 ↔ R-Occidente | 172.22.255.0/30 | R-Núcleo1 | Se0/0/0 | 172.22.255.1 | R-Occidente | Se0/0/0 | 172.22.255.2 |
| Núcleo1 ↔ R-Norte | 172.22.255.4/30 | R-Núcleo1 | Se0/0/1 | 172.22.255.5 | R-Norte | Se0/0/0 | 172.22.255.6 |
| Núcleo2 ↔ R-Oriente | 172.22.255.8/30 | R-Núcleo2 | Se0/0/0 | 172.22.255.9 | R-Oriente | Se0/0/0 | 172.22.255.10 |
| Núcleo2 ↔ R-Central1 | 172.22.255.12/30 | R-Núcleo2 | Se0/0/1 | 172.22.255.13 | R-Central1 | Se0/0/0 | 172.22.255.14 |
| R-Central1 ↔ R-Central2 | 172.22.255.20/30 | R-Central1 | Gi0/0 | 172.22.255.21 | R-Central2 | Gi0/0 | 172.22.255.22 |

#### Sede Occidente

| Dispositivo | Interfaz | VLAN | IP / Máscara | Gateway |
|-------------|----------|------|--------------|---------|
| **R-Occidente** | Se0/0/0 | - | 172.22.255.2/30 | - |
| | Gi0/0.12 | 12 | 172.22.12.1/26 | - |
| | Gi0/0.22 | 22 | 172.22.22.1/27 | - |
| | Gi0/0.32 | 32 | 172.22.32.1/28 | - |
| | Gi0/0.42 | 42 | 172.22.42.1/28 | - |
| **PC-Cajas 1** | Fa0 | 12 | 172.22.12.2/26 | 172.22.12.1 |
| **PC-Cajas 2** | Fa0 | 12 | 172.22.12.3/26 | 172.22.12.1 |
| **PC-Cajas 3** | Fa0 | 12 | 172.22.12.4/26 | 172.22.12.1 |
| **PC-Cajas 4** | Fa0 | 12 | 172.22.12.5/26 | 172.22.12.1 |
| **PC-Cajas 5** | Fa0 | 12 | 172.22.12.6/26 | 172.22.12.1 |
| **PC-Asesores 1** | Fa0 | 22 | 172.22.22.2/27 | 172.22.22.1 |
| **PC-Asesores 2** | Fa0 | 22 | 172.22.22.3/27 | 172.22.22.1 |
| **PC-Asesores 3** | Fa0 | 22 | 172.22.22.4/27 | 172.22.22.1 |
| **PC-Gerencia** | Fa0 | 32 | 172.22.32.2/28 | 172.22.32.1 |
| **PC-Seguridad** | Fa0 | 42 | 172.22.42.2/28 | 172.22.42.1 |

#### Sede Norte

| Dispositivo | Interfaz | VLAN | IP / Máscara | Gateway |
|-------------|----------|------|--------------|---------|
| **R-Norte** | Se0/0/0 | - | 172.22.255.6/30 | - |
| | Gi0/0.52 | 52 | 172.22.52.1/26 | - |
| | Gi0/0.62 | 62 | 172.22.62.1/27 | - |
| | Gi0/0.72 | 72 | 172.22.72.1/28 | - |
| **PC-Analistas 1** | Fa0 | 52 | 172.22.52.2/26 | 172.22.52.1 |
| **PC-Analistas 2** | Fa0 | 52 | 172.22.52.3/26 | 172.22.52.1 |
| **PC-Analistas 3** | Fa0 | 52 | 172.22.52.4/26 | 172.22.52.1 |
| **PC-Analistas 4** | Fa0 | 52 | 172.22.52.5/26 | 172.22.52.1 |
| **PC-Analistas 5** | Fa0 | 52 | 172.22.52.6/26 | 172.22.52.1 |
| **PC-Auditoria 1** | Fa0 | 62 | 172.22.62.2/27 | 172.22.62.1 |
| **PC-Auditoria 2** | Fa0 | 62 | 172.22.62.3/27 | 172.22.62.1 |
| **PC-Auditoria 3** | Fa0 | 62 | 172.22.62.4/27 | 172.22.62.1 |
| **PC-Legal** | Fa0 | 72 | 172.22.72.2/28 | 172.22.72.1 |

#### Sede Oriente

| Dispositivo | Interfaz | VLAN | IP / Máscara | Gateway (HSRP) |
|-------------|----------|------|--------------|----------------|
| **R-Oriente** | Se0/0/0 | - | 172.22.255.10/30 | - |
| | Gi0/0 | - | (Trunk, sin IP) | - |
| **MS1** | VLAN 82 | 82 | 172.22.82.2/26 | 172.22.82.1 (virtual) |
| | VLAN 92 | 92 | 172.22.92.2/26 | 172.22.92.1 (virtual) |
| **MS2** | VLAN 82 | 82 | 172.22.82.3/26 | 172.22.82.1 (virtual) |
| | VLAN 92 | 92 | 172.22.92.3/26 | 172.22.92.1 (virtual) |
| **PC-Bóveda 1** | Fa0 | 82 | 172.22.82.4/26 | 172.22.82.1 |
| **PC-Bóveda 2** | Fa0 | 82 | 172.22.82.5/26 | 172.22.82.1 |
| **PC-Bóveda 3** | Fa0 | 82 | 172.22.82.6/26 | 172.22.82.1 |
| **PC-Bóveda 4** | Fa0 | 82 | 172.22.82.7/26 | 172.22.82.1 |
| **PC-Plataforma 1** | Fa0 | 92 | 172.22.92.4/26 | 172.22.92.1 |
| **PC-Plataforma 2** | Fa0 | 92 | 172.22.92.5/26 | 172.22.92.1 |
| **PC-Plataforma 3** | Fa0 | 92 | 172.22.92.6/26 | 172.22.92.1 |
| **PC-Plataforma 4** | Fa0 | 92 | 172.22.92.7/26 | 172.22.92.1 |
| **PC-Plataforma 5** | Fa0 | 92 | 172.22.92.8/26 | 172.22.92.1 |
| **PC-Plataforma 6** | Fa0 | 92 | 172.22.92.9/26 | 172.22.92.1 |

#### Data Center

| Dispositivo | Interfaz | VLAN | IP / Máscara | Gateway |
|-------------|----------|------|--------------|---------|
| **R-Central1** | Se0/0/0 | - | 172.22.255.14/30 | - |
| | Gi0/0 | - | 172.22.255.21/30 | - |
| **R-Central2** | Gi0/0 | - | 172.22.255.22/30 | - |
| | Gi0/1.102 | 102 | 172.22.102.1/28 | - |
| | Gi0/1.112 | 112 | 172.22.112.1/27 | - |
| | Gi0/1.122 | 122 | 172.22.122.1/28 | - |
| **S-CoreBD** | Fa0 | 102 | 172.22.102.2/28 | 172.22.102.1 |
| **S-WebApps 1** | Fa0 | 112 | 172.22.112.2/27 | 172.22.112.1 |
| **S-WebApps 2** | Fa0 | 112 | 172.22.112.3/27 | 172.22.112.1 |
| **S-WebApps 3** | Fa0 | 112 | 172.22.112.4/27 | 172.22.112.1 |
| **S-NOC** | Fa0 | 122 | 172.22.122.2/28 | 172.22.122.1 |

### 2.4 Justificación de topologías

**Occidente:** Se eligió Router-on-a-Stick con dos switches de acceso porque es una solución económica y fácil de administrar. La segmentación por VLANs aísla el tráfico crítico de cajas (VLAN 12) del resto de áreas, cumpliendo la necesidad operativa de que una falla en asesores no afecte las transacciones.

**Norte:** Se implementó un triángulo de switches con Rapid PVST+ para eliminar puntos únicos de falla. SW-Norte1 fue configurado como Root Bridge por su ubicación central en la topología, garantizando caminos de respaldo predecibles. Esta redundancia de Capa 2 asegura que los analistas de crédito nunca pierdan conectividad.

**Oriente:** Se utilizaron dos switches multicapa (3560) con HSRP para proporcionar alta disponibilidad del gateway. MS1 tiene prioridad 150 (activo) y MS2 prioridad 100 (standby). Si MS1 falla, MS2 asume el gateway virtual sin que los usuarios perciban interrupción.

**Data Center:** Se implementó EtherChannel con LACP entre SW-Access y R-CoreBD (servidor de base de datos crítico) para sumar ancho de banda y proporcionar redundancia. Los servidores web y NOC usan enlaces simples por ser menos críticos. El segmento usa rutas estáticas por políticas de seguridad.

---

## Fase 3: Implementación en Packet Tracer

### 3.1 Cableado físico
- Fibra óptica (Serial rojo) entre routers del backbone
- UTP Cat6 entre switches dentro de cada sede
- UTP Cat5e para PCs finales

### 3.2 Configuración básica de switches
```cisco
enable
configure terminal
hostname (Nombre del switch)
enable secret cisco
line console 0
password cisco
login
line vty 0 15
password cisco
login
banner motd #Acceso no autorizado. Este sistema es propiedad de BanTech GT#
no ip domain-lookup
```

--- 

## Fase 4: Configuración VTP

### 4.1 Servidor VTP en la sede Occidente, Norte, Oriente y Central (SW-Occidente1, SW-Norte1, SW-Oriente y SW-Dist1)
```cisco
enable
configure terminal
vtp domain bantech22
vtp password cisco
vtp version 2
vtp mode server
end
show vtp status
```

### 4.2 Clientes VTP (resto de switches)
```cisco
enable
configure terminal
vtp domain bantech22
vtp password cisco
vtp version 2
vtp mode client
end
show vtp status
```

### 4.3 Switch transparente (Oriente - MS1 y MS2)
```cisco
enable
configure terminal
vtp domain bantech22
vtp password cisco
vtp version 2
vtp mode transparent
end
show vtp status
```

---

## Fase 5: Creación de VLANs (solo en Server y Transparente)

### 5.1 En VTP Server en la sede occidente (SW-Occidente1)
```cisco
vlan 12
name Cajas
vlan 22
name Asesores
vlan 32
name Gerencia
vlan 42
name Seguridad
```

### 5.2 En VTP Server en la sede norte (SW-Norte1)
```cisco
vlan 52
name Analistas
vlan 62
name Auditoria
vlan 72
name Legal
```

### 5.3 En VTP Transparente en la sede oriente (MS1 y MS2)
```cisco
vlan 82
name Boveda
vlan 92
name Plataforma
```

### 5.4 En VTP Server en la sede central (SW-Dist1)
```cisco
vlan 102
name CoreBD
vlan 112
name WebApps
vlan 122
name NOC
```

---

## Fase 6: Puertos de acceso y trunk

### 6.1 Puertos de acceso (entre Switch ↔ PC o MS ↔ PC)
```cisco
enable
configure terminal
interface fastEthernet 0/1
switchport mode access
switchport access vlan 12
no shutdown
```

### 6.2 Puertos trunk (entre Switch ↔ Switch o entre MS ↔ MS)
**En la sede occidente**
```cisco
enable
configure terminal
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42
no shutdown
```

**En la sede norte**
```cisco
enable
configure terminal
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 52,62,72
no shutdown
```

**En la sede oriente**
```cisco
enable
configure terminal
interface gigabitEthernet 0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 82,92
no shutdown
```

**En la sede central**
```cisco
enable
configure terminal
interface gigabitEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 102,112,122
no shutdown
```


### 6.3 Puertos trunk (entre MS ↔ Switch)
```cisco
enable
configure terminal
interface g0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 82,92
no shutdown
```

---

## Fase 7: Router-on-a-Stick (Sede Occidente)

### 7.1 Configuración del router R-Occidente para Inter-VLAN
```cisco
interface gigabitEthernet 0/0.12
encapsulation dot1Q 12
ip address 172.22.12.1 255.255.255.192
interface gigabitEthernet 0/0.22
encapsulation dot1Q 22
ip address 172.22.22.1 255.255.255.224
interface gigabitEthernet 0/0.32
encapsulation dot1Q 32
ip address 172.22.32.1 255.255.255.240
interface gigabitEthernet 0/0.42
encapsulation dot1Q 42
ip address 172.22.42.1 255.255.255.240
```

### 7.2 Configuración del trunk (entre SW-Occidente1 ↔ R-Occidente)
```cisco
enable
configure terminal
interface range fastEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 12,22,32,42
```

---

## Fase 8: STP (Rapid PVST+) (Norte)

### 8.1 Root Bridge en Norte (SW-Norte1)
```cisco
enable
configure terminal
spanning-tree mode rapid-pvst
spanning-tree vlan 52,62,72 priority 4096
```

### 8.2 Demás switches (SW-Norte2 y SW-Norte3)
```cisco
enable
configure terminal
spanning-tree mode rapid-pvst
```

### 8.3 Activar PortFast en puertos de usuarios
```cisco
enable
configure terminal
interface range fastEthernet 0/1-10
spanning-tree portfast
```

### 8.4 Configuración del router R-Norte para Inter-VLAN
```cisco
interface gigabitEthernet 0/0.52
encapsulation dot1Q 52
ip address 172.22.52.1 255.255.255.192
interface gigabitEthernet 0/0.62
encapsulation dot1Q 62
ip address 172.22.62.1 255.255.255.224
interface gigabitEthernet 0/0.72
encapsulation dot1Q 72
ip address 172.22.72.1 255.255.255.240
```

### 8.5 Configuración del trunk (entre SW-Norte1 ↔ R-Norte)
```cisco
enable
configure terminal
interface range fastEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 52,62,72
```

---

## Fase 9: HSRP (Oriente)

### 9.1 En MS1 (activo)
```cisco
enable
configure terminal
ip routing

interface vlan 82
ip address 172.22.82.2 255.255.255.192
standby 1 ip 172.22.82.1
standby 1 priority 150
standby 1 preempt

interface vlan 92
ip address 172.22.92.2 255.255.255.192
standby 2 ip 172.22.92.1
standby 2 priority 150
standby 2 preempt
```

### 9.2 En MS2 (standby)
```cisco
enable
configure terminal
ip routing

interface vlan 82
ip address 172.22.82.3 255.255.255.192
standby 1 ip 172.22.82.1
standby 1 priority 100

interface vlan 92
ip address 172.22.92.3 255.255.255.192
standby 2 ip 172.22.92.1
standby 2 priority 100
```

---

## Fase 10: EtherChannel (Data Center)

### 10.1 En SW-Central1
```cisco
enable
configure terminal

interface range fa0/1-2
switchport mode trunk
channel-group 1 mode active

interface port-channel 1
switchport mode trunk
```

### 10.2 En SW-Central2
```cisco
enable
configure terminal

interface range fa0/1-2
switchport mode trunk
channel-group 1 mode active

interface port-channel 1
switchport mode trunk
```

### 10.3 Configuración del router R-Central2 para Inter-VLAN
```cisco
interface gigabitEthernet 0/1.102
encapsulation dot1Q 102
ip address 172.22.102.1 255.255.255.240
interface gigabitEthernet 0/1.112
encapsulation dot1Q 112
ip address 172.22.112.1 255.255.255.224
interface gigabitEthernet 0/1.122
encapsulation dot1Q 122
ip address 172.22.122.1 255.255.255.240
```

### 10.4 Configuración del trunk (entre SW-Central1 ↔ R-Central2)
```cisco
enable
configure terminal
interface range fastEthernet 0/3
switchport mode trunk
switchport trunk allowed vlan 102,112,122
``` 

---


## Fase 11: Backbone Core y enrutamiento

### 11.1 Protocolos por router
| Router | Protocolo |
| R-Occidente | RIP |
| R-Norte | OSPF |
| R-Oriente | EIGRP |
| R-Central1 | OSPF + Redistribución estática |
| R-Central2 | Solo rutas estáticas |
| R-Núcleo1 | OSPF + redistribución RIP |
| R-Núcleo2 | OSPF + redistribución EIGRP |

### 11.2 Configuración de R-Núcleo1
```cisco
enable
configure terminal

router ospf 1
router-id 1.1.1.1
network 172.22.255.0 0.0.0.3 area 0
network 172.22.255.4 0.0.0.3 area 0
network 172.22.255.16 0.0.0.3 area 0
network 172.22.255.20 0.0.0.3 area 0

redistribute rip subnets

router rip
version 2
no auto-summary
network 172.22.0.0
```

### 11.3 Configuración de R-Núcleo2
```cisco
enable
configure terminal

router ospf 1
router-id 2.2.2.2
network 172.22.255.8 0.0.0.3 area 0
network 172.22.255.12 0.0.0.3 area 0
network 172.22.255.16 0.0.0.3 area 0
network 172.22.255.20 0.0.0.3 area 0

redistribute eigrp 10 subnets

router eigrp 10
no auto-summary
network 172.22.0.0
```

### 11.4 Configuración de R-Occidente (RIP)
```cisco
enable
configure terminal

router rip
version 2
no auto-summary
network 172.22.0.0
```

### 11.5 Configuración de R-Norte (OSPF)
```cisco
enable
configure terminal

router ospf 1
router-id 3.3.3.3
network 172.22.255.4 0.0.0.3 area 0
network 172.22.52.0 0.0.0.63 area 0
network 172.22.62.0 0.0.0.31 area 0
network 172.22.72.0 0.0.0.15 area 0
```

### 11.6 Configuración de R-Oriente (EIGRP)
```cisco
enable
configure terminal

router eigrp 10
no auto-summary
network 172.22.0.0
```

### 11.7 Configuración de R-Central1
```cisco
enable
configure terminal

ip route 172.22.102.0 255.255.255.240 172.22.255.22
ip route 172.22.112.0 255.255.255.224 172.22.255.22
ip route 172.22.122.0 255.255.255.240 172.22.255.22

router ospf 1
router-id 4.4.4.4

network 172.22.255.12 0.0.0.3 area 0
network 172.22.255.20 0.0.0.3 area 0

redistribute static subnets
```

### 11.8 Configuración de R-Central2 (estático)
```cisco
enable
configure terminal

ip route 0.0.0.0 0.0.0.0 172.22.255.21
```

### 11.9 Sobre el doble enlace entre los núcleos
#### En R-Núcleo1
```cisco
enable
configure terminal

interface gigabitEthernet0/0/0
ip address 172.22.255.17 255.255.255.252
no shutdown

interface gigabitEthernet0/0/1
ip address 172.22.255.21 255.255.255.252
no shutdown
```

#### En R-Núcleo2
```cisco
enable
configure terminal

interface gigabitEthernet0/0/0
ip address 172.22.255.18 255.255.255.252
no shutdown

interface gigabitEthernet0/0/1
ip address 172.22.255.22 255.255.255.252
no shutdown
```

## 11.10 Comprobar que todo funciona correctamente
```cisco
enable
show ip route
show ip interface brief
```
Se tendría que ver algo como:
**Rutas R (RIP)**
**Rutas D (EIGRP)**
**Rutas O (OSPF)**
**Rutas S (Static)**

---

## Fase 12: Dispositivos finales (PCs)
| Sede | VLAN | IP asignada |
| Occidente | 12 | 172.22.12.2/26 |
| Occidente | 22 | 172.22.12.66/27 |
| Norte | 52 | 172.22.13.2/26 |
| Oriente | 82 | 172.22.14.2/26 | 172.22.255
| Data Center | 102 | 172.22.15.2/28 |
**Gateway = primera IP de cada subred**

---

## Fase 13: Pruebas de conectividad

### 13.1 Dentro de misma VLAN (debe funcionar)
ping 172.22.12.2 → 172.22.12.3

### 13.2 Entre VLANs distintas (debe funcionar por router-on-a-stick)
ping 172.22.12.2 → 172.22.13.2

### 13.3 Entre sedes diferentes
ping 172.22.12.2 → 172.22.15.2

### 13.4 Prueba de redundancia HSRP
Apagar interfaz de MS1 → verificar que MS2 toma el gateway virtual

### 13.5 Prueba de EtherChannel
Apagar un cable del port-channel → tráfico sigue funcionando

---

## Fase 14: Documentación final (ManualTecnico.md)

### 14.1 Estructura del ManualTecnico
- Introducción
- Tablas de subnetting y VLANs
- Topologías justificadas por sede
- Capturas de configuración (show running-config)
- Capturas de pruebas de ping y failover
- Enlace al archivo .pkt

### 14.2 Entregables
- Repositorio GitHub privado
- ManualTecnico.md con todo lo anterior
- Archivo .pkt funcional
- Agregar como colaborador a auxiliar según sección

---

## Fase 15: Verificaciones finales (checklist)
```cisco
show vlan brief
show interfaces trunk
show vtp status
show etherchannel summary
show spanning-tree
show standby
show ip route
show running-config
ping de extremo a extremo
```

--- 

## Resumen de hitos por fecha (Cronograma)
| Fase | Actividad | Fecha estimada |
| 0-1 | Investigación y subnetting | 13-15/04 |
| 2 | Diseño topologías | 16-18/04 |
| 3-6 | Cableado, VTP, VLANs, trunks | 19-22/04 |
| 7-10 | EtherChannel, STP, Router-on-a-Stick, HSRP | 23-25/04 |
| 11 | Backbone y enrutamiento | 26-27/04 |
| 12-13 | PCs y pruebas | 28/04 |
| 14-15 | Documentación y entrega | 29-30/04 |