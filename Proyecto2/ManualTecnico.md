## Universidad de San Carlos de Guatemala
## Facultad de ingeniería
## Laboratorio Redes de Computadoras 1
## Sección A
## Auxiliar: Roni Eduardo Vásquez Flores

![Logo USAC](Imagenes/LogoUSAC.jpeg)

### PROYECTO # 2

### Infraestructura Financiera y Alta Disponibilidad "BanTech GT"
### Manual Técnico

| Nombre | Carnet |
|---|---|
| Raúl Emanuel Yat Cancinos | 202300722 |

## Introducción
El presente proyecto consiste en el diseño e implementación de una infraestructura de red corporativa multisede para la entidad financiera BanTech GT, simulada mediante Cisco Packet Tracer. La red propuesta busca garantizar alta disponibilidad, segmentación segura, eficiencia en el direccionamiento IP y tolerancia a fallos, características esenciales en entornos financieros donde la continuidad del servicio es crítica.

La arquitectura de la red se divide en dos componentes principales: el Backbone Nacional, encargado de interconectar todas las sedes mediante múltiples protocolos de enrutamiento, y las redes de borde, correspondientes a cada sede operativa del banco. En el backbone se integran distintos dominios de enrutamiento (OSPF, EIGRP, RIP y rutas estáticas), los cuales se interconectan mediante mecanismos de redistribución de rutas para permitir la comunicación entre todas las áreas de la organización.

Cada sede fue diseñada considerando sus necesidades operativas específicas, aplicando técnicas como VLANs para segmentación de tráfico, Router-on-a-Stick para enrutamiento inter-VLAN, Rapid PVST+ para evitar bucles de capa 2, EtherChannel para incrementar ancho de banda y protocolos de redundancia como HSRP para asegurar la disponibilidad del gateway.

El resultado es una red escalable, segura y tolerante a fallos, capaz de mantener la comunicación entre todas las sedes incluso ante la caída de enlaces o dispositivos críticos.

## Tabla de VLANs
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

## Tablas de subnetting
### Backbone Core

| Enlace | Red / Máscara | Dispositivo A | Interfaz | IP A | Dispositivo B | Interfaz | IP B |
|--------|---------------|---------------|----------|------|---------------|----------|------|
| Núcleo1 ↔ Núcleo2 | 172.22.255.16/30 | R-Núcleo1 | Port-channel1 | 172.22.255.17 | R-Núcleo2 | Port-channel1 | 172.22.255.18 |
| Núcleo1 ↔ R-Occidente | 172.22.255.0/30 | R-Núcleo1 | Se0/0/0 | 172.22.255.1 | R-Occidente | Se0/0/0 | 172.22.255.2 |
| Núcleo1 ↔ R-Norte | 172.22.255.4/30 | R-Núcleo1 | Se0/0/1 | 172.22.255.5 | R-Norte | Se0/0/0 | 172.22.255.6 |
| Núcleo2 ↔ R-Oriente | 172.22.255.8/30 | R-Núcleo2 | Se0/0/0 | 172.22.255.9 | R-Oriente | Se0/0/0 | 172.22.255.10 |
| Núcleo2 ↔ R-Central1 | 172.22.255.12/30 | R-Núcleo2 | Se0/0/1 | 172.22.255.13 | R-Central1 | Se0/0/0 | 172.22.255.14 |
| R-Central1 ↔ R-Central2 | 172.22.255.20/30 | R-Central1 | Gi0/0 | 172.22.255.21 | R-Central2 | Gi0/0 | 172.22.255.22 |

### Sede Occidente

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

### Sede Norte

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

### Sede Oriente

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

### Data Center

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


## Topologías justificadas por sede
### Sede Occidente – Agencia Regional Comercial
La topología implementada para la sede Occidente sigue un modelo jerárquico en estrella, donde un switch principal conecta a los switches de acceso y al router de borde (R-Occidente). Esta arquitectura fue seleccionada debido a su simplicidad administrativa, facilidad de expansión y eficiencia para redes de oficinas comerciales.

En esta sede se implementó segmentación mediante VLANs para separar el tráfico de las áreas de Cajas, Asesores, Gerencia y Seguridad. Esta segmentación permite que los servicios críticos de transacciones bancarias operen de forma independiente, evitando que problemas en áreas administrativas o de atención al cliente afecten las operaciones financieras.

El enrutamiento entre VLANs se realiza mediante la técnica Router-on-a-Stick, utilizando subinterfaces en el router R-Occidente. Esto permite que un solo enlace troncal transporte múltiples VLANs, optimizando el uso del hardware. Las ventajas de esta topología son la administración centralizada de la red, el aislamiento de tráfico mediante VLANs, la reducción de dominios de broadcast y facilidad de escalabilidad en caso de expansión de la agencia.

### Sede Norte – Centro de Autorización de Créditos
La sede Norte fue diseñada con una topología redundante de capa 2, utilizando múltiples switches interconectados en una estructura con caminos alternos en forma de triángulo de switches. Esta decisión responde a la necesidad operativa de garantizar que los analistas de riesgo mantengan conectividad constante con el sistema bancario nacional.

Debido a la presencia de enlaces redundantes, se implementó el protocolo Rapid PVST+, el cual permite prevenir tormentas de broadcast y seleccionar automáticamente el mejor camino hacia el switch raíz de la red. Se configuró un Root Bridge específico para controlar la topología de spanning tree y optimizar el flujo de tráfico dentro de la red. Las ventajas de esta arquitectura son la eliminación de puntos únicos de falla, la recuperación rápida ante caída de enlaces, la mayor estabilidad en redes con múltiples switches y la alta disponibilidad para los servicios de aprobación de créditos. Este diseño asegura que la caída de un enlace o dispositivo no interrumpa las operaciones críticas de la sede.

### Sede Oriente – Centro Financiero Legado
La sede Oriente utiliza una arquitectura orientada a alta disponibilidad del gateway, debido a la criticidad de las operaciones financieras realizadas en esta sede. La red de acceso se conecta simultáneamente a dos switches multicapa (MS1 y MS2), lo que permite mantener conectividad hacia el núcleo incluso si uno de los dispositivos falla.

Para lograr esta redundancia se implementó el protocolo HSRP (Hot Standby Router Protocol), el cual permite crear una puerta de enlace virtual compartida entre ambos switches multicapa. De esta forma, los dispositivos finales utilizan una única dirección IP de gateway, mientras que los switches se alternan entre estado activo y de respaldo.

En caso de que el switch principal falle, el switch secundario asume automáticamente el rol activo, manteniendo la conectividad sin afectar a los usuarios. Las ventajas de esta arquitectura son la alta disponibilidad del gateway, la conmutación automática ante fallos, la transparencia para los usuarios finales y la continuidad operativa en servicios financieros críticos.

### Data Center y Sede Central – Alta Seguridad
El Data Center y la Sede Central representan el núcleo operativo del banco, donde se alojan los servidores de bases de datos, aplicaciones web y sistemas de monitoreo. Debido a la alta demanda de tráfico entre los servidores, se implementó EtherChannel, el cual permite agrupar múltiples enlaces físicos en un único enlace lógico de mayor capacidad. 

Esto aumenta el ancho de banda disponible y proporciona redundancia en caso de fallo de alguno de los enlaces físicos. La red se segmentó mediante VLANs para separar los servicios de servidores de base de datos, servidores de aplicaciones web y el centro de operaciones de red (NOC)

Adicionalmente, este segmento opera mediante rutas estáticas por políticas de seguridad, limitando la propagación automática de rutas dentro del entorno de servidores. Para permitir la comunicación con el resto de la red nacional, el router R-Central1 redistribuye estas rutas hacia el dominio OSPF del backbone. Las ventajas principales de esta arquitectura son la mayor capacidad de transmisión mediante EtherChannel, la redundancia en enlaces críticos, el control estricto del enrutamiento mediante rutas estáticas y la alta seguridad en la infraestructura de servidores.

## Arquitectura del Backbone
El backbone de la red nacional de BanTech GT constituye el núcleo de interconexión entre todas las sedes del banco, permitiendo el intercambio de información entre agencias regionales, centros financieros y el data center. Para lograr una infraestructura robusta, escalable y tolerante a fallos, el backbone fue diseñado utilizando una arquitectura híbrida de protocolos de enrutamiento, en la cual se integran diferentes tecnologías dependiendo de las características operativas de cada segmento de la red.

El diseño incorpora protocolos de enrutamiento dinámico y rutas estáticas, los cuales se interconectan mediante mecanismos de redistribución controlada. Esta estrategia permite integrar redes con distintos niveles de complejidad y requerimientos de seguridad sin comprometer la estabilidad del sistema.

### Núcleo de red con OSPF
El núcleo del backbone está implementado mediante el protocolo OSPF (Open Shortest Path First), utilizado en los routers principales del backbone debido a su alta eficiencia y escalabilidad en redes de gran tamaño.

OSPF es un protocolo de enrutamiento de estado de enlace que calcula las rutas óptimas utilizando el algoritmo Shortest Path First (SPF). Esto permite que los routers del núcleo mantengan una visión completa de la topología de red, facilitando la convergencia rápida ante cambios en la infraestructura.

Dentro del backbone, los routers R-Núcleo1 y R-Núcleo2 operan como puntos centrales de interconexión entre las diferentes sedes. Ambos routers participan en el área principal del protocolo (Área 0), lo que garantiza una comunicación directa entre los distintos dominios de enrutamiento.

Las ventajas de utilizar OSPF en el núcleo son convergencia rápida ante fallos de red, la escalabilidad para redes empresariales grandes, el soporte para múltiples rutas redundantes y la optimización automática del camino más corto. Gracias a estas características, OSPF permite mantener la estabilidad del backbone incluso ante cambios en la topología o fallos de enlaces.

### Expansión regional con EIGRP
Para la sede Oriente se implementó el protocolo EIGRP (Enhanced Interior Gateway Routing Protocol), el cual facilita la administración de redes empresariales con múltiples subredes internas. EIGRP es un protocolo híbrido que combina características de protocolos de vector de distancia y estado de enlace. Utiliza el algoritmo DUAL (Diffusing Update Algorithm) para calcular rutas eficientes y garantizar una rápida convergencia.

La implementación de EIGRP en la sede Oriente permite que haya una distribución automática de rutas dentro de la sede, una alta eficiencia en el uso del ancho de banda, una convergencia rápida ante cambios de red y una fácil integración con otros protocolos mediante redistribución. El router R-Núcleo2 actúa como punto de interconexión entre el dominio EIGRP y el backbone OSPF, redistribuyendo las rutas aprendidas en el sistema autónomo hacia el resto de la red.

### Red legada con RIP
La sede Occidente utiliza el protocolo RIP versión 2, el cual fue seleccionado para simular un entorno de red legado que aún opera con tecnologías tradicionales de enrutamiento. RIP es un protocolo de vector de distancia que utiliza el número de saltos como métrica para determinar la mejor ruta hacia una red. Aunque es menos eficiente que protocolos modernos como OSPF o EIGRP, su simplicidad lo hace adecuado para redes pequeñas o infraestructuras heredadas.

Las principales características de RIP son la configuración sencilla, la compatibilidad con dispositivos de red antiguos y el soporte para subnetting mediante RIP versión 2. En la arquitectura del proyecto, el router R-Núcleo1 redistribuye las rutas aprendidas mediante RIP hacia el dominio OSPF del backbone, permitiendo que las demás sedes puedan comunicarse con la red de Occidente.

### Segmento seguro con rutas estáticas
El segmento correspondiente al Data Center y la Sede Central utiliza rutas estáticas, debido a los requisitos de seguridad y control del tráfico en esta parte crítica de la infraestructura.Las rutas estáticas permiten definir manualmente el camino que deben seguir los paquetes dentro de la red. Este enfoque evita la propagación automática de rutas dentro del entorno del data center, reduciendo el riesgo de cambios inesperados en la topología de enrutamiento.

Entre las ventajas de utilizar rutas estáticas en este segmento se encuentran la mayor control sobre el flujo de tráfico, la reducción de actualizaciones dinámicas de enrutamiento, el incremento en la seguridad del entorno de servidores y la estabilidad en redes críticas. Para integrar este segmento con el resto del backbone, el router R-Central1 redistribuye las rutas estáticas hacia el dominio OSPF, permitiendo que las demás sedes puedan acceder a los servicios alojados en el data center.

### Redistribución de rutas entre protocolos

Debido a la presencia de múltiples protocolos de enrutamiento dentro de la arquitectura del backbone, fue necesario implementar redistribución de rutas entre los distintos dominios de enrutamiento. La redistribución permite que las rutas aprendidas por un protocolo puedan ser anunciadas dentro de otro protocolo diferente, facilitando la comunicación entre redes heterogéneas.

En el diseño del proyecto se implementaron los siguientes puntos de redistribución:
- R-Núcleo1: redistribuye rutas del protocolo RIP hacia OSPF.
- R-Núcleo2: redistribuye rutas del protocolo EIGRP hacia OSPF.
- R-Central1: redistribuye rutas estáticas hacia OSPF.

De esta manera, OSPF funciona como el protocolo central del backbone, recibiendo rutas provenientes de diferentes dominios y propagándolas hacia todas las sedes conectadas. Este mecanismo permite que redes que utilizan distintos protocolos puedan interoperar dentro de una misma infraestructura de red, manteniendo la coherencia en la tabla de enrutamiento global.

### Justificación del Backbone Completo
El diseño del backbone fue desarrollado siguiendo principios de alta disponibilidad, escalabilidad y compatibilidad con diferentes tecnologías de red. La utilización de OSPF como protocolo principal del núcleo permite mantener una topología estable y eficiente, mientras que la integración de EIGRP y RIP permite conectar redes con diferentes niveles de complejidad tecnológica.

Asimismo, el uso de rutas estáticas en el data center proporciona un mayor nivel de control sobre el tráfico que accede a los sistemas críticos del banco. Otro aspecto importante del diseño es la redundancia entre los routers del núcleo, los cuales se encuentran conectados mediante múltiples enlaces físicos. Esta configuración permite que el tráfico pueda redirigirse automáticamente en caso de que uno de los enlaces falle, garantizando la continuidad de las operaciones del sistema financiero.

En conjunto, la arquitectura implementada permite que todas las sedes del banco mantengan comunicación constante, incluso ante fallos parciales en la infraestructura de red. Este enfoque híbrido de enrutamiento proporciona un equilibrio adecuado entre rendimiento, seguridad y facilidad de administración, convirtiéndose en una solución adecuada para entornos empresariales distribuidos como el planteado en este proyecto.

## Capturas de configuración (show running-config)
### Configuración de switch server (Sede Occidente)
![SW-Occidente1](Imagenes/I1.png)
![SW-Occidente1](Imagenes/I2.png)

### Configuración de switch client (Sede Occidente)
![SW-Occidente2](Imagenes/I3.png)
![SW-Occidente2](Imagenes/I4.png)

### Configuración de switch transparente (Sede Oriente)
![Multilayer Switch 1](Imagenes/I5.png)
![Multilayer Switch 1](Imagenes/I6.png)
![Multilayer Switch 1](Imagenes/I7.png)

### Configuración de router de sede (Sede Norte)
![R-Norte](Imagenes/I8.png)
![R-Norte](Imagenes/I9.png)
![R-Norte](Imagenes/I10.png)

### Configuración de router de núcleo
![R-Núcleo1](Imagenes/I11.png)
![R-Núcleo1](Imagenes/I12.png)
![R-Núcleo1](Imagenes/I13.png)

## Capturas de protocolos en routers
### OSPF (R-Núcleo1)
![OSPF](Imagenes/I18.png)

### EIGRP (R-Núcleo2)
![EIGRP](Imagenes/I19.png)

## Capturas de pruebas de ping
### Sede Occidente ↔ Sede Norte
![Prueba de ping](Imagenes/I14.png)

### Sede Norte ↔ Sede Oriente
![Prueba de ping](Imagenes/I15.png)

### Sede Oriente ↔ Sede Central
![Prueba de ping](Imagenes/I16.png)

### Sede Central ↔ Sede Occidente
![Prueba de ping](Imagenes/I17.png)