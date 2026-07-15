# Vectra — Raster a SVG en el navegador

SPA estática que convierte imágenes JPG/PNG/GIF a SVG vectorial, 100% en el cliente.

## Archivos

- `index.html` — estructura y carga de la librería de vectorización
- `style.css` — estilos
- `script.js` — lógica de subida, conversión y descarga

## Librería de vectorización

Usa **[ImageTracer.js](https://github.com/jankovicsandras/imagetracerjs)**, cargada por CDN en `index.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/imagetracerjs@1.2.6/imagetracer_v1.2.6.js"></script>
```

Se eligió sobre `potrace.js` porque potrace es originalmente una librería en C pensada para Node/servidor;
ImageTracer.js está escrita en JS puro específicamente para correr en el navegador, sin WASM ni build step,
lo que la hace ideal para GitHub Pages. Si preferís no depender de un CDN, descargá el archivo
`imagetracer_v1.2.6.js` y colocalo junto a `index.html`, cambiando el `src` a `imagetracer_v1.2.6.js`.

## Parámetros de calidad (modo "máximo detalle")

En `script.js`, la función `runConversion` arma las opciones de ImageTracer:

- `ltres` / `qtres`: umbral de error de línea/curva — controlado por el slider "Detalle de curvas" (valores bajos = más nodos y más fidelidad)
- `pathomit`: se deriva del mismo slider, para no perder trazos finos
- `rightangleenhance`: checkbox "Reforzar ángulos rectos"
- `numberofcolors`: paleta de color (modo Color) o fijo en 2 (modo Blanco y negro)
- El **umbral de brillo** (modo Blanco y negro) se aplica manualmente sobre un `<canvas>` antes de pasarle los píxeles a ImageTracer, replicando el comportamiento clásico de potrace

## Formas de subir una imagen

- Arrastrar y soltar sobre la zona de carga
- Clic para elegir un archivo del dispositivo
- **Pegar desde el portapapeles**: `Ctrl+V` / `Cmd+V` en cualquier parte de la página (funciona sin permisos especiales), o el botón "Pegar desde el portapapeles", que usa la Clipboard API asíncrona (puede pedir permiso la primera vez, y no está disponible en todos los navegadores — en ese caso se sugiere usar el atajo de teclado)

## Publicar en GitHub Pages

1. Creá un repositorio nuevo y subí estos tres archivos (`index.html`, `style.css`, `script.js`) a la raíz (o a `/docs`).
2. En GitHub: **Settings → Pages → Source**, elegí la rama (`main`) y la carpeta (`/root` o `/docs`).
3. Guardá — GitHub te da una URL tipo `https://tu-usuario.github.io/tu-repo/`.
4. No hace falta build, backend ni variables de entorno: todo el procesamiento ocurre en el navegador del visitante.
