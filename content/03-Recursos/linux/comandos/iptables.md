# Iptables: Gestión de Firewall en Linux

`iptables` es la herramienta de línea de comandos utilizada para configurar, mantener e inspeccionar las tablas de reglas de filtrado de paquetes IP en el kernel de Linux. Forms parte del sistema Netfilter y es esencial para la seguridad de redes, permitiendo actuar como firewall, realizar NAT (Network Address Translation) y filtrar paquetes según diversos criterios.

> **Nota**: En distribuciones modernas de Linux, `nftables` está reemplazando gradualmente a `iptables` como el framework subyacente, pero el comando `iptables` sigue siendo ampliamente utilizado y disponible como interfaz de compatibilidad.

## Conceptos Fundamentales

### Tablas y Cadenas
Iptables organiza las reglas en **tablas**, y cada tabla contiene **cadenas** (chains) donde se evalúan los paquetes:

| Tabla | Propósito | Cadenas Predeterminadas |
|-------|-----------|-------------------------|
| `filter` | Filtrado de paquetes (funcionalidad de firewall principal) | `INPUT`, `FORWARD`, `OUTPUT` |
| `nat` | Traducción de direcciones de red (NAT) | `PREROUTING`, `INPUT`, `OUTPUT`, `POSTROUTING` |
| `mangle` | Alteración especializada de paquetes | `PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING` |
| `raw` | Configuración de excepciones para el seguimiento de conexiones | `PREROUTING`, `OUTPUT` |
| `security` | Reglas de seguridad MAC (Mandatory Access Control) | `INPUT`, `OUTPUT`, `FORWARD` |

### Objetivos (Targets)
Cuando un paquete coincide con una regla, se especifica qué acción tomar mediante un **objetivo**:

| Objetivo | Descripción |
|----------|-------------|
| `ACCEPT` | Permite que el paquete continúe su camino |
| `DROP` | Elimina el paquete sin notificar al remitente |
| `REJECT` | Elimina el paquete y envía una notificación de error al remitente |
| `LOG` | Registra información del paquete en el log del sistema y continúa evaluando reglas |
| `DNAT` | Traducción de Dirección de Destino (solo en tabla `nat`) |
| `SNAT` | Traducción de Dirección de Origen (solo en tabla `nat`) |
| `MASQUERADE` | Forma especial de SNAT para IPs dinámicas (solo en tabla `nat`) |
| `REDIRECT` | Redirige paquetes al propio máquina (solo en tabla `nat`) |

## Sintaxis Básica

```bash
iptables [-t tabla] operación [cadena] [especificadores de coincidencia] -j objetivo
```

- `-t tabla`: Especifica la tabla (por defecto: `filter`)
- `operación`: Acción a realizar (ej: `-A` para agregar, `-D` para eliminar, `-L` para listar)
- `cadena`: Cadena donde aplicar la regla (ej: `INPUT`, `OUTPUT`)
- `especificadores de coincidencia`: Condiciones que deben cumplirse para que la regla se aplique
- `-j objetivo`: Especifica qué hacer cuando se cumple la coincidencia

## Operaciones Principales

| Operación | Descripción | Ejemplo |
|-----------|-------------|---------|
| `-A` | Agrega una regla al final de la cadena | `iptables -A INPUT -p tcp --dport 22 -j ACCEPT` |
| `-I` | Inserta una regla en una posición específica | `iptables -I INPUT 3 -p tcp --dport 80 -j ACCEPT` |
| `-D` | Elimina una regla (por especificación o número de línea) | `iptables -D INPUT -p tcp --dport 22 -j ACCEPT` |
| `-R` | Reemplaza una regla existente | `iptables -R INPUT 2 -s 10.0.0.5 -j DROP` |
| `-L` | Lista todas las reglas en una cadena o tabla | `iptables -L INPUT` |
| `-F` | Vacía (flush) todas las reglas en una cadena o tabla | `iptables -F` |
| `-Z` | Poner a cero los contadores de paquetes y bytes | `iptables -Z` |
| `-N` | Crea una nueva cadena definida por el usuario | `iptables -N mi_cadena` |
| `-X` | Elimina una cadena definida por el usuario | `iptables -X mi_cadena` |
| `-P` | Establece la política predeterminada para una cadena | `iptables -P INPUT DROP` |

## Especificadores de Coincidencia Más Utilizados

### Especificadores Generales
| Especificador | Descripción | Ejemplo |
|---------------|-------------|---------|
| `-p protocolo` | Especifica el protocolo (tcp, udp, icmp, etc.) | `-p tcp` |
| `-s dirección[/máscara]` | Dirección de origen | `-s 192.168.1.100` |
| `-d dirección[/máscara]` | Dirección de destino | `-d 10.0.0.0/24` |
| `-i interfaz` | Interfaz de entrada | `-i eth0` |
| `-o interfaz` | Interfaz de salida | `-o eth0` |
| `--dport puerto` | Puerto de destino (para TCP/UDP) | `--dport 80` |
| `--sport puerto` | Puerto de origen (para TCP/UDP) | `--sport 22` |
| `--tcp-flags máscara comprobación` | Banderas TCP específicas | `--tcp-flags SYN,RST,ACK SYN` |
| `-m state --state estado` | Estado de la conexión (requiere módulo state) | `-m state --state ESTABLISHED,RELATED` |

### Ejemplos de Coincidencias Comunes
```bash
# Paquetes TCP hacia el puerto 22 (SSH) en interfaz eth0
iptables -A INPUT -p tcp -i eth0 --dport 22 -j ACCEPT

# Paquetes desde una red específica
iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT

# Paquetes hacia un rango de puertos
iptables -A INPUT -p tcp --dport 8000:8010 -j ACCEPT

# Paquetes de conexiones establecidas o relacionadas
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Paquetes ICMP (ping)
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
```

## Configuración Práctica

### Configuración Básica de Firewall
```bash
# 1. Establecer políticas predeterminadas seguras
iptables -P INPUT DROP    # Descartar todo lo que no esté explícitamente permitido
iptables -P FORWARD DROP  # No reenviar paquetes por defecto
iptables -P OUTPUT ACCEPT # Permitir tráfico saliente (ajustar según política)

# 2. Permitir tráfico esencial
iptables -A INPUT -i lo -j ACCEPT                    # Loopback (local)
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT  # Conexiones existentes
iptables -A INPUT -p tcp --dport 22 -j ACCEPT        # SSH
iptables -A INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT  # HTTP/HTTPS

# 3. Registrar intentos no autorizados (opcional)
iptables -A INPUT -j LOG --log-prefix "IPTABLES-DROP: " --log-level 4
```

### Configuración de NAT (Network Address Translation)
#### IP Masquerading (para compartir conexión a Internet)
```bash
# Asumiendo que eth0 es la interfaz conectada a Internet y eth1 a la red local
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Permitir forwarding (reenvío) entre interfaces
iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
iptables -A FORWARD -i eth0 -o eth1 -m state --state ESTABLISHED,RELATED -j ACCEPT

# Habilitar IP forwarding en el kernel
echo 1 > /proc/sys/net/ipv4/ip_forward
# O para hacerlo permanente, agregar a /etc/sysctl.conf:
# net.ipv4.ip_forward = 1
```

#### Redirección de Puertos (Port Forwarding)
```bash
# Redirigir tráfico HTTP externo al puerto 8080 interno
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

# O redirigir a un servidor interno específico (DNAT)
iptables -t nat -A PREROUTING -p tcp -d 200.100.50.25 --dport 80 -j DNAT --to-dest 192.168.1.100:80
```

### Reglas Avanzadas
#### Limitación de Tasa (Rate Limiting)
```bash
# Limitar conexiones SSH a 3 por minuto por IP
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP

# Alternativa usando el módulo limit
iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j DROP
```

#### Bloqueo de Escaneos de Puertos
```bash
# Detectar y bloquear paquetes XMAS (todos los flags activos)
iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP

# Detectar y bloquear paquetes NULL (ningún flag activo)
iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP

# Detectar y bloquear escaneo FIN
iptables -A INPUT -p tcp --tcp-flags FIN,PSH,URH FIN,PSH,URH -j DROP
```

## Gestión y Persistencia

### Guardar y Restaurar Reglas
Las reglas de iptables se pierden al reiniciar el sistema a menos que se guarden explícitamente.

#### Métodos de Guardado
```bash
# Guardar reglas actuales en un archivo
iptables-save > /etc/iptables/rules.v4

# Para IPv6 (usando ip6tables)
ip6tables-save > /etc/iptables/rules.v6

# Restaurar desde archivo
iptables-restore < /etc/iptables/rules.v4
ip6tables-restore < /etc/iptables/rules.v6
```

#### Herramientas de Distribución
- **Debian/Ubuntu**: Instalar `iptables-persistent` para guardar/restaurar automáticamente
- **RHEL/CentOS/Fedora**: Usar `service iptables save` o `iptables-service`
- **Arch Linux**: Usar `iptables-restored` y `iptables-stored` con systemd

### Verificación y Monitoreo
```bash
# Mostrar reglas con contadores de paquetes y bytes
iptables -L -v -n

# Mostrar reglas de una tabla específica
iptables -t nat -L -v -n
iptables -t mangle -L -v -n

# Mostrar reglas con números de línea (útil para eliminación precisa)
iptables -L INPUT --line-numbers

# Mostrar reglas en formato que puede ser usado directamente con iptables-restore
iptables-save

# Monitorear cambios en tiempo real (requiere watch)
watch -n 1 'iptables -L -v -n'
```

## Buenas Prácticas y Consejos

### Para Configuración Segura
1. **Empiece con políticas predeterminadas restrictivas**:
   ```bash
   iptables -P INPUT DROP
   iptables -P FORWARD DROP
   iptables -P OUTPUT DROP  # O ACCEPT según necesite controlar salida también
   ```

2. **Permita explícitamente lo necesario**:
   ```bash
   iptables -A INPUT -i lo -j ACCEPT                    # Siempre permitir loopback
   iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT  # Esencial
   ```

3. **Use rangos de puertos cuando sea posible** para reducir número de reglas:
   ```bash
   # En lugar de múltiples reglas para 80, 443, 8080:
   iptables -A INPUT -p tcp -m multiport --dports 80,443,8080 -j ACCEPT
   ```

4. **Evite usar `-j ACCEPT` en reglas generales** sin especificadores suficientes:
   ```bash
   # Peligroso: acepta TODO el tráfico TCP
   iptables -A INPUT -p tcp -j ACCEPT
   
   # Seguro: solo TCP desde red interna específica al puerto 80
   iptables -A INPUT -p tcp -s 192.168.1.0/24 --dport 80 -j ACCEPT
   ```

### Para Solución de Problemas
1. **Verifique el orden de las reglas**: Las reglas se evalúan de arriba abajo, la primera coincidencia gana
   ```bash
   # Esta regla nunca se alcanzará porque la anterior coincide primero
   iptables -A INPUT -s 10.0.0.5 -j DROP
   iptables -A INPUT -s 10.0.0.0/24 -j ACCEPT  # Nunca se evalúa para 10.0.0.5
   
   # Orden correcto:
   iptables -A INPUT -s 10.0.0.0/24 -j ACCEPT
   iptables -A INPUT -s 10.0.0.5 -j DROP  # Esta SÍ se evaluará para 10.0.0.5
   ```

2. **Use el módulo `limit` para evitar llenar los logs**:
   ```bash
   iptables -A INPUT -j LOG --log-prefix "IPTables-DROP: " -m limit --limit 5/min
   ```

3. **Pruebe cambios en una sesión de consola** antes de aplicar remotamente para evitar bloquearse fuera del sistema
   ```bash
   # Abrir una segunda sesión SSH antes de hacer cambios críticos
   ```

## Limitaciones y Consideraciones
- **Estado vs. Stateless**: Las reglas basadas en `-m state` requieren que el módulo de seguimiento de conexiones esté activo y funcionando correctamente
- **IPv6**: Use `ip6tables` para gestionar tráfico IPv6 (tiene sintaxis similar pero maneja paquetes diferentes)
- **Rendimiento**: En sistemas con miles de reglas, el rendimiento puede degradarse; considere usar `ipset` para listas grandes de IPs o puertos
- **Complejidad**: Para configuraciones muy complejas, considere usar firewalls de aplicación más avanzados como `nftables`, `firewalld` o soluciones basadas en GUI

## Integración con Otras Herramientas

### Con `ipset` para listas grandes
```bash
# Crear un conjunto de IPs bloqueadas
ipset create bloqueadas hash:ip

# Añadir IPs al conjunto
ipset add bloqueadas 192.168.1.100
ipset add bloqueadas 10.0.0.50

# Usar el conjunto en una regla iptables
iptables -A INPUT -m set --match-set bloqueadas src -j DROP
```

### Con `fail2ban` para bloqueo automático
`fail2ban` monitorea logs y crea reglas iptables automáticamente para bloquear IPs que muestren comportamiento malicioso (intentos de login fallidos, escaneos, etc.)

### Con `ufw` (Uncomplicated Firewall)
Interfaz más sencilla para configurar iptables:
```bash
ufw allow 22/tcp      # Permitir SSH
ufw allow 80/tcp      # Permitir HTTP
ufw enable            # Activar el firewall
```

## Recursos Adicionales

- Para ver el manual completo: `man iptables`
- Para información detallada sobre coincidencias: `man iptables-extensions`
- Para ejemplos avanzados: `/usr/share/doc/iptables/examples/` (en muchas distribuciones)
- Para monitorización: `iptables -L -v -n` combinado con `watch` o herramientas como `iptstate`
- Para aprendizaje interactivo: Herramientas como `iptables-tutorial` o laboratorios en línea

> **Nota**: Aunque `iptables` es poderoso, su verdadero dominio viene de entender cómo interactúa con el stack de red de Linux y cómo las reglas afectan el flujo de paquetes en diferentes puntos de inspección. Siempre pruebe configuraciones en entornos controlados antes de aplicarlas en sistemas de producción, y mantenga una forma de recuperar el acceso (como una consola física o acceso fuera de banda) al realizar cambios críticos de firewall.