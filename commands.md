# ◼ CISCO IOS · REFERENCIA JERÁRQUICA DE COMANDOS

> Clasificación estructural: **FROM BEGINNER TO ADVANCED**  
> Simbología: `[PB]` Pantalla Blanca (Cisco IOS) · `[PN]` Pantalla Negra (Host) · `[E2E]` Ambos entornos

---

## NIVEL 1 — BEGINNER
### Navegación y Control de Sesión

Dominar estos comandos es el requisito previo absoluto para operar cualquier dispositivo Cisco.

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `enable` | `Device>` | Escala al modo EXEC Privilegiado (`#`) |
| `[PB]` | `configure terminal` | `Device#` | Ingresa al modo de Configuración Global |
| `[PB]` | `exit` | Cualquiera | Retrocede al modo anterior en la jerarquía |
| `[PB]` | `end` | Cualquier sub-modo | Regresa directamente al modo `#` |
| `[PB]` | `logout` | `Device>` o `Device#` | Cierra la sesión activa del usuario |
| `[PB]` | `?` | Cualquiera | Lista todos los comandos disponibles en el modo actual |
| `[PB]` | `[comando] ?` | Cualquiera | Muestra opciones disponibles para un comando |

---

### Configuración Básica del Dispositivo

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `hostname [nombre]` | `(config)#` | Asigna nombre identificador al dispositivo |
| `[PB]` | `no ip domain-lookup` | `(config)#` | Deshabilita resolución DNS; evita bloqueos por errores de tipeo |
| `[PB]` | `cdp run` | `(config)#` | Asegura la ejecución del Protocolo de Descubrimiento Cisco |
| `[PB]` | `lldp run` | `(config)#` | Habilita LLDP para interoperabilidad con dispositivos no-Cisco |

---

### Configuración de Consola

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `line console 0` | `(config)#` | Accede a la configuración de la línea de consola |
| `[PB]` | `logging synchronous` | `(config-line)#` | Sincroniza mensajes del sistema con la entrada del usuario |
| `[PB]` | `history size [n]` | `(config-line)#` | Define el tamaño del historial de comandos |
| `[PB]` | `exec-timeout [min] [seg]` | `(config-line)#` | Tiempo de inactividad antes de cierre automático de sesión |

---

### Persistencia y Auditoría Básica

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `copy running-config startup-config` | `Device#` | Guarda configuración de RAM a NVRAM (persistente) |
| `[PB]` | `show running-config` | `Device#` | Muestra la configuración activa en RAM |
| `[PB]` | `show startup-config` | `Device#` | Muestra la configuración guardada en NVRAM |
| `[PB]` | `show history` | `Device#` | Lista los últimos comandos ejecutados en la sesión |
| `[PB]` | `show version` | `Device#` | Muestra versión de IOS, hardware y tiempo de actividad |

---

## NIVEL 2 — ELEMENTARY
### Diagnóstico y Conectividad End-to-End

Comandos basados en ICMP y TCP para validar la comunicación entre dispositivos.

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[E2E]` | `ping [ip-address]` | Cualquiera | Envía ICMP Echo Request; verifica alcance de Capa 3 |
| `[PN]` | `tracert [ip-address]` | CMD (Windows) | Mapea la ruta salto a salto (sintaxis Windows) |
| `[PB]` | `traceroute [ip-address]` | `Device#` | Equivalente de tracert en Cisco IOS |
| `[E2E]` | `telnet [ip-address]` | Cualquiera | Acceso remoto al CLI de un dispositivo (no cifrado) |
| `[PN]` | `ipconfig` | CMD (Windows) | Muestra configuración IP del host local |
| `[PN]` | `ipconfig /all` | CMD (Windows) | Muestra configuración IP detallada incluyendo MAC |
| `[PN]` | `nslookup [dominio]` | CMD (Windows) | Resuelve nombres DNS desde el host |

---

### Verificación de Interfaces

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `show ip interface brief` | `Device#` | Resumen de estado y IPs de todas las interfaces |
| `[PB]` | `show interface [int-id]` | `Device#` | Información detallada de una interfaz específica |
| `[PB]` | `show mac address-table` | `Device#` | Tabla de MACs aprendidas (exclusivo de Switches) |

> **Tip:** Desde el modo `(config)#`, antepone `do` para ejecutar comandos de verificación:
> ```
> Device(config)# do show ip interface brief
> ```

---

## NIVEL 3 — INTERMEDIATE
### Configuración de Interfaces

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `interface [int-id]` | `(config)#` | Accede a la configuración de una interfaz |
| `[PB]` | `interface range [tipo][n]-[n]` | `(config)#` | Configura múltiples interfaces simultáneamente |
| `[PB]` | `ip address [ip] [máscara]` | `(config-if)#` | Asigna dirección IP a la interfaz |
| `[PB]` | `no shutdown` | `(config-if)#` | Activa la interfaz (por defecto están apagadas en routers) |
| `[PB]` | `shutdown` | `(config-if)#` | Desactiva administrativamente la interfaz |
| `[PB]` | `description [texto]` | `(config-if)#` | Agrega descripción identificadora a la interfaz |

---

### Rutas Estáticas

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `ip route [red] [máscara] [next-hop]` | `(config)#` | Ruta estática estándar. AD = 1 |
| `[PB]` | `ip route 0.0.0.0 0.0.0.0 [next-hop]` | `(config)#` | Ruta por defecto (Gateway of Last Resort) |
| `[PB]` | `ip route [red] [máscara] [next-hop] [AD]` | `(config)#` | Ruta flotante (AD mayor = menor prioridad, actúa como respaldo) |
| `[PB]` | `show ip route` | `Device#` | Muestra la tabla de enrutamiento completa |
| `[PB]` | `show ip route static` | `Device#` | Filtra y muestra solo las rutas estáticas |

---

### VLANs y Conmutación

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `vlan [ID]` | `(config)#` | Crea una VLAN con el número de identificación especificado |
| `[PB]` | `name [nombre]` | `(config-vlan)#` | Asigna nombre descriptivo a la VLAN activa |
| `[PB]` | `no vlan [ID]` | `(config)#` | Elimina la VLAN especificada |
| `[PB]` | `switchport mode access` | `(config-if)#` | Configura el puerto en modo acceso (host final) |
| `[PB]` | `switchport access vlan [ID]` | `(config-if)#` | Asigna el puerto a una VLAN específica |
| `[PB]` | `no switchport access vlan [ID]` | `(config-if)#` | Retira el puerto de la VLAN (vuelve a VLAN 1) |
| `[PB]` | `switchport mode trunk` | `(config-if)#` | Configura puerto troncal (transporta múltiples VLANs) |
| `[PB]` | `switchport trunk native vlan [ID]` | `(config-if)#` | Define la VLAN nativa del enlace troncal |
| `[PB]` | `switchport trunk allowed vlan [lista]` | `(config-if)#` | Define VLANs permitidas en el trunk |
| `[PB]` | `show vlan` | `Device#` | Lista todas las VLANs configuradas con detalles |
| `[PB]` | `show vlan brief` | `Device#` | Resumen de VLANs, estados, nombres y puertos |
| `[PB]` | `show interfaces trunk` | `Device#` | Verifica enlaces troncales activos |

---

### Router-on-a-Stick (ROAS — Inter-VLAN Routing)

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `interface [int-id].[vlan-id]` | `(config)#` | Crea subinterfaz lógica para la VLAN |
| `[PB]` | `encapsulation dot1q [vlan-id]` | `(config-subif)#` | Habilita encapsulación 802.1Q |
| `[PB]` | `encapsulation dot1q [vlan-id] native` | `(config-subif)#` | Declara la subinterfaz como la VLAN nativa |
| `[PB]` | `ip address [ip] [máscara]` | `(config-subif)#` | Asigna gateway predeterminado para esa VLAN |

---

## NIVEL 4 — ADVANCED
### OSPF — Enrutamiento Dinámico

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `router ospf [PID]` | `(config)#` | Inicia el proceso OSPF. PID: 1–65535 |
| `[PB]` | `router-id [a.b.c.d]` | `(config-router)#` | Asigna manualmente el Router ID (formato IP) |
| `[PB]` | `network [red] [wildcard] area [id]` | `(config-router)#` | Anuncia red directamente conectada al proceso OSPF |
| `[PB]` | `passive-interface [int-id]` | `(config-router)#` | Evita que la interfaz envíe actualizaciones OSPF |
| `[PB]` | `show ip ospf neighbor` | `Device#` | Lista los vecinos OSPF establecidos |
| `[PB]` | `show ip ospf` | `Device#` | Información del proceso: PID, Router ID, área, SPF |
| `[PB]` | `show ip ospf interface [int-id]` | `Device#` | Detalles OSPF de una interfaz específica |
| `[PB]` | `show ip route ospf` | `Device#` | Filtra tabla de enrutamiento: solo entradas OSPF |
| `[PB]` | `show ip protocols` | `Device#` | Verificación rápida de protocolos de enrutamiento activos |

---

### ACLs — Listas de Control de Acceso

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `access-list [num] [permit/deny] [addr] [wildcard]` | `(config)#` | Crea entrada en ACL numerada estándar |
| `[PB]` | `no access-list [num]` | `(config)#` | Elimina la ACL completa (todas sus entradas) |
| `[PB]` | `ip access-list standard [nombre]` | `(config)#` | Crea ACL estándar nombrada |
| `[PB]` | `ip access-list extended [nombre]` | `(config)#` | Crea ACL extendida nombrada |
| `[PB]` | `ip access-group [num/nombre] [in/out]` | `(config-if)#` | Aplica la ACL a una interfaz en dirección in o out |
| `[PB]` | `show ip access-lists` | `Device#` | Muestra todas las ACLs configuradas con contadores de coincidencia |

> **Regla de ubicación:**  
> ACL Estándar → colocar cerca del **Destino**  
> ACL Extendida → colocar cerca del **Origen**

---

### DHCPv4

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `ip dhcp excluded-address [inicio] [fin]` | `(config)#` | Excluye IPs del pool DHCP (gateways, servidores) |
| `[PB]` | `ip dhcp pool [nombre]` | `(config)#` | Crea y nombra el pool de direcciones DHCP |
| `[PB]` | `network [red] [máscara]` | `(dhcp-config)#` | Define el rango de direcciones del pool |
| `[PB]` | `default-router [ip]` | `(dhcp-config)#` | Define el gateway predeterminado para los clientes |
| `[PB]` | `dns-server [ip]` | `(dhcp-config)#` | Especifica el servidor DNS para los clientes |
| `[PB]` | `lease [días] [horas] [minutos]` | `(dhcp-config)#` | Define el tiempo de arrendamiento de la dirección IP |
| `[PB]` | `ip helper-address [ip-servidor-dhcp]` | `(config-if)#` | Reenvía solicitudes DHCP a un servidor externo (DHCP Relay) |
| `[PB]` | `show ip dhcp binding` | `Device#` | Muestra las asignaciones DHCP activas |

---

### Port Security (Switches)

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `switchport port-security` | `(config-if)#` | Habilita Port Security (requiere modo access primero) |
| `[PB]` | `switchport port-security maximum [n]` | `(config-if)#` | Número máximo de MACs permitidas en el puerto |
| `[PB]` | `switchport port-security mac-address sticky` | `(config-if)#` | Aprende MACs dinámicamente y las persiste en la config |
| `[PB]` | `switchport port-security violation [modo]` | `(config-if)#` | Acción ante violación: protect / restrict / shutdown |
| `[PB]` | `show port-security` | `Device#` | Resumen de Port Security en todas las interfaces |
| `[PB]` | `show port-security interface [int-id]` | `Device#` | Detalles de Port Security en una interfaz |
| `[PB]` | `show port-security address` | `Device#` | MACs seguras configuradas en todas las interfaces |

---

### SSH — Acceso Remoto Seguro

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `ip domain-name [dominio]` | `(config)#` | Requisito previo para generar llaves RSA |
| `[PB]` | `crypto key generate rsa` | `(config)#` | Genera el par de llaves criptográficas RSA |
| `[PB]` | `ip ssh version 2` | `(config)#` | Habilita exclusivamente SSH versión 2 |
| `[PB]` | `username [usuario] secret [contraseña]` | `(config)#` | Crea usuario local con contraseña cifrada |
| `[PB]` | `line vty 0 15` | `(config)#` | Accede a la configuración de las líneas VTY |
| `[PB]` | `transport input ssh` | `(config-line)#` | Restringe acceso remoto exclusivamente a SSH |
| `[PB]` | `login local` | `(config-line)#` | Autentica usando la base de datos de usuarios local |
| `[PB]` | `ip ssh time-out [segundos]` | `(config)#` | Modifica el timeout de la sesión SSH |
| `[PB]` | `ip ssh authentication-retries [n]` | `(config)#` | Número máximo de intentos de autenticación |
| `[PB]` | `show ip ssh` | `Device#` | Verifica la configuración y estado del servidor SSH |
| `[PB]` | `crypto key zeroise rsa` | `(config)#` | Elimina el par de llaves RSA |

---

## NIVEL 5 — EXPERT
### NAT — Traducción de Direcciones de Red

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `ip nat inside source static [local] [global]` | `(config)#` | Configura NAT estático: mapeo 1:1 de IP local a global |
| `[PB]` | `ip nat pool [nombre] [inicio] [fin] netmask [máscara]` | `(config)#` | Crea pool de IPs públicas para NAT dinámico |
| `[PB]` | `ip nat inside source list [acl] pool [pool]` | `(config)#` | Vincula el pool a la ACL para NAT dinámico |
| `[PB]` | `ip nat inside` | `(config-if)#` | Marca la interfaz como interior (red privada) |
| `[PB]` | `ip nat outside` | `(config-if)#` | Marca la interfaz como exterior (red pública) |
| `[PB]` | `show ip nat translations` | `Device#` | Muestra la tabla de traducciones NAT activas |
| `[PB]` | `clear ip nat translation *` | `Device#` | Limpia todas las entradas de la tabla NAT |

---

### HSRP — Alta Disponibilidad de Gateway

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `standby version 2` | `(config-if)#` | Habilita HSRPv2 (recomendado; soporta IPv6 y 4095 grupos) |
| `[PB]` | `standby [grupo] ip [ip-virtual]` | `(config-if)#` | Define grupo y dirección IP del gateway virtual |
| `[PB]` | `standby [grupo] priority [0-255]` | `(config-if)#` | Prioridad para la elección del router activo |
| `[PB]` | `standby [grupo] preempt` | `(config-if)#` | Permite recuperar el rol activo si la prioridad es mayor |
| `[PB]` | `standby [grupo] timers [hello] [hold]` | `(config-if)#` | Define temporizadores hello y hold |
| `[PB]` | `show standby` | `Device#` | Verifica estado y configuración de HSRP |
| `[PB]` | `show standby brief` | `Device#` | Resumen de grupos HSRP activos |

---

### STP — Spanning Tree Protocol

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `spanning-tree vlan [id] root primary` | `(config)#` | Garantiza que este switch sea el root bridge primario |
| `[PB]` | `spanning-tree vlan [id] root secondary` | `(config)#` | Configura root bridge alternativo (respaldo) |
| `[PB]` | `spanning-tree vlan [id] priority [valor]` | `(config)#` | Configura manualmente la prioridad del bridge (múltiplo de 4096) |
| `[PB]` | `spanning-tree mode rapid-pvst` | `(config)#` | Habilita Rapid PVST+ como modo STP |
| `[PB]` | `spanning-tree portfast` | `(config-if)#` | Activa PortFast en puerto de acceso (elimina retardo STP) |
| `[PB]` | `spanning-tree bpduguard enable` | `(config-if)#` | Protege puertos PortFast; desactiva al recibir BPDUs |
| `[PB]` | `show spanning-tree` | `Device#` | Información completa de instancias STP y root bridge |
| `[PB]` | `show spanning-tree vlan [id]` | `Device#` | STP información para una VLAN específica |
| `[PB]` | `show spanning-tree summary` | `Device#` | Resumen del estado de puertos STP |

---

### EtherChannel — Agregación de Enlaces

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `interface range [inicio]-[fin]` | `(config)#` | Selecciona el grupo de interfaces para el bundle |
| `[PB]` | `channel-group [num] mode [modo]` | `(config-if-range)#` | Crea el EtherChannel con protocolo especificado |
| `[PB]` | `interface port-channel [num]` | `(config)#` | Accede a la interfaz lógica del EtherChannel |
| `[PB]` | `show etherchannel summary` | `Device#` | Estado y modo de todos los EtherChannels |
| `[PB]` | `show etherchannel port-channel` | `Device#` | Información detallada de los port channels |

**Modos de EtherChannel:**

| Modo | Protocolo | Comportamiento |
|------|-----------|----------------|
| `active` | LACP | Negocia activamente con el extremo |
| `passive` | LACP | Responde solo si el otro extremo negocia |
| `desirable` | PAgP | Negocia activamente (propietario Cisco) |
| `auto` | PAgP | Responde solo si el otro extremo negocia |
| `on` | Ninguno | Fuerza EtherChannel sin negociación |

---

### VTP — VLAN Trunking Protocol

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `vtp mode [server/client/transparent]` | `(config)#` | Define el rol del switch en el dominio VTP |
| `[PB]` | `vtp domain [nombre]` | `(config)#` | Asigna el nombre del dominio VTP (sensible a mayúsculas) |
| `[PB]` | `vtp password [contraseña]` | `(config)#` | Define contraseña del dominio VTP |
| `[PB]` | `vtp pruning` | `(config)#` | Activa VTP pruning en el servidor |
| `[PB]` | `vtp version 2` | `(config)#` | Habilita VTP versión 2 |
| `[PB]` | `show vtp status` | `Device#` | Verifica configuración y estado del VTP |
| `[PB]` | `show vtp password` | `Device#` | Muestra la contraseña VTP configurada |

---

### EIGRP — Enhanced Interior Gateway Routing Protocol

| Entorno | Comando | Modo requerido | Descripción |
|---------|---------|----------------|-------------|
| `[PB]` | `router eigrp [AS]` | `(config)#` | Inicia el proceso EIGRP con el número de AS especificado |
| `[PB]` | `eigrp router-id [a.b.c.d]` | `(config-router)#` | Asigna manualmente el Router ID |
| `[PB]` | `network [red] [wildcard]` | `(config-router)#` | Anuncia red directamente conectada a EIGRP |
| `[PB]` | `no auto-summary` | `(config-router)#` | Deshabilita la sumarización automática de redes |
| `[PB]` | `redistribute static` | `(config-router)#` | Propaga la ruta estática por defecto |
| `[PB]` | `variance [valor]` | `(config-router)#` | Configura balanceo de carga de costo desigual |
| `[PB]` | `show ip eigrp neighbors` | `Device#` | Tabla de vecinos EIGRP reconocidos |
| `[PB]` | `show ip route eigrp` | `Device#` | Filtra tabla de enrutamiento: solo entradas EIGRP |
| `[PB]` | `show ip eigrp interfaces` | `Device#` | Interfaces habilitadas para EIGRP |

---

## REFERENCIA RÁPIDA — FILTRADO DE SALIDA

Aplicable a cualquier comando `show` que genere múltiples líneas de salida:

```
Device# show running-config | section [expresión]
Device# show running-config | include [expresión]
Device# show running-config | exclude [expresión]
Device# show running-config | begin  [expresión]
```

**Ejemplo práctico:**
```
Device# show running-config | include line con
Device# show ip route | begin Gateway
```

---

## TABLA DE DISTANCIAS ADMINISTRATIVAS (AD)

| Código | Tipo | AD | Confiabilidad |
|--------|------|----|---------------|
| `C` | Directamente conectada | 0 | Máxima |
| `S` | Ruta estática | 1 | Muy alta |
| `D` | EIGRP interno | 90 | Alta |
| `O` | OSPF | 110 | Alta |
| `R` | RIP | 120 | Media |
| `EX` | EIGRP externo | 170 | Baja |

---
