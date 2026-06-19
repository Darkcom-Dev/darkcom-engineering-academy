# Comandos de Red en Linux

Los comandos de red en Linux son esenciales para configurar, diagnosticar y solucionar problemas de conectividad. Esta guía cubre los comandos más utilizados para gestionar interfaces de red, probar conectividad, analizar tráfico y configurar servicios de red.

## Visualización y Configuración de Interfaces

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **ip** | Herramienta moderna para gestionar interfaces, rutas y más | `ip addr show` |
| **ifconfig** | Herramienta tradicional (en desuso) para configurar interfaces | `ifconfig eth0` |
| **iwconfig** | Configura interfaces inalámbricas | `iwconfig wlan0` |
| **nmcli** | Cliente de línea de comandos para NetworkManager | `nmcli device status` |
| **netstat** | Muestra conexiones, tablas de enrutamiento, estadísticas de interfaces | `netstat -tulnp` |
| **ss** | Sustituto moderno de netstat, más rápido y con más información | `ss -tulnp` |
| **ethtool** | Consulta y configura controladores de red Ethernet | `ethtool eth0` |

## Pruebas de Conectividad

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **ping** | Prueba de conectividad usando ICMP Echo | `ping 8.8.8.8` |
| **ping6** | Versión IPv6 de ping | `ping6 2001:4860:4860::8888` |
| **traceroute** | Muestra el camino que siguen los paquetes a un destino | `traceroute google.com` |
| **tracepath** | Similar a traceroute pero sin requerir privilegios | `tracepath 8.8.8.8` |
| **hping** | Herramienta avanzada para pruebas de red personalizables | `hping3 -S -p 80 -c 5 google.com` |
| **fping** | Ping a múltiples hosts en paralelo | `fping 192.168.1.1 192.168.1.2` |
| **nping** | Parte de Nmap, permite generar paquetes personalizados | `nping --tcp -p 80 -c 5 google.com` |

## Análisis y Monitoreo de Tráfico

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **tcpdump** | Capturador y analizador de paquetes de red | `tcpdump -i eth0 port 80` |
| **wireshark** | Analizador gráfico de paquetes (también tiene versión CLI: tshark) | `tshark -i eth0 -Y http` |
| **iftop** | Muestra ancho de banda utilizado por host en tiempo real | `iftop -i eth0` |
| **nethogs** | Agrupa ancho de banda por proceso | `nethogs` |
| **iptraf** | Monitor de tráfico LAN en modo texto | `iptraf` |
| **vnstat** | Monitor de consumo de tráfico de red basado en historial | `vnstat -l` |
| **bmon** | Monitor de ancho de banda y tasa de error con soporte para múltiples interfaces | `bmon` |
| **arp** | Muestra y modifica la caché ARP | `arp -a` |
| **netstat -s** | Muestra estadísticas detalladas de protocolos de red | `netstat -s` |

## Herramientas de Resolución de Nombres

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **nslookup** | Consulta servidores DNS para obtener información de hosts | `nslookup google.com` |
| **dig** | Herramienta flexible para consultar servidores DNS | `dig @8.8.8.8 google.com` |
| **host** | Simple utilidad para realizar consultas DNS | `host google.com` |
| **whois** | Consulta registros de dominios y direcciones IP | `whois google.com` |
| **getent** | Obtiene entradas de bases de datos administrativas (incluyendo hosts) | `getent hosts google.com` |

## Configuración de Servicios de Red

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **iptables** | Configura tablas de filtrado y NAT de IPv4 | `iptables -L -v -n` |
| **ip6tables** | Versión IPv6 de iptables | `ip6tables -L` |
| **ufw** | Interfaz sencilla para configurar firewall (Uncomplicated Firewall) | `ufw status verbose` |
| **firewall-cmd** | Cliente para firewalld (default en RHEL/CentOS/Fedora) | `firewall-cmd --list-all` |
| **nft** | Sucesor de iptables usando la nueva infraestructura nftables | `nft list ruleset` |
| **sysctl** | Configura parámetros del kernel en tiempo real | `sysctl net.ipv4.ip_forward=1` |
| **ethtool** | Configura parámetros de velocidad, duplex y offloading de interfaces NIC | `ethtool -s eth0 speed 1000 duplex full autoneg off` |

## Diagnóstico Avanzado

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| **mtr** | Combina funcionalidad de ping y traceroute con actualización continua | `mtr google.com` |
| **tracepath** | Similar a traceroute pero interpreta MTU en el camino | `tracepath google.com` |
| **netcat (nc)** | Herramienta versátil para lectura y escritura a través de conexiones de red | `nc -zv google.com 443` |
| **socat** | Multiplexor de canales bidireccional de datos | `socat - TCP4:google.com:80` |
| **lsof** | Lista archivos abiertos, incluyendo sockets de red | `lsof -i -P -n` |
| **ss -s** | Muestra estadísticas resumidas de sockets | `ss -s` |

## Buenas Prácticas

1. **Preferir `ip` sobre `ifconfig`**: El comando `ip` es parte del paquete `iproute2` y es más potente y moderno.
2. **Usar `ss` en lugar de `netstat`**: `ss` es más rápido y muestra más información, especialmente en sistemas con muchas conexiones.
3. **Combinar herramientas**: Para diagnóstico completo, use múltiples comandos (ej: `ping` para conectividad básica, `traceroute` para ruta, `tcpdump` para captura de paquetes).
4. **Persistir configuraciones**: Los cambios hechos con `ip` o `ifconfig` son temporales. Para hacerlos permanentes, use los archivos de configuración de su distribución (`/etc/network/interfaces` en Debian/Ubuntu, o los perfiles de NetworkManager).
5. **Monitoreo continuo**: Para entornos de producción, considere usar herramientas de monitoreo como `Prometheus` + `node_exporter` o `Zabbix` en lugar de comandos puntuales.
6. **Seguridad**: Siempre revise las reglas de firewall (`iptables`, `nft`, `ufw`) después de hacer cambios de red para evitar dejar el sistema expuesto.
7. **IPv6**: No olvide probar y configurar IPv6 cuando esté disponible; muchos de los comandos tienen equivalentes IPv6 (`ping6`, `ip -6 addr`, etc.).

> **Nota**: La gestión de red en Linux es vasta y estos comandos representan solo las herramientas más comunes. Para tareas especializadas (como configuración de VPN, VLANs, bonding, bridges, etc.) existen comandos y archivos de configuración adicionales específicos para cada caso.