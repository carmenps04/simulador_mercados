# Mercado Móvil

Simulador de empresa de telefonía móvil para el alumnado de 4.º de ESO (Economía y Emprendimiento) del IES Hiponova (Montefrío). Cada grupo dirige una empresa: diseña el móvil, decide componentes, mercado, transporte, precio, producción y publicidad, y compara sus resultados semana a semana con el resto de la clase.

## Uso

- **Alumnado:** abre el enlace, elige su curso y crea su empresa (un dispositivo por grupo).
- **Profesorado:** «Código de profesor» en la pantalla de inicio. Desde el panel se publican los resultados, se escriben recomendaciones, se fijan las reglas y se proyecta la clasificación.

## Datos

La app es un único `index.html` sin paso de compilación. Los datos se guardan en Firestore (proyecto `simulador-maes`, carpeta `mercado-movil/v1/`) con inicio de sesión anónimo. Las reglas de seguridad están en `firestore.rules` y se aplican a mano desde la consola de Firebase.
