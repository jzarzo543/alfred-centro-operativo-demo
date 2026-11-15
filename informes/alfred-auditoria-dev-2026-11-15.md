# Alfred DEV — Auditoría de resultados (15-11-2026)

**Estado:** auditoría documental terminada; integración con Ollama real en DEV pendiente. Este informe público resume evidencias comunicadas por la auditoría local; no constituye una nueva ejecución de pruebas.

| Área | Estado | Alcance de la evidencia |
|---|---|---|
| Fase 4: streaming, historial, recuerdos y cancelación | Verificado en simulación | Regresión de navegador con NDJSON, persistencia y cancelación; no inferencia real |
| Fase 5A: resistencia y compatibilidad | Parcial | Trazas simuladas y revisión estática; falta modelo real |
| Fase 5C: calculadora y llamadas a herramientas | Parcial | 11 casos unitarios y simulación; falta ciclo tool_calls real |
| Diagnóstico, Ubuntu y Home Assistant | Parcial | Diagnóstico Ubuntu real histórico; HA ficticio y salud de copias sin evidencia |
| Piper | Verificado solo en simulación | WAV fijo de silencio; no acredita síntesis de voz real |
| RC1 consolidado | Fallido y revertido | Dos intentos reales revertidos; causa del timeout de Piper sin determinar |
| Parche «Detener» | Parcial | Prueba histórica en iframe, modo JSON; no acredita streaming real integrado |
| Fase 6B: modelos DEV | Verificado estáticamente | Integridad SHA-256 de alfred:latest (4B) y qwen3:1.7b, sin enlaces |
| Fase 6B: Ollama DEV e inferencia | Pendiente | Sin arranque, respuesta ni telemetría por umbrales de memoria |

## Hallazgos de calidad de evidencia

- Los informes de navegador de fases 4 y 5C son idénticos byte a byte; no cuentan como dos pruebas independientes.
- En la resistencia, solo la segunda traza acredita 144 peticiones POST de streaming; la primera documenta 144 turnos y 47 peticiones.
- El timeout de RC1 ocurrió después de navegar del iframe al chat directo; no se verificó la petición HTTP posterior, por lo que no puede atribuirse el fallo exclusivamente a Piper.
- Algunos archivos de respaldo y candidatos tienen hashes diferentes; no deben confundirse por nombre de carpeta.
- Git local DEV: rama master en 18473ab, sin remoto; release/ sin seguimiento y .alfred-dev/ ignorado en el momento de la auditoría.
- No se detectaron contenedores, redes, procesos o escuchas DEV temporales. Se conservaron artefactos locales de pruebas.

## Bloqueo y continuación

El arranque DEV de 4B se aplazó por memoria. Para la prueba de 1.7B no se alcanzó el margen preventivo de compromiso de **3.000 MiB** aunque la RAM física alcanzó en una medición el umbral de **3.700 MiB**. No se cambió el archivo de paginación ni producción.

**Próximo paso, sin fecha obligatoria:** elegir una vía de inferencia real que preserve producción, verificar aislamiento y recursos, ejecutar un turno con 1.7B y después validar conversación, herramientas y cancelación reales. La voz Piper queda aplazada.

**Política:** no publicar modelos, blobs, credenciales, copias, registros privados ni información personal. Esta publicación documenta el estado; no despliega código ni altera producción.
