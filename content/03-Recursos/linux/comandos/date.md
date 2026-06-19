# Date: Visualización y Configuración de Fecha y Hora

El comando `date` es una herramienta esencial en sistemas Unix-like que permite mostrar la fecha y hora actual del sistema, así como configurarla cuando sea necesario (requiriendo privilegios de administrador). Además, ofrece capacidades poderosas para formatear la salida, trabajar con zonas horarias y procesar cadenas de fecha relativas.

## ¿Para qué se utiliza?

- **Ver la fecha y hora actual** del sistema
- **Formatear la salida** según necesidades específicas (logs, nombres de archivo, informes)
- **Configurar la fecha y hora del sistema** (requiere sudo/root)
- **Trabajar con zonas horarias** diferentes
- **Calcular fechas futuras o pasadas** usando expresiones relativas
- **Extraer componentes específicos** de fecha/hora para scripts

## Sintaxis Básica

```bash
date [opciones] [+FORMATO]
date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
```

- La primera forma muestra la fecha/hora actual con formato opcional
- La segunda forma establece la fecha y hora del sistema

## Visualización de Fecha y Hora

### Formato Predeterminado
```bash
date
# Ejemplo de salida: Vie 24 May 2026 15:30:45 CEST
```

### Formatos Personalizados
Utilizando especificadores de formato con el prefijo `+`:
```bash
# Solo fecha en formato AAAA-MM-DD
date +%Y-%m-%d
# Salida: 2026-05-24

# Hora en formato 24 horas
date +%H:%M:%S
# Salida: 15:30:45

# Fecha y hora completa para nombres de archivo
date +"%Y%m%d_%H%M%S"
# Salida: 20260524_153045

# Día de la semana y mes
date +"%A, %d de %B de %Y"
# Salida: viernes, 24 de mayo de 2026
```

## Especificadores de Formato Más Utilizados

| Especificador | Descripción | Ejemplo |
|---------------|-------------|---------|
| `%Y` | Año con siglo (YYYY) | `2026` |
| `%y` | Año sin siglo (YY) | `26` |
| `%m` | Mes (01-12) | `05` |
| `%d` | Día del mes (01-31) | `24` |
| `%H` | Hora (00-23) | `15` |
| `%I` | Hora (01-12) | `03` |
| `%M` | Minuto (00-59) | `30` |
| `%S` | Segundo (00-60) | `45` |
| `%a` | Día de semana abreviado | `Vie` |
| `%A` | Día de semana completo | `viernes` |
| `%b` | Mes abreviado | `may` |
| `%B` | Mes completo | `mayo` |
| `%p` | AM o PM | `PM` |
| `%P` | am o pm en minúsculas | `pm` |
| `%Z` | Zona horaria | `CEST` |
| `%z` | Desplazamiento de zona horaria | `+0200` |
| `%s` | Segundos desde époch (1970-01-01 UTC) | `1732410645` |

### Modificadores de Formato
Estos modificadores afectan cómo se muestran los valores numéricos:
```bash
date +"%-H:%M:%S"  # Sin cero inicial: 3:30:45 (en lugar de 03:30:45)
date +"%_H:%M:%S"  # Con espacios:  3:30:45
date +"%0H:%M:%S"  # Con ceros: 03:30:45 (predeterminado)
```

## Trabajo con Zonas Horarias

### Mostrar Hora en Diferentes Zonas
```bash
# Hora actual en UTC
date -u
# o
date --utc

# Hora en zona específica usando la variable TZ
TZ="America/New_York" date
TZ="Asia/Tokyo" date
TZ="Europe/London" date

# Ver todas las zonas horarias disponibles
timedatectl list-timezones | grep -i "america/new_york"
```

### Configuración Permanente de Zona Horaria
```bash
# Ver zona horaria actual
timedatectl

# Cambiar zona horaria (requiere sudo)
sudo timedatectl set-timezone America/New_York
```

## Establecer Fecha y Hora (Requiere Privilegios de Administrador)

⚠️ **Advertencia**: Cambiar la fecha y hora del sistema puede afectar servicios, registros y aplicaciones. Solo hacerlo cuando sea absolutamente necesario y entender las implicaciones.

### Establecer una Fecha/Hora Específica
```bash
# Formato: MMDDhhmm[[CC]YY][.ss]
sudo date 0524153026.00  # Establece 24 de mayo de 2026, 15:30:00

# Alternativa usando --set
sudo date --set="2026-05-24 15:30:00"
```

### Sincronizar con Servidor NTP
```bash
# Sincroniza automáticamente con servidores de tiempo
sudo timedatectl set-ntp true

# Ver estado de sincronización
timedatectl status
```

## Trabajando con Cadenas de Fecha (Date Strings)

La opción `-d` o `--date` permite interpretar cadenas de fecha flexibles:

### Fechas Relativas
```bash
# Mañana a esta misma hora
date -d "tomorrow"

# Ayer a las 10:30
date -d "yesterday 10:30"

# La próxima semana
date -d "next week"

# Hace 3 días
date -d "3 days ago"

# Dentro de 2 horas y 30 minutos
date -d "2 hours 30 minutes"

# El último viernes
date -d "last friday"

# El próximo lunes a las 9:00
date -d "next monday 9:00"
```

### Fechas Absolutas
```bash
# Fecha específica
date -d "2026-12-25"
date -d "25 dec 2026"
date -d "december 25, 2026"

# Con hora específica
date -d "2026-05-24 15:30:45"
date -d "24 may 2026 3:30pm"
```

### Conversión de Tiempo Époch
```bash
# Desde segundos desde époch a fecha legible
date -d "@1732410645"

# Desde milisegundos (dividir por 1000 primero)
date -d "@$((1732410645000 / 1000))"
```

## Ejemplos Prácticos

### Nombres de Archivo con Marcas de Tiempo
```bash
# Crear un respaldo con timestamp
cp documento.txt documento_$(date +%Y%m%d_%H%M%S).txt

# Crear directorio de logs diario
mkdir -p /var/log/aplicacion/$(date +%Y/%m/%d)
```

### Generación de Reportes con Timestamps
```bash
# Agregar timestamp a entrada de log
echo "[$(date +%Y-%m-%d\ %H:%M:%S)] Iniciando proceso de backup" >> backup.log

# Crear reporte diario
{
    echo "=== Reporte del Sistema ==="
    echo "Fecha: $(date +'%A, %d de %B de %Y')"
    echo "Hora: $(date +%H:%M:%S)"
    echo ""
    echo "Uso de disco:"
    df -h /
} > reporte_$(date +%Y%m%d).txt
```

### Cálculos de Fecha en Scripts
```bash
# Calcular fecha de exacción (30 días desde hoy)
EXPIRY_DATE=$(date -d "+30 days" +%Y-%m-%d)
echo "La licencia expira el: $EXPIRY_DATE"

# Verificar si un certificado expira pronto (menos de 7 days)
CERT_EXPIRY=$(date -d "$(openssl x509 -enddate -noout -in cert.pem | cut -d= -f2)" +%s)
NOW=$(date +%s)
DAYS_LEFT=$(( (CERT_EXPIRY - NOW) / 86400 ))

if [ $DAYS_LEFT -lt 7 ]; then
    echo "Advertencia: El certificado expira en $DAYS_LEFT días"
fi
```

### Formateo para Solicitudes HTTP y Cookies
```bash
# Formato RFC 1123 (usado en headers HTTP)
date -u +"%a, %d %b %Y %H:%M:%S GMT"
# Ejemplo: Fri, 24 May 2026 15:30:45 GMT

# Fecha de expiración para cookies (1 año desde ahora)
EXPIRY=$(date -u -d "+1 year" +"%a, %d-%b-%Y %H:%M:%S GMT")
echo "Set-Cookie: sessionid=abc123; Expires=$EXPIRY; Path=/"
```

## Consejos y Buenas Prácticas

1. **Usa comillas dobles** alrededor de las cadenas de fecha para evitar problemas con espacios y caracteres especiales
   ```bash
   # Correcto
   date -d "next monday 9:00"
   
   # Problemático si la cadena contiene espacios no protegidos
   date -d next monday 9:00  # Esto fallará
   ```

2. **Prefiere el formato ISO 8601** (YYYY-MM-DD) para fechas en archivos y bases de datos
   ```bash
   # Formato estándar internacional que es lexicográficamente ordenable
   date +%Y-%m-%d
   ```

3. **Usa UTC para registros y timestamps** cuando la zona horaria local no sea relevante
   ```bash
   # En aplicaciones distribuidas, UTC evita confusiones de zona horaria
   date -u +"%Y-%m-%dT%H:%M:%SZ"
   ```

4. **Combina con otras herramientas** mediante tuberías para flujos de trabajo poderosos
   ```bash
   # Encuentra archivos modificados en los últimos 2 días
   find . -type f -mtime -2 -exec ls -l {} \;
   
   # O usando date para comparación
   find . -type f -newermt "$(date -d '2 days ago' +%Y-%m-%d)" -ls
   ```

5. **Ten cuidado con cambios de horario de verano** al trabajar con fechas relativas
   ```bash
   # En zonas con DST, "24 hours" puede no ser exactamente un día
   date -d "now + 24 hours"  # Puede saltar o repetir una hora según la transición DST
   ```

## Recursos Adicionales

- Para ver el manual completo: `man date`
- Para información detallada sobre formatos: `info coreutils 'date invocation'`
- Para configuración NTP avanzada: consulta la documentación de `chrony` o `ntpd`
- Para zonas horarias: explora el directorio `/usr/share/zoneinfo/`

> **Nota**: Aunque `date` parece simple, su verdadero poder radica en su capacidad para interpretar flexible mente cadenas de fecha y generar formatos personalizados. Dominar estas capacidades permite crear scripts más robustos, generar nombres de archivo significativos y trabajar eficazmente con tiempos en aplicaciones distribuidas.