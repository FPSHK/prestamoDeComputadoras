# 02 · Instalación y ejecución

Referencia ejecutable para salas, computadoras o tutorías. Panel local para una persona operadora y datos ficticios. Exactamente tres tablas: personas, recursos y reservas.

## Requisitos

Python 3.11 o posterior y MySQL 8.4 o MariaDB 10.11 o posterior. Cliente SQL y navegador moderno. Las versiones directas están fijadas en requirements.txt. No se requiere conexión a un CDN para la interfaz.

## Instalación

1. Abre una terminal en la carpeta principal del repositorio. Ejecuta `cd codigo`. Desde ahí, comprueba que ves `app.py` y `requirements.txt`. Usa esa carpeta para los comandos siguientes.
2. Crea el entorno: `python3 -m venv .venv` (Windows: `py -m venv .venv`).
3. Activa con `source .venv/bin/activate`. En Windows puedes usar directamente `.venv\Scripts\python.exe` en vez de `python` en todos los comandos siguientes.
4. Instala: `python -m pip install -r requirements.txt`.
5. Con una cuenta administradora, abre `mysql -u root -p` o `mariadb -u root -p`. Ejecuta **un solo** script con `SOURCE sql/salas.sql;`, `SOURCE sql/equipos.sql;` o `SOURCE sql/tutorias.sql;`. También puedes abrirlo en Workbench. No repetir en una base existente.
6. Copia `sql/usuario_app.sql` a `sql/usuario_app.local.sql`. En la copia local, cambia la clave y la base del GRANT y ejecútala con la cuenta administradora. Git excluye `*.local.sql`. Si el laboratorio ya te asignó una cuenta, usa esa.
7. Copia `.env.example` a `.env` dentro de `codigo/`. Edita únicamente tu copia: ajusta `PROYECTO`, `DB_NAME`, `DB_USER` y `DB_PASSWORD`. Usa la tabla de abajo para elegir el proyecto y la base que le corresponde. El archivo `.env` se queda en tu computadora. [Qué es un .env](../guia/00-Guia_del_proyecto_final.md#qué-es-un-env)
8. Ejecuta `python -m flask --app app:create_app run`.
9. Abre http://127.0.0.1:5000. Elige un recurso y una fecha futura para probar un bloque libre.

| Variante | PROYECTO | DB_NAME |
|---|---|---|
| Salas | salas | fullstack_salas |
| Computadoras | equipos | fullstack_equipos |
| Tutorías | tutorias | fullstack_tutorias |

La cuenta de ejemplo usa host local. Para un laboratorio con servidor remoto, el docente debe proporcionar host, puerto y una cuenta autorizada para ese origen; no basta con cambiar el nombre de host del ejemplo.

## Reglas

Bloques de 60 minutos con inicio de 08:00 a 16:00, hora de Puerto Rico, todos los días. Solo reservas futuras, hasta 30 días. Persona y recurso no pueden repetirse en la misma hora. Cancelar una futura elimina la fila y libera el bloque. El prototipo no conserva historial de cancelaciones. Cada tutor aparece una sola vez y tiene una materia. Las computadoras son unidades físicas únicas. No se incluyen multas, asistencia ni autenticación.

La persona registrada es solicitante, no usuario autenticado. Este panel es educativo y local, no un servicio público listo para despliegue.

## Endpoints

- GET /api/personas y POST /api/personas con nombre y email.
- GET /api/recursos.
- GET /api/disponibilidad?fecha=AAAA-MM-DD&recurso_id=1.
- GET /api/reservas?fecha=AAAA-MM-DD con recurso_id opcional.
- POST /api/reservas con persona_id, recurso_id, fecha, hora y motivo.
- DELETE /api/reservas/ID.
- GET /api/resumen?fecha=AAAA-MM-DD.

Las entradas POST son objetos JSON con Content-Type application/json. Estados: 200 consulta; 201 creación; 204 cancelación sin cuerpo; 400 entrada inválida; 404 relación o reserva inexistente; 409 duplicado; 503 fallo de base. El resumen usa reservas conservadas / (recursos × 9) × 100.

## Pruebas

`python -m pytest -q` ejecuta pruebas aisladas en SQLite en memoria, con reloj fijo. No borra la base del proyecto. Completa además P1–P14 de la guía en MySQL/MariaDB: instalación, concurrencia, interfaz, teclado y pantalla estrecha. La validación automática del servidor no equivale a una auditoría completa de accesibilidad.

## Desarrollo por etapas

Consulta la [guía instruccional del proyecto final](../guia/00-Guia_del_proyecto_final.md). `models.py` corresponde a las tablas; `app.py` configura Flask, valida y expone la API; `index.html` presenta los formularios; `app.js` conecta la interfaz; `styles.css` adapta la distribución. El SQL instala la base de entrega; no uses create_all como sustituto. Los tests sí usan create_all en su base temporal.

## Antes de entregar

Agrega integrantes y aportes, motor y versiones verificadas, cuarto recurso del proyecto, decisión UX y una prueba propia. Revisa la [lista de entrega](../plantillas/Lista_entrega.md). Incluye el SQL elegido actualizado, código y .env.example. Excluye .env, .venv y cachés. Declara fuentes, código reutilizado y limitaciones. El código de referencia se proporciona para uso educativo y adaptación en esta unidad.

## GitHub obligatorio

Trabajen en un repositorio propio del equipo, derivado de la base del curso. Cada integrante debe abrir al menos un pull request técnico y revisar otro. Completen la [ficha de entrega](../plantillas/Ficha_entrega.md) antes de etiquetar. Entreguen URL, etiqueta `v1.0-entrega` y SHA del commit en el espacio de entrega del curso. El ZIP es respaldo y no sustituye GitHub. La guía incluye el procedimiento completo de clonación, ramas, revisión y etiquetado.

**Después de instalar** · Vuelvan a la [etapa 3 de la guía](../guia/00-Guia_del_proyecto_final.md#9-etapa-3---conectar-flask-y-representar-los-datos) y trabajen hasta terminar la etapa 6. Para la etapa 7, abran [03 · Pruebas e integración](03-PRUEBAS.md).
