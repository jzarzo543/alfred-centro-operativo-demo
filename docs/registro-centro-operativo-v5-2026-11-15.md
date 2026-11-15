# Registro de avance — Centro Operativo v5

Fecha: 2026-11-15
Ámbito: CASA / Alfred — Centro Operativo
Estado: Fase de monitorización Home Assistant completada y validada (según pruebas comunicadas desde Codex y Ubuntu).
Alcance: Este hito NO implica que Alfred, como asistente personal, esté terminado.

## Hitos completados
- Programa Python de Ubuntu que comprueba la respuesta HTTP local de Home Assistant.
- Publicación autenticada en GitHub del archivo público `estado-ubuntu.json` sin publicar credenciales ni direcciones privadas.
- Primera creación y posteriores actualizaciones del archivo verificadas (HTTP 201 y HTTP 200).
- Automatización mediante cron cada cinco minutos con bloqueo `flock`.
- Publicación solo ante cambio de estado o vencimiento de latido de diez minutos.
- Latido automático verificado el 2026-11-15 a las 17:45:01 (Europe/Madrid).
- Permisos comprobados: directorio de configuración 700, token 600 y registro 600.
- Centro Operativo v5 publicado con lectura del estado real desde GitHub.
- Según informe de Codex: probados estados operativo, incidencia y datos desactualizados; actualización cada 60 segundos y al recuperar foco; errores de GitHub/JSON inválido; diseño iPhone; conservación del código y registros de v4.
- Según informe de Codex: Home Assistant aparecía operativo a las 17:55:01 (Europe/Madrid).
- No se modificaron contenedores, Home Assistant ni servicios de producción durante la integración del panel.

## Pendientes de mantenimiento
- Renovar o reemplazar el token de GitHub antes de su caducidad (2026-12-15).
- Establecer rotación o límite de tamaño de `sync.log`.
- Observar la estabilidad del flujo y revisar el volumen de commits de latido si resulta excesivo.

## Límites y criterios
- La comprobación HTTP indica que Home Assistant responde, no garantiza el estado de sus integraciones o dispositivos.
- El Site debe mostrar «datos desactualizados» si dejan de recibirse comprobaciones recientes; no inferir que el servicio sigue operativo.
- Mantener separados los datos de demostración y los reales.
- No ejecutar nuevas instalaciones o cambios de producción sin autorización.
- No incluir información personal, laboral ni financiera en el repositorio público.

## Pendiente de reflejar en el Site
Actualizar la ficha existente de Alfred/Centro Operativo, el historial de cambios y las tareas de mantenimiento sin duplicar proyectos ni borrar registros anteriores. Este documento es un registro de seguimiento en GitHub; por sí solo no modifica el Site publicado.
