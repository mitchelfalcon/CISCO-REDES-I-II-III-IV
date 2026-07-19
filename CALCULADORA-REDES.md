# ◼ CALCULADORA DE REDES — GUÍA DE USO Y REFERENCIA

> Herramienta de cálculo para subredes IPv4.  
> Diseñada para ingenieros de redes y estudiantes del ecosistema Cisco.

---

## ◻ ÍNDICE

- [01 · ¿Qué es la Calculadora de Redes?](#01--qué-es-la-calculadora-de-redes)
- [02 · Fundamentos Previos Indispensables](#02--fundamentos-previos-indispensables)
- [03 · Operaciones que Puede Resolver](#03--operaciones-que-puede-resolver)
- [04 · Guía de Uso Paso a Paso](#04--guía-de-uso-paso-a-paso)
- [05 · Interpretación de Resultados](#05--interpretación-de-resultados)
- [06 · Casos de Uso Prácticos en Cisco](#06--casos-de-uso-prácticos-en-cisco)
- [07 · Tabla de Referencia Rápida](#07--tabla-de-referencia-rápida)
- [08 · Errores Frecuentes](#08--errores-frecuentes)

---

## 01 · ¿QUÉ ES LA CALCULADORA DE REDES?

La **Calculadora de Redes** es una herramienta de apoyo al diseño y diagnóstico de infraestructuras IPv4. Su función principal es resolver automáticamente los cálculos de subredes (subnetting) que, de hacerse manualmente, consumen tiempo considerable y son propensos a errores en entornos de examen o producción.

**¿Para qué sirve en la práctica?**
- Determinar la dirección de red, broadcast y rango de hosts de una subred.
- Validar si dos dispositivos pertenecen al mismo segmento de red.
- Planificar el esquema de direccionamiento antes de configurar un router.
- Convertir entre notación CIDR (`/24`) y máscara decimal (`255.255.255.0`).

---

## 02 · FUNDAMENTOS PREVIOS INDISPENSABLES

Para utilizar la calculadora de forma correcta e interpretar sus resultados, el estudiante debe dominar los siguientes conceptos:

### Estructura de una Dirección IPv4

```
  192    .   168    .    1    .   100
└──────┘   └──────┘   └──────┘   └──────┘
 Octeto 1  Octeto 2  Octeto 3  Octeto 4

Cada octeto = 8 bits → Total: 32 bits por dirección
Rango por octeto: 0 a 255
```

### Notación CIDR y su Equivalencia en Máscara

| Prefijo CIDR | Máscara Decimal | Bits de Host | Hosts Disponibles |
|-------------|-----------------|--------------|-------------------|
| `/8` | 255.0.0.0 | 24 | 16,777,214 |
| `/16` | 255.255.0.0 | 16 | 65,534 |
| `/24` | 255.255.255.0 | 8 | 254 |
| `/25` | 255.255.255.128 | 7 | 126 |
| `/26` | 255.255.255.192 | 6 | 62 |
| `/27` | 255.255.255.224 | 5 | 30 |
| `/28` | 255.255.255.240 | 4 | 14 |
| `/29` | 255.255.255.248 | 3 | 6 |
| `/30` | 255.255.255.252 | 2 | 2 |

> **Fórmula:** Hosts disponibles = 2^(bits de host) − 2  
> Se restan 2: la dirección de red y la dirección de broadcast.

### Clases de Direcciones IPv4

| Clase | Rango (1er octeto) | Máscara por defecto | Uso típico |
|-------|--------------------|---------------------|------------|
| A | 1 – 127 | /8 | Redes muy grandes |
| B | 128 – 191 | /16 | Redes medianas |
| C | 192 – 223 | /24 | Redes pequeñas |
| D | 224 – 239 | — | Multicast (reservado) |
| E | 240 – 255 | — | Investigación (reservado) |

### Rangos Privados (RFC 1918 — No enrutables en Internet)

| Clase | Rango CIDR | Descripción |
|-------|-----------|-------------|
| A | `10.0.0.0/8` | Redes corporativas grandes |
| B | `172.16.0.0/12` | Redes medianas |
| C | `192.168.0.0/16` | Redes domésticas y laboratorios |

---

## 03 · OPERACIONES QUE PUEDE RESOLVER

```
┌────────────────────────────────────────────────────────────┐
│           CALCULADORA DE REDES — CAPACIDADES               │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  ENTRADA: IP + Máscara (decimal o CIDR)                    │
│                                                            │
│  SALIDAS CALCULADAS:                                       │
│   · Dirección de Red      (Network Address)                │
│   · Dirección de Broadcast                                 │
│   · Primer Host Disponible                                 │
│   · Último Host Disponible                                 │
│   · Número Total de Hosts Utilizables                      │
│   · Clase de la Dirección (A / B / C)                      │
│   · Máscara Wildcard (inversa de la máscara de subred)     │
│   · Prefijo CIDR equivalente                               │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## 04 · GUÍA DE USO PASO A PASO

### Paso 1 — Identificar los datos de entrada

Necesitas exactamente dos datos:
1. La **dirección IP** (de host o de red), por ejemplo: `192.168.10.50`
2. La **máscara de subred** en cualquiera de sus formatos:
   - Decimal: `255.255.255.0`
   - CIDR: `/24`

### Paso 2 — Ingresar los datos en la calculadora

```
┌─────────────────────────────────────┐
│  Dirección IP:  [ 192.168.10.50   ] │
│  Máscara:       [ 255.255.255.0   ] │
│                  ─ o bien ─         │
│  Prefijo CIDR:  [ /24             ] │
│                                     │
│         [ CALCULAR ]                │
└─────────────────────────────────────┘
```

### Paso 3 — Leer e interpretar los resultados

La calculadora devolverá el conjunto completo de parámetros de la subred. Ver sección siguiente para interpretación.

---

## 05 · INTERPRETACIÓN DE RESULTADOS

**Ejemplo de entrada:** `192.168.10.50 / 255.255.255.0`

```
┌───────────────────────────────────────────────────────┐
│  RESULTADOS PARA: 192.168.10.50 / 24                  │
├───────────────────────────────────────────────────────┤
│  Dirección de Red:        192.168.10.0                │
│  Máscara de Subred:       255.255.255.0               │
│  Prefijo CIDR:            /24                         │
│  Máscara Wildcard:        0.0.0.255                   │
│  Primer Host:             192.168.10.1                │
│  Último Host:             192.168.10.254              │
│  Broadcast:               192.168.10.255              │
│  Hosts Utilizables:       254                         │
│  Clase:                   C (red privada)             │
└───────────────────────────────────────────────────────┘
```

### ¿Qué significa cada campo?

| Campo | Definición | Usos en Cisco IOS |
|-------|------------|-------------------|
| **Dirección de Red** | Identifica la subred. No asignable a hosts. | Usada en `ip route` y comandos `network` de OSPF/EIGRP |
| **Broadcast** | Dirección de difusión. No asignable a hosts. | Referencia para delimitar el rango de la subred |
| **Primer Host** | Primera IP asignable a un dispositivo | Generalmente asignada al gateway (router) |
| **Último Host** | Última IP asignable a un dispositivo | Asignable a cualquier host de la red |
| **Máscara Wildcard** | Inversa de la máscara. Usada en ACLs y OSPF | `network 192.168.10.0 0.0.0.255 area 0` |
| **Hosts Utilizables** | Total de IPs disponibles para dispositivos | Clave para el diseño del esquema de direccionamiento |

---

## 06 · CASOS DE USO PRÁCTICOS EN CISCO

### Caso 1: Configurar la interfaz de un Router

Después de calcular la subred, aplica los resultados directamente en el CLI:

```
! Resultado de la calculadora:
! IP de host: 192.168.10.1 / 255.255.255.0

Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
```

---

### Caso 2: Agregar una red al proceso OSPF

La máscara wildcard calculada se usa directamente en el comando `network`:

```
! Resultado de la calculadora:
! Red: 192.168.10.0 | Wildcard: 0.0.0.255

Router(config)# router ospf 1
Router(config-router)# network 192.168.10.0 0.0.0.255 area 0
```

---

### Caso 3: Crear una ACL para una subred

```
! Resultado de la calculadora:
! Red: 192.168.10.0 | Wildcard: 0.0.0.255

Router(config)# access-list 10 permit 192.168.10.0 0.0.0.255
```

---

### Caso 4: Verificar si dos hosts están en la misma subred

**Host A:** `192.168.10.50 /24` → Red: `192.168.10.0`  
**Host B:** `192.168.10.200 /24` → Red: `192.168.10.0`  
**Conclusión:** Misma subred. La comunicación es directa, sin necesidad de router.

**Host A:** `192.168.10.50 /24` → Red: `192.168.10.0`  
**Host B:** `192.168.20.100 /24` → Red: `192.168.20.0`  
**Conclusión:** Subredes distintas. Requiere enrutamiento inter-VLAN o router.

---

### Caso 5: Planificación VLSM (Variable Length Subnet Masking)

La calculadora permite calcular subredes de tamaños distintos para optimizar el espacio de direccionamiento:

```
Red base: 10.0.0.0/8

Departamento A (500 hosts)  →  /23  →  10.0.0.0  –  10.0.1.255
Departamento B (200 hosts)  →  /24  →  10.0.2.0  –  10.0.2.255
Departamento C (50 hosts)   →  /26  →  10.0.3.0  –  10.0.3.63
Enlace WAN (2 hosts)        →  /30  →  10.0.3.64 –  10.0.3.67
```

---

## 07 · TABLA DE REFERENCIA RÁPIDA

### Potencias de 2 para Cálculo de Hosts

| 2^n | Valor | Hosts disponibles (2^n − 2) | Prefijo CIDR |
|-----|-------|----------------------------|--------------|
| 2^1 | 2 | 0 (no útil) | /31 |
| 2^2 | 4 | 2 | /30 |
| 2^3 | 8 | 6 | /29 |
| 2^4 | 16 | 14 | /28 |
| 2^5 | 32 | 30 | /27 |
| 2^6 | 64 | 62 | /26 |
| 2^7 | 128 | 126 | /25 |
| 2^8 | 256 | 254 | /24 |
| 2^9 | 512 | 510 | /23 |
| 2^10 | 1024 | 1022 | /22 |

### Conversión Binaria de Octetos de Máscara

| Decimal | Binario | CIDR parcial |
|---------|---------|--------------|
| 0 | 00000000 | +0 bits |
| 128 | 10000000 | +1 bit |
| 192 | 11000000 | +2 bits |
| 224 | 11100000 | +3 bits |
| 240 | 11110000 | +4 bits |
| 248 | 11111000 | +5 bits |
| 252 | 11111100 | +6 bits |
| 254 | 11111110 | +7 bits |
| 255 | 11111111 | +8 bits |

---

## 08 · ERRORES FRECUENTES

| Error | Causa | Solución |
|-------|-------|----------|
| Asignar la dirección de red a un host | Confundir `192.168.10.0` con una IP de host | La dirección de red siempre tiene los bits de host en cero |
| Asignar la dirección de broadcast a un host | Usar `192.168.10.255` como IP de dispositivo | La broadcast tiene todos los bits de host en uno |
| Usar wildcard incorrecta en OSPF | Copiar la máscara de subred en lugar de invertirla | Wildcard = 255.255.255.255 − máscara. Para /24: `0.0.0.255` |
| Hosts en subredes distintas sin gateway | Asumir conectividad directa entre subredes diferentes | Verificar con la calculadora que ambas IPs pertenecen a la misma red |
| Subredes solapadas en VLSM | No respetar los bloques de subred calculados | Siempre iniciar la siguiente subred desde la dirección siguiente al broadcast anterior |

---

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Creado por Nailea Falcón para todos los estudiantes de redes
de habla hispana.
Universidad del Valle de México · Redes 1, 2, 3 y 4
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
