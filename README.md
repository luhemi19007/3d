# Forma — Estudio de diseño 3D con asistente de IA

Editor 3D que corre entero en el navegador (Three.js, sin backend) para crear
escenas a partir de primitivas — o describiéndolas en lenguaje natural a un
asistente integrado que interpreta el texto y construye la geometría.

![tecnología](https://img.shields.io/badge/three.js-0.161-8fa3ad)
![sin backend](https://img.shields.io/badge/backend-ninguno-5fd4c4)

## Qué incluye

- **Lienzo 3D** con Three.js: cámara orbital, luces, sombras y una cuadrícula de referencia.
- **Primitivas**: cubo, esfera, cilindro, cono, toroide y plano, con materiales estándar, metálico, cristal y mate.
- **Herramientas de transformación**: mover, rotar y escalar con gizmo interactivo (atajos `G`, `R`, `S`).
- **Inspector** de posición, rotación, escala, color y material del objeto seleccionado.
- **Jerarquía** de la escena, duplicar/eliminar objetos, deshacer/rehacer.
- **Asistente de diseño**: una consola de texto donde escribes comandos y la app los ejecuta. No usa ninguna API externa — interpreta el lenguaje con un conjunto de reglas (`js/ai-assistant.js`), incluyendo "prefabs" que componen varias primitivas a la vez a partir de una sola palabra.
- **Guardar / abrir** la escena como `.json`, y **exportar** a `.gltf` para usar en Blender, Unity, etc.

## Comandos de ejemplo para el asistente

```
añade un cubo rojo
crea una esfera de cristal
construye una casa
construye un árbol verde
haz un robot
construye una torre
mueve el cubo a 2 0 0
rota la esfera 45
escala el cono 1.5
pinta el cubo de azul
duplica la esfera
elimina el cilindro
limpia todo
```

## Cómo ejecutarlo

No hay proceso de build. Solo necesitas servir la carpeta como archivos estáticos
(el navegador bloquea los módulos ES si abres `index.html` con `file://`):

```bash
# con Python
python3 -m http.server 8000

# o con Node
npx serve .
```

Luego abre `http://localhost:8000`.

### Publicarlo con GitHub Pages

1. Sube este repositorio a GitHub.
2. Ve a *Settings → Pages*.
3. En *Source*, elige la rama `main` y la carpeta `/ (root)`.
4. Guarda — tu estudio quedará en `https://tu-usuario.github.io/tu-repo/`.

## Estructura

```
├── index.html          interfaz de la app
├── css/style.css        estilos
├── js/app.js             escena 3D, herramientas, inspector, historial
├── js/ai-assistant.js    intérprete de comandos en lenguaje natural
└── README.md
```

## Ampliarlo con un modelo de lenguaje real

`js/ai-assistant.js` expone una única función, `interpret(texto, api)`, que
recibe la frase del usuario y un pequeño API para manipular la escena
(`addShape`, `removeObject`, `findByName`, etc.). Para conectar un LLM real
(por ejemplo la API de Anthropic) en vez de las reglas actuales, basta con
sustituir el cuerpo de esa función por una llamada al modelo pidiéndole que
devuelva una lista de acciones en JSON, y ejecutarlas contra `api`. El resto
de la aplicación no necesita cambios.

## Licencia

MIT — usa, modifica y redistribuye libremente. Ver `LICENSE.md`.
