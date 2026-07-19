# ◼ CISCO IOS · GUÍA OPERATIVA DE REDES

> Repositorio de estudio estructurado para el ecosistema Cisco.  
> Taxonomía de comandos · Arquitectura de modos · Principios End-to-End.
> Guia de Comandos Completa · Básico a Avanzado · 200 - 301 ·  600 - 801 · 
> Calculadora de Redes 

---
## ◻ COMANDOS OFICIALES COMPLETOS

- [01 · CCNA 200 - 301]
(#)
Su contenido está organizado en capítulos que cubren desde configuraciones 
básicas hasta características avanzadas de seguridad y enrutamiento. 

#Infraestructura de Switching: 
Detalla la creación de VLANs estáticas, Voice VLANs, y el uso de VLAN Trunking Protocol (VTP). 
Cubre exhaustivamente el Spanning Tree Protocol (STP) en sus diversas variantes 
(PVST+, Rapid PVST+, MSTP) y EtherChannel (LACP/PAgP) para agregación de enlaces.  

#Enrutamiento e Inter-VLAN: 
Describe la comunicación entre VLANs mediante Router-on-a-stick y SVI en switches multicapa. 
Incluye configuraciones de enrutamiento estático (IPv4 e IPv6) y OSPFv2.  

#Servicios y Gestión: 
Incluye guías para la configuración de DHCP, Network Address Translation (NAT/PAT), 
y Network Time Protocol (NTP).  

#Seguridad: 
Proporciona procedimientos para el endurecimiento de dispositivos (device hardening), 
configuración de SSH, Port Security (incluyendo Sticky MAC), 
DHCP Snooping y Dynamic ARP Inspection (DAI).  

#Listas de Control de Acceso (ACLs): 
Documenta la creación, aplicación y gestión de ACLs estándar, extendidas y nombradas 
tanto para IPv4 como para IPv6.  

#Protocolos de Descubrimiento: 
Explica la configuración y verificación de CDP y LLDP.  

- [02 · Los Dos Entornos de Operación](#02--los-dos-entornos-de-operación)
- [03 · Máquina de Estados del CLI de Cisco](#03--máquina-de-estados-del-cli-de-cisco)

## ◻ ÍNDICE

- [01 · Fundamentos del Ecosistema](#01--fundamentos-del-ecosistema)
- [02 · Los Dos Entornos de Operación](#02--los-dos-entornos-de-operación)
- [03 · Máquina de Estados del CLI de Cisco](#03--máquina-de-estados-del-cli-de-cisco)
- [04 · Base de Comandos por Categoría](#04--base-de-comandos-por-categoría)
- [05 · Principios Arquitectónicos](#05--principios-arquitectónicos)
- [06 · Apéndices de Referencia](#06--apéndices-de-referencia)

---

## 01 · FUNDAMENTOS DEL ECOSISTEMA

Este repositorio aborda el error más frecuente en estudiantes de redes: **confundir dónde se ejecuta cada acción**. La separación entre el dispositivo final y la infraestructura no es una convención — es una frontera arquitectónica con implicaciones de seguridad y resiliencia.

**Objetivo pedagógico:** Al terminar este material, el estudiante podrá identificar, sin dudar, en qué entorno ejecutar cada comando y por qué intentarlo en el entorno equivocado es un error conceptual, no solo operativo.

---

## 02 · LOS DOS ENTORNOS DE OPERACIÓN

La red corporativa se divide en dos planos de interacción **mutuamente excluyentes en propósito**:

```
┌─────────────────────────────────┐   ┌─────────────────────────────────┐
│         PANTALLA NEGRA          │   │         PANTALLA BLANCA         │
│   Command Prompt / Terminal     │   │        Cisco IOS CLI            │
├─────────────────────────────────┤   ├─────────────────────────────────┤
│ Dispositivo: PC, Servidor, Host │   │ Dispositivo: Router, Switch     │
│ Sistema: Windows, Linux         │   │ Sistema: Cisco IOS              │
├─────────────────────────────────┤   ├─────────────────────────────────┤
│ PROPÓSITO:                      │   │ PROPÓSITO:                      │
│  · Generación de tráfico        │   │  · Configuración del plano de   │
│  · Pruebas End-to-End           │   │    control                      │
│  · Validación de aplicaciones   │   │  · Diseño de topologías         │
│  · Resolución de DNS            │   │  · Gestión de enrutamiento      │
├─────────────────────────────────┤   ├─────────────────────────────────┤
│ RESTRICCIÓN CRÍTICA:            │   │ RESTRICCIÓN CRÍTICA:            │
│  Un host final jamás configura  │   │  Regida por una Máquina de      │
│  la infraestructura subyacente. │   │  Estados Finitos jerárquica.    │
│  Intentarlo es un error         │   │  No puedes alterar configuración │
│  arquitectónico.                │   │  sin escalar privilegios primero.│
└─────────────────────────────────┘   └─────────────────────────────────┘
```

### ◻ Simbología del sistema (Lex-Net)

| Símbolo | Significado |
|---------|-------------|
| `[PB]` | Exclusivo Pantalla Blanca — Cisco IOS CLI |
| `[PN]` | Exclusivo Pantalla Negra — Host / PC |
| `[E2E]` | Universal — ejecutable en ambos entornos |

---

## 03 · MÁQUINA DE ESTADOS DEL CLI DE CISCO

El CLI de Cisco implementa el **Patrón de Diseño State**: las acciones disponibles mutan en función del estado actual del sistema. El prompt te indica en todo momento tu nivel de autoridad.

```
                    ┌─────────────────────────────────────────┐
                    │           CISCO IOS CLI                 │
                    │         MÁQUINA DE ESTADOS              │
                    └─────────────────────────────────────────┘

  ┌──────────────────────┐
  │   User EXEC Mode     │  prompt: Device>
  │   (Solo lectura)     │  · Diagnósticos básicos únicamente
  │                      │  · NO permite alteraciones de config
  └──────────┬───────────┘
             │ enable
             ▼
  ┌──────────────────────┐
  │  Privileged EXEC     │  prompt: Device#
  │  (Nivel root/admin)  │  · Ver config completa (RAM + NVRAM)
  │                      │  · Guardar cambios, depurar el sistema
  └──────────┬───────────┘
             │ configure terminal
             ▼
  ┌──────────────────────┐
  │  Global Config Mode  │  prompt: Device(config)#
  │  (Arquitectura)      │  · Modificar hostname, contraseñas
  │                      │  · Activar protocolos globales
  └──────────┬───────────┘
             │ interface / router
             ▼
  ┌──────────────────────────────────────────┐
  │           Sub-modos de configuración     │
  │  Device(config-if)#    → interfaces      │
  │  Device(config-router)# → protocolos     │
  │  Device(config-line)#  → consola/VTY     │
  └──────────────────────────────────────────┘
```

> **Regla de oro:** `exit` sube un nivel. `end` o `Ctrl+Z` vuelve directamente al modo `#`.

---

## 04 · BASE DE COMANDOS POR CATEGORÍA

### ◼ Categoría I — Inicialización y Auditoría del Sistema `[BASE]`

Secuencia recomendada antes de cualquier configuración o diagnóstico.

| Entorno | N° | Comando | Descripción |
|---------|----|---------|-------------|
| `[PB]` | 1 | `enable` | Eleva privilegios del modo `>` al modo `#` |
| `[PB]` | 2 | `configure terminal` | Ingresa al modo de configuración global `(config)#` |
| `[PB]` | 3 | `no ip domain-lookup` | Deshabilita búsqueda DNS; evita bloqueos por errores de tipeo |
| `[PB]` | 4 | `hostname [nombre]` | Asigna nombre identificador al dispositivo |
| `[PB]` | 5 | `show running-config` | Muestra la configuración en ejecución (RAM) |
| `[PB]` | 6 | `copy running-config startup-config` | Persiste la configuración en NVRAM |

> **Analogía:** `copy running-config startup-config` equivale a `git commit && git push`.  
> La RAM es tu entorno local volátil. La NVRAM es la rama de producción.  
> Si el router se apaga sin ejecutarlo, pierdes todos los cambios no confirmados.

---

### ◼ Categoría II — Diagnóstico y Conectividad End-to-End `[INTERMEDIO]`

Validación de las capas físicas y lógicas del modelo OSI.

| Entorno | N° | Comando | Descripción |
|---------|----|---------|-------------|
| `[E2E]` | 7 | `ping [ip-address]` | Envía ICMP Echo Request para verificar alcance de Capa 3 |
| `[PN]` | 8 | `tracert [ip-address]` | Mapea la ruta salto a salto (sintaxis Windows) |
| `[PB]` | 8 | `traceroute [ip-address]` | Equivalente en Cisco IOS |
| `[E2E]` | 9 | `telnet [ip-address]` | Acceso remoto al CLI de un dispositivo |
| `[PB]` | 10 | `show ip interface brief` | Resumen de IPs y estado físico/lógico de interfaces |
| `[PB]` | 11 | `show mac address-table` | Tabla de MACs aprendidas (exclusivo de switches) |
| `[PB]` | 12 | `show interfaces trunk` | Verifica enlaces troncales activos en un switch |

> **Principio crítico:** Monitorear la red únicamente desde la `[PB]` genera falsos positivos.  
> Un router puede tener la ruta, pero las ACLs podrían bloquear al host.  
> **Las pruebas reales siempre deben originarse en la `[PN]`.**

---

### ◼ Categoría III — Enrutamiento y Conmutación `[AVANZADO]`

Inyección de lógica de red y diseño de topologías.

#### Rutas Estáticas

| Comando | Descripción |
|---------|-------------|
| `ip route [red] [máscara] [next-hop]` | Ruta estática estándar. AD = 1 |
| `ip route 0.0.0.0 0.0.0.0 [next-hop]` | Ruta estática por defecto |
| `ip route [red] [máscara] [next-hop] [AD]` | Ruta estática flotante (respaldo) |

#### OSPF

| Comando | Descripción |
|---------|-------------|
| `router ospf [PID]` | Inicializa el proceso OSPF. PID: 1–65535 |
| `router-id [a.b.c.d]` | Asigna manualmente el Router ID |
| `network [red] [wildcard] area [id]` | Anuncia redes directamente conectadas |
| `show ip route` | Tabla de enrutamiento completa |
| `show ip ospf neighbor` | Lista de vecinos OSPF |
| `show ip protocols` | Verificación rápida de protocolos activos |

#### VLANs y Conmutación

| Comando | Descripción |
|---------|-------------|
| `vlan [ID]` | Crea una VLAN y asigna su identificador |
| `name [nombre]` | Asigna nombre a la VLAN activa |
| `switchport mode access` | Configura puerto en modo acceso (host final) |
| `switchport mode trunk` | Configura puerto troncal (múltiples VLANs) |
| `switchport access vlan [ID]` | Asigna puerto a una VLAN específica |
| `switchport trunk native vlan [ID]` | Define la VLAN nativa del enlace troncal |
| `show vlan brief` | Resumen de VLANs, estados y puertos asignados |

#### Router-on-a-Stick (ROAS)

| Comando | Descripción |
|---------|-------------|
| `interface g0/0.[vlan-id]` | Crea subinterfaz para la VLAN |
| `encapsulation dot1q [vlan-id]` | Configura encapsulación 802.1Q |
| `ip address [ip] [máscara]` | Asigna IP a la subinterfaz |
| `no shutdown` | Activa la interfaz física (activa todas las subinterfaces) |

---

### ◼ Categoría IV — Seguridad de Red `[AVANZADO]`

#### ACLs (Listas de Control de Acceso)

| Comando | Descripción |
|---------|-------------|
| `access-list [num] [permit/deny] [addr] [wildcard]` | Crea entrada en ACL numerada estándar |
| `ip access-group [num] [in/out]` | Aplica la ACL a una interfaz |
| `ip access-list standard [nombre]` | Crea ACL nombrada estándar |

> **Regla mnemotécnica:**  
> ACL Estándar → más cerca del **D**estino (S-D)  
> ACL Extendida → más cerca del **S**origen (E-S)

#### Port Security (Switches)

| Comando | Descripción |
|---------|-------------|
| `switchport port-security` | Habilita Port Security en la interfaz |
| `switchport port-security maximum [n]` | Máximo de MACs permitidas |
| `switchport port-security mac-address sticky` | Aprende MACs dinámicamente y las persiste |
| `switchport port-security violation [modo]` | Define acción ante violación: protect / restrict / shutdown |
| `show port-security interface [int]` | Verifica configuración de Port Security |

#### SSH

| Comando | Descripción |
|---------|-------------|
| `ip domain-name [dominio]` | Requisito previo para generar llaves RSA |
| `crypto key generate rsa` | Genera el par de llaves criptográficas |
| `ip ssh version 2` | Habilita SSH versión 2 (recomendado) |
| `line vty 0 15` | Accede a las líneas VTY |
| `transport input ssh` | Restringe acceso remoto únicamente a SSH |

---

### ◼ Categoría V — Alta Disponibilidad y Protocolos Avanzados `[EXPERTO]`

#### HSRP (Hot Standby Router Protocol)

| Comando | Descripción |
|---------|-------------|
| `standby version 2` | Habilita HSRPv2 (soporta IPv6 y 4095 grupos) |
| `standby [grupo] ip [ip-virtual]` | Define grupo y gateway virtual |
| `standby [grupo] priority [0-255]` | Prioridad para elección de router activo |
| `standby [grupo] preempt` | Permite recuperar el rol activo si la prioridad es mayor |

#### EtherChannel

| Comando | Descripción |
|---------|-------------|
| `interface range [inicio]-[fin]` | Selecciona interfaces para el bundle |
| `channel-group [num] mode [modo]` | Define grupo y modo (active/passive/on/auto/desirable) |
| `interface port-channel [num]` | Accede a la interfaz lógica del EtherChannel |

#### STP (Spanning Tree Protocol)

| Comando | Descripción |
|---------|-------------|
| `spanning-tree vlan [id] root primary` | Fuerza este switch como root bridge primario |
| `spanning-tree vlan [id] root secondary` | Configura root bridge secundario (respaldo) |
| `spanning-tree mode rapid-pvst` | Habilita Rapid PVST+ como modo STP |
| `spanning-tree portfast` | Activa PortFast en un puerto de acceso |
| `spanning-tree bpduguard enable` | Protege puertos de acceso contra BPDUs |
| `show spanning-tree` | Verifica instancias STP y root bridges |

---

## 05 · PRINCIPIOS ARQUITECTÓNICOS

### Principio de Menor Privilegio
La jerarquía `> → # → config#` introduce **fricción operativa intencional**.  
Ralentiza la configuración manual, pero previene inyecciones de comandos destructivos por error.

### Separación de Planos
- La `[PN]` prueba el **Data Plane** (tráfico real del usuario).  
- La `[PB]` configura el **Control Plane** (cómo la red toma decisiones).  
Ambos planos son necesarios para un diagnóstico completo y correcto.

### Analogías con Ingeniería de Software

| Concepto de Redes | Equivalente en Software |
|-------------------|------------------------|
| Modos CLI (>, #, config) | Patrón de diseño **State** |
| `copy running-config startup-config` | `git commit && git push` |
| RAM (running-config) | Entorno local / estado volátil |
| NVRAM (startup-config) | Rama de producción / base de datos persistente |
| Router apagado sin guardar | Pod caído sin checkpoint de pesos en Deep Learning |

---

## 06 · APÉNDICES DE REFERENCIA

### Distancias Administrativas (AD) por Defecto

| Código | Tipo de ruta | AD |
|--------|-------------|-----|
| `C` | Directamente conectada | 0 |
| `S` | Ruta estática | 1 |
| `D` | EIGRP interno | 90 |
| `O` | OSPF | 110 |
| `R` | RIP | 120 |

### Clases de Direcciones IPv4

| Clase | Rango (primer octeto) | Uso |
|-------|-----------------------|-----|
| A | 1 – 127 | Redes grandes |
| B | 128 – 191 | Redes medianas |
| C | 192 – 223 | Redes pequeñas |
| D | 224 – 239 | Multicast (reservado) |
| E | 240 – 255 | Investigación (reservado) |

### Rangos de Direcciones IPv4 Privadas (RFC 1918)

| Clase | Notación CIDR | Rango |
|-------|--------------|-------|
| A | `10.0.0.0/8` | 10.0.0.0 – 10.255.255.255 |
| B | `172.16.0.0/12` | 172.16.0.0 – 172.31.255.255 |
| C | `192.168.0.0/16` | 192.168.0.0 – 192.168.255.255 |

### Filtrado de Salida en Comandos `show`

```
Device# show running-config | section [expresión]
Device# show running-config | include [expresión]
Device# show running-config | exclude [expresión]
Device# show running-config | begin  [expresión]
```

### Abreviaciones Comunes del CLI

```
enable           →  en / ena
configure terminal →  conf t / config term
interface        →  int
show             →  sh
copy             →  cop
```

---

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Creado por Nailea Falcón para todos los estudiantes de redes
de habla hispana.
Universidad del Valle de México · Redes 1, 2, 3 y 4
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
