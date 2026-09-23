# BATILIGA NFL 26/27 — guía del comisionado

## Qué es esto

Un tablero web de la quiniela. Tres archivos y ya:

| Archivo | Qué hace |
|---|---|
| `data.json` | **Los datos.** Es lo único que cambia cada semana. |
| `index.html`| El tablero. Lee `data.json` y dibuja todo. Casi nunca se toca. |

El tablero pide `data.json` con un sello de tiempo en cada carga, para que
nadie se quede viendo la tabla vieja por el caché de GitHub.

## Cómo lo actualizo yo

Cada jueves, viernes, domingo y lunes:

1. Entro a Yahoo con la sesión que dejaste abierta en mi navegador.
2. Saco de `Weekly Performance` los puntos de la semana y de `Group Picks`
   del survival quién cayó.
3. Escribo eso en `data.json` y hago push.
4. El sitio queda actualizado solo, sin que toques nada.

**Cuando la sesión de Yahoo se venza te aviso**, y nada más te vuelves a loguear
una vez en el panel del navegador. No me pases la contraseña por chat.

## Si lo quieres actualizar tú

Abre `data.json` y busca la parte que te interesa:

- **Puntos de la semana** → en `standings`, el arreglo `semanas` de cada quien.
  El orden es Semana 1, Semana 2, Semana 3... Agrega el nuevo número al final.
  Actualiza también `pts` (la suma) y `w` / `l`.
- **Ganador de la semana** → en `semanales`, cambia `estado` a `"cerrada"`,
  pon el nombre en `ganadores` y los puntos en `pts`.
  Si empatan dos, pon los dos nombres: el premio se parte solo.
- **Alguien cayó del survival** → móvelo de `vivos` a `eliminados` y ponle
  `semana` y `pick_fatal`.
- Sube `semana_actual` y `ultima_semana_cerrada`, y cambia `actualizado`
  a la fecha de hoy en formato `2026-09-28`.

Luego, en la Terminal:

```bash
cd ~/batiliga && git add -A && git commit -m "Semana X" && git push
```

El sitio se actualiza solo un par de minutos después del push.

## Semanas a medias

Mientras una semana no haya terminado, `data.json` lleva un bloque `parcial`
con la semana abierta y qué partido falta. El tablero entonces pinta un aviso
arriba, le pone `*` a esa columna y marca al puntero de la semana como
**PROVISIONAL** — nadie cobra los $500 hasta que cierre.

Cuando termine el último partido: borra el bloque `parcial`, sube
`ultima_semana_cerrada` y pasa al ganador de `lider_parcial` a `ganadores`.

## Ver el tablero en tu Mac

Con `fetch` de por medio, abrir el archivo con doble clic ya no funciona
(el navegador lo bloquea). Levanta el servidorcito:

```bash
cd ~/batiliga && python3 -m http.server 8765
```

Y abres http://localhost:8765 en el navegador.

O de plano el sitio de verdad: **https://wombatido.github.io/batiliga/**

## Lo que el tablero calcula solo

No lo metas a mano, sale de los datos:

- Movimiento de posiciones (▲▼) contra la semana pasada
- Dinero ganado por cada quien en semanales
- Promedio del grupo y mejor semana de la temporada
- Cuánto se ha repartido y cuánto falta por jugarse
- Cuántos siguen vivos en el survival

## Calendario: los días raros de la temporada

La NFL no siempre juega jueves, domingo y lunes. Estas son las fechas de la
temporada 2026/27 que se salen del patrón (hora de León):

| Fecha | Día | Hora | Qué es |
|---|---|---|---|
| 25 nov | miércoles | 7:00 pm | Víspera de Thanksgiving |
| 26 nov | jueves | 11:00 am, 2:30 pm, 6:20 pm | Thanksgiving, tres juegos |
| 27 nov | viernes | 2:00 pm | Black Friday |
| 19 dic | sábado | 4:00 pm y 7:20 pm | Doble sabatino |
| 25 dic | viernes | 12:00 pm, 3:30 pm, 7:15 pm | Navidad, tres juegos |
| 26 dic | sábado | por confirmar | Semana 16 |
| 2 ene | sábado | por confirmar | Semana 17 |
| 9 ene | sábado | por confirmar | Semana 18 (cierre) |

Los horarios "por confirmar" los define la NFL ya avanzada la temporada. La
rutina de días especiales corre esos días de todos modos.

**Domingos con juego internacional temprano** (arrancan 7:30 u 8:30 am y
terminan antes del mediodía): 4, 11, 18 y 25 de octubre; 8 y 15 de noviembre.
Por eso la rutina dominical empieza a las 10 am y no a las 11.

## El link

**https://wombatido.github.io/batiliga/** — repo `wombatido/batiliga`.
Lleva `noindex`, así que no sale en Google: solo lo abre quien tenga el link.

## De dónde salen los datos

- Quiniela: Yahoo Pro Football Pick'em, grupo **19553**
- Survival: grupo **15166**

Ojo: los nombres de los pick sets **no son los mismos** en las dos bolsas.
El mapeo correcto (sacado de los correos registrados en Yahoo) es:

| Quiniela | Survival |
|---|---|
| Wombatido | Batidos pick |
| Potro | El Patrón |
| Chefs | My Rad Pick Set |
| Adrián Reyna | Montepick |
| Eduao | My Bold Pick Set |
| RUBA | ruben's Spectacular Pick Set |
| Ponypicks | Ponysurvivor |

Los demás se llaman igual en ambas.
