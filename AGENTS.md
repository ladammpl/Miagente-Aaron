# AGENTS

## System Prompt Principal del Bot (Soul / MiAgente-Aaron)

## Heartbeat Tasks (Tareas que el bot revisa solo periódicamente)

Estas son tareas automáticas que Soul debería revisar cada cierto tiempo (idealmente vía cron o scheduler interno):

- [ ] Revisar si hay recordatorios geolocalizados pendientes que deban activarse según la ubicación actual del usuario (cada 15 min).
- [ ] Verificar si hay recordatorios por tiempo que vencen en las próximas 2 horas y enviar notificación push/email (cada 30 min).
- [ ] Limpiar recordatorios expirados o completados de más de 30 días (cada 24 h).
- [ ] Comprobar si hay actualizaciones pendientes en dependencias del proyecto (pip list --outdated) y sugerir upgrade si es seguro (cada 7 días).
- [ ] Analizar logs de errores del backend Django de las últimas 24 h y reportar los más frecuentes (cada 24 h).
- [ ] Recordar a Aaron tareas pendientes marcadas como "alta prioridad" en la base de datos (cada 12 h).
- [ ] Sincronizar tareas pendientes con calendario externo si está configurado (Google Calendar / Outlook) (cada 6 h).

## Configuración de Recordatorios usando la herramienta cron

Para que Soul pueda gestionar recordatorios de forma autónoma, se configuran jobs cron (o Celery Beat si usas Celery) en el servidor o en desarrollo local.

### Ejemplo de configuración con crontab (Linux/Mac o WSL en Windows)

```bash
# Editar crontab
crontab -e

# Agregar estas líneas (ajusta rutas según tu proyecto)

# Cada 15 minutos: revisar recordatorios de ubicación
*/15 * * * * cd /ruta/a/tu/proyecto && python manage.py check_geofence_reminders >> /ruta/logs/cron.log 2>&1

# Cada 30 minutos: revisar recordatorios por tiempo
*/30 * * * * cd /ruta/a/tu/proyecto && python manage.py check_time_reminders >> /ruta/logs/cron.log 2>&1

# Cada día a las 3:00 AM: limpieza de recordatorios viejos
0 3 * * * cd /ruta/a/tu/proyecto && python manage.py cleanup_old_reminders >> /ruta/logs/cron.log 2>&1

# Cada lunes a las 9:00 AM: checar dependencias
0 9 * * 1 cd /ruta/a/tu/proyecto && python -m pip list --outdated > /ruta/logs/dependencies.log 2>&1