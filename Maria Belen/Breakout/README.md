# Breakout retro

Videojuego web 2D tipo Breakout, con estética retro y minimalista. Está hecho con HTML, CSS y JavaScript puro: no necesita instalar nada, ni servidor, ni dependencias.

## Cómo jugar

1. Abrí `index.html` con doble clic. Funciona directamente desde el protocolo `file://`.
2. Mové la paleta y lanzá la pelota.
3. Destruí los 78 ladrillos sin quedarte sin vidas.

### Controles

| Acción            | Cómo                                              |
| ----------------- | ------------------------------------------------- |
| Mover la paleta   | Mouse, flechas izquierda/derecha, o teclas `A`/`D` |
| Lanzar la pelota  | Barra espaciadora, flecha arriba o clic/toque      |

La paleta solo se mueve en **horizontal**. En pantallas táctiles alcanza con arrastrar el dedo sobre el juego.

## Reglas

- **Área de juego:** un marco visible limita la pantalla. La pelota rebota en las paredes izquierda, derecha y superior.
- **Ladrillos:** 78 ladrillos en 13 columnas por 6 filas. Cada choque destruye el ladrillo impactado. Los colores por fila, de arriba hacia abajo, son: rojo, naranja, amarillo, verde, azul y violeta.
- **Vidas:** se empieza con 3, mostradas como corazones en la parte superior. Se pierde una vida cada vez que la pelota toca el límite inferior del marco. Después de perder una vida, la pelota vuelve a la paleta y hay que lanzarla de nuevo.
- **Derrota:** al llegar a 0 vidas aparece el cartel `Perdiste :(` con el botón `Quiero revancha`, que reinicia el juego de inmediato con 3 vidas y 78 ladrillos nuevos.
- **Victoria:** al destruir los 78 ladrillos aparece el cartel `Ganaste :)` con el botón `Voy por más`, que inicia una nueva partida.
- **Sin extras:** no hay sonido, ni música, ni niveles, ni dificultad creciente. La velocidad de la pelota es siempre la misma.

## Estructura del proyecto

```
index.html   Estructura de la aplicación
style.css    Estilos y estética retro
game.js      Reglas del juego y lógica de dominio (sin dependencias del navegador)
ui.js        Renderizado en Canvas, entradas del usuario y sincronización con game.js
test.js      Pruebas unitarias para Node.js
README.md    Esta documentación
```

## Arquitectura

El proyecto separa a propósito las reglas del juego de su presentación.

### `game.js`: lógica de dominio

Contiene la clase `Juego`, que guarda todo el estado (vidas, ladrillos, paleta, pelota y estado de la partida) y aplica las reglas: movimiento, rebotes, destrucción de ladrillos, pérdida de vidas, victoria, derrota y reinicio.

- **No usa** `document`, `window` ni `HTMLCanvasElement`, por lo que se puede ejecutar y probar en Node.js.
- Se carga con una etiqueta `<script>` común. En el navegador expone un único objeto global, `Breakout`. En Node.js se exporta con `module.exports`.
- Los ladrillos activos viven en un `Map`: destruir un ladrillo es eliminarlo del mapa, y el juego se gana cuando el mapa queda vacío.
- Los estados posibles son `esperando`, `jugando`, `derrota` y `victoria`.
- La simulación avanza en sub-pasos cortos para que la pelota nunca atraviese un ladrillo ni la paleta, aunque el cuadro de animación sea largo.
- Los textos de los carteles y botones (`Perdiste :(`, `Quiero revancha`, `Ganaste :)`, `Voy por más`) están definidos aquí, no en la interfaz.

API principal de `Juego`:

| Miembro                     | Descripción                                                        |
| --------------------------- | ------------------------------------------------------------------ |
| `vidas`                     | Vidas restantes                                                    |
| `ladrillos`                 | `Map` de ladrillos activos                                         |
| `estado`                    | `esperando`, `jugando`, `derrota` o `victoria`                     |
| `finalizado`                | `true` si la partida terminó                                       |
| `mensaje` / `textoBoton`    | Textos del cartel de fin de partida (`null` si la partida sigue)   |
| `lanzar()`                  | Lanza la pelota desde la paleta                                    |
| `colocarPaleta(centroX)`    | Centra la paleta en una posición X (limitada al marco)             |
| `moverPaleta(direccion, dt)`| Mueve la paleta en horizontal a velocidad fija                     |
| `actualizar(dt)`            | Avanza la simulación `dt` segundos                                 |
| `destruirLadrillo(id)`      | Elimina un ladrillo y detecta la victoria                          |
| `perderVida()`              | Resta una vida y detecta la derrota                                |
| `reiniciar()`               | Vuelve a 3 vidas, 78 ladrillos y limpia los carteles               |

### `ui.js`: interfaz

Dibuja el juego en el `<canvas>`, muestra un corazón por cada vida, lee teclado, mouse y toque, y muestra el cartel de fin de partida. Ejecuta el bucle de animación (`requestAnimationFrame`) que llama a `actualizar` en cada cuadro. No contiene reglas de juego.

## Pruebas automatizadas

Las pruebas usan el test runner nativo de Node.js (`node:test` y `node:assert`), sin instalar nada. Hace falta Node.js 18 o superior.

```bash
node --test test.js
```

Cubren:

1. **Estado inicial:** 3 vidas, 78 ladrillos activos y partida no finalizada.
2. **Pérdida de vidas y derrota:** la pelota que toca el fondo resta una vida, y al llegar a 0 el estado pasa a derrota (`Perdiste :(`).
3. **Destrucción de ladrillos:** la colisión con un ladrillo lo elimina del mapa de ladrillos activos.
4. **Condición de victoria:** al eliminar el ladrillo número 78 el estado pasa a victoria (`Ganaste :)`).
5. **Reinicio:** `reiniciar()` restablece 3 vidas y 78 ladrillos, y limpia los carteles de victoria y derrota.

## Decisiones de diseño

- **Pelota cuadrada:** encaja con la estética retro y simplifica las colisiones, que se calculan entre rectángulos.
- **Ángulo de rebote en la paleta:** depende del punto de impacto. En el centro la pelota sale casi vertical, y en los bordes sale más inclinada (hasta 60 grados).
- **Lanzamiento con ángulo aleatorio:** la pelota nunca sale en vertical exacta, para evitar rebotes repetitivos. El generador de azar se puede inyectar (`new Juego({ aleatorio })`), lo que hace las pruebas deterministas.
- **Sin dependencias ni fuentes externas:** todo funciona sin conexión a internet.
