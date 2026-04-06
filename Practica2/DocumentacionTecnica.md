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
### 1. Hospital Roosevelt
| Área funcional | N° Área | ID de VLAN | Hosts |
|---|---|---|---|
| Quirófanos | Área 1 | 12 | 10 |
| UCI | Área 2 | 22 | 8 |
| Emergencias | Área 3 | 32 | 12 |
| Administración | Área 4 | 42 | 10 |
| Laboratorios | Área 5 | 52 | 8 |
| Hospitalización | Área 6 | 62 | 15 |

**TOTAL de VLANs: 6**

### 2. Hospital San Juan de Dios
| Área funcional | N° Área | ID de VLAN | Hosts |
|---|---|---|---|
| Quirófanos | Área 1 | 12 | 6 |
| Emergencias | Área 2 | 22 | 8 |
| Administración | Área 3 | 32 | 6 |
| Consultas externas | Área 4 | 42 | 5 |

**TOTAL de VLANs: 4**

### 3. Hospital General de Accidentes (IGSS)
| Área funcional | N° Área | ID de VLAN | Hosts |
|---|---|---|---|
| Emergencias | Área 1 | 12 | 10 |
| UCI | Área 2 | 22 | 6 |
| Quirófanos | Área 3 | 32 | 6 |
| Administración | Área 4 | 42 | 5 |
| Farmacia | Área 5 | 52 | 5 |

**TOTAL de VLANs: 5**

### 4. Hospital de Especialidades (IGSS)
| Área funcional | N° Área | ID de VLAN | Hosts |
|---|---|---|---|
| Consultas externas | Área 1 | 12 | 6 |
| Laboratorios | Área 2 | 22 | 5 |
| Administración | Área 3 | 32 | 4 |

**TOTAL de VLANs: 3**

### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR)
| Área funcional | N° Área | ID de VLAN | Hosts |
|---|---|---|---|
| Emergencias pediátricas | Área 1 | 12 | 8 |
| Hospitalización infantil | Área 2 | 22 | 8 |
| Consultas externas | Área 3 | 32 | 5 |
| Administración | Área 4 | 42 | 5 |

**TOTAL de VLANs: 4**

### 6. Hospital de Salud San Miguel Petapa
| Área funcional | N° Área | ID de VLAN | Hosts |
|---|---|---|---|
| Consultas externas | Área 1 | 12 | 4 |
| Administración | Área 2 | 22 | 4 |

**TOTAL de VLANs: 2**

## Propuesta de una red base y tablas de subnetting
Mi propuesta para la red base es `172.16.0.0 /16` ya que es una dirección privada clase B que proporciona suficiente espacio para escalar. Pero se aplicó VLSM que es la Máscara de Subred de Longitud Variable para poder optimizar el uso de direcciones.
- Se usa `/27 (255.255.255.224)` para VLAN's de hospitales, permitiendo hasta 30 hosts por subred, suficiente para cubrir el requerimiento máximo de 15 hosts por VLAN.
- Se usa `/30 (255.255.255.252)` para enlaces punto a punto entre switches, usando solo 4 direcciones por enlace.
- Se usa `/27` para la red de administración.
Acá todas las subredes son contiguas y están ordenadas para facilitar la agregación de rutas (route summarization) y el troubleshooting.

### Tablas de subnetting
#### 1. Hospital Roosevelt (172.16.0.0 hasta 172.16.0.160/27)
| VLAN ID | Área funcional | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|---------|----------------|--------|---------|---------|----------------|------------|
| 12 | Quirófanos | 172.16.0.0 | 255.255.255.224 | 172.16.0.1 | 172.16.0.2 - 172.16.0.30 | 172.16.0.31 |
| 22 | UCI | 172.16.0.32 | 255.255.255.224 | 172.16.0.33 | 172.16.0.34 - 172.16.0.62 | 172.16.0.63 |
| 32 | Emergencias | 172.16.0.64 | 255.255.255.224 | 172.16.0.65 | 172.16.0.66 - 172.16.0.94 | 172.16.0.95 |
| 42 | Administración | 172.16.0.96 | 255.255.255.224 | 172.16.0.97 | 172.16.0.98 - 172.16.0.126 | 172.16.0.127 |
| 52 | Laboratorios | 172.16.0.128 | 255.255.255.224 | 172.16.0.129 | 172.16.0.130 - 172.16.0.158 | 172.16.0.159 |
| 62 | Hospitalización | 172.16.0.160 | 255.255.255.224 | 172.16.0.161 | 172.16.0.162 - 172.16.0.190 | 172.16.0.191 |

#### 2. Hospital San Juan de Dios (172.16.0.192/27 hasta 172.16.0.288/27)
| VLAN ID | Área funcional | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|---------|----------------|--------|---------|---------|----------------|------------|
| 12 | Quirófanos | 172.16.0.192 | 255.255.255.224 | 172.16.0.193 | 172.16.0.194 - 172.16.0.222 | 172.16.0.223 |
| 22 | Emergencias | 172.16.0.224 | 255.255.255.224 | 172.16.0.225 | 172.16.0.226 - 172.16.0.254 | 172.16.0.255 |
| 32 | Administración | 172.16.1.0 | 255.255.255.224 | 172.16.1.1 | 172.16.1.2 - 172.16.1.30 | 172.16.1.31 |
| 42 | Consultas externas | 172.16.1.32 | 255.255.255.224 | 172.16.1.33 | 172.16.1.34 - 172.16.1.62 | 172.16.1.63 |

#### 3. Hospital General de Accidentes (IGSS) (172.16.1.64/27 hasta 172.16.1.192/27)
| VLAN ID | Área funcional | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|---------|----------------|--------|---------|---------|----------------|------------|
| 12 | Emergencias | 172.16.1.64 | 255.255.255.224 | 172.16.1.65 | 172.16.1.66 - 172.16.1.94 | 172.16.1.95 |
| 22 | UCI | 172.16.1.96 | 255.255.255.224 | 172.16.1.97 | 172.16.1.98 - 172.16.1.126 | 172.16.1.127 |
| 32 | Quirófanos | 172.16.1.128 | 255.255.255.224 | 172.16.1.129 | 172.16.1.130 - 172.16.1.158 | 172.16.1.159 |
| 42 | Administración | 172.16.1.160 | 255.255.255.224 | 172.16.1.161 | 172.16.1.162 - 172.16.1.190 | 172.16.1.191 |
| 52 | Farmacia | 172.16.1.192 | 255.255.255.224 | 172.16.1.193 | 172.16.1.194 - 172.16.1.222 | 172.16.1.223 |

#### 4. Hospital de Especialidades (IGSS) (172.16.1.224/27 hasta 172.16.2.32/27)
| VLAN ID | Área funcional | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|---------|----------------|--------|---------|---------|----------------|------------|
| 12 | Consultas externas | 172.16.1.224 | 255.255.255.224 | 172.16.1.225 | 172.16.1.226 - 172.16.1.254 | 172.16.1.255 |
| 22 | Laboratorios | 172.16.2.0 | 255.255.255.224 | 172.16.2.1 | 172.16.2.2 - 172.16.2.30 | 172.16.2.31 |
| 32 | Administración | 172.16.2.32 | 255.255.255.224 | 172.16.2.33 | 172.16.2.34 - 172.16.2.62 | 172.16.2.63 |

#### 5. Hospital Infantil de Infectología y Rehabilitación (HIIR) (172.16.2.64/27 hasta 172.16.2.192/27)
| VLAN ID | Área funcional | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|---------|----------------|--------|---------|---------|----------------|------------|
| 12 | Emergencias pediátricas | 172.16.2.64 | 255.255.255.224 | 172.16.2.65 | 172.16.2.66 - 172.16.2.94 | 172.16.2.95 |
| 22 | Hospitalización infantil | 172.16.2.96 | 255.255.255.224 | 172.16.2.97 | 172.16.2.98 - 172.16.2.126 | 172.16.2.127 |
| 32 | Consultas externas | 172.16.2.128 | 255.255.255.224 | 172.16.2.129 | 172.16.2.130 - 172.16.2.158 | 172.16.2.159 |
| 42 | Administración | 172.16.2.160 | 255.255.255.224 | 172.16.2.161 | 172.16.2.162 - 172.16.2.190 | 172.16.2.191 |

#### 6. Hospital de Salud San Miguel Petapa (172.16.2.64/27 hasta 172.16.2.192/27)
| VLAN ID | Área funcional | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|---------|----------------|--------|---------|---------|----------------|------------|
| 12 | Consultas externas | 172.16.2.192 | 255.255.255.224 | 172.16.2.193 | 172.16.2.194 - 172.16.2.222 | 172.16.2.223 |
| 22 | Administración | 172.16.2.224 | 255.255.255.224 | 172.16.2.225 | 172.16.2.226 - 172.16.2.254 | 172.16.2.255 |

### Enlaces entre switches
| Enlace | Subred /30 | Máscara | IP Switch A | IP Switch B | Broadcast |
|--------|------------|---------|-------------|-------------|-----------|
| Central ↔ Nodo Este | 172.16.3.0 | 255.255.255.252 | 172.16.3.1 | 172.16.3.2 | 172.16.3.3 |
| Central ↔ Nodo Oeste | 172.16.3.4 | 255.255.255.252 | 172.16.3.5 | 172.16.3.6 | 172.16.3.7 |
| Nodo Este ↔ Nodo Oeste | 172.16.3.8 | 255.255.255.252 | 172.16.3.9 | 172.16.3.10 | 172.16.3.11 |

### Administración/Management
| VLAN | Propósito | Subred | Máscara | Gateway | Rango de hosts | Broadcast |
|------|-----------|--------|---------|---------|----------------|------------|
| Management | Administración de switches | 172.16.3.64 | 255.255.255.224 | 172.16.3.65 | 172.16.3.66 - 172.16.3.94 | 172.16.3.95 |