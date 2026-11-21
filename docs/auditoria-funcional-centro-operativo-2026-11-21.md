# Auditoría funcional del Centro Operativo — 21/11/2026

Estado: diagnóstico documental y visual; **NO** implica que se haya modificado el Site publicado. Evidencias: nueve capturas aportadas por el usuario (Proyectos, Actualizaciones, Monitorización, Tareas, Servicios, Incidencias, Autorizaciones, Validaciones, Historial), más capturas previas de Resumen y Entornos. No se dispone del código fuente del Site en este repositorio.

## Hallazgos
- Proyectos: 18 proyectos CASA visibles, fechas/estados no reconciliados con eventos recientes; no asumir que sus fichas se sincronizan con GitHub.
- Actualizaciones: lee `actualizaciones-proyectos.json`; aparece red privada Gigabit de 17/11.
- Monitorización Ubuntu: Home Assistant se muestra operativo, comprobación 21/11 12:30:01 Madrid. Fuente `estado-ubuntu.json`, independiente del registro manual.
- Servicios: seis áreas 'Sin verificar' y lista vacía, pese a monitorización de Home Assistant.
- Tareas: cuatro entradas manuales genéricas; no aparecen los pendientes de `actualizaciones-proyectos.json` en esta vista.
- Incidencias, Autorizaciones y Validaciones: sin registros en el navegador de la captura. No implica ausencia de incidencias, permisos o pruebas fuera de la vista.
- Historial: en la captura se ven registros del 14–15/11, pero no el hito de red Gigabit del 17/11.
- Entornos: Producción y Desarrollo 'Equipo por confirmar' pese a documentación de MINISFORUM y alfred-dev.
- Varias vistas indican: 'Registro manual. Los datos se guardan en este navegador. Para llevarlos al iPhone, exporta el JSON e impórtalo allí.' No hay sincronización multidispositivo confirmada.

## Causa probable y límites
Hay al menos dos fuentes independientes: JSON de GitHub para Actualizaciones/Monitorización y almacenamiento local del navegador para registros manuales. Las capturas no permiten verificar la implementación interna ni el esquema de persistencia. **No asumir que editar JSON de GitHub rellena Servicios/Tareas/Historial.**

## Requisitos de corrección (pendientes de aplicar al Site)
1. Antes de modificar: exportar copia íntegra de 'Datos del panel' y preservar datos locales; no borrar registros.
2. Identificar el código fuente real del Site y sus funciones de lectura/escritura. Este repositorio contiene JSON y documentación, **no** el código del Site.
3. Definir un esquema centralizado por IDs estables para entornos, proyectos, servicios, tareas, incidencias, autorizaciones, validaciones e historial; distinguir `observado`, `documentado`, `pendiente`, `sin verificar`.
4. Conservar las actualizaciones de GitHub como solo lectura y enlazarlas por `project_key`/ID a fichas; no marcar tareas como completadas solo por recibir un evento sin validación.
5. Servicios: reflejar la comprobación de Home Assistant como estado monitorizado, con fecha/caducidad, sin extrapolar a integraciones.
6. Entornos: MINISFORUM Windows 11, Alfred producción y alfred-dev separados; Ubuntu como infraestructura independiente. Confirmar estado actual antes de mostrar 'operativo'.
7. Incorporar registros de red Gigabit a historial/validaciones (ping 4/4, enlaces negociados 1 Gbps); mantener caudal real y reinicio como pendientes.
8. Proteger datos: repositorio actual público; no publicar secretos, información privada, datos laborales o financieros. Si se sincronizan datos privados, usar almacenamiento autenticado privado, no JSON público.
9. Probar con datos de prueba y copia de seguridad: navegación, alta/edición, persistencia, actualización cruzada, móvil, fallos de red, no duplicación y reversión.
10. No modificar equipos, contenedores, red ni instalaciones sin autorización específica.

## Prioridad
P0: copia y análisis del Site; P1: sincronizar estados/proyectos/tareas/servicios e historial; P2: incidencias, autorizaciones y validaciones; P3: mejoras visuales. El trabajo actual es solo auditoría y documentación.
