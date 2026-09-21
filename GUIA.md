# BATILIGA NFL 26/27 — guía del comisionado

## Qué es esto

Un tablero web de la quiniela. Tres archivos y ya:

| Archivo | Qué hace |
|---|---|
| `data.json` | **Los datos.** Es lo único que cambia cada semana. |
| `data.js`   | Copia de `data.json` que el navegador puede leer. Se genera solo. |
| `index.html`| El tablero. Lee `data.js` y dibuja todo. Casi nunca se toca. |

Regla de oro: **nunca edites `data.js` a mano.** Edita `data.json` y regenera.

## Cómo lo actualizo yo

Cada jueves, viernes, domingo y lunes:

1. Entro a Yahoo con la sesión que dejaste abierta en mi navegador.
2. Saco de `Weekly Performance` los puntos de la semana y de `Group Picks`
   del survival quién cayó.
3. Escribo eso en `data.json`, regenero `data.js` y hago push.
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
cd ~/batiliga && printf 'const DATA = ' > data.js && cat data.json >> data.js && printf ';\n' >> data.js && git add -A && git commit -m "Semana X" && git push
```

## Ver el tablero en tu Mac sin internet

```bash
cd ~/batiliga && python3 -m http.server 8765
```

Y abres http://localhost:8765 en el navegador.

## Lo que el tablero calcula solo

No lo metas a mano, sale de los datos:

- Movimiento de posiciones (▲▼) contra la semana pasada
- Dinero ganado por cada quien en semanales
- Promedio del grupo y mejor semana de la temporada
- Cuánto se ha repartido y cuánto falta por jugarse
- Cuántos siguen vivos en el survival

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
