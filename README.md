# Creador de add-ons para Minecraft Education

https://marianofernaandez.github.io/-creador-addons/

Página de un solo archivo (`index.html`) que convierte lo que exporta Blockbench en un add-on `.mcaddon`, sin pasar por Bridge.

## Cómo se usa

1. Exportá de Blockbench el modelo (`File → Export → Export Bedrock Geometry`), las animaciones y la textura.
2. Abrí la página y soltá los archivos (todos juntos, la página reconoce cada uno sola).
3. Completá los datos de la entidad y elegí cuándo se usa cada animación.
4. Descargá el `.mcaddon` y abrilo con doble clic.
5. Activá los dos packs en el mundo y escribí `/summon prefijo:nombre` en el chat.

Todo se procesa en el navegador. Los archivos no se suben a ningún servidor.

## Qué se puede configurar

| Opción | Qué hace |
| --- | --- |
| Comportamiento | Pasiva, Neutral, Hostil o Estática. |
| Vida, velocidad, daño | Valores de la entidad. La velocidad se oculta en Estática y el daño solo aparece en Neutral y Hostil. |
| Caja de golpes | Ancho y alto en bloques. Se calcula a partir del modelo y se puede cambiar. |
| Colores del huevo | Colores del huevo de aparición. |
| Tipo de ataque | Cuerpo a cuerpo, a distancia, o ambos (pega hasta cierta distancia y más lejos lanza). Solo con Neutral u Hostil. |
| Qué lanza | Bola de fuego chica (como el blaze), grande (como el ghast) o flecha, en ráfagas de 1 a 10, con alcance, espera entre ráfagas, altura de salida y si las explosiones rompen bloques. |
| Efecto al golpear | Veneno, lentitud, debilidad, ceguera, hambre, náusea o wither, con duración en segundos. Solo con Neutral u Hostil y con ataque cuerpo a cuerpo (o Ambos). |
| Experiencia al morir | Puntos de experiencia. Solo los da si la mata un jugador. |
| Lo que suelta al morir | Lista de ítems con cantidad mínima y máxima y probabilidad. Se elige de una lista o se escribe el id de cualquier ítem del juego. |
| Propiedades | Inmune al fuego, respira bajo el agua, no recibe daño por caída, se quema con el sol. |
| Animaciones | Cuando está quieta, camina, nada, ataca, carga la bola, lanza la bola o siempre activa. La página las asigna sola según el nombre. |

### Ataque a distancia y animaciones de carga

- **Tiempos automáticos:** la carga antes de disparar es lo que dura tu animación «cargar», y el tiempo entre bolas es lo que dura «lanzar». Así coinciden con el movimiento. Se pueden poner a mano destildando «Tiempos según mis animaciones».
- **Una animación por bola:** «Cuando lanza la bola» se repite una vez por cada bola de la ráfaga.
- **Proyectil propio:** el add-on trae su propio proyectil (`nombre_proyectil`), copia de los del juego pero sin dispersión y con la salida a la altura del pecho (70 % del alto de la entidad, o la que pongas). Los proyectiles originales salen a una altura pensada para el ghast y el blaze, y con un cuerpo distinto chocan con el piso.
- **Ambos:** la entidad pega hasta la distancia elegida; más lejos (un bloque más, para que no cambie de modo sin parar) lanza proyectiles.
- **Explosiones:** por defecto no rompen bloques, para usar en el aula.

### Cómo reconoce las animaciones

Lee solo el nombre de cada animación y busca palabras clave (atac, nad, camin, idle, quiet, reposo...). Si no reconoce el nombre, la deja en «No usar» y se elige a mano.

## Guardar y cargar la ficha

El botón **Guardar ficha** descarga un archivo `nombre.ficha.json` con todo: los datos de la entidad, el modelo, la textura y las animaciones. Para hacer otra versión, soltá solo la ficha: el formulario aparece completo, cambiás lo que quieras (efecto, vida, botín, propiedades...) y descargás un `.mcaddon` nuevo. Las fichas hechas con la versión anterior no traen los archivos: con esas hay que soltar también el modelo y la textura.

## Qué genera

- `NOMBRE_BP`: pack de comportamiento (vida, movimiento, IA, ataque, botín, experiencia, propiedades y, si lanza cosas, su proyectil).
- `NOMBRE_RP`: pack de recursos (modelo, textura, animaciones, controlador de animaciones, textos y huevo de aparición).
- Los dos manifests con UUID nuevos y enlazados entre sí.

## Si algo falla en el juego

- Anotá el mensaje de error que muestre Minecraft.
- En «Opciones avanzadas» se puede cambiar la versión mínima del juego (por defecto `1.16.0`) y el prefijo.
- Si un ítem del botín no aparece, probá con otro id: puede que no exista en tu versión de Education.
- La animación «Cuando ataca» tiene que estar con Loop en «Once» en Blockbench; si se repite, el ataque queda trabado.

## Para quien quiera modificarla

El código fuente está separado en `src/core.js` (lógica sin interfaz), `src/app.js` (interfaz) y `src/template.html` (diseño). `build.py` los junta con JSZip en un único `index.html`. Las pruebas están en `tests/`.
