# El Metro

Juego de carrera de un basquetbolista uruguayo. Empezás con 16 años en un cuadro
de la Liga de Ascenso y jugás una temporada por turno hasta que el cuerpo diga
basta. Una partida entera son unos 5 minutos.

**Jugar:** https://elidolo.pages.dev/

(`dmotta98.github.io/elidolo` queda como entorno de test.)

## Cómo se juega

Cada turno es una temporada completa. Antes de jugarla decidís dos cosas:

- **En qué gastás la plata.** Kinesiólogo, chef, preparador físico, psicólogo,
  entrenador de tiro, analista de video, representante. Nunca te alcanza para
  todo, y los precios suben cuando subís de liga.
- **En qué ponés la pretemporada.** Gimnasio, cancha, video, o salir con los
  gurises.

Después apretás jugar y el motor resuelve la temporada: minutos, estadísticas,
campaña del club, y dos a cuatro cosas que te pasan. Lesiones, convocatorias,
peleas con el técnico, ofertas de otros clubes. Lo que contrataste cambia las
probabilidades de todo eso.

No hay forma de ganar. Hay formas de llegar más lejos.

## Cómo funciona por dentro

Un solo archivo HTML sin dependencias ni build. Todo el estado vive en un objeto
y se guarda en `localStorage` (envuelto en `try/catch`, así que si el navegador
lo bloquea el juego sigue andando sin persistir).

Las constantes que controlan el balance están todas arriba del `<script>`:

| Qué | Dónde |
|---|---|
| Clubes por liga | `CLUBES_ASCENSO`, `CLUBES_LUB`, `CLUBES_EXT` |
| Pesos de atributos por posición | `POSICIONES` |
| Precios del carrito | `SERVICIOS` y `MULT_COSTO` |
| Curva de crecimiento por edad | `crecimientoBase()` |
| Probabilidad de lesión | `riesgoLesion()` |
| Eventos aleatorios | `EV` (50) |
| Ranking dentro de la liga | `rankLiga()` |

Cada jugador nace con un **techo** oculto entre 58 y 94. El crecimiento se
escala por `1 - (atributo/techo)^2.2`, así que cuanto más cerca del techo estás,
menos rinde cada hora de gimnasio. Por eso dos partidas con las mismas
decisiones terminan distinto.

### Ojo al tocar el estado del jugador

Si agregás o sacás un campo del objeto que devuelve `nuevaPartida()`, **subí la
versión de `KEY`** (`elmetro_v3` → `elmetro_v4`) y agregá el campo nuevo a
`CAMPOS_REQ`. Si no, las partidas guardadas de la versión anterior se cargan
incompletas y se corrompen sin avisar. `cargar()` valida y descarta cualquier
partida que no tenga todos los campos.

## Correrlo local

No necesita servidor. Abrí `index.html` en el navegador y listo.

## Estado

Primera versión. Solo Liga de Ascenso, LUB y una liga genérica del exterior.
La idea es que el juego sea sobre el deporte y la carrera, no sobre una liga en
particular, así que más adelante se pueden sumar otras.

Pendientes conocidos: comparar tu carrera contra la de otros jugadores de la
liga temporada a temporada, y un modo con varias ligas.

## Aviso

Ficción. Los nombres de clubes son reales pero el juego no tiene ningún vínculo
con esas instituciones, con la FUBB ni con la Liga Uruguaya de Básquetbol. No
representa a personas reales ni pretende reflejar hechos.

Inspirado en la mecánica de los juegos de carrera deportiva por turnos.