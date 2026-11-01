# Mercado Móvil

Simulador de empresa de telefonía móvil para el alumnado de 4.º de ESO (Economía y Emprendimiento) del IES Hiponova (Montefrío). Cada grupo dirige una empresa: diseña el móvil, decide componentes, mercado, transporte, precio, producción y publicidad, y compara sus resultados semana a semana con el resto de la clase.

Enlace: https://carmenps04.github.io/simulador_mercados/

## Uso

- **Alumnado:** abre el enlace, escribe los nombres del grupo, elige su curso y crea su empresa (un dispositivo por grupo). Antes de enviar las decisiones, el grupo escribe cuántos móviles cree que venderá.
- **Profesorado:** «Código de profesor» en la pantalla de inicio y acceso con correo y contraseña.

## Tareas habituales

### Crear un curso
Panel → **Crear curso**. Elige curso, letra, código de entrada y reglas. Si marcas «Publicar los resultados manualmente», las rondas solo avanzan cuando pulsas **Publicar resultados** en **Cursos**.

### Publicar una semana
Panel → **Cursos** → **Publicar resultados y pasar a la semana…**. Si faltan grupos por enviar, pide una segunda pulsación.

### Si una ronda automática no se cierra
La app no tiene servidor: la ronda se cierra cuando alguien tiene la app abierta después de la hora de cierre. Abre el panel (o cualquier dispositivo con el curso) y espera unos segundos. Si no avanza, pulsa **Publicar resultados**.

### Proyectar en clase
**Cursos** → **Proyectar clasificación**. Arriba a la derecha se elige **Clasificación** o **Mercado** (precio y ventas de cada empresa en una semana).

### Exportar
En **Cursos**:
- **Exportar resultados en PDF**: informe del curso.
- **Decisiones en Excel**: una fila por empresa y semana con todas las decisiones, la previsión de ventas y los resultados, y una hoja de resumen. Las semanas jugadas antes de noviembre de 2026 no tienen componentes, extras ni previsión.

### Copias de seguridad
- **Descargar copia de seguridad** (en **Cursos** y en la ventana de reiniciar o borrar) guarda el curso entero en un archivo `.json`, con imágenes. Guárdalo en el ordenador o en Drive.
- **Restaurar una copia** (en **Cursos** o en **Crear curso**) vuelve a dejar el curso como estaba en la copia. Si el curso existe, se sustituye. El plazo de la ronda en curso empieza a contar de nuevo.

Conviene descargar una copia al final de cada semana y siempre antes de reiniciar o borrar.

## Reglas de Firestore

Las reglas están en `firestore.rules` y **no se aplican solas**. Cada vez que cambie ese archivo:

1. Abre el archivo y copia todo su contenido.
2. Cambia `TU_CORREO@ejemplo.com` por tu correo de profesora (el de Authentication).
3. En https://console.firebase.google.com abre el proyecto `simulador-maes`.
4. Ve a **Firestore Database** → **Reglas**.
5. Borra lo que hay, pega el texto y pulsa **Publicar**.

Si el panel muestra el aviso «Las reglas de Firestore están sin actualizar», falta este paso. La app sigue funcionando, pero guarda las imágenes nuevas dentro de cada empresa, que es más lento.

## Datos

La app es un único `index.html` sin paso de compilación. Los datos se guardan en Firestore (proyecto `simulador-maes`, carpeta `mercado-movil/v1/`) con inicio de sesión anónimo para el alumnado:

- `partidas/{curso}`: reglas y semana actual del curso.
- `partidas/{curso}/equipos/{empresa}`: decisiones, historial y resultados de cada empresa.
- `partidas/{curso}/imagenes/{empresa}`: logo y diseño de cada empresa.
- `ajustes/centro`: pantalla de inicio (logo y nombre del centro).
- `bloqueos/…`: evita que dos dispositivos cierren la misma semana a la vez.

Cada dispositivo guarda una copia local de los datos, así que la app aguanta cortes breves de la wifi y sincroniza al volver la conexión.
