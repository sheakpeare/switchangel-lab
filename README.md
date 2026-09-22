# Switch Angel Pattern Lab

Botonera brutalista para experimentar rápidamente con patrones de [Strudel](https://strudel.cc/). La aplicación es una página estática: no necesita Node, dependencias ni proceso de compilación.

## Uso rápido

1. Abrí la aplicación desde GitHub Pages o localmente.
2. Elegí un preset:
   - **MANTRA**: patrón base.
   - **STUTTER**: cortes y silencios alternados.
   - **DEGRADAR**: chop granular.
   - **CLÍMAX**: chop, crush y gain.
3. Ajustá los controles de **velocidad**, **chop**, **crush** y **reverb**.
4. Presioná **REPRODUCIR** para enviar el patrón al entorno disponible.
5. Usá **COPIAR CÓDIGO** para copiar el código generado.
6. Presioná **PROBAR EN STRUDEL**: copia el patrón y abre [strudel.cc](https://strudel.cc/) en otra pestaña.
7. Pegá el código en el editor de Strudel y ejecutalo allí para escucharlo.

> La aplicación no genera audio por sí sola cuando se abre como HTML independiente. El botón de reproducción intenta usar una integración `window.evaluate()` si existe; en caso contrario, copia el código para que puedas probarlo en Strudel.

## Controles

| Control | Acción |
| --- | --- |
| `▶ REPRODUCIR` | Activa el patrón seleccionado. Cambia a `■ DETENER` mientras está activo. |
| `SILENCIO` | Detiene la reproducción y llama a `hush()` cuando está disponible. |
| `PROBAR EN STRUDEL` | Copia el patrón y abre Strudel en una pestaña nueva. |
| `COPIAR CÓDIGO` | Copia únicamente el patrón actual al portapapeles. |
| `COMPARTIR` | Usa el diálogo nativo de compartir; si no está disponible, copia el código. |
| `RANDOM` | Genera una combinación aleatoria de preset y parámetros y la reproduce. |
| `MODO CLARO/OSCURO` | Alterna el tema visual y guarda la preferencia. |
| `CONSOLA` | Muestra eventos, avisos y errores de la interfaz. |
| Versión | Abre información de la aplicación y permite restablecer los ajustes. |

## Atajos de teclado

- `1`: MANTRA
- `2`: STUTTER
- `3`: DEGRADAR
- `4`: CLÍMAX
- `P`: reproducir o detener
- `0` o `Espacio`: detener

Los atajos no se ejecutan mientras el foco está en un botón o control deslizante.

## Persistencia

Los ajustes se guardan automáticamente en `localStorage` con la clave:

```text
switchangel-patternlab-v1.5
```

Se guardan el preset, los valores de los controles, el estado de reproducción y el tema. Para volver a los valores iniciales:

1. Abrí el botón de versión.
2. Elegí **RESETEAR AJUSTES**.

También podés borrar los datos del sitio desde las herramientas del navegador.

## Probar localmente

Cloná el repositorio:

```bash
git clone https://github.com/sheakpeare/switchangel-lab.git
cd switchangel-lab
```

Levantá un servidor estático:

```bash
python3 -m http.server 8000
```

Abrí `http://localhost:8000`.

También podés abrir `index.html` directamente, aunque un servidor local ofrece mejor compatibilidad para portapapeles, compartir y ventanas nuevas.

## Cómo editarlo

La aplicación está concentrada en `index.html`.

### Cambiar la apariencia

Dentro de `<style>` se encuentran las variables principales:

```css
:root {
  --bg: #f4f0e8;
  --surface: #fff;
  --border: #111;
  --accent: #ff4d00;
}
```

- `--bg`: fondo general.
- `--surface`: paneles y botones.
- `--border`: bordes y sombras brutalistas.
- `--accent`: color principal.
- `[data-theme="dark"]`: valores del modo oscuro.
- `--display`: tipografía de títulos y controles.
- `--mono`: tipografía del código y la consola.

Para cambiar la fuente de los títulos, editá `--display`. Si usás una fuente externa, agregá su `<link>` dentro de `<head>` y mantené una fuente alternativa local.

### Agregar o cambiar un preset

1. Agregá una entrada en el objeto `patterns` del script:

```js
const patterns = {
  base: 's("bd*4").fast(SPEED)',
  nuevo: 's("bd sd hh*2").fast(SPEED).gain(0.8)'
};
```

2. Agregá los reemplazos de parámetros que necesite el patrón o reutilizá `SPEED`, `CHOP`, `CRUSH` y `ROOM`.
3. Agregá un botón `.preset` en el HTML con el mismo valor en `data-preset`.
4. Si querés un atajo, incorporá el nombre en el arreglo de la función de teclado.

### Cambiar el enlace de Strudel

La URL está definida en:

```js
const STRUDEL_URL = 'https://strudel.cc/';
```

### Cambiar la versión

Actualizá de forma consistente:

- el texto `v1.5.0` del encabezado;
- el título del modal;
- la clave `switchangel-patternlab-v1.5` si querés iniciar un almacenamiento nuevo;
- el mensaje inicial de la consola;
- este README y el changelog.

## Publicar en GitHub Pages

1. Entrá a **Settings → Pages**.
2. Seleccioná **Deploy from a branch**.
3. Elegí la rama `main` y la carpeta `/ (root)`.
4. Guardá y esperá la publicación.
5. Si no ves cambios, hacé una recarga forzada (`Ctrl/Cmd + Shift + R`) y verificá que Pages esté usando `main`.

## Changelog

### v1.5.0 — Botón de Strudel y tipografía display

- Agregado el botón **PROBAR EN STRUDEL**.
- El botón copia el patrón actual antes de abrir Strudel en otra pestaña.
- Agregado registro de eventos para informar si el copiado fue exitoso.
- Incorporada una tipografía display basada en `Arial Black`, `Arial Narrow` e `Impact`.
- Conservada una fuente monoespaciada para código, consola y valores técnicos.
- Reforzada la estética brutalista con jerarquía tipográfica más marcada.
- Actualizada la clave de persistencia a `switchangel-patternlab-v1.5`.
- Actualizada la versión visible a `v1.5.0`.

### v1.4.0 — Tema y control de reproducción

- Agregado modo claro y modo oscuro.
- Persistencia del tema elegido en `localStorage`.
- Agregado botón **REPRODUCIR / DETENER**.
- Agregado atajo `P` para reproducir o detener.
- Mejorado el estado visual de reproducción.
- Los presets ahora seleccionan y reproducen el patrón.
- `RANDOM` genera parámetros y reproduce la combinación resultante.
- Implementada la estética brutalista: bordes gruesos, sombras duras, alto contraste y botones tipo placa.
- Incorporado soporte para `window.evaluate()` y `window.hush()` cuando existe una integración externa.

### v1.3.0 — Interfaz interactiva

- Rediseñada la interfaz como una botonera responsive.
- Agregados presets de Mantra, Stutter, Degradar y Clímax.
- Agregados controles de velocidad, chop, crush y reverb.
- Agregado copiado de código al portapapeles.
- Agregada función de compartir con fallback al portapapeles.
- Agregado generador de patrones aleatorios.
- Agregada consola expandible.
- Agregado modal informativo.
- Agregada persistencia de controles mediante `localStorage`.
- Agregados atajos de teclado básicos.

### v1.2.0 — Mobile-first

- Rediseño orientado a dispositivos móviles.
- Touch targets mínimos de aproximadamente 44 px.
- Soporte para safe areas en dispositivos con notch.
- Sliders más grandes para interacción táctil.
- Consola inferior tipo drawer.
- Modal responsive.
- Meta viewport optimizada para móvil.

### v1.1.0 — Integración inicial

- Carga dinámica experimental del entorno de Strudel.
- Splash screen con progreso.
- Persistencia inicial en `localStorage`.
- Modal de changelog.

### v1.0.0 — Lanzamiento inicial

- Primera versión de Switch Angel Pattern Lab.
- Botonera básica de presets.
- Controles iniciales para experimentar con patrones.

## Limitaciones conocidas

- Abrir Strudel no pega automáticamente el código dentro de su editor por restricciones de seguridad del navegador; por eso el flujo copia el patrón al portapapeles.
- La reproducción directa depende de que exista una función global `window.evaluate()` compatible.
- La función de compartir depende del soporte del navegador.
- GitHub Pages puede tardar unos minutos en reflejar un commit nuevo.

## Licencia

No se ha definido una licencia para este repositorio. Si querés permitir reutilización explícita, agregá un archivo `LICENSE`.
