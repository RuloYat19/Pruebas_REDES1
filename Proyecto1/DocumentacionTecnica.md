## Universidad de San Carlos de Guatemala
## Facultad de ingeniería
## Laboratorio Redes de Computadoras 1
## Sección A
## Auxiliar: Roni Eduardo Vásquez Flores

![Logo USAC](Imagenes/LogoUSAC.jpeg)

### PROYECTO # 1

### NetCore Academy
### Documentación Técnica

| Nombre | Carnet |
|---|---|
| Raúl Emanuel Yat Cancinos | 202300722 |

## Capturas de la topología completa y de cada área
### Captura completa
![Captura completa](Imagenes/I1.png)

### Edificio A
![Edificio A](Imagenes/I2.png)

### Edificio B
![Edificio B](Imagenes/I3.png)

### Edificio C
![Edificio C](Imagenes/I4.png)

### Edificio D
![Edificio D](Imagenes/I5.png)

### Edificio E o Parte E
![Edificio E o Parte E](Imagenes/I6.png)

## Topología etiquetando los tipos de interfaces y medios de transmisión por segmento
### Captura completa
![Captura completa](Imagenes/I7.png)

### Edificio A
![Edificio A](Imagenes/I8.png)

### Edificio B
![Edificio B](Imagenes/I9.png)

### Edificio C
![Edificio C](Imagenes/I10.png)

### Edificio D
![Edificio D](Imagenes/I11.png)

### Edificio E o Parte E
![Edificio E o Parte E](Imagenes/I12.png)

## Tabla de dominios de colisión por área
| Edificio | Números de dominios de colisión | ¿Cuáles son? |
|---|---|---|
| Edficio A | 5 | Conexiones de SW-A2 a Admin2, Laboratorio4, AC-A1, SW-A1 y SW-A3 |
| Edficio B | 8 | 1 dominio del Hub-B1 + 3 enlaces entre switches + 4 PCs conectadas directamente a switches |
| Edficio C | 1 | Único dominio compartido por todo el edificio debido al Hub-C1 |
| Edficio D | 11 | 1 dominio del Repeater-D1 + 6 enlaces entre switches + 4 PCs fuera del repeater |

## Tabla de dominios de broadcast
| VLAN ID | Nombre VLAN | Dominio de broadcast |
|---------|-------------|----------------------|
| 12 | ADMIN | 1 dominio donde todos los puertos están en VLAN 15 |
| 22 | DOCENTES | 1 dominio donde todos los puertos están en VLAN 25 |
| 32 | BIBLIOTECA | 1 dominio donde todos los puertos están en VLAN 35 |
| 42 | LABORATORIO | 1 dominio donde todos los puertos están en VLAN 45 |
| 52 | VISITANTES | 1 dominio donde todos los puertos están en VLAN 55 |

**Total de dominios de broadcast:** 5

## Lista de comandos utilizadas en cada dispositivo
### SW-A1 (Server VTP - Edificio A)
![SW-A1](Imagenes/I13.png)

### SW-A2 (Cliente VTP - Edificio A)
![SW-A2](Imagenes/I14.png)

### SW-A3 (Cliente VTP - Edificio A)
![SW-A3](Imagenes/I15.png)

### SW-B1 (Cliente VTP - Edificio B)
![SW-B1](Imagenes/I16.png)

### SW-B2 (Cliente VTP - Edificio B)
![SW-B2](Imagenes/I17.png)

### SW-C1 (Cliente VTP - Edificio C)
![SW-C1](Imagenes/I19.png)

### SW-C2 (Cliente VTP - Edificio C)
![SW-C2](Imagenes/I20.png)

### SW-C3 (Cliente VTP - Edificio C)
![SW-C3](Imagenes/I21.png)

### SW-C4 (Cliente VTP - Edificio C)
![SW-C4](Imagenes/I18.png)

### SW-D1 (Cliente VTP - Edificio D)
![SW-D1](Imagenes/I22.png)

### SW-D2 (Cliente VTP - Edificio D)
![SW-D2](Imagenes/I23.png)

### SW-D3 (Cliente VTP - Edificio D)
![SW-D3](Imagenes/I24.png)

### SW-D4 (Cliente VTP - Edificio D)
![SW-D4](Imagenes/I25.png)

### SW-D5 (Cliente VTP - Edificio D)
![SW-D5](Imagenes/I26.png)

### SW-E1 (VTP Transparente - Edificio D)
![SW-E1](Imagenes/I27.png)

## Tabla de IPs asignadas por cada dispositivo

| Edificio | Dispositivo | VLAN | Dirección IP | Máscara | Gateway |
|----------|-------------|------|--------------|---------|---------|
| A | Admin2 (PC) | 12 - ADMIN | 192.168.12.10 | 255.255.255.0 | — |
| A | Laboratorio2 (PC) | 42 - LAB | 192.168.12.11 | 255.255.255.0 | — |
| A | Laboratorio4 (PC) | 42 - LAB | 192.168.12.12 | 255.255.255.0 | — |
| A | Docencia1 (Smartphone) | 22 - DOCENTES | 192.168.12.13 | 255.255.255.0 | — |
| A | Docencia2 (Laptop) | 22 - DOCENTES | 192.168.12.14 | 255.255.255.0 | — |
| A | Docencia3 (Laptop) | 22 - DOCENTES | 192.168.12.15 | 255.255.255.0 | — |
| B | Biblioteca1 (PC) | 32 - BIBLIOTECA | 192.168.12.20 | 255.255.255.0 | — |
| B | Docencia6 (Laptop) | 22 - DOCENTES | 192.168.12.21 | 255.255.255.0 | — |
| B | Admin1 (Laptop) | 12 - ADMIN | 192.168.12.22 | 255.255.255.0 | — |
| B | Biblioteca2 (Laptop) | 32 - BIBLIOTECA | 192.168.12.23 | 255.255.255.0 | — |
| B | Biblioteca4 (PC) | 32 - BIBLIOTECA | 192.168.12.24 | 255.255.255.0 | — |
| B | Biblioteca3 (PC) | 32 - BIBLIOTECA | 192.168.12.25 | 255.255.255.0 | — |
| B | Biblioteca5 (PC) | 32 - BIBLIOTECA | 192.168.12.26 | 255.255.255.0 | — |
| C | Docencia7 (Laptop) | 22 - DOCENTES | 192.168.12.30 | 255.255.255.0 | — |
| C | Docencia9 (PC) | 22 - DOCENTES | 192.168.12.31 | 255.255.255.0 | — |
| C | Biblioteca6 (PC) | 32 - BIBLIOTECA | 192.168.12.32 | 255.255.255.0 | — |
| C | Docencia8 (PC) | 22 - DOCENTES | 192.168.12.33 | 255.255.255.0 | — |
| C | Admin3 (PC) | 12 - ADMIN | 192.168.12.34 | 255.255.255.0 | — |
| D | Admin4 (PC) | 12 - ADMIN | 192.168.12.40 | 255.255.255.0 | — |
| D | Docencia10 (Servidor) | 22 - DOCENTES | 192.168.12.41 | 255.255.255.0 | — |
| D | Admin5 (PC) | 12 - ADMIN | 192.168.12.42 | 255.255.255.0 | — |
| D | Laboratorio3 (PC) | 42 - LAB | 192.168.12.43 | 255.255.255.0 | — |
| D | Biblioteca7 (PC) | 32 - BIBLIOTECA | 192.168.12.44 | 255.255.255.0 | — |
| D | Visitantes1 (PC) | 52 - VISITANTES | 192.168.12.50 | 255.255.255.0 | — |
| D | Visitantes2 (PC) | 52 - VISITANTES | 192.168.12.51 | 255.255.255.0 | — |
| D | Visitantes2 (PC) | 52 - VISITANTES | 192.168.12.52 | 255.255.255.0 | — |

## Tabla de VLANs

| VLAN ID | Nombre VLAN | Red |
|---------|-------------|------|
| 12 | ADMIN | 192.168.12.0/24 |
| 22 | DOCENTES | 192.168.12.0/24 |
| 32 | BIBLIOTECA | 192.168.12.0/24 |
| 42 | LABORATORIO | 192.168.12.0/24 |
| 52 | VISITANTES | 192.168.12.0/24 |

## Pruebas de ping
### Para la VLAN 12 - ADMIN
**Ping de Admin1 hacia Admin2**
![Ping1](Imagenes/I28.png)

**Ping de Admin3 hacia Admin4**
![Ping2](Imagenes/I29.png)

### Para la VLAN 22 - DOCENTES
**Ping de Docencia6 hacia Docencia1**
![Ping1](Imagenes/I30.png)

**Ping de Docencia8 hacia Docencia10**
![Ping2](Imagenes/I31.png)

### Para la VLAN 32 - BIBLIOTECA
**Ping de Biblioteca1 hacia Biblioteca6**
![Ping1](Imagenes/I32.png)

**Ping de Biblioteca3 hacia Biblioteca7**
![Ping2](Imagenes/I33.png)

### Para la VLAN 42 - LABORATORIO
**Ping de Laboratorio2 hacia Laboratorio4**
![Ping1](Imagenes/I34.png)

**Ping de Laboratorio4 hacia Laboratorio3**
![Ping2](Imagenes/I35.png)

### Para la VLAN 52 - VISITANTES
**Ping de Visitantes1 hacia Visitantes2**
![Ping1](Imagenes/I36.png)

**Ping de Visitantes2 hacia Visitantes3**
![Ping2](Imagenes/I37.png)

## Capturas de show
### show spanning-tree
#### Edificio A
![Captura Switch Edificio A](Imagenes/I38.png)
![Captura Switch Edificio A](Imagenes/I39.png)
![Captura Switch Edificio A](Imagenes/I40.png)

#### Edificio B
![Captura Switch Edificio B](Imagenes/I41.png)
![Captura Switch Edificio B](Imagenes/I42.png)
![Captura Switch Edificio B](Imagenes/I43.png)

#### Edificio C
![Captura Switch Edificio C](Imagenes/I44.png)
![Captura Switch Edificio C](Imagenes/I45.png)
![Captura Switch Edificio C](Imagenes/I46.png)

#### Edificio D
![Captura Switch Edificio D](Imagenes/I47.png)
![Captura Switch Edificio D](Imagenes/I48.png)
![Captura Switch Edificio D](Imagenes/I49.png)

### show etherchannel summary
#### Edificio A
![Captura Switch Edificio A](Imagenes/I50.png)

#### Edificio B
![Captura Switch Edificio B](Imagenes/I51.png)

#### Edificio C
![Captura Switch Edificio C](Imagenes/I52.png)

#### Edificio D
![Captura Switch Edificio D](Imagenes/I53.png)

### show interfaces trunk
#### Edificio A
![Captura Switch Edificio A](Imagenes/I54.png)

#### Edificio B
![Captura Switch Edificio B](Imagenes/I55.png)

#### Edificio C
![Captura Switch Edificio C](Imagenes/I56.png)

#### Edificio D
![Captura Switch Edificio D](Imagenes/I57.png)

## Presupuesto estimado de equipos

| Cantidad | Equipo | Precio unitario (Q) | Subtotal (Q) |
|----------|--------|---------------------|--------------|
| 5 | Switch Cisco 2960-24TT | Q 2,500 | Q 12,500 |
| 3 | Switch-PT (backbone) | Q 3,000 | Q 9,000 |
| 6 | Módulo fibra 1FFE | Q 800 | Q 4,800 |
| 4 | Cable UTP Cat6 (rollo 100m) | Q 400 | Q 1,600 |
| 6 | Cable UTP Cat5e (rollo 100m) | Q 250 | Q 1,500 |
| 3 | Cable Fibra OM3 (rollo 100m) | Q 600 | Q 1,800 |
| 2 | Access Point | Q 1,200 | Q 2,400 |
| 10 | PC de escritorio | Q 3,500 | Q 35,000 |
| 8 | Laptop | Q 4,500 | Q 36,000 |
| 1 | Hub | Q 300 | Q 300 |
| 1 | Repetidor | Q 250 | Q 250 |
| 1 | Conectores, canaletas, mano de obra | Q 2,000 | Q 2,000 |
| | **TOTAL ESTIMADO** | | **Q 107,150** |

## Parte física
### Comandos
enable
configure terminal
hostname SW-Practica

! Verificar que los puertos estén en VLAN 1 por defecto (ya lo están)
! pero por si acaso:
interface fastEthernet 0/1
switchport mode access
switchport access vlan 1
no shutdown
exit

interface fastEthernet 0/2
switchport mode access
switchport access vlan 1
no shutdown
exit

! Verificación rápida
do show vlan brief
do show interfaces status

### Configuración de la PC Cliente
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Default Gateway: -

### Configuración de la PC Servidor
IP Address: 192.168.1.20
Subnet Mask: 255.255.255.0
Default Gateway: -

### Prueba de conectividad
Desde la PC Cliente (192.168.1.10):
ping 192.168.1.20