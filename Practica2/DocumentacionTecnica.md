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

## Propuesta de una red base