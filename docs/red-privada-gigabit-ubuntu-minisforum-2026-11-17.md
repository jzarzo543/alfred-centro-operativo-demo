# Red privada Gigabit — Ubuntu ↔ MINISFORUM

**Registro:** 17 de noviembre de 2026. **Ámbito:** CASA / infraestructura de Alfred y servidor Ubuntu. **Estado:** conexión básica verificada; rendimiento real pendiente.

## Topología y finalidad

Servidor Ubuntu (julio-VOSS) ↔ TP-Link TL-SG105 (switch no gestionado Gigabit) ↔ MINISFORUM Windows 11 (Alfred).

La red Ethernet `192.168.50.0/24` es un segmento local independiente, **sin puerta de enlace ni DNS configurados en estas interfaces**. El acceso a Internet continúa por Wi-Fi. Usos previstos: transferencia de archivos y, si se verifica compatibilidad y se autoriza, comunicaciones de apagado por SAI/NUT. **El SAI no está instalado ni configurado.**

| Equipo | Interfaz | IPv4 Ethernet | Comprobación |
|---|---|---|---|
| Ubuntu 24.04 (julio-VOSS) | `enp3s0` | `192.168.50.1/24` | UP, enlace 1.000 Mbps |
| MINISFORUM Windows 11 | `Ethernet` | `192.168.50.2/24` | Up, enlace 1 Gbps, IP Preferred |

## Cambios realizados

**Ubuntu — NetworkManager:** se modificó el perfil existente `Conexión cableada 1` (no se creó uno nuevo):

```bash
sudo nmcli connection modify "Conexión cableada 1" ipv4.method manual ipv4.addresses 192.168.50.1/24 ipv4.gateway "" ipv4.dns "" ipv4.never-default yes ipv6.method disabled
sudo nmcli --wait 10 connection up "Conexión cableada 1"
```

**Windows 11 — PowerShell como administrador:**

```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.50.2 -PrefixLength 24
```

El primer intento sin privilegios de administrador devolvió «Acceso denegado»; la repetición elevada se completó. El estado inicial Tentative/Invalid fue transitorio: una consulta posterior confirmó `Preferred`.

## Evidencias observadas

- Ubuntu: `ip -4 -br addr show enp3s0` → `enp3s0 UP 192.168.50.1/24`.
- Ubuntu: `ip -4 route get 1.1.1.1` → vía `192.168.1.1`, interfaz Wi-Fi `wlxa0d76840a0f9` (origen `192.168.1.53`).
- Windows: `Get-NetIPAddress -InterfaceAlias "Ethernet" -AddressFamily IPv4` → `192.168.50.2/24`, estado `Preferred`.
- Windows → Ubuntu: `ping -n 4 192.168.50.1` → 4 recibidos, 0 perdidos; primera respuesta 94 ms, otras tres <1 ms. La primera latencia puede relacionarse con ARP, pero no se diagnosticó.
- Windows: `Find-NetRoute -RemoteIPAddress 1.1.1.1` → interfaz `Wi-Fi`, siguiente salto `192.168.1.1`.
- Windows: `Get-NetAdapter -Name "Ethernet"` → `Up`, `1 Gbps`.

**Alcance de validación:** conectividad IP y negociación física de 1 Gbps en ambos equipos. **No** equivale a medir caudal de transferencia, probar persistencia tras reinicio, verificar todos los servicios o realizar una copia de seguridad de configuración.

## Pendientes (sin ejecutar)

1. Medir caudal real entre ambos equipos. Primera comprobación prevista en Ubuntu: `command -v iperf3 || echo "iperf3 no está instalado"`. No instalar sin autorización.
2. Evaluar el kit de **3 devolo dLAN 500 duo**: uno junto al router, otros disponibles. Los puertos Ethernet son Fast Ethernet (100 Mbps); comprobar emparejamiento, velocidad y rutas **antes** de unir el PLC al switch. No se han conectado a esta nueva topología.
3. Investigar compatibilidad del SAI propuesto **Phasak Smart Lite PH 9499 950 VA / 570 W** con NUT; no dar por confirmada la compatibilidad ni comprar/configurar sin autorización.
4. Validar persistencia de la red tras reinicio planificado y revisar estado de servicios, sin reiniciar por este registro.

## Comprobaciones de solo lectura

Ubuntu:
```bash
nmcli -g ipv4.method,ipv4.addresses,ipv4.gateway,ipv4.never-default connection show "Conexión cableada 1"
ip -4 -br addr show enp3s0
ip -4 route get 1.1.1.1
```

Windows (PowerShell):
```powershell
Get-NetIPAddress -InterfaceAlias "Ethernet" -AddressFamily IPv4 | Format-Table IPAddress,PrefixLength,AddressState,PrefixOrigin -AutoSize
Get-NetAdapter -Name "Ethernet" | Select-Object Name,Status,LinkSpeed
Find-NetRoute -RemoteIPAddress 1.1.1.1 | Select-Object InterfaceAlias,NextHop
ping -n 4 192.168.50.1
```

## Reversión prevista (NO ejecutada)

**Solo con autorización y verificando antes el estado actual y el acceso remoto.** No borrar perfiles ni tocar Wi-Fi/Tailscale/Docker.

- Ubuntu: restaurar el perfil `Conexión cableada 1` a `ipv4.method auto` y reactivar solo si se decide volver al estado DHCP original; revisar `ipv4.never-default` y `ipv6.method` para restaurar también los valores previos apropiados. El perfil original usaba DHCP, sin IP ni gateway asignados; no se capturó una copia completa de todos sus ajustes.
- Windows: eliminar **solo** la dirección `192.168.50.2` de la interfaz `Ethernet`, previa comprobación de la configuración actual; no restablecer el adaptador entero.

## Seguridad documental

Este archivo describe la configuración observada, **no es un respaldo exportado** de NetworkManager o Windows. No contiene credenciales, tokens, direcciones MAC ni identificadores de dispositivos. El repositorio del Centro Operativo puede ser público: evitar incorporar secretos en futuros registros.
