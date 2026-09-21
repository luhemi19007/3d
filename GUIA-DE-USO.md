# Guía de uso — Forma

Estudio de diseño 3D que corre en el navegador, con un asistente que
interpreta comandos en lenguaje natural y una preparación básica para
impresión 3D. Esta guía cubre desde subirlo a GitHub hasta el manejo
completo del editor.

## 1. Puesta en marcha

### Si tienes la versión de un solo archivo (`index.html`)
Sube ese único archivo a la raíz de tu repositorio de GitHub. No necesita
ninguna carpeta adicional — el diseño y el código ya están incluidos
dentro de él.

### Si tienes la versión de varios archivos
Mantén exactamente esta estructura de carpetas; las rutas dentro de
`index.html` dependen de ella:

```
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── app.js
│   └── ai-assistant.js
├── README.md
├── LICENSE.md
└── .gitignore
```

Si subes los archivos uno por uno a mano, recrea las carpetas `css/` y
`js/` — si `style.css` o `app.js` quedan sueltos en la raíz, la página
carga sin estilos y sin funcionar (verás texto sin formato y botones
por defecto del navegador).

### Verlo en local
Los módulos de JavaScript no funcionan abriendo el archivo directamente
(doble clic, `file://`) por restricciones del navegador. Sirve la
carpeta como archivos estáticos:

```bash
python3 -m http.server 8000
```

y abre `http://localhost:8000`.

### Publicarlo con GitHub Pages
*Settings → Pages → Source: rama `main`, carpeta `/ (root)` → Save.*
Tu estudio quedará en `https://tu-usuario.github.io/tu-repo/`.

Si algo se ve roto tras publicar: recarga forzada (`Ctrl+Shift+R` /
`Cmd+Shift+R`), el navegador cachea agresivamente.

## 2. El lienzo 3D

| Acción | Cómo hacerlo |
|---|---|
| Orbitar la cámara | Arrastrar con el botón izquierdo (ratón) o un dedo (táctil) |
| Zoom | Rueda del ratón, o pellizcar con dos dedos |
| Seleccionar un objeto | Clic/toque sobre él (sin arrastrar) |
| Deseleccionar | Clic en un área vacía |
| Recentrar la cámara | Botón ⌂ en la barra superior |

## 3. Herramientas de transformación

Tres modos, elegibles desde la barra superior o con teclado:

- **Mover** (`G`) — arrastra las flechas de color del gizmo
- **Rotar** (`R`) — arrastra los anillos
- **Escalar** (`S`) — arrastra los cuadrados

Con un objeto seleccionado, `Supr`/`Backspace` lo elimina. `Ctrl+Z` /
`Ctrl+Y` deshacen y rehacen.

## 4. Añadir formas

Desde el panel izquierdo: cubo, esfera, cilindro, cono, toroide o
plano, con un color aleatorio de la paleta.

## 5. Inspector (panel derecho)

Con un objeto seleccionado puedes editar a mano:
- **Nombre**
- **Posición, rotación, escala** (X/Y/Z)
- **Color** y **material** (estándar, metálico, cristal, mate)
- **Duplicar** o **eliminar** el objeto

## 6. El asistente de diseño

Escribe una frase en la consola inferior y pulsa *Generar*. No usa
ningún servicio externo — interpreta el texto con reglas propias
(`js/ai-assistant.js`), así que funciona sin conexión a internet.

### Comandos simples

| Quieres... | Escribe algo como... |
|---|---|
| Añadir una forma | `añade un cubo rojo`, `crea una esfera de cristal` |
| Mover | `mueve el cubo a 2 0 0` (coordenadas X Y Z) |
| Rotar | `rota la esfera 45` (grados) |
| Escalar | `escala el cono 1.5` |
| Cambiar color | `pinta el cubo de azul` |
| Duplicar | `duplica la esfera` |
| Eliminar | `elimina el cilindro` |
| Vaciar la escena | `limpia todo` |

Colores reconocidos: rojo, azul, verde, amarillo, naranja, morado,
rosa, blanco, negro, gris, cian, marrón, dorado, plateado.

Si el comando no identifica el objeto por nombre (por ejemplo dice
"pinta de azul" sin decir cuál), el asistente usa el objeto que tengas
seleccionado en ese momento en el lienzo.

### Prefabs — construir algo con una sola palabra

El asistente compone automáticamente varias piezas cuando reconoce
alguna de estas palabras en la frase:

`casa` · `árbol` · `muñeco de nieve` · `mesa` · `robot` · `cohete` · `torre`

Ejemplos: `construye una casa`, `haz un árbol verde`, `crea un robot`,
`construye una torre`. También aceptan coordenadas y color, por
ejemplo: `construye una casa azul en 3 0 0`.

## 7. Guardar, abrir y exportar

- **Guardar** — descarga la escena como `.json` (posiciones, colores,
  formas — todo lo necesario para reconstruirla).
- **Abrir** — carga un `.json` guardado previamente.
- **Exportar** — genera un `.gltf`, útil para llevar la escena a
  Blender, Unity u otro software 3D.

## 8. Preparar para impresión 3D

Panel izquierdo, sección *Impresión 3D*:

1. **Ajusta la escala** con el deslizante — define cuántos milímetros
   reales equivale 1 unidad de la escena (por defecto, 10 mm).
2. **"Apoyar todo en la base"** — baja cada objeto hasta que su punto
   más bajo toque Y=0. Es necesario porque una impresora construye
   capa a capa desde el plato y no puede depositar material flotando
   en el aire sin soportes.
3. **"Exportar STL"** — descarga un `.stl` binario, ya escalado según
   el paso 1, listo para abrir en tu laminador (Cura, PrusaSlicer,
   Bambu Studio, OrcaSlicer, Chitubox...).

**Limitación a tener en cuenta:** cada forma se exporta como un sólido
independiente dentro del STL. Si quieres que un conjunto (por ejemplo
la casa completa) imprima como una sola pieza, las formas deben
tocarse o solaparse entre sí — el editor no fusiona geometría con
operaciones booleanas.

## 9. Solución de problemas

| Síntoma | Causa probable | Solución |
|---|---|---|
| Página sin estilos, texto y botones por defecto del navegador | `css/style.css` o `js/app.js` no se encuentran (404) | Revisa que las carpetas `css/` y `js/` existan en el repo con esos archivos dentro, o usa la versión de un solo archivo |
| El lienzo está en negro, nada responde | Módulos de JavaScript bloqueados por `file://` | Sirve los archivos con un servidor local (`python3 -m http.server`) o publícalos con GitHub Pages |
| El asistente no reconoce un comando | Palabra clave no incluida, o texto ambiguo | Revisa la lista de comandos y colores de esta guía; si sigue fallando, abre la consola del navegador (F12) para ver errores |
| El panel derecho se ve cortado o desbordado | Versión desactualizada de los archivos | Asegúrate de tener la última versión de `css/style.css` |
| Cambios que no se reflejan al recargar | Caché del navegador | Recarga forzada: `Ctrl+Shift+R` / `Cmd+Shift+R` |

## 10. Ampliarlo

`js/ai-assistant.js` expone una única función, `interpret(texto, api)`.
Para conectar un modelo de lenguaje real en vez de las reglas actuales,
basta con sustituir su cuerpo por una llamada al modelo pidiéndole una
lista de acciones en JSON y ejecutarlas contra `api` (que ya expone
`addShape`, `removeObject`, `findByName`, etc.). El resto de la
aplicación no necesita cambios.
