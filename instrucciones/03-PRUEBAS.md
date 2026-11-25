# 03 · Pruebas e integración

Abran esta lectura al llegar a la etapa 7. Van a comprobar que otra persona puede instalar su proyecto y completar una reserva desde el formulario.

1. Preparen el [registro de pruebas](../plantillas/Plan_pruebas.md). Anoten la variante, el motor y su versión, el navegador y quién hará cada comprobación.
2. En una terminal abierta en `codigo/` y con el entorno de Python del proyecto, ejecuten `python -m pytest -q`. En Windows pueden usar `.venv\Scripts\python.exe -m pytest -q`. Guarden el resultado.
3. Busquen los casos P1 a P14 en la [etapa 7 de la guía](../guia/00-Guia_del_proyecto_final.md#13-etapa-7---probar-e-integrar). Usen su instalación de MySQL o MariaDB y el navegador para verificar los casos de integración e interfaz. Registren lo observado y añadan al menos una prueba propia.
4. Pidan a otro integrante que siga [02 · Instalación y ejecución](02-INSTALACION.md) con una base nueva. Comprueben juntos que los datos de una reserva se conservan al recargar. Anoten los ajustes necesarios y repitan las pruebas afectadas.

## Resultados de la verificación del material de referencia

Preparado el 10 de noviembre de 2026.

- 23 pruebas automáticas del servidor aprobadas en SQLite temporal, con reloj fijo y claves foráneas activadas.
- Sintaxis JavaScript comprobada con `node --check`.
- Los tres scripts SQL se ejecutaron en bases nuevas de una instancia temporal MySQL 5.7.24 disponible en el entorno de autoría. Cada base creó exactamente tres tablas y sus semillas.
- En cada variante se comprobó la página inicial, creación concurrente del mismo bloque (una respuesta 201 y otra 409), persistencia de una sola fila, resumen y cancelación.
- En Chrome se comprobó el alta de una persona y una reserva desde los formularios, el bloqueo de su hora y el cambio del resumen a 1 de 27 bloques. La cancelación se verificó mediante la API.
- La guía propone MySQL 8.4 o MariaDB 10.11 o posterior para el laboratorio. No se ejecutó aquí una instancia de esas versiones; corresponde repetir la matriz de integración en el motor del curso. MySQL 5.7 no se recomienda como instalación nueva.
- Las pruebas del servidor no constituyen una auditoría completa de accesibilidad. La matriz pide comprobar teclado, foco, pantalla estrecha y zoom en el navegador del laboratorio.

Las verificaciones se refieren al código de referencia, no a futuras modificaciones de los equipos.

**Siguiente lectura** · [04 · Entrega del proyecto](04-ENTREGA.md)
