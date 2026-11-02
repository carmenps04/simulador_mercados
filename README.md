# Mercado Móvil

Simulador de empresa de telefonía móvil para el alumnado de 4.º de ESO (Economía y Emprendimiento) del IES Hiponova (Montefrío). Cada grupo dirige una empresa: diseña el móvil, decide componentes, mercado, transporte, precio, producción y publicidad, y compara sus resultados semana a semana con el resto de la clase.

Enlace: https://carmenps04.github.io/simulador_mercados/

## Uso

- **Alumnado:** abre el enlace, escribe los nombres del grupo, elige su curso y crea su empresa (un dispositivo por grupo). Antes de enviar las decisiones, el grupo escribe cuántos móviles cree que venderá.
- **Profesorado:** «Código de profesor» en la pantalla de inicio. Se entra con Google o con correo y contraseña. Solo entran la administradora y los correos que ella añada en **Profesorado con acceso**.

## Profesorado y perfiles

- La **administradora** es el correo que aparece en `firestore.rules`. La primera vez que entra se crea la lista de profesorado.
- En **Profesorado con acceso** (solo la administradora) se añaden o quitan correos. Si se quita un correo, esa persona pierde el acceso en ese momento.
- Cada docente ve y gestiona **solo los cursos que crea**. La administradora los ve todos (los de otros docentes llevan su correo al lado). Los cursos creados antes de noviembre de 2026 son de la administradora.
- Quien entra con correo y contraseña por primera vez crea su cuenta en «Es mi primera vez» y tiene que confirmar el correo con el enlace que recibe.
- En **Perfil** cada docente pone su nombre, de qué es profesor/a, si es tutor/a y de qué curso, y los cursos a los que da clase.

## Código de cada empresa

Los dispositivos se comparten entre grupos, así que cada empresa tiene un **código** que elige el grupo al crearla (de 4 a 12 letras o números).

- Cada vez que se abre la web, o al pulsar **Salir de la empresa**, el grupo tiene que elegir su empresa y escribir su código. El dispositivo no recuerda la empresa.
- El alumnado no puede ver los códigos. Los comprueba Firebase.
- En **Cursos**, debajo del nombre de cada grupo, aparece su código con el botón **Cambiar**. Si otro grupo lo ha descubierto, cámbialo y dale el nuevo al grupo. Los dispositivos que estaban dentro tendrán que volver a entrar con el código nuevo.
- Las empresas creadas antes de esta versión aparecen como **Sin código**. Ponles uno desde **Cursos**. Si no, el primer grupo que entre en ella fijará el código que escriba.
- Las copias de seguridad incluyen los códigos.

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

## Configuración de Firebase (una sola vez)

En https://console.firebase.google.com, proyecto `simulador-maes`:

1. **Authentication → Método de acceso**: activa **Google** (elige tu correo como correo de asistencia y guarda). Deja también activos **Correo electrónico/contraseña** y **Anónimo**.
2. **Authentication → Configuración → Dominios autorizados**: comprueba que está `carmenps04.github.io`. Si no, pulsa «Añadir dominio» y escríbelo.
3. **Authentication → Plantillas**: en «Verificación de la dirección de correo electrónico», cambia el idioma de la plantilla a español (lápiz → Idioma de la plantilla).
4. Publica las reglas (apartado siguiente).

## Reglas de Firestore

Las reglas están en `firestore.rules` y **no se aplican solas**. Cada vez que cambie ese archivo:

1. Abre el archivo y copia todo su contenido.
2. Cambia `TU_CORREO@ejemplo.com` por tu correo de administradora, todo en minúsculas.
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
- `ajustes/profesorado`: administradora y lista de correos con acceso.
- `perfiles/{correo}`: perfil de cada docente.
- `partidas/{curso}/claves/{empresa}`: código de cada empresa (solo lo lee el profesorado).
- `accesos/{dispositivo}`: empresa en la que ha entrado cada dispositivo con su código.
- `bloqueos/…`: evita que dos dispositivos cierren la misma semana a la vez.

Cada dispositivo guarda una copia local de los datos, así que la app aguanta cortes breves de la wifi y sincroniza al volver la conexión.
